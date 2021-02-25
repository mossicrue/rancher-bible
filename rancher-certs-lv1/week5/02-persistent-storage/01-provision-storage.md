# Provisioning Storage

Applications that need to retain data will need persistent storage.  
Kubernetes delivers this through Persistent Volumes (PVs), Persistent Volume Claims (PVCs), and StorageClasses.

Storage can be provisioned statically or dynamically.

## Static Provisioning
If you already have storage provisioned and wish to use it with your Kubernetes workloads, create a persistent volume (PV) that points to it.  
Creating a PV in Rancher doesn't provision the storage for you. It simply creates an object within the cluster that points to the existing storage.

## Dynamic Provisioning Using Storage Classes
Storage classes are about removing bottlenecks.  
Instead of waiting for someone to provision storage and make it available for your workload, you can request storage dynamically from the cluster when your workload launches.

Cluster admins configure these storage classes with the information on where to acquire storage, and when you make the request, the storage class does the provisioning for you and connects your workload to the storage.

Rancher includes storage class drivers for cloud provider volumes, VMWare vSphere volumes, Rancher Longhorn, and a Custom option.

You can also add storage classes from the App Catalog, where Rancher partners have apps that connect your cluster to storage solutions from HPE, OpenEBS, Portworx, and StorageOS.  
There's also an app that provisions storage dynamically from a larger NFS volume, and more storage apps are available within the upstream stable Helm catalog.

As apps add storage classes, they will show up in the list of available classes when provisioning a workload.

## Using Persisten Storage

Volumes are attached to workloads when they're launched.  
A workload can create a new PersistentVolumeClaim (PVC) that points to an existing PV or requests a new PV to be created according to a particular storage class.

Once the workload has been created, the PV will be mounted inside the Pod at the designated mountpoint.
