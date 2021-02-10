# Troubleshooting Controlplane Nodes
The control plane is where the cluster-wide API and logic engines run.  
We can check that these are running correctly by first looking at their logs, and like with worker and etcd roles elements, we do this on the hosts themselves.

## Kube-Scheduler and Controller Manager
Some of the components use leader election, so only one at a time is handling the requests. Before we can analyze the log data, we first need to determine which is the leader.

To find which Pods hold the leader of the kube-controller-manager run
```bash
kubectl get endpoints -n kube-system kube-controller-manager \
-o jsonpath='{.metadata.annotations.control-plane\.alpha\.kubernetes\.io/leader}{"\n"}'
```
To find whihc Pods hold the leader of the kube-scheduler, instead, run
```bash
kubectl get endpoints -n kube-system kube-scheduler \
-o jsonpath='{.metadata.annotations.control-plane\.alpha\.kubernetes\.io/leader}{"\n"}'
```

These commands will output something like this:
```json
{"holderIdentity":"node-a_ab79fcef-0eef-48b6-803c-0f99b58dbde3","leaseDurationSeconds":15,"acquireTime":"2021-02-10T20:57:47Z","renewTime":"2021-02-10T22:44:00Z","leaderTransitions":5}
```
The leader information is under the holderIdentity.

Now I can retrieve kube-controller-manager leader container logs on the leader host running:
```bash
docker logs kube-controller-manager
```
For retrieve kube-scheduler leader container logs on the leader host run:
```bash
docker logs kube-scheduler
```

## API Server
Some kubernetes components elect a leader and have some passive replicas.  
The API server runs multiple active replicas in parallel, this means that the API server will handle requests with all of its containers, similar to the Rancher API we looked at earlier.  
To pull its log data we can look at each of the nodes, but because any one of them might be the source of the issue, we'll have to look at the logs for every container.
