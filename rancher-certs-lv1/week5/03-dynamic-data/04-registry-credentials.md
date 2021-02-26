# Registry Credentials

A Docker Registry as a private registry that holds Docker container images.  
A Kubernetes registry is the set of credentials needed to access a particular Docker Registry.  
Workloads use the Kubernetes registry to access the Docker registry, pull an image from it, and deploy it.

The Kubernetes registry secret is automatically included if you deploy workloads from the UI that need to access a private Docker registry.

If you deploy workloads with kubectl, you need to specify the secret with the imagePullSecrets key on the Pod spec.

## Create Registry Credentials in Cluster Explorer

- Click the dropdown menu on the top left of the page and select "Clusetr Explorer"

- In the "Storage" section choose the "Secrets" entry

- Now click on the "Create" button the top right and choose the "Registry" secret type

- Fill the forms with the requested data and remember to select the Registry to connect with the creating credentials.  
In case of a custom or Artifactory registry Rancher will prompt you to add also the registry URL address

## Create Registry Credentials in Cluster Manager

- Click on the dropdown cluster / project menu on the top and select the project where create the ConfigMap

- In the new page click "Resources" on the dropdown menu and select the "Secrets" entry

- Click on the "Registry Credentials" tab link to switch the using resources to "Registry Credentials"

- Click on the "Add Registry" button

- Fill the forms with the requested data and remember to select the Registry to connect with the creating credentials.  
In case of a custom registry Rancher will prompt you to add also its URL address

- When it's all ready, click on the "Save" button
