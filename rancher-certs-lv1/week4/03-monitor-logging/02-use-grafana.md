# Use the Grafana Dashboards

After Rancher finishes installing Prometheus and Grafana, the cluster overview page will visibly change. Each of the sections below the gauges expands to show summary and detailed graphs for the most important metrics.  
If you want to see more information, you can click the Grafana icon in any section, and Rancher will take you to the relevant Grafana dashboard for that component.

Rancher installs a cluster-level Prometheus/Grafana resource and one or more project-level Prometheus/Grafana resources for projects with monitoring enabled.

Access to Grafana is controlled by the role-based access control provisions within Rancher. A user will be able to see graph data for their project or cluster if they have access to view those items within Rancher.

Access is from the top down. A global admin can see monitoring for all clusters and projects. A cluster admin can see all projects within their cluster and the cluster itself. A project member can only see information for their project.

## Direct Access to Prometheus and Grafana in Rancher < 2.5.0
It's possible to access Prometheus and Grafana for the cluster and
projects directly.

### Cluster Installation
- Visit the Apps menu in the System project
- Mouse over the /index.html links in the cluster-monitoring app.  
One will point to Grafana and the other to Prometheus
- Click the corresponding link for the service you wish to visit
- A new tab or window will open to that service

### Project Installation
- Visit the Apps menu in the project
- Mouse over the /index.html links in the project-monitoring app.
One will point to Grafana and the other to Prometheus
- Click the corresponding link for the service you wish to visit
- A new tab or window will open to that service

## Direct Access to Prometheus and Grafana in Rancher +2.5.0
- In the top left of the page click on the "Cluster Explorer" drop-down menu
- Now select the "Monitoring" entry
- Here are listed all the Prometheus and Grafana apps deployed, click on the service you want to use
