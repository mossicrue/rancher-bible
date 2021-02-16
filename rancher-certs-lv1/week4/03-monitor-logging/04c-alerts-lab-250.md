# Configure Alerts in Rancher +2.5.0
Since from Rancher 2.5.0 the monitoring sections was improved and migrated from Cluster Overview Page to Cluster Explorer Dashboard this guides will be splitted in 2, by showing how to set up a notified alerts in Rancher with a version prior to 2.5.0 and over 2.5.0.  
Here are the guide on how to Configure Alerts on Rancher +2.5.0

## Configure a Receiver
- In the top left of the Dahsboard page click on the "Cluster Explorer" drop-down menu

- Now select the "Monitoring" entry

- Click on "Receiver" link in the left menu entries

- In the new page click on the "Create" button

- Choose the notification provider you want to use

- Fill the form with the requested data and click "Create"

## Configure a Route

- In the top left of the Dahsboard page click on the "Cluster Explorer" drop-down menu

- Now select the "Monitoring" entry

- Click on "Route" link in the left menu entries

- Fill the form for "Receiver" section by selecting the desired receiver in the dropdown menu

- Fill the form for "Grouping" section setting:
  - set the "Group By" value with the labels by which incoming alerts are grouped together, in this example: `test-pod-unavailability-alert`
  - set the "Group Wait" time to "5s"
  - set the "Group Interval" time to "1s"
  - set the "Repeat Interval" time to "4h"

- Fill the form for "Matching" section setting this:
  - set the "Match" key/value fields to a set of equality matchers used to identify the alerts, in this example: `alert-app: simple-app`

- Finally, create the Route by clicking on "Create"

> **NOTE:** More info on how to configure a receiver is present in the [Alert Manager documentation page](https://rancher.com/docs/rancher/v2.x/en/monitoring-alerting/v2.5/configuration/alertmanager/)

## Create a Prometheus Rules

- In the top left of the Dahsboard page click on the "Cluster Explorer" drop-down menu

- Now select the "Monitoring" entry

- Click on the "Advanced" menu on the left to reveal new contents

- Select "Prometheus Rules" entry and click on the "Create" button

- Select the namespace for the rules and insert a name and, optionally, a description for the rules

- Fill the "Rule Group 1" section with settings:
  - set the "Group Name"
  - click on "Add Alert" to add an Alerting Rules
  - set an "Alert Name"
  - set the "PromQL Expression" with the value `kube_deployment_status_replicas_available{deployment="simple-app"} < kube_deployment_spec_replicas{deployment="simple-app"}`
  - set the "Severity" value of the alert

> **NOTE:** In order to help users to create their custom Prometheus rules, Rancher has created this [page](https://rancher.com/docs/rancher/v2.x/en/monitoring-alerting/v2.5/configuration/expression/) with some useful PromQL example. Also, in the kubernetes documentation on github there are [docs](https://github.com/kubernetes/kube-state-metrics/tree/master/docs) with all the kube-state-metrics to use

## Test that works
To test that this example of custom Alert Manager, cordon the node and delete a pod, the alerts will be fired.
