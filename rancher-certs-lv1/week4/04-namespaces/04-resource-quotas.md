# Resource Quotas

Resource quotas define the total amount of a particular resource that the project can use.  
This calculation is an aggregate of all namespaces in the project and keeps a single project from monopolizing resources that are shared across the cluster.

When creating a resource quota, you set the project limit as well as a default limit for each namespace. The namespace default is assigned to each namespace automatically but can be adjusted on a namespace-bynamespace basis.

The total of all namespace limits should not exceed the project limit, and this information is visible when adjusting individual namespace quotas.

## Set Projects Resource Quotas
- Navigate to the "Cluster" details page using the Cluster Manager dropdown menu and click on the "Projects/Namespaces" link

- In the project entry you want to update resource quotas click on its ""3 dots"" menu and select "Edit"

- In the "Project Editor" page extend the "Resource Quotas" settings and click on the "Add Quota" button


- Now fill the forms with the value you want to set:
  - Resource Type: set the type of quota, for example "CPU Limit"
  - Project Limit: set the total amount of resoruce for the project
  - Namespaces Default Limit: set the default amount of that resources in a project namespace


- It's possible to add multiple Resources Quotas by re-clicking "Add Quota" button

- At the end of the Resource Quotas creation click on the "Save" button at the bottom of the page

## Set Namespaces Resource Quotas
Not all the namespaces in a project are created equally and it's possible that there aren't equal amount of resources limits between different namespaces in our project

- Navigate to the "Cluster" details page using the Cluster Manager dropdown menu and click on the "Projects/Namespaces" link

- In the namespace entry you want to update resource quotas click on its ""3 dots"" menu and select "Edit"

- Set the Resource Quotas you want customize for this namespace

- At the end of the updates click on the "Save" button at the bottom of the page
