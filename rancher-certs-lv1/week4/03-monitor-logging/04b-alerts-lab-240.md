# Configure Alerts in Rancher < 2.5.0
Since from Rancher 2.5.0 the monitoring sections was improved and migrated from Cluster Overview Page to Cluster Explorer Dashboard this guides will be splitted in 2, by showing how to set up a notified alerts in Rancher with a version prior to 2.5.0 and over 2.5.0.  
Here are the guide on how to Configure Alerts on Rancher < 2.5.0

## Configure a notifier
- Navigate to the Cluster view page of the cluster in which you want to install the notifier

- Click on the "Tools" dropdown menu on top and select the "Notifiers" entry

- In the new page click on "Add Notifier"

- Choose the notification type and fill the form that is presented

## Set up a cluster level Alert
To set up a Cluster level alerts in Rancher version prior to 2.5.0, first assign a notifier to the alert group you want to use and then create the alert.

### Assign a Notifiers to an Alert Group in Rancher
Follow this guide to assign a notifiers to a node group alert:

- Navigate to the Cluster view page of the cluster in which you want to install the notifier

- Click on the "Tools" dropdown menu on top and select the "Alert" entry

- Click on the "3 dots" menu in the "Node Alert" group and select "Edit"

- Configure the notifier at the bottom and click on "Save"

### Create a new Cluster level Alert
This step shows how to create an alert for "Node Alert":

- Navigate to the Cluster view page of the cluster in which you want to install the notifier

- Click on the "Tools" dropdown menu on top and select the "Alert" entry

- Click on the "Add Alert Rule" button on the "Node Alert" group

- Call the Alert Rule "Node Aint Alright"


- Fill the forms with the requested data, in this example:
  - set the "When A" to "Normal" and from the dropdown select "Node"
  - set the "Happens" selecting "Warning" option in "Send A"


- Configure the Notifiers time options in the "Advanced Options" in order to get notifications right away:
  - Group Inherited: Disabled
  - Group Wait Time: 5 seconds
  - Group Interval Time: 1 second
  - Repeat Interval Time: 1 hour


- At the end click on "Create" button

## Set up a Project level Alerts
To set up a Project level alerts in Rancher version prior to 2.5.0, first assign a notifier to the alert group you want to use and then create the alert

### Assign a Notifiers to an Alert Group

- Navigate to the Apps page of the cluster which the apps to monitor resides.

- Click on the "Tools" dropdown menu on top and select the "Alert" entry

- Click on "3 dots" menu and sleect "Edit" on the "A set of alerts for workload, pod, container" group

- Configure the notifier at the bottom and click on "Save"

### Create a new Project level alerts
Now, this steps will show how to set an alert on the workloads that fires when pods aren't present. Remember to pause orchestration before deleting all the pods forcefully.

- Navigate to the Apps page of the cluster which the apps to monitor resides.

- Click on the "Tools" dropdown menu on top and select the "Alert" entry

- Click on the "Add Alert Rule" button on the "A set of alerts for workload, pod, container" group

- Call the Alert Rule "These pods arent tide"


- Fill the forms with the requested data, in this example:
  - set the "When A" to "Workload" and from the dropdown select the applicatin to monitor
  - set the "Is less then" to "100% available"
  - set the "Send A" to "Critical"


- Configure the Notifiers time options in the "Advanced Options" in order to get notifications right away:
  - Group Inherited: Disabled
  - Group Wait Time: 1 seconds
  - Group Interval Time: 3 minutes
  - Repeat Interval Time: 1 hour

#### Testing that works
- Go to the Application workload page and select the workload details view
- Click in the "3 dots" menu on the top right and select "Pause Orchestration"
- Now select one or more pod from the leftmost checkbox
- Click on the delete
