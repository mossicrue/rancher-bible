# Configuration and Templates

## RKE Configuration

When deploying an RKE cluster, there are a myriad of options available to you, from Kubernetes version to the NodePort range, to whether or not you want the Nginx ingress controller deployed in the cluster.
These options can inherit from an RKE template, which you’ll learn about in the next module, and that may control whether they can be edited and what sort of editing is allowed.

More informations are available in this [page](https://rancher.com/docs/rancher/v2.x/en/cluster-provisioning/rke-clusters/options/) of the documentation.

## RKE Template
RKE templates not only allow for quickly launching clusters with the same Kubernetes configuration, but they also allow operations teams to define that configuration and selectively allow users to override parts of it when launching a cluster.  
For example, the organization might require that all clusters run a specific version of Kubernetes.

The RKE template can lock down the major version and allow users to deploy clusters of any minor version within that release. This enables users to deploy and patch their own clusters, and once the organization decides on a new baseline for the cluster version, operations teams update the template.

When an administrator updates a template, Rancher will show an icon next to the cluster that indicates that a newer version of the template is available. Cluster administrators can schedule an upgrade using the new parameters.

If an RKE template is available, the template and its versions will appear in a drop down in the cluster deployment screen.  
Selecting the template will adjust the availability of the available options for deploying the cluster.

### Create a RKE template
The following steps explain how to create a RKE template:
- Log in to the Rancher Console
- From the Global view, click Tools > RKE Templates.
- Click Add Template.
- Provide a name for the template. An auto-generated name is already provided for the template’ first version, which is created along with this template.
- Optional: Share the template with other users or groups by adding them as members. You can also make the template public to share with everyone in the Rancher setup.
- Then follow the form on screen to save the cluster configuration parameters as part of the template’s revision.
  The revision can be marked as default for this template.

Result: An RKE template with one revision is configured. You can use this RKE template revision later when you provision a Rancher-launched cluster.  
After a cluster is managed by an RKE template, it cannot be disconnected and the option to uncheck Use an existing RKE Template and Revision will be unavailable.

## Node Template
Node templates answer all of the questions for deploying a particular type of node in a cloud provider. They make it easier to reuse an existing configuration instead of creating it again for every node or every cluster that you launch.  
You can create node templates from the cluster launch screen or from your profile avatar in the top right.

Cloud credentials define how to communicate with a cloud provider.  
They allow you to configure multiple accounts for use with different providers or in different provider regions. When creating a node template, you select the cloud credentials to use.

### Create a Cloud Credential
The following steps shows how to create a Cloud Credentials:
- Log in to the Rancher Console
- Click on your User Avatar in the upper-right corner, and select Cloud Credentials
- Click Add Cloud Credential
- Setup the cloud credentials based on your choice

### Create a Node Template
The following steps shows how to create a Node template:
- Log in to the Rancher Console
- Click on your User Avatar in the upper-right corner, and select Node Templates
- Click Add Template
- Choose your cloud of choice. You’ll be creating a new Cloud Credential, that allows Docker Machine to automate the provisioning of your infrastructure.
- Setup the node template based upon your cloud of choice.

## Cloud Providers
Infrastructure and Custom clusters allow you to set a cloud provider.
This makes changes to how the cluster operates. For example, if you choose the Amazon cloud provider, deployment of a LoadBalancer Service into the cluster will create an Amazon ELB.  
Cloud providers require additional configuration of the cluster nodes to operate correctly. In some cases, if you choose a cloud provider without
first completing that configuration, the cluster won’t deploy at all.  
See [Rancher documentation](https://rancher.com/docs/rancher/v2.x/en/cluster-provisioning/rke-clusters/cloud-providers/) for more information on cloud providers and the additional configuration required by the providers.
