# Resource Limits

When you set CPU or memory resource quotas on a project, this propagates to all of its namespaces. Workloads deployed in one of those namespaces will require the corresponding field be set during creation, or if the workloads already exist, users will have to set the limits when they next edit the workload.

To make this process smoother, the project can define a default container resource limit for CPU and Memory. Workloads which do not set a limit of their own will inherit this default.

The defaults will be applied to any namespace created after the project defaults are set. Any namespace that already exists will need to be adjusted manually for defaults to be applied to workloads running within it.

Container default resource limits are an excellent way to make sure no single workload can spin out of control and consume all of the available resources on a cluster. Many users forget to set these limits on their workloads, so having a reasonable default set on the project will help keep the cluster secure.

Be careful not to set a threshold that is too low, or else the cluster will regularly restart workloads that use more resources as part of their normal operation.

## Set a default container resource limits for a Project
After creating the Resource Quotas, we have to set the Resource Limits

- Navigate to the "Cluster" details page using the Cluster Manager dropdown menu and click on the "Projects/Namespaces" link

- In the project entry you want to update resource quotas click on its "kebab" menu and select "Edit"

- In the "Project Editor" page extend the "Container Default Resource Limit" settings and set the default resoruce limits you need


> **NOTE:** After adding a default container limits we can't update any workloads since no resource limits are provided to that workload.
