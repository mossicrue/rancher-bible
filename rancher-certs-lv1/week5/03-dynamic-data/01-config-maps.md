# Dynamic Data
Our applications are likely going to have depnedencies on small bits of code that we want to reuse across the pods in our environment.  
Some of these will be more senstive in nature than others, which is why Kubernetes has developed the ConfigMap, Secret, and Certificate resource types.  
Each of them stores an artifact of code, but each one does it slightly differently.

The tooling around these resources enables you to modify them without restarting container workloads, and in the case of secret-type data (Secrets and Certificates), adds additional attributes to limit their visibility in the cluster.

# ConfigMaps
A ConfigMap is simply a set of key/value pairs that Kubernetes stores
for us.  
The Rancher UI lets us create a new ConfigMap or edit existing ones, and once a ConfigMap has been assigned to a Workload, it shows us which workloads are using it.

The interfaces for ConfigMaps and Secrets both support pasting in multiple lines of `key=value` from the clipboard, and the ConfigMap interface also supports loading `key=value` pairs from an external file.

ConfigMaps can only be assigned to a namespace. They cannot be assigned to a project.

> **NOTE:** When copy-paste key value pairs remember that the only 2 supported formats for the pairs are:
> - key = value
> - key=value
>
>The following formats, instead, aren't supported:
> - key:value
> - key,value

## Create a ConfigMap in Cluster Explorer

- Click the dropdown menu on the top left of the page and select "Clusetr Explorer"

- In the "Storage" section choose the "ConfigMaps" entry

- Now click on the "Create" button the top right and fill the forms with the requested data

- Fill the "Data" section with `key=value` pairs that need to be in the ConfigMap

- Once it's all ready, click on the "Save" button

## Create a ConfigMap in Cluster Manager

- Click on the dropdown cluster / project menu on the top and select the project where create the ConfigMap

- In the new page click "Resources" on the dropdown menu and select the "Config" entry

- Click on the "Add Config Map" button on the top right and fill the forms with the requested data

- Fill the "Config Map Values" section with `key=value` pairs that need to be in the ConfigMap

- Once it's all ready, click on the "Save" button
