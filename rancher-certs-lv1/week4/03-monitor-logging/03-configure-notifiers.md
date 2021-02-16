# Configure Notifiers

Notifiers are the services which inform you of alert events.  
These are configured at the cluster level and are then available at the cluster and
project levels for users to connect to the alerts.

Rancher integrates with the following services:
- Slack, which also works with Mattermost

- Email

- PagerDuty

- Webhook

- WeChat

Information about configuring each service is available in the [documentation](https://rancher.com/docs/rancher/v2.x/en/monitoring-alerting/v2.0.x-v2.4.x/notifiers/).

The Webhook service is a generic webhook and expects that you will be running a listener that will parse the information and send it on to any target not included in the other four services.

Although it might sound obvious when we say it, it's important that the target for your notifier (e.g. Mattermost or a webhook endpoint) is not also running inside of the same Kubernetes cluster that’s sending the notification.

If the cluster is failing, the target might also be down, and notifications will never go out.

## Configure notifier in Rancher < 2.5.0
- Navigate to the Cluster view page of the cluster in which you want to install the notifier

- Click on the "Tools" dropdown menu on top and select the "Notifiers" entry

- In the new page click on "Add Notifier"

- Choose the notification type and fill the form that is presented

## Configure notifier in Rancher +2.5.0
From Rancher 2.5.0 the Notifier section was improved in the new Monitoring section available on the Cluster Explorer Dashboards.
Now to create and configure a notification for an alarm you must create 2 elments: a Receiver and a Route

### Create a Receiver
- In the top left of the Dahsboard page click on the "Cluster Explorer" drop-down menu

- Now select the "Monitoring" entry

- Click on "Receiver" link in the left menu entries

- In the new page click on the "Create" button

- Choose the notification provider you want to use

- Fill the form with the requested data and click "Create"

### Create a Route for a Receiver
- In the top left of the Dahsboard page click on the "Cluster Explorer" drop-down menu

- Now select the "Monitoring" entry

- Click on "Route" link in the left menu entries

- Fill the form for the 3 sections: Receiver, Grouping and Matching

- Finally, create the Route by clicking on "Create"

> **NOTE:** More info on how to configure a receiver is present in the [Alert Manager documentation page](https://rancher.com/docs/rancher/v2.x/en/monitoring-alerting/v2.5/configuration/alertmanager/)
