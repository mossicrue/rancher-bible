# Deploying a Cluster
The process for deploying a cluster from the Rancher UI is very intuitive.

## Hosted provider
If you choose a [hosted provider](https://rancher.com/docs/rancher/v2.x/en/cluster-provisioning/hosted-kubernetes-clusters/), you'll be walked through a series of pages that begin with authentication.  
After providing your authentication credentials, the information presented on subsequent pages has been pulled directly from your account with the provider.  
Continue through the configuration pages to deploy the cluster:
- On the the Global clusters page click on Add Cluster
- Choose one of the hosted Kubernetes providers like Amazon EKS or Google GKE
- Insert the Cluster Name
- Fill the Account Access information with the asked ones and click on "Next: Configure Cluster"
- Now select the Kubernetes Version, network settings, worker nodes sizing and some VM details


## Infrastructure Provider
Clusters deployed in an infrastructure provider utilize node templates
and can also use RKE templates.  

> **NOTE:** for infrastructure provider deploy both cloud credentials and note template are required

These are the required steps for deploy a k8s cluster:
- On the the Global clusters page click on Add Cluster
- Choose one of the infrastructure provider like Amazon EC2 or vSphere
- Insert the Cluster Name
- Add the control plane nodes pool and select their specs like replicas, template and roles
- Add the worker nodes pool and select their specs like for control plane pool
- Then select all the other cluster information like version, network settings and so on

Once the cluster is created it will deploy the master nodes first that will bootstrap the kubernetes cluster datastore and API server, without theese a cluster doesn't even exists.

Worker will be added after the bootstrapping phase to provide capacity for the intended workloads.

One all the nodes are configured and added to the cluster a new healthy no driver cluster in the UI


## Custom Clusters
When you provision a cluster with an infrastructure provider, Rancher first stands up the infrastructure and then builds an RKE cluster on it.  
A custom cluster is the second part of this process. Use it when you're provisioning infrastructure via any other means, including on bare
metal.

The only requirement for a custom cluster is that the hosts have a supported version of Docker installed. Rancher provides an easy [installation script](https://github.com/rancher/install-docker) that you can use on any host to make sure that you're using the correct, unmodified, upstream version of Docker.

In the Rancher UI, during the Cluster Creation process, there will be only some kubernetes options to set as Rancher has no control over any nodes in this situation.

Rancher will generate a custom install command that can be added to an external provisioner, and when the node scale up with the provisioner, the new nodes will automatically join the cluster.

### Windows Workers in Custom Clusters
Rancher supports Kubernetes on Windows and treats Windows workers as first class citizens.
When deploying a cluster with Windows workers, you must first deploy a Linux control plane and one Linux worker to act as the ingress controller.
Kubernetes on Windows only works with the Flannel CNI, and once you choose this option, you can enable Windows support.

> **NOTE:** In a windows kubernetes cluster, etcd and control plane nodes must run on Linux as 1 worker nodes because Ingress Controller work only on Linux

## Imported Clusters
If you want to manage an existing cluster with Rancher, you can import it.  
This option gives you a `kubectl apply` command that you can run against the cluster. It will pull its information from the Rancher server and install the agents to manage the cluster.

Rancher provides two versions of the command:
- One is for a Rancher installation that uses real certificates.
- The other is for a Rancher installation that uses a certificate signed by an unknown CA. Kubectl won't connect to this type of endpoint, so you have to pull the manifest with curl and pipe it to kubectl.
