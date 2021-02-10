# Troubleshooting Hosts

## Docker Runtime

You'll also find information in the logs for the container engine itself like Docker or Cri-O.  
This can reveal information about why it's unable to pull an image or why it may be unable to start a container.  
These logs will be on the host, either in an engine-specific location or in a logfile attached to the syslog service.

A good point to start search it's to check the docker services running
```bash
systemctl status docker.service | head -n 14
```

In Linux distribution like Ubuntu or RedHat you can see log with the journalctl command running

```bash
journalctl -u docker.service | less
```

There could be many warnings and logs depsite a healthy Docker engine.
Any error messages should be noted as a possible root cause while warnings should probably only be considered when the issue is known to be in the engine layer.

## Node Conditions

The kubelet maintains a set of sensors about the state of every node.  
If one of these sensors is triggered, the node will emit a "node condition" that indicates an issue is affecting its ability to serve pods.
These sensors report on network availability, disk pressure, memory pressure, PID pressure, and a general "Ready" state for the node.  
You can see these in the output of `kubectl describe` for each of the nodes, or you can use a `go template` to filter and display any nodes with issues.

```bash
kubectl describe node <node-name> | less
```

Ensure that the _"Conditions"_ section output all is ok by checking if all the _"Status"_ values are false and true for the _"Ready"_ condition.

A quick way to check if there is any problem is to use this go template

```bash
kubectl get nodes -o go-template=\
'{{range .items}}{{$node := .}}{{range .status.conditions}}{{if ne .type "Ready"}}{{if eq .status "True"}}{{$node.metadata.name}}{{": "}}{{.type}}{{":"}}{{.status}}{{"\n"}}{{end}}{{else}}{{if ne .status "True"}}{{$node.metadata.name}}{{": "}}{{.type}}{{": "}}{{.status}}{{"\n"}}{{end}}{{end}}{{end}}{{end}}'
```

A sample outpus is:

```
node-b: DiskPressure:True
```

## Kubelet and Worker Node
The kubelet is responsible for invoking containers on the nodes.  
If there are infrastructure issues on a particular node, the kubelet container might show information about this in its logs.

Worker nodes require a healthy kubelet and kubeproxy to ensure pods are successfully scheduled and started on the worker.

First make sure that both of these containers are running.
```bash
docker ps -a -f=name='kubelet|kube-proxy'
```

Example output that all works well is:
```
CONTAINER ID        IMAGE                                COMMAND                  CREATED             STATUS              PORTS               NAMES
158d0dcc33a5        rancher/hyperkube:v1.11.5-rancher1   "/opt/rke-tools/en..."   3 hours ago         Up 3 hours                              kube-proxy
a30717ecfb55        rancher/hyperkube:v1.11.5-rancher1   "/opt/rke-tools/en..."   3 hours ago         Up 3 hours                              kubelet
```

If the kubelet isn't started API changes won't be reflected on the worker and if kube-proxy isn't running the pods on the worker may experience Network issues.
If they are botjh started we can move on to fetching the logs from these containers.

```bash
docker logs kubelet
