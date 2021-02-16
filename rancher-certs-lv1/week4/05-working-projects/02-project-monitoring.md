# Project Monitoring
Monitoring is available for installation on a project-by-project basis and adheres to the security boundaries of the project and its users.

Activating it will deploy Prometheus and Grafana into the project and makes metrics about its workloads available. These pods will set CPU and memory reservations and limits, so it's important that your cluster and the project have enough resources available for them to run.

After activating project monitoring, you can configure your workloads with an annotation that tells Prometheus where it can get custom metrics from them.  
If you expose an endpoint that Prometheus can scrape, those metrics will be available for you to use with Grafana.

## Set project monitoring on Rancher < 2.5.0

- Navigate to the "Cluster" details page using the Cluster selection dropdown menu and choose the Project you want to monitor

- From the top drop-down menu click on "Tools" and select "Monitoring" entry

- Fill the forms in the page that will load by adding the value you have choosen. Check the other monitoring pages for detailed steps.

- Click on "Save" button at the bottom of the page

## Set project monitoring on Rancher +2.5.0
