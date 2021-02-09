# Designing Clusters

## Cluster locations

Rancher is up and you're ready to deploy a Kubernetes cluster with it.
The first question to answer is where your cluster will live.

### Hosted / Infrastructure Provider
Rancher can deploy clusters into a hosted provider's solution, such as EKS, AKS, or GKE.  
It can also deploy nodes and then deploy Kubernetes into it.  
In both cases it does full lifecycle management of the resources, which means you can add and remove nodes from the Rancher UI, and if you delete the cluster in Rancher, Rancher will delete the infrastructure components out in the provider.

### Custom Provider
If you do your provisioning with Terraform, Ansible, Puppet, Chef, Cloud Init, Shell scripts, autoscaling groups, or anything else, you can use the Custom driver to build those nodes into a Kubernetes cluster.  
The only software requirement on the host is a supported version of Docker.  
The Custom driver gives you a `docker run` command that is unique for the cluster you're creating. You can add it to the end of your provisioner, and when you deploy new hosts, they'll automatically join the cluster.

### Imported Clusters
Finally, if you already have Kubernetes clusters running out there, or if you're using something like K3s, you can import those clusters into Rancher.  
Rancher will give you a kubectl apply command to run against that cluster. This will install the Rancher agent and connect it back to the Rancher server, and that cluster will be available for Rancher to manage.

## Do Provider Differences Matter?
Rancher provides workload management on all cluster types. Anything that can be done with Kubernetes, through Kubernetes, can be done with any managed cluster.  
Limitations with certain cluster types are controlled by Rancher's access to the infrastructure layer or the data plane.  

Infrastructure and Custom clusters both run RKE, so all features are available within Rancher.
Hosted provider clusters include lifecycle management, but because Rancher has no access to the data plane or control plane, it is unable to perform backups or restores for those clusters.

Imported clusters are the most opaque to Rancher. It doesn't know where they're running, how they were built, or how to interact with them outside of the Kubernetes API.  
These clusters do not include lifecycle management or backup/restore processes.

When working with a non-RKE cluster, you will need to use the provider's tools or other tools to provide business continuity for the
clusters.

## Sizing Requirements
It's easy to undersize a Kubernetes cluster: the resource requirements for Kubernetes change as the cluster grows, and the resource requirements for your application are something that only you can know.  

Kubernetes has documentation that recommends the CPU and memory configuration for clusters of different sizes.  

Rancher recommend that control plane, data plane, and worker roles reside on different node pools.  
This allows you to configure and scale them without affecting other roles.

Kubernetes and your applications will always run better with more resources available to them.  
When you start adding monitoring, backups, service mesh, and other cluster components, you'll need bigger machines to provide those services.  

If the cluster is misbehaving, with components failing and not restarting or services frequently moving between hosts, look at the resource constraints. Make sure that you have enough CPU and RAM to do what you're trying to do, and if you're not sure, add more and see if the problems cease.  
Always deploy workloads with CPU and memory reservations and limits, Kubernetes uses the reservations for scheduling and to not oversubscribe nodes. The limits will prevent processes from taking over all of the resources on a node and creating failures in other applications running there.


## Networking and Port Requirements
One network configuration is different from another. Some environments require stronger security than others, so this course won't tell you what you need to do in your environment.  

The [Rancher documentation](https://rancher.com/docs/rke/latest/en/os/#ports) has detailed information on the source and destination port requirements for each role.

Be mindful of locking down the network!  
If you block access that Kubernetes needs, it will manifest as strange communication errors, service failures, and cluster instability.  

Check logfiles and test that nodes in each role are able to communicate with nodes in other roles according to the charts in the documentation.  

Also verify that cross-host networking is available from with Kubernetes by testing that Pods on one host are able to communicate with Pods on other hosts.

## Cluster RBAC Within Rancher
When you create a cluster, Rancher sets you as the Cluster Owner.  
You can add additional users and assign them the role of Cluster Owner, Cluster Member, or a custom permissions set:

- Cluster Owners have full control over everything in the cluster,
including user access.

- Cluster Members can view most cluster-level resources and create
new projects.

- Selecting Custom presents a list of roles that you can assign to the
user.

Users with cluster-level membership can see and interact with all projects on the cluster. For greater control over what users are allowed to see and do, configure access at the Project (namespace group) level
