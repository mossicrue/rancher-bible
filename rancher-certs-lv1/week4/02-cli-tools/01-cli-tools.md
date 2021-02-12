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

To retrieve more infromation about the installed applications run
```bash
rancher app show notes pgadmin4
```

#### Proxying Kubectl Commands
If you're using the Rancher CLI, you don't also need a kubectl config file.  
After authenticating with the cluster, you can run rancher kubectl to access all of the power of kubectl directly through the API connection.
