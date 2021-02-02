# INSTALL RANCHER ON KUBERNETES

Rancher is an application that runs inside of Kubernetes, so when deployed into an RKE cluster, Rancher uses the high availability infrastructure of Kubernetes (etcd and controlplane) to become highly available and resilient to failures.

Prior to version 2.4, Rancher can only be deployed into an RKE cluster and from 2.4 onward it can also be deployed into a K3s cluster.

If deployed into any other Kubernetes distribution or hosted provider solution, it will not operate reliably and is not eligible for support under the Rancher Enterprise Subscription.

## Choose an IP and hostname for Rancher Server
Select a hostname for the Rancher server environment.
The IP address you attach to this hostname later must be reachable by downstream clusters.
Although it's theoretically possible to change this hostname after installation, doing so creates instability in the environment, so it's far better to choose a name like `rancher.mydomain.com` and keep that hostname for the life of the environment.

## Provision an RKE Cluster
We've already deployed an RKE cluster and see how RKE's flexibility means like deploy a single-node cluster and add more nodes later to make it HA.
This installation process has more steps than the Docker container method, but if you're considering using Rancher in production and want to have the most options available for using it in the future, you can follow all of the steps here but with a single node RKE cluster.
At the beginning the cluster won't be highly available, but you can convert it later.

## Deploy a Load Balancer in front of the cluster
Even with a multi-node RKE cluster, you'll be accessing the Rancher server via a single URL. The ingress controller will listen on all nodes in the cluster, so deploy a load balancer to receive traffic for your chosen URL and route it to the hosts in the cluster.

This can be a hardware load balancer, a cloud load balancer like an ELB or NLB, or a software load balancer like Nginx or HAProxy.

Whatever you choose, operate the load balancer at Layer 4 only, passing TCP directly through on 80 and 443. Do not configure the load balance at Layer 7 for HTTP and HTTPS.

Although it's possible to configure TLS termination on the load balancer and communicate between the load balancer and the Rancher server over an unencrypted connection on port 80, this is not a recommended
method of install.

This load balancer will be dedicated to the Rancher server cluster, which will appear inside of Rancher as the “local” cluster. Workloads other than Rancher should never run on the “local” cluster, so each downstream Kubernetes cluster that Rancher manages will need its own load balancer.
After the load balancer has been deployed, configure DNS to point your chosen hostname to the addresses or hostnames of the load balancer, according to the instructions for your chosen architecture.

### Alternate Options For Cloud Native Environments
If the cluster is located on premises or in an environment where you can dynamically attach multiple IP addresses to a node, you can look at [MetalLB](https://metallb.org/) as an option.
MetalLB enables a service of type LoadBalancer on the cluster and assigns a dedicated IP to the service. This service would sit in front of the Ingress Controller and pass traffic to it the same way an external load balancer would.

## Install Rancher with HELM
Rancher is installed with Helm, the package manager for Kubernetes, these instructions cover Helm 3.
All the steps must be done on the Bastion Host, the system where you ran `rke up` to install RKE, and where you have the configuration file for kubectl.
Install Helm and verify that you're pointed at the correct cluster by running kubectl get nodes before continuing.

### Add the Helm Chart Repository
Rancher provides three repositories for Helm charts:

- Latest: Recommended for trying out the newest features. Not recommended for production.
- Stable: Recommended for production environments
- Alpha: Experimental previews of upcoming releases. Definitely not recommended for production.

If you use an Alpha release, there is no support for upgrading to, from, or between releases. Alpha releases are designed to be viewed and then deleted.
