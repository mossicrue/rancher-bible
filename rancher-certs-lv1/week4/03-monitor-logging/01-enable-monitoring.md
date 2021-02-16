# Advanced Monitoring

The cluster overview page for the cluster shows some basic red/green status information, but for true insight into the Kubernetes cluster, enable advanced monitoring from this screen.

Advanced monitoring deploys Prometheus and Grafana, and the worker nodes will need to have enough resources to accommodate the extra load. Recommended CPU and memory sizes are available in the Rancher documentation.

## Prometheus Configuration
Advanced monitoring is delivered by Prometheus, and there are a number of options available that control how the monitoring pods behave.

In addition to the standard memory and CPU reservation and limits, consider the following options for your cluster:

- Data retention period
How long Prometheus retains data scraped from Rancher objects before purging it.

- Persistent Storage for Prometheus and Grafana
With persistent storage enabled and a storage class selected, the collected data will survive beyond the life of the Pod with which it is collected.

- Node Exporter configuration
The Node Exporter enables Prometheus to monitor the host on which it's running. This component requires that the pod run with hostNetwork: true, and that you enable a port on which it can listen that doesn't conflict with other services running on the host.

- Selectors and Tolerations
These configure where the monitoring workloads will be deployed.

## Enable Advanced Monitoring
- Navigate to the "Global Clusters" page

- Select the cluster you want to enable monitoring

- Click on the "Enable Monitoring to see live metrics" link above the gauge graph of the number of POD

- Select the desired options and then click on the "Enable" button at the end of the page

> **NOTE:** In the new page you can directly click on Enable to set basic cluster monitoring, if you leave default value Prometheus data retain is only for 12 hours without the possibility to store it in persistent storage

To really make the data useful on long term from day one it's necessary only male just a couple of changes:
- Increase data retention to 3 days
This will give time to examine any over the weekend events, 3 days should be a good starting point.

- Enable persistent storage for Prometheus
This allow to our data survives pod rotation or destruction

- Enable persistent storage for Grafana

After this, Rancher will deploy 2 apps in the System project of the cluster: `monitoring-operator` and `cluster-monitoring`.

After the deployment of these apps it's now possible to see the actual memory and cpu usage.

It's also possible to get similar benefits at the project and application level using project monitoring tool.

> **NOTE:** Cluster monitoring is necessary in order to enable project monitoring

### Enable Project Monitoring
From the a "Project" view page, it is possibnle to enable monitoring by following this steps:
- Click on "Tools" from the dropdown menu and select "Monitoring"

- Like for Cluster Monitoring, select the desired options and enable monitoring.

> **NOTE:** The best choice for the application monitoring parameters is to apply the same settings for retention, persistent storage, etc... used for the cluster monitoring

## Enable Cluster Monitoring Rancher +2.5.0
From Rancher 2.5.0 monitoring in Cluster Manager has been deprecated and is deployed from Cluster Explorer.
If you are migrating from an older version of Rancher with monitoring enabled,  disable monitoring in Cluster Manager before attempting to install the new Rancher Monitoring chart in Cluster Explorer.

### Disable Cluster Manager Monitoring from Cluster Manager
- Navigate to the "Global Clusters" page

- Select the cluster you want to enable monitoring

- From the dropdown menu click on Tools and select the Monitoring entry

- In the monitoring options page click on the "Disable" red button

- Wait for the `monitoring-operator` and `cluster-monitoring` apps to be removed

### Enable Cluster Monitoring from Marketplace

- In the Rancher UI, go to the cluster where you want to install monitoring and click Cluster Explorer.

- Click Apps.

- Click the rancher-monitoring app.

- Optional: Click Chart Options and configure alerting, Prometheus and Grafana. For help, refer to the [configuration reference](https://rancher.com/docs/rancher/v2.x/en/monitoring-alerting/v2.5/configuration/).

- Scroll to the bottom of the Helm chart README and click Install.

### Create Services Monitor
- In the Cluster Explorer dropdown menu select "Monitoring" entry

- Now, select on "Service Monitors" and click on "Create from YAML"

- Paste this snippet changing metadata.name, metadata.namespace and the spec.selector.matchLabels values

```yaml
apiVersion: monitoring.coreos.com/v1
kind: ServiceMonitor
metadata:
  name: simple-app-service-monitor
  namespace: simple-app
spec:
  selector:
    matchLabels:
      app: simple-app
  endpoints:
  - port: web
```

> **NOTE:** still need to study the new Rancher 2.5.x monitoring, for now and for certs requisites I will leave things as they are and adding them after the courses. For now more information [here](https://rancher.com/blog/2020/maximize-monitoring-in-rancher-2-5-with-prometheus)
