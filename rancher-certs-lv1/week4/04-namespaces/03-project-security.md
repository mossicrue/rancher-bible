# Project Security

## User Authorization
Users who have access to a project have access to all of the namespaces and resources inside of it.  
Project members can be given four levels of access:

- Owner  
This account has full control over the project and all its resources.

- Member  
This account can manage resources in the project but cannot change the project itself.

- Read Only  
This account can view resources in the project but cannot change them.

- Custom  
This account has access according to the specific individual roles selected for it.

### Add a Project member
This procedure assumes that the user is already present in Rancher.  
If not, create them from the Global cluster page and selecting "Users" entry from "Security" in the dropdown menu.

- Navigate to the "Project" page by selecting the Cluster and the Project from the drop-down menu on the top of the Cluster Manager page

- In the new page then click on "Members" from the entry in the same drop-down menu at the top of the page

- Click on "Add Member" button on the top-right of the page

- Now fill the form with the requested data: the user to add and their role

> **NOTE:** If you grant to a project member a set of custom permission, Rancher will create a record for any permission granted to that users.   
eg: If we grant to "User-A" the "View Secrets" and "View Ingress", Rancher will generate the "User-A View Secrets" entry and the "User-A View Ingress" entry


## Network Isolation
If the cluster was launched with Canal as its network provider, Project Network Isolation is an available option.  
If you select this option, resources in one project will not be able to see or communicate with resources in any other project.  
This creates single-cluster multitenancy, because users can only see the resources in their project, and those resources can only see other resources in the same project.

If Project Network Isolation is enabled, it does not apply to the System project or its namespaces. Resources that run within this project have access to all resources in other projects to operate correctly.

## Pod Security Policies
Rancher allows operators to assign Pod Security Policies to Projects but for the best management experience and easy troubleshooting recommends that PSPs are only assigned to clusters.

Whether you adjust PSPs at the project or cluster level, remember that they only apply to workloads that are created after the PSP is applied. If you have any running workloads, redeploy them from the Rancher UI.
