# Services in Rancher

Pods are ephemeral. Although they have IP addresses, there is no guarantee that a Pod will exist at that IP in the future. Pods might fail health checks and be recreated, nodes can fail, or any number of tradedies can befall them.

Kubernetes solves this with a Service, which gives a stable IP and DNS name to a group of Pods.

Services are automatically created for Workloads that expose a port, and in the Workload deployment screen, you can choose what kind of service to deploy: ClusterIP, NodePort, or LoadBalancer.

These can also be created from the Service Discovery tab of the Workloads screen, and this page offers additional options for service discovery.  
For example: An ExternalName service can point to a specific IP address or hostname. This acts like an A record or a CNAME.

An Alias service acts like an internal CNAME to another DNS record in the cluster. This can point to a workload in the same namespace or a different one.

## External Name
From external DNS services addresses them locally.  
For example, take the `mongo.simpleblocks.net:32701` and link in the env of the workload
```yaml
  env:
    - name: MONGOURL
      value: "mongo.simpleblocks.net:32701"
```

What to do if the URL is changed? We had to edit the workload and change the value of MONGOURL or, we can create a "Hostname" service discovery that point to the external dns target.  
When the ExternalName service is created, change the env value of MONGOURL in the workloads by referring to the name of the External Hostname service just created

### Create ExternalName in Cluster Explorer

- Click the dropdown menu on the top left of the page and select "Clusetr Explorer"

- In the "Service Discovery" section choose "Service"

- In the new page click on "Create" and choose the "ExternalName" service type

- Add to the forms the data requested: "Namespace", the "Name" of the external record and the "Target Hostname"

- When it's all fine click on the "Create" button

### Create ExternalName in Cluster Manager

- Click on the dropdown cluster / project menu on the top and select the project where create the External Name services

- In the new page click "Resources" on the dropdown menu and select the "Workloads" entry

- Choose to visualize the "Service Discovery" sections and then click the "Add Record" button

- In the "Resolves To" choose "An External Hostname"

- Now add the "Name" of the external record and the "Target Hostname"

- At the end click on the "Create" button

## Alias Service
Alias services can point to other services in our cluster and that can be super handly for doing a/b deployments.  
Example: there are the services demo-a and demo-b and an Alias demo-alias service that points to demo-B service.  
It's also present an Ingress that point to the demo-alias service.

To change the point of an ingress to demo-b service to demo-a service you have only to change the target service of the demo-alias service and voilà!
