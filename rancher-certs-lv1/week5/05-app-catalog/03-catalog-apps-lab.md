# Catalog Apps Lab

## Helm Application Lifecycle in Cluster Explorer
Following a list of operation to perform for handle the lyfecicle of a Helm Rancher App like deploy, clone, upgrade, rollback and delete in Cluster Explorer.

### Deploy a Helm Application

- Click the dropdown menu on the top left of the page and select "Apps & Marketplace"

- Select the "Charts" section from the menu on the left.

- In the new page, click on the Helm App to install

- Fill the requested forms and choose the namespace in which deploy the application


- Add all the other data in the "Values YAML" sections.  
For this example I add this 2 values:
  - `env.mail = mossicrue@gmail.com`
  - `persistentVolume.enabled = false`


- When it's all ready click on the "Install" button and follow the logs in the terminal that will be displayed

### Cloning a Helm Application
Currently in the 2.5.6 version that I'm using while writing this notes there isn't any easy clone feature in Cluster Explorer like the one present in Cluster Manager.

### Upgrade a Helm Application

- Click the dropdown menu on the top left of the page and select "Apps & Marketplace"

- Select the "Installed Apps" section from the menu on the left.

- Now click on the "3 dots" menu of the application entry you want to upgrade and select "Edit/Upgrade"

- In the new page select the new application versions and then click the "Upgrade" button

### Rollback a Helm Application
Currently in the 2.5.6 version that I'm using while writing this notes there isn't any easy rollback feature in Cluster Explorer like the one present in Cluster Manager.  
It's possible to rollback the deployment generated in the Cluster Manager but I don't think it's so secure to do.

### Delete a Helm Application

- Click the dropdown menu on the top left of the page and select "Apps & Marketplace"

- Select the "Installed Apps" section from the menu on the left.

- Now click on the "3 dots" menu of the application entry you want to upgrade and select "Delete"

- In the modal menu click on the "Delete" button

## Helm Application Lifecycle in Cluster Manager
Following a list of operation to perform for handle the lyfecicle of a Helm Rancher App like deploy, clone, upgrade rollback and delete in Cluster Manager.

### Deploy a Helm Application

- Click on the dropdown cluster / project menu on the top and select the project where launch an apps

- In the new page click "Apps" on the dropdown menu

- Click on "Launch" button to explore all the available apps from the configured helm repositories

- Select the application you want deploy, for example pgAdmin

- Select not the latest release on the "Template Version"


- Add all the other requested data in the  "Answers" section.
For this example I add this 2 values in the "Variables" sub-section:
  - `env.mail = mossicrue@gmail.com`
  - `persistentVolume.enabled = false`


- When it's all ready click on "Launch" button, a new namespace containing the app and all the necessary objects like services ecc

### Cloning a Helm Application

- Click on the dropdown cluster / project menu on the top and select the project where cloning an apps

- In the new page click "Apps" on the dropdown menu

- Click the "3 dots" button of the Rancher Apps you want clone and then slect the "Clone" entry

- In the new page check that all the data are the same used

### Upgrade a Helm Application

- Click on the dropdown cluster / project menu on the top and select the project where upgrade the app

- In the new page click "Apps" on the dropdown menu

- Click the "3 dots" button of the Rancher Apps you want clone and then slect the "Upgrade" entry

- In the same forms of "Deploy" and "Clone" choose the new "Template Versions" to apply

- Click on the "Upgrade" button and a new version of the app will roll out

### Rollback a Helm Application
- Click on the dropdown cluster / project menu on the top and select the project where rollback the app

- In the new page click "Apps" on the dropdown menu

- Click the "3 dots" button of the Rancher Apps you want clone and then slect the "Rollback" entry

- In the modal form choose the revision to where do the rollback

- Click on the "Rollback" button and wait for the Applications to return to an earlier revision

### Delete a Helm Application
- Click on the dropdown cluster / project menu on the top and select the project where delete the app

- In the new page click "Apps" on the dropdown menu

- Click the "3 dots" button of the Rancher Apps you want clone and then slect the "Delete" entry

> **NOTE:** After delete the app, Kubernetes may take few minutes to run any finilazer and delete the actual resources
