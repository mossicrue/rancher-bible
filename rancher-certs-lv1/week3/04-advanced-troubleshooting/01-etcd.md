# ETCD Troubleshooting
The next place to look is etcd. It stores the state for Kubernetes and the Rancher application.  
Things to look for are:
- Is etcd constantly electing new leaders?  
This might indicate that there's something causing leaders to fail.

- Are all the etcd nodes members of the same cluster?  
If there's been a node partition, they may be trying to connect to each other but are finding that their cluster IDs are different

- Are there any active alarms in etcd that might prevent the cluster from receiving write data?

Etcd on RKE runs as a container, so you can log directly into each of the data plane nodes and look at the log output for this component with the docker logs command.  
```bash
docker logs etcd
```

To check for alarms, use the `etcdctl` command within each of the etcd containers.  
```bash
docker exec etcd etcdctl alarm list
```

Etcd can also manifest poor performance as a result of disk latency.  
Etcd requires fast responses from its members, so if an etcd member is unable to commit writes to disk fast enough, it will fall behind its peers.  
When its peers perceive that lag as being unresponsive, they might eject it from the cluster.

To avoid this, ensure that etcd nodes have enough available IO performance available. You can use a tool like [fio](https://github.com/axboe/fio) to benchmark the performance of your disks and decide if they meet the requirements.
