# Project as Namespace Groups

Rancher uses Projects to group namespaces and apply a common configuration for RBAC to all of them.  
This reduces the potential for human error and simplifies the tasks associated with creating and managing multiple namespaces in a cluster.

Some Kubernetes resources are assigned to specific namespaces, whereas others can be assigned to a namespace or to the project itself.  
Resources that are assigned to a project are visible to all namespaces within the project.

The following resources can be assigned to a Project:

- ConfigMaps

- Secrets

- Certificates

- Registry Credentials

The following resources can only be assigned to a namespace within the project:

- Workloads

- Load Balancers / Ingress

- Services Discovery records (service)

- Persistent Volume Claims

Rancher automatically creates two projects when you launch a cluster:

- Default.  
This project contains the default namespace and exists for user workloads. You can delete it and create projects and namespaces with more descriptive names if desired. Simple clusters that don't need multiple namespaces also don't need more than the Default project.

- System.  
This project contains system-related namespaces and their workloads. It cannot be deleted.
