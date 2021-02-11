# Troubleshooting CNI and Networking

When troubleshooting network or CNI issues, it helps to first validate that the transport layer (layer 4) between all hosts is working correctly.  
If any firewalls or proxies are blocking those connections, the CNI will fail.  
The Rancher documentation has [instructions](https://rancher.com/docs/rancher/v2.x/en/troubleshooting/networking/) that show how to deploy a DaemonSet on all nodes in the cluster and then use those Pods to perform a ping test across the network fabric.

Any problems in the network overlay could cause connectivity issues between nodes in a cluster.

We deploy a daemonset of busybox that never exit to give us the abiltiy to execute commands from all the Pods deployed in the cluster.
Using the following manifest and wait untill the daemonset it's deployed to all nodes, including ETCD and Control Plane, because of its toleration.

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: overlay-test
spec:
  selector:
    matchLabels:
      name: overlay-test
  template:
    metadata:
      labels:
        name: overlay-test
    spec:
      tolerations:
      - operator: Exists
      containers:
      - image: busybox:1.28
        imagePullPolicy: Always
        name: busybox
        command: ["sh", "-c", "tail f- /dev/null"]
        terminationMessagePath: /dev/termination-log
```

Then execute the following bash script from the bastion host.

```bash
#!/bin/bash
echo "=> Start network overlay test"
  kubectl get pods -l name=overlaytest -o jsonpath='{range .items[*]}{@.metadata.name}{" "}{@.spec.nodeName}{"\n"}{end}' |
  while read spod shost
    do kubectl get pods -l name=overlaytest -o jsonpath='{range .items[*]}{@.status.podIP}{" "}{@.spec.nodeName}{"\n"}{end}' |
    while read tip thost
      do kubectl --request-timeout='10s' exec $spod -c overlaytest -- /bin/sh -c "ping -c2 $tip > /dev/null 2>&1"
        EXIT_CODE=$?
        if [ $EXIT_CODE -ne 0 ]
          then echo FAIL: $spod on $shost cannot reach pod IP $tip on $thost
          else echo $shost can reach $thost
        fi
    done
  done
echo "=> End network overlay test"
```

In summary, this scripts iterates through every node in the cluster and reaches out from that nodes busybox pod to all the IP associated with the busybox daemonset.  
Once complete, the final messages will be printed showing the test is finished and ideally new issues will be detected.

A positive test results, will display only the start and the end messages, like this:

```
=> Start network overlay test
=> End network overlay test
```

If a connection fails a line will be printed indicating the target and destination node names to allow further debugging of the underlying networking issues.

```
=> Start network overlay test
Error from server (NotFound): pods "wk2" not found
FAIL: overlaytest-5bglp on wk2 cannot reach pod IP 10.42.7.3 on wk2
Error from server (NotFound): pods "wk2" not found
FAIL: overlaytest-5bglp on wk2 cannot reach pod IP 10.42.0.5 on cp1
Error from server (NotFound): pods "wk2" not found
FAIL: overlaytest-5bglp on wk2 cannot reach pod IP 10.42.2.12 on wk1
command terminated with exit code 1
FAIL: overlaytest-v4qkl on cp1 cannot reach pod IP 10.42.7.3 on wk2
cp1 can reach cp1
cp1 can reach wk1
command terminated with exit code 1
FAIL: overlaytest-xpxwp on wk1 cannot reach pod IP 10.42.7.3 on wk2
wk1 can reach cp1
wk1 can reach wk1
=> End network overlay test
```

If you see error in the output, there is some issue with the route between the pods on the two hosts.  
In the above output the node `wk2` has no connectivity over the overlay network. This could be because the required ports for overlay networking are not opened for wk2.
