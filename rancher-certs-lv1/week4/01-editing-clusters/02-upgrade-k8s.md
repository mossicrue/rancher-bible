# Upgrading Kubernetes

Kubernetes releases minor versions at regular intervals and patch versions as necessary. Rancher releases minor version updates with each new Rancher versions. For example, Rancher 2.3.5 supports Kubernetes v1.15.4, v1.14.7, and v1.13.11.  
After upgrading the Rancher server, new minor versions may be available for installation and upgrade of downstream clusters.  
Patch versions are available independent of Rancher server upgrades.

The Rancher server regularly syncs the Kubernetes version metadata, and if a cluster permits upgrades to new patch versions of Kubernetes, cluster admins can perform those upgrades as soon as the upgrades are available.

When performing any upgrade of Kubernetes, first take a backup.

Kubernetes upgrade with RKE doesn't require you to take the entire system down for maintenance and can be done live by upgrading one node at a time and shifting the workloads by draining the "in maintenance" node.

To update directly from the "Edit Cluster" view:
- Navigate to the Global cluster page
- On the "3 dots" menu of the cluster you want update and select Edit
- Under Cluster Options, change the Kubernetes Version value
- Scroll to the bottom of the page and click Save

Once the updates are saved, the cluster will rebuild one node at a time, allowing it to continue to stay in service
