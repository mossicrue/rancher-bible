# INSTALL RANCHER ON KUBERNETES

Rancher is an application that runs inside of Kubernetes, so when deployed into an RKE cluster, Rancher uses the high availability infrastructure of Kubernetes (etcd and controlplane) to become highly available and resilient to failures.

Prior to version 2.4, Rancher can only be deployed into an RKE cluster and from 2.4 onward it can also be deployed into a K3s cluster.

If deployed into any other Kubernetes distribution or hosted provider solution, it will not operate reliably and is not eligible for support under the Rancher Enterprise Subscription.

## Choose an IP and hostname for Rancher Server
Select a hostname for the Rancher server environment.
The IP address you attach to this hostname later must be reachable by downstream clusters.
Although it's theoretically possible to change this hostname after installation, doing so creates instability in the environment, so it's far better to choose a name like `rancher.mydomain.com` and keep that hostname for the life of the environment.

## Provision an RKE Cluster
We've already deployed an RKE cluster and see how RKE's flexibility means like deploy a single-node cluster and add more nodes later to make it HA.
This installation process has more steps than the Docker container method, but if you're considering using Rancher in production and want to have the most options available for using it in the future, you can follow all of the steps here but with a single node RKE cluster.
At the beginning the cluster won't be highly available, but you can convert it later.

## Deploy a Load Balancer in front of the cluster
Even with a multi-node RKE cluster, you'll be accessing the Rancher server via a single URL.
The ingress controller will listen on all nodes in the cluster, so deploy a load balancer to receive traffic for your chosen URL and route it to the hosts in the cluster.
This can be a hardware load balancer, a cloud load balancer like an ELB or NLB, or a software load balancer like Nginx or HAProxy.

Whatever you choose, operate the load balancer at Layer 4 only, passing TCP directly through on 80 and 443. Do not configure the load balance at Layer 7 for HTTP and HTTPS.

Although it's possible to configure TLS termination on the load balancer and communicate between the load balancer and the Rancher server over an unencrypted connection on port 80, this is not a recommended
method of install.

This load balancer will be dedicated to the Rancher server cluster, which will appear inside of Rancher as the “local” cluster. Workloads other than Rancher should never run on the “local” cluster, so each downstream Kubernetes cluster that Rancher manages will need its own load balancer.
After the load balancer has been deployed, configure DNS to point your chosen hostname to the addresses or hostnames of the load balancer, according to the instructions for your chosen architecture.

**NOTE**: For a single node cluster is not necessary to install a load balancer.

### Alternate Options For Cloud Native Environments
If the cluster is located on premises or in an environment where you can dynamically attach multiple IP addresses to a node, you can look at [MetalLB](https://metallb.org/) as an option.
MetalLB enables a service of type LoadBalancer on the cluster and assigns a dedicated IP to the service. This service would sit in front of the Ingress Controller and pass traffic to it the same way an external load balancer would.

## Install Rancher with HELM
Rancher is installed with Helm version 3, the package manager for Kubernetes, that can be installed following its [installation guide](https://helm.sh/docs/intro/install/)
All the steps must be done on the Bastion Host, the system where you ran `rke up` to install RKE, and where you have the configuration file for kubectl.
Install Helm and verify that you're pointed at the correct cluster by running kubectl get nodes before continuing.

### Add the Helm Chart Repository
Rancher provides three repositories for Helm charts:

- Latest: Recommended for trying out the newest features. Not recommended for production.
- Stable: Recommended for production environments
- Alpha: Experimental previews of upcoming releases. Definitely not recommended for production.

If you use an Alpha release, there is no support for upgrading to, from, or between releases. Alpha releases are designed to be viewed and then deleted.

```bash
helm repo add rancher-stable https://releases.rancher.com/server-charts/stable
helm repo update
```

### Create a Namespace for Rancher
Rancher will be installed into the cattle-system namespace, which must exist before we install the chart.

```bash
kubectl create namespace cattle-system
```

### Choose Your SSL Configuration
Rancher will always be protected by TLS, and can be provisioned with one of these options:
- Rancher-generated self-signed certificates
- Real certificates from Let's Encrypt
- BYO Certificates (self provided, real or self-signed)

The Rancher-generated and Let's Encrypt certificates options require a Kubernetes package called cert-manager that handles certificate generation and renewal from external sources.
If the cluster use private self-signed or real certificates, skip the next step.

#### Install cert-manager
cert-manager is under active deployment by a company called JetStack, follow their [installation guides](https://cert-manager.io/docs/installation/) to install cert-manager
NOTE: For the course I followed the the [Install with Manifest](https://cert-manager.io/docs/installation/kubernetes/#installing-with-regular-manifests) guide. TL;DR below

```bash
# Kubernetes 1.16+
kubectl apply -f https://github.com/jetstack/cert-manager/releases/download/v1.1.0/cert-manager.yaml

# Kubernetes <1.16
kubectl apply --validate=false -f https://github.com/jetstack/cert-manager/releases/download/v1.1.0/cert-manager-legacy.yaml
```

Check that cert-manager is working running a `kubectl get deployments -n cert-manager` and a `kubectl rollout status <deployment-name> -n cert-manager` for any deployment configured

### Install Rancher
Depending on the TLS Certificate option choosen, additional flags need to be passed to the installation command to complete the procedure.

#### Option 1: Rancher-Generated Self-Signed Certificates
This is the default option when installing Rancher and it's the easyer one.
It doesn't require any additional configuration, only specify the namespace and the hostname when installing.

This installation option requires this two parameters:
- `--namespace cattle-system`
- `--set hostname=rancher.mydomain.com`

The installation command for this option is:
```bash
helm install rancher rancher-stable/rancher --version v2.4.13 \
--namespace cattle-system --set hostname=rancher.mossicrue.com
```

#### Option 2: Real Certificates from Let's Encrypt
To request a certificate from Let's Encrypt you must have the load balancer and hostname properly configured. Let's Encrypt will issue an http-01 challenge that cert-manager will process.
Certificates issued by Let's Encrypt will be automatically renewed before their expiration date.

This installation option requires this parameters:
- `--namespace cattle-system`
- `--set hostname=rancher.mydomain.com`
- `--set ingress.tls.source=letsEncrypt`
- `--set letsEncrypt.email=johndoe@mydomain.com`

The installation command for this option is:
```bash
helm install rancher rancher-stable/rancher --version v2.4.13 \
--namespace cattle-system --set hostname=rancher.mossicrue.com \
--set ingress.tls.source=letsEncrypt \
--set letsEncrypt.email=mossicrue@example.com
```

Let's Encrypt uses the email to communicate with you about any issues with the certificate, such as its upcoming expiration.

#### Option 3: Certificates that you provide

If you have your own certificates, either from a public or private CA, they will be loaded into a Kubernetes Secret and tell Rancher and the Ingress Controller to use that secret to provision TLS.

This installation option requires this parameters:
- `--namespace cattle-system`
- `--set hostname=rancher.mydomain.com`
- `--set ingress.tls.source=secret`
- `--set privateCA=true`, only if the certificates are self-signed or by a private CA

The installation command for this option is:
```bash
helm install rancher rancher-stable/rancher --version v2.4.13 \
--namespace cattle-system --set hostname=rancher.mossicrue.com \
--set ingress.tls.source=secret
```

After installing Rancher and tell it to use private CA certificates we now need to create the TLS secret in the kubernetes cluster and populate it with the certificate data

- create the local file tls.crt with the certificate
- create the local file tls.key with the private key
- create the `tls` type secret called `tls-rancher-ingress` from those files
```bash
kubectl create secret tls tls-rancher-ingress --cert=server.crt --key=server.key -n cattle-system
```

If the installation was done with a private CA, create the CA secret
- create the local file cacerts.pem with the private CA information
- create the `generic` type secret called `tls-ca` from that file

```bash
kubectl create secret generic tls-ca --from-file=cacerts.pem=<ca.crt> -n cattle-system
```

### Check Rancher installation

Check if rancher is deployed correctly by running
```bash
kubectl rollout status deployment rancher -n cattle-system
```

Now open the URL provided to the Rancher installation and set the admin password.
At the homepage there is a little bug that says that the cluster called `local` is unhealthy with the description "Waiting for server-url setting to be set".
To solve this click the kebab-menu (3 vertical dots button) of the unhealthy cluster and then choose "Edit".
In the Cluster Edit Page click directly on "save" button, now the cluster is healthy
