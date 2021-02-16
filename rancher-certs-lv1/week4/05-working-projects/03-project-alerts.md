# Project Alerts
We set up notifiers for the cluster in an earlier unit, and we showed how to configure alert groups and alerts for cluster-level issues like etcd leader changes.

Project alerts use the same notifiers but send alerts for workloads running within the project.

Options are available for specific Pods, Workloads, or selectors, and if project-level monitoring is active, you can use arbitrary PromQL expressions to define the criteria.

Alert rules can be set to an urgency of critical, warning, or info, and they can either inherit the wait and interval times from the alert group or override them.

## Set up project Alerts
Now, this steps will show how to set an alert on the workloads that fires when pods aren't present.  
Remember to pause orchestration before deleting all the pods forcefully.

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
