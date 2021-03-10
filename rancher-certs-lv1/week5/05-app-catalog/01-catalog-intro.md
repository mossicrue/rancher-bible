# How the Catalog Works

The Rancher Application Catalog integrates Helm and Rancher Apps into a workflow that makes it easy for users to deploy and upgrade applications in their Kubernetes clusters.

When a user requests that Rancher launch an app, Rancher launches an ephemeral instance of a Helm service account with the permissions of the requesting user.  
This prevents a user from acquiring any greater privileges through Helm or an app they are deploying.

Once a Helm repository, known as a Catalog, is added to Rancher, its apps become available for installation at the corresponding scope.

Rancher refreshes the catalogs automatically at a fixed interval.

# Catalog Scope
Catalogs are scoped at the Global, Cluster, and Project levels.  

- Project-scoped catalogs are the most specific, with their apps only visible to that Project.  

- Cluster-scoped catalogs are visible to all Projects in that cluster

- Globally scoped catalogs are available to all clusters and all projects.

Scope is an important feature.  
Certain teams may run their own catalogs with their own applications that should not be visible to other users of the cluster.  
By scoping catalogs at the various levels, global operators can delegate responsibility for catalog management to cluster or project administrators, giving them the power and flexibility to do their work securely.

# Included Global Catalogs
Rancher includes three default catalogs at the Global level, although only one is active by default.

The Library is a curated list of apps for which Rancher provides catalog entries.  
The apps themselves are not supported by Rancher, but the installation instructions, ported from the upstream Helm charts, are maintained by Rancher.

Rancher also includes the Helm Stable and Helm Incubator repositories.  
Global admins can activate these, after which their apps will be available at the cluster and project levels.

## Activate Helm repositories
If you need to activate Helm Stable or Incubators repository follow the next guides depending on how you want to add this.

## Activate Helm repositories in Cluster Explorer

- Click the dropdown menu on the top left of the page and select "Apps & Marketplace"

- Click on the "Chart Repositories" section

- Click on the "3 dots" menu of the desired repositories and click "Enable"

## Activate Helm repositories in Cluster Manager

- Click on the dropdown cluster / project menu on the top and select the cluster where activate the Helm Stable and Incubators repositories

- In the new page click "Tools" on the dropdown menu and select the "Catalogs" entry

- Click on the "3 dots" menu of the desired repositories and click "Enable"

## Add Helm Stable and Incubators repositories
Maybe due to [this change](https://helm.sh/blog/new-location-stable-incubator-charts/), in my lab the Helm Stable and Incubators repositories wasn't present.  

The url to add
These steps shows how to activated him depending on how you want to add this.

### Add Helm repositories in Cluster Explorer
- Click the dropdown menu on the top left of the page and select "Apps & Marketplace"

- Click on the "Chart Repositories" section

- Click on the "Create" button


- In the creation page fill the forms with the requested data.  
Target is always `https` while "Index URL" is:
  - `https://charts.helm.sh/stable` for Helm Stable repository
  - `https://charts.helm.sh/incubator` for the Helm Incubator repository


- At the end click on the "Create" button to add the helm repositories

### Add Helm repositories in Cluster Manager

- Click on the dropdown cluster / project menu on the top and select the cluster where activate the Helm Stable and Incubators repositories

- In the new page click "Tools" on the dropdown menu and select the "Catalogs" entry

- Click on the "Add Catalog"


- Fill the forms with the requested data.  
Helm Version is always "Helm v3" while Catalog URL is:
  - `https://charts.helm.sh/stable` for Helm Stable repository
  - `https://charts.helm.sh/incubator` for the Helm Incubator repository

- At the end click on the "Create" button to add the helm repositories


## Browse a Helm Repository
After you add an Helm Repository to Rancher you can browse the repository's apps

### Browse a Helm Repository in Cluster Explorer

- Click the dropdown menu on the top left of the page and select "Apps & Marketplace"

- Click on the "Charts" section

### Browse a Helm Repository in Cluster Manager

- Click on the dropdown cluster / project menu on the top and select the project where launch an apps

- In the new page click "Apps" on the dropdown menu

- Click on "Launch" button to explore all the available apps from the configured helm repositories
