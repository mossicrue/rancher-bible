# Workloads Labs

## Workloads in Cluster Explorer - Rancher +2.5.0

### Deploy Workloads
- Click the dropdown menu on the top left of the page and select "Clusetr Explorer"

- In the "Workloads" section choose the entry for the Workloads type you want to deploy: Cronjob, Deployment, DaemonSet, ecc

- Make sure to have "All Namespaces" select on the Namspaces selector at the top

- In the new page click on the "Create" button

- Fill the various forms with the needed settings, it's also possible to switch to "Edit as YAML"

- At the end of the creation phase click on "Create"

### Update Workloads

- Click the dropdown menu on the top left of the page and select "Clusetr Explorer"

- In the "Workloads" section choose the entry for the Workloads type you want to deploy: Cronjob, Deployment, DaemonSet, ecc

- Make sure to have "All Namespaces" select on the Namspaces selector at the top

- Find the Workload entry you want to edit and click on the "3 dots" menu at the right

- Here you can choose to edit the workload via forms or yaml

- Update the data you need to change and then click the "Save" button

### Delete Workloads

- Click the dropdown menu on the top left of the page and select "Clusetr Explorer"

- In the "Workloads" section choose the entry for the Workloads type you want to deploy: Cronjob, Deployment, DaemonSet, ecc

- Make sure to have "All Namespaces" select on the Namspaces selector at the top

- Find the Workload entry you want to edit and click on the "3 dots" menu at the right

- Now select the "Delete" entry and wait for the workload to be terminated

## Workloads in Cluster Manager - Rancher <2.5.0

### Deploy Workloads

- Click on the dropdown cluster / project menu on the top and select the project where deploy the workloads

- In the new page click "Resources" on the dropdown menu and select the "Workloads" entry

- Click the "Deploy" button at the right top of the page

- In the new page, fill the various forms with the needed settings

- At the end of the creation phase click on "Launch"

### Update Workloads

- Click on the dropdown cluster / project menu on the top and select the project where deploy the workloads

- In the new page click "Resources" on the dropdown menu and select the "Workloads" entry

- In the desired workload entry, click on the "3 dots" menu and select "Edit"

- In the new page, make all the change you need and click the "Save" button at the bottom of the page

- Wait for the workload to be re-deployed with a new version

### Rollback Workloads

- Click on the dropdown cluster / project menu on the top and select the project where deploy the workloads

- In the new page click "Resources" on the dropdown menu and select the "Workloads" entry

- In the desired workload entry, click on the "3 dots" menu and select "Rollback"

- In the dialog box, choose the revision to rollback and then click the "Rollback" button

- Wait for the workload to be re-deployed with the revision selected

### Delete Workloads
- Click on the dropdown cluster / project menu on the top and select the project where deploy the workloads

- In the new page click "Resources" on the dropdown menu and select the "Workloads" entry

- In the desired workload entry, click on the "3 dots" menu and select "Delete"

- In the dialog box, click the "Delete" red button

- Wait for the workload to be terminated
