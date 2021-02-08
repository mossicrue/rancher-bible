# Cluster sizing

It's easy to undersize a Kubernetes cluster: the resource requirements for Kubernetes change as the cluster grows, and the resource requirements for your application are something that only you can know.  

Kubernetes has documentation that recommends the CPU and memory configuration for clusters of different sizes.  

Rancher recommend that control plane, data plane, and worker roles reside on different node pools.  
This allows you to configure and scale them without affecting other roles.

Kubernetes and your applications will always run better with more resources available to them.  
When you start adding monitoring, backups, service mesh, and other cluster components, you'll need bigger machines to provide those services.  

If the cluster is misbehaving, with components failing and not restarting or services frequently moving between hosts, look at the resource constraints.  
Make sure that you have enough CPU and RAM to do what you're trying to do, and if you're not sure, add more and see if the problems cease.  
Always deploy workloads with CPU and memory reservations and limits, Kubernetes uses the reservations for scheduling and to not oversubscribe nodes.  
The limits will prevent processes from taking over all of the resources on a node and creating failures in other applications running there.
