# Rancher’s API Server

When troubleshooting Rancher itself, the Rancher API server is the first place to look in case of problem.  
If you're running an HA Rancher cluster, there will be three pods running the API server, so you'll need to check each of them for logs that might show signs of a problem.  
You can view the logs of the pods running within the cluster by looking at pods in the cattle-system namespace with the label app=rancher.
```bash
kubectl logs -n cattle-system -l app=rancher
```

## Troubleshooting Checklist
Following is a list of all the checks that are suggested to perform for analyze and try to fix the problem you are facing.

### Accessing the cluster
First you’ll need cluster access. Depending on the state of the system how to access the methods may vary.

#### UI Access
If we are using an account that can access to the UI, the logs we’ll be looking at will be as follow:
- Navigate to the local cluster.
- Select the system project.
- From "Resources" drop down menu, select the workloads.
- Necessary resources will be in the cattle-system namespace

#### CLI Access
If the Rancher Authentication Proxy is still running, you can use the kubeconfig file provided by Rancher and make the troubleshooting by using the `kubectl` command to check the current situation.

#### Authentication Proxy down
If the Authentication Proxy is down, you’ll need to connect to the cluster directly.  
There are a number of ways to do this, some can be founded [here](https://rancher.com/docs/rancher/v2.x/en/cluster-admin/cluster-access/kubectl/#authenticating-directly-with-a-downstream-cluster). An [Authorized Cluster Endpoint](https://rancher.com/docs/rancher/v2.x/en/cluster-admin/cluster-access/ace/) is how this is accomplished with RKE.

### Rancher pods
Rancher pods are deployed as a Deployment in the cattle-system namespace.  
Check if the pods are running on all nodes:
```bash
kubectl -n cattle-system get pods -l app=rancher -o wide
```

Example output:
```
NAME                       READY   STATUS    RESTARTS   AGE   IP          NODE
rancher-7dbd7875f7-n6t5t   1/1     Running   0          8m    x.x.x.x     x.x.x.x
rancher-7dbd7875f7-qbj5k   1/1     Running   0          8m    x.x.x.x     x.x.x.x
rancher-7dbd7875f7-qw7wb   1/1     Running   0          8m    x.x.x.x     x.x.x.x
```

If a pod is unable to run (Status is not Running, Ready status is not showing 1/1 or you see a high count of Restarts), check the pod details, logs and namespace events.

### Pods details
To gather additional informations from the Rancher pods, check their details running:

```bash
kubectl -n cattle-system describe pods -l app=rancher
```

### Namespace Events
Namespaces will aggregate events for the various pods. This will also show events like the frequency of restarts.
To check this kind of data run:
```bash
kubectl -n cattle-system get events
```

### Rancher Ingress
The next layer that could be affected is the Rancher ingress.  
Ingress should have the correct HOSTS (showing the configured FQDN) and ADDRESS (host address(es) it will be routed to).
```bash
kubectl -n cattle-system get ingress
```
Example output:
```
NAME      HOSTS                    ADDRESS                   PORTS     AGE
rancher   rancher.mossicrue.com    x.x.x.x,x.x.x.x,x.x.x.x   80, 443   2m
```

### Ingress Controller
When accessing your configured Rancher FQDN does not show you the UI, check the ingress controller logging to see what happens when you try to access Rancher.  
The Ingress Controller that handles the Rancher ingress rule is in the `ingress-nginx` namespace.

```bash
kubectl -n ingress-nginx logs -l app=ingress-nginx
```

### Leader Election
Finally, if Rancher is misbehaving, they’ll be a significant bouncing of the leader election.  
The leader is determined by a leader election process. After the leader has been determined, the leader (holderIdentity) is saved in the `cattle-controllers` ConfigMap (in this example, rancher-7dbd7875f7-qbj5k).

```bash
kubectl -n kube-system get configmap cattle-controllers \
-o jsonpath='{.metadata.annotations.control-plane\.alpha\.kubernetes\.io/leader}'
```

Example output:
```
{
  "holderIdentity":"rancher-7dbd7875f7-qbj5k",
  "leaseDurationSeconds":45,
  "acquireTime":"2019-04-04T11:53:12Z",
  "renewTime":"2019-04-04T12:24:08Z",
  "leaderTransitions":0
}
```
