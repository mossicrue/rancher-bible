# Editing Cluster Options

After provisioning a cluster with Rancher you can edit it and change many of this options.  
You can edit cluster membership for hosted, imported and rke clusters (deployed via the infrastructure or custom options).  
For the RKE clusters you can also edit cluster options and manage node pools.

The only option that you cannot change after installation is the network provider.

## Best practice for editing node roles
If you want to split a 3 nodes all of them with all the 3 roles in order to have 3 "master" (etcd and control-plane) and 3 worker nodes we have to follow this step:
- Add a new node pool with "worker" role using the same template of the first pool
- After the new workers are ready, drain the old nodes in order to avoid loss of data and servie
- Edit the first node pool roles by removing the "worker" role

In the Edit Cluster UI we can also configure a lot of other options like Nginx Ingress, Pod Security Policy, etcd snapshots and their storage locations
