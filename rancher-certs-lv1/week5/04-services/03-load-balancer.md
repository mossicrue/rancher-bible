# The LoadBalancer Service

For clusters deployed in a cloud provider, and which have been properly configured to interface with the cloud provider's API, Kubernetes deploys an instance of the provider's load balancer and configures it to route traffic to the cluster.

Rancher provides cloud provider configuration for AWS, GCE, and Azure, with other cloud providers manually configurable via the Custom option.

In addition to AWS and Azure, the Rancher documentation has detailed information on the required steps for vSphere and Openstack.

If you operate in an on-premises environment and wish to configure LoadBalancer services for your workloads, you can use MetalLB.

This application accepts a pool of addresses and assigns them to Services. It then announces these addresses via ARP or BGP.  
When traffic arrives for the address, MetalLB routes it to the corresponding backend pods for the service.
