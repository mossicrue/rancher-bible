# Rancher System Tools
System Tools is a tool to perform operational tasks on Rancher Launched Kubernetes clusters or installations of Rancher on an RKE cluster. The tasks include:
- Collect logging and system metrics from nodes.
- Remove Kubernetes resources created by Rancher.

The following commands are available:
- `logs`: Collect Kubernetes cluster component logs from nodes.
- `stats`: Stream system metrics from nodes.
- `remove`: Remove Kubernetes resources created by Rancher.

## Download System Tools
You can download the latest version of System Tools from the GitHub releases page. Download the version of system-tools for the OS that you are using to interact with the cluster.

- MacOS: `system-tools_darwin-amd64`
- Linux: `system-tools_linux-amd64`
- Windows: `system-tools_windows-amd64.exe`

After you download the tools, complete the following actions:

- Rename the file to `system-tools`
- Give the file executable permissions
- Move the file to an executable path like `/usr/local/bin`

## Remove
> **WARNING**: This command will remove data from your etcd nodes. Make sure you have created a backup of etcd before executing the command.

When you install Rancher on a Kubernetes cluster, it will create Kubernetes resources to run and to store configuration data. If you want to remove Rancher from your cluster, you can use the `remove` subcommand to remove the Kubernetes resources.  
When you use the remove subcommand, the following resources will be removed:

- The Rancher deployment namespace, `cattle-system` by default.
- Any `serviceAccount`, `clusterRoles`, and `clusterRoleBindings` that Rancher applied the `cattle.io/creator:norman` label to. Rancher applies this label to any resource that it creates from v2.1.0.
- Labels, annotations, and finalizers.
- Rancher Deployment.
- Machines, clusters, projects, and user custom resource deployments (CRDs).
- All resources create under the `management.cattle.io` API Group.
- All CRDs created by Rancher v2.x.

### Usage
When you run the command below, all the resources listed above will be removed from the cluster.

> **WARNING:** This command will remove data from your etcd nodes. Make sure you have created a backup of etcd before executing the command.

```bash
system-tools remove --kubeconfig <KUBECONFIG> --namespace <NAMESPACE>
```

The following are the options for the remove command:
- `--kubeconfig <KUBECONFIG_PATH>`, `-c <KUBECONFIG_PATH>`: the cluster’s kubeconfig file
- `--namespace <NAMESPACE>`, `-n cattle-system`: Rancher 2.x deployment namespace. If no namespace is defined, the options defaults to cattle-system.
- `--force`: Skips the interactive removal confirmation and removes the Rancher deployment without prompt.
