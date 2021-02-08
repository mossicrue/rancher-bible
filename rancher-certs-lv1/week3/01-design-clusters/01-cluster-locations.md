# Cluster locations

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
