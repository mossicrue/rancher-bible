# Install Apps on Rancher via Helm
This is a guide on how to install apps on Rancher using the `rancher app install` that use helm charts.

## Search for apps Helm Repo
I use ArtifactHub.io, the official helm charts repository from the guys who created helm.
When you find the application you want to deploy, click on the "Install" button on the right.

It will appear a screen, select the Helm version you installed and copy the helm repository url used in the `helm repo add`

For example, for PgAdmin the command to add repository was
```bash
helm repo add runix https://helm.runix.net/
```

So the helm repo is `https://helm.runix.net/`


## Add the helm repository
Now that we have the Helm Repository URL, we must add it to the Rancher catalog.
We can do it in 2 ways:
- using the Cluster Explorer Marketplace, Rancher +2.5.x

- using the Cluster Manager catalog, Rancher < 2.5.x

### Cluster Explorer Marketplace, Rancher +2.5.x
- Open the Cluster Explorer dashboard of Rancher
It is usually located at `https://<rancher-url>/dashboard/`, for me is `https://rancher.mossicrue.com/dashboard/`

- From the Top Left menu choose "Apps & Marketplace"

- In the list on the left, select "Chart Repositories" entry

- Now click on "Create"

- Fill the form and in the "Index URL" paste the repo url retrieved before

- Click "Create" and wait to synch the new products

### Cluster Manager catalog, Rancher < 2.5.x
- Navigate to the "Global" cluster manager page of Rancher

- On the Top toolbar click on "Tool" and select from its dropdown menu the "Catalogs" entry

- Now click on "Add Catalog"

- Fill the form and in the "Catalog URL" paste the repo url retrieved before

- Save all and wait to refresh

- Run rancher app install to install the app
