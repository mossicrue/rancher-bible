# Persistent Storage Lab

## Persistent Storage in Rancher +2.5.0

### Create Static Persistent Volume

- Click the dropdown menu on the top left of the page and select "Clusetr Explorer"

- In the "Storage" section choose the "PersistentVolumes" entry

- Click on "Create from YAML" and create the file with the requested file. Here an example for a NFS Share

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv0001
spec:
  capacity:
    storage: 5Gi
  accessModes:
  - ReadWriteMany
  nfs:
    path: /tmp
    server: 172.17.0.2
  persistentVolumeReclaimPolicy: Retain
```

- When it's all ready, click on "Create"

### Create Persistent Volume Claim

- Click the dropdown menu on the top left of the page and select "Clusetr Explorer"

- In the "Storage" section choose the "PersistentVolumesClaims" entry

- Click on "Create from YAML" and create the file with the requested file. Here an example for a NFS Share

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: nfs-claim1
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
```

- When it's all ready, click on "Create"

### Add a PVC to Workload

- Click the dropdown menu on the top left of the page and select "Clusetr Explorer"

- In the "Workloads" section choose the entry for the Workloads type you want to deploy: Cronjob, Deployment, DaemonSet, ecc

- Make sure to have "All Namespaces" select on the Namspaces selector at the top

- Find the Workload entry you want to edit and click on the "3 dots" menu at the right

- Select the "Storage" section, select "Add Volume" and choose the "PersistentVolumeClaim" entry

- Now fill the forms with requested data and then click "Save"

### Create a Storage Class

- Click the dropdown menu on the top left of the page and select "Clusetr Explorer"

- In the "Storage" section choose the "StorageClasses" entry

- Click on "Create from YAML" and create the file with the requested file

- When it's all ready, click on "Create"

## Persistent Storage in Rancher <2.5.0

### Create Static Persistent Volume

- Click on the dropdown cluster / project menu on the top and select the cluster where you want add the Persistent Volume

- In the top bar click on the "Storage" dropdown menu and then select "Persistent Volumes"

- In the new page click the "Add Volume" button on the top-right of the page

- Fill the form with the requested data and click on the "Save" button when it's all ready

### Create Persistent Volume Claim

- Click on the dropdown cluster / project menu on the top and select the Project where you want add the Persistent Volume Claim

- In the tab menu with "Workloads", "Load Balancing", "Service Discovery" and "Volumes" click on "Volumes" link

- Click on the "Add Volume" button

- Fill the forms with the requested data and click on "Create" button

### Add a PVC to a Workload

- Click on the dropdown cluster / project menu on the top and select the project where deploy the workloads

- In the new page click "Resources" on the dropdown menu and select the "Workloads" entry

- In the desired workload entry, click on the "3 dots" menu and select "Edit"

- In the new page, expand the Volumes section, click on the "Add Volume" button and then select the "Use an existing persistent volume (claim)" option

- Now, select the Persistent Volume Claim to use and fill the form with the requested data

- When all is ready click on the "Save" button

### Configure a Storage Class

- Click on the dropdown cluster / project menu on the top and select the cluster where you want to configure the Storage Class

- In the top bar click on the "Storage" dropdown menu and then select "Storage Classes"

- Now click on the "Add Class" blue button

- Fill the form with the data requested and then click on "Save" button at the bottom
