# Configure Alerts

## Default Alerts
When Rancher builds a cluster with RKE, it installs a number of alerts for the cluster components.  
These need a notifier, so after you create one above, visit these alerts and attach the notifier to them.

The alerts that it ships with are reasonable. They're things like:
- Send an alert if etcd leader changes more than three times in three minutes. This could indicate a networking issue affecting etcd quorum.

- Send an alert if one of the Kubernetes masters enters an unhealthy state.

- Send an alert if a node's resources are under excessive load.

Rancher also installs a set of alerts when monitoring is activated for a project. One of these demonstrates the use of a workload selector, but
it points at `app=workload`. If you change this to point to one of your workloads and add a notifier, you'll receive alerts when less than half of
its pods are available.

The other default project alert demonstrates the use of an expression to query Prometheus directly and will alarm when any particular container's memory usage is greater than 100% of its quota for more than three minutes.

## Alert Groups
You can group alerts into arbitrary collections. The defaults have them grouped by component or node, but you can use any system that helps you manage your configuration.  
When bundling alerts into groups, the advanced options for alert timing carry through to the group members unless they are overridden when creating the child alerts.

## Alert Timing
When grouping alerts, consider how Rancher will treat alerts from the same group.  
These notification options are controlled in the Advanced Options of the alert group or individual alerts:
- Group Wait Time.  
How long to buffer alerts of the same group before sending out the first alert

- Group Interval Time.  
How long to wait before sending out a new notification for an alert that is added to a group that contains alerts which have already fired

- Repeat Interval Time.  
How long to wait before resending any alert that has already been sent.

## Alert Urgency
All alerts have an urgency level. Rancher includes this information when sending the alert to help you prioritize your response actions.

## Cluster Alerts
Configure these alerts at the cluster level.  
Alert configuration is available for different types:
- System services  
Configure alerts for controller-manager, etcd, or scheduler.

- Resource events  
Configure alerts for normal or deviant operations of Deployments, DaemonSets, StatefulSets, Pods, or Nodes.

- Node events  
Configure alerts for unresponsive nodes or nodes that exceed a threshold of allocated CPU or memory.

- Node selector events  
Similar to Node events but for all nodes that match a selector.

- Metric expression  
Configure alerts for arbitrary PromQL expressions. Metric expression alerts require that advanced monitoring is active for the cluster.

## Project Alerts
Configure these alerts at the project (namespace group) level.
Alert configuration is available for different types:

- Pod alerts  
Configure alerts for a specific Pod.

- Workload alerts  
Configure alerts for a specific workload and all Pods within it.

- Workload selector alerts  
Configure alerts for workloads that match a selector.

- Metric expression  
Configure alerts for arbitrary PromQL expressions.  
Metric expression alerts require that advanced monitoring is active for the cluster.

## Limitations for Imported and Hosted Clusters
Imported and hosted clusters don't provide access to etcd, so Rancher is unable to perform monitoring of the data plane.  
You'll see this after activating advanced monitoring: the default alerts that it installs will report “No metric graph data”, and the Etcd tile in the cluster overview screen will not have a Grafana icon.
