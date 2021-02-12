# Introduction to CLI Tools

Rancher and Kubernetes are designed with an API first approach.  
The command line utiltiy for interacting with kubernetes is kubectlm also know as kubecontrol or kubecuttle.  
It interacts with kubernetes by making API calls to what is called the Kubernetes API Endpoint. The same is true for Rancher: any actions that take place in the UI are actually making API calls to Ranchers API endpoint.  
Every UI action can be done via an API call either via HTTP calls to the restful service or using the Rancher CLI

## Kubectl
The de facto way of interacting with Kubernetes is via kubectl.

Rancher acts as an authentication proxy, handling the authentication and authorization components of access control before handing the communication off to the downstream cluster.

You can use the kubectl shell directly in the Rancher UI, or you can download a kubectl config file to use locally on a workstation.

Each Kubernetes cluster within Rancher will have its own kubectl config file, which, as you’ll learn in the next module, might be a reason to use the Rancher CLI instead.

## Rancher CLI
Everything that you can do with Kubernetes you can do with Rancher, which means that you can do all of it with kubectl.  
What about the benefits that Rancher provides above and beyond what kubectl can do? For those items there's the Rancher CLI.   
The Rancher CLI wraps authenticated API calls in simple commands and is a great solution to use with automation or situations where a CLI is more appropriate than a UI.

You can download the Rancher CLI for your platform from a link in the bottom right corner of the UI.

### Creating an API Key
The Rancher CLI works with bearer tokens derived from an API Key.  
These keys can be unscoped, which will work with the Rancher Server, or scoped to a particular cluster, in which case they will also work with the Authorized Cluster Endpoint for that particular cluster without being proxied through the Rancher server.

API keys can have an expiration date, which can help keep the cluster secure.  
When you generate an API key, you're shown the access key and secret key. These can be combined into a bearer token for the Rancher CLI by separating them with a colon.

> **NOTE:** The API key inforamtions are only shown once at the time you generated it, so be sure to copy both Access Key and Secret Key to a safe location

API Keys are managed from your account profile settings and that is accessible from the Avatar icon in the top right.
The screen that is shown for API and keys not only has a link to the API itself but it also shows you all of the keys that are active for your account.

When you add a new key, set a description that will allow to identity in the future what the keys was used for. Keys are stored with a non descriptive name, so that description field is important.

You may be tempted to not set an expiration date for a key but setting one saves you from a situation where a key might be compromised without your knowledge.  
By forcing keys to expire, you also force yourself to rotate keys and engage in good security hygiene for your kubenetes clusters.

Keys can be scoped to a cluster.
This is helpful if you have access to multiple clusters but don't want to create a key with a larger scope of privilege than it needs.  
The "no scope" options will leave access to all the clusters.

#### Login to Rancher with API Keys
Log into the Rancher server using the Access Key and Secret Key that we generated before joining the two of them togheter with a colon

```bash
rancher login https://rancher.mossicrue.com --token token-2klr5:scbwdsmntg7g7mcmcf98rmcc224dctt8pcxmbnmqcfnffdtnft4ffg
```

After the login, rancher will ask which project you want to use, if there aren't any project, rancher will use "default"

### Selecting Projects With the CLI
Most commands in the Rancher CLI apply to a specific project.  
A cluster and project, when combined, are known as a "context".
Before you can run commands against a project, you have to switch to the correct context by running:

```bash
rancher context switch
```

It will be show the list of projects where we can be quickly hop between clusters and projects.  
The list shows exactly what is allowed to access and communication is proxied to those clusters with the correct RBAC permissions.  
If you were logged in as a non admin user, you would have a much smaller list of things that you can access.  
Projects where a user can't access wouldn't even show up in the list

### Executing Commands With the CLI
Many of the functions from the UI are also available in the CLI. Help is available with --help or by adding --help to any of the subcommands.

Install an applications with Rancher CLI
```bash
rancher app install --set persistentVolume.enabled=false --set env.email=mossicrue@gmail.com pgadmin4 pgadmin4
```
The application is PgAdmin 4 (a user interface for PostgreSQL) and will be installed using helm charts `pgadmin4` and naming the app `pgadmin4`  . The `--set` are used to disable the persistent volume and set the admin email for the login.

The command `rancher app ls` is useful also to check the application deployment status, when the State is active, the application would be online
```bash
rancher app ls
ID                 NAME       STATE       CATALOG       TEMPLATE   VERSION
p-lx5f9:pgadmin4   pgadmin4   deployng    local/runix   pgadmin4   1.4.6
```

To retrieve more infromation about the installed applications run
```bash
rancher app show notes pgadmin4
NOTES:
1. Get the application URL by running these commands:
  export POD_NAME=$(kubectl get pods --namespace pgadmin4-m2jfx -l "app.kubernetes.io/name=pgadmin4,app.kubernetes.io/instance=pgadmin4" -o jsonpath="{.items[0].metadata.name}")
  echo "Visit http://127.0.0.1:8080 to use your application"
  kubectl port-forward $POD_NAME 8080:80
```

When we look at those notes for the app, the output shows us how to get the pod name so that we can port forward a connection to it.

#### Why not using Helm?
Why not directly using Helm for install an helm applications?  
Well, because:
- In addition to helm you can also use this with Rancher apps (find more in the next chapters)
- Because you can replace all `kubectl` and `kubernetes` config file with the `rancher` command

One of the useful addition with the Rancher cli is an SSH proxy to your cluster nodes.  
This works with rke clusters launched in an infrastructure provider because rancher has the SSH keys to make those connections.

You don't need to know where nodes are, what the address are or really anything.  

The output of `rancher nodes` shows you information about the nodes and, if they were part of a deployed cluster we will see info about which pool they are in and their description.

```bash
rancher nodes
ID                    NAME      STATE     POOL      DESCRIPTION
local:machine-lfww9   node-b    active
```

We use rancher ssh to connect to it

```bash
rancher ssh node-b
```

### Proxying Kubectl Commands
If you're using the Rancher CLI, you don't also need a kubectl config file.  
After authenticating with the cluster, you can run rancher kubectl to access all of the power of kubectl directly through the API connection that rancher provides.

After you made a rancher login you can simply use `rancher kubectl` to execute all the `kubectl` commands and subcommands via the Rancher api.  
For example, running `kubectl get nodes` command will return an error output like below because the `$KUBECONFIG` is not set.

```bash
kubectl get nodes
The connection to the server localhost:8080 was refused - did you specify the right host or port?
```

To perform this via the `rancher` commands, simply  run the following command:

```bash
rancher kubectl get nodes
NAME     STATUS   ROLES                      AGE    VERSION
node-b   Ready    controlplane,etcd,worker   7d8h   v1.19.7
```

Another adds on top of the existing Kubernetes resources is with namespaces.  
With kubectl you can create and delete namespaces but when using the `rancher` cli you can also add them to namespace groups or move them between projects.  
The output is also cleaner with the rancher command, in this example it shows you the namespaces in the context you are working which helps focus on the informations that is needed right now

```bash
rancher namespaces ls
ID               NAME             STATE     PROJECT         DESCRIPTION
pgadmin4-m2jfx   pgadmin4-m2jfx   active    local:p-lx5f9
```

```bash
rancher kubectl get namespace
NAME                                     STATUS   AGE
cattle-global-data                       Active   7d7h
cattle-global-nt                         Active   7d7h
cattle-system                            Active   7d8h
cert-manager                             Active   7d8h
cluster-fleet-local-local-1a3d67d0a899   Active   4h
default                                  Active   7d8h
fleet-clusters-system                    Active   4h1m
fleet-default                            Active   3h59m
fleet-local                              Active   4h
fleet-system                             Active   4h1m
ingress-nginx                            Active   7d8h
kube-node-lease                          Active   7d8h
kube-public                              Active   7d8h
kube-system                              Active   7d8h
local                                    Active   7d7h
p-62snh                                  Active   7d7h
p-6bch4                                  Active   7d7h
p-lx5f9                                  Active   4h33m
pgadmin4-m2jfx                           Active   177m
rancher-operator-system                  Active   4h1m
user-7swkp                               Active   7d7h
```
