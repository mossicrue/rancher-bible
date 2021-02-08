# Upgrading Rancher on Kubernetes
In this guides the old version to upgrade is v2.3.4 and the new will be v2.3.5

Rancher is an application packaged with Helm, so the upgrade procedure is based on him, just follow this step:

- Make a one-time snapshot of Rancher before continuing.
```bash
rke etcd snapshot-save --name rke-etcd-snapshot-pre-upgrade
```
- Update the Helm repo
```bash
helm repo update
```
- Fetch the most recent version of the chart for your repository
```bash
helm list --all-namespaces -f rancher -o yaml
```
  Example output is something like this
```yaml
 - app_version: v2.3.4
    chart: rancher-2.3.4
    name: rancher
    namespace: cattle-system
    revision: "1"
    status: deployed
    updated: 2021-02-07 23:32:01.691050743 +0000 UTC
```

- Retrieve the values used in the original installation
```bash
helm get values -n cattle-system rancher -o yaml > values.yaml
```
- Use helm upgrade with the namespace and values retrieved in the last step.
```bash
helm upgrade rancher rancher-stable/rancher \
 --version v2.3.5 \
 --namespace cattle-system \
 --values values.yaml
```
- Verify that the upgrade was successful by logging into Rancher and viewing the version in the bottom left corner of the UI.
```bash
kubectl rollout status deployment -n cattle-system rancher
```

> **NOTE:** The Rancher Helm repository should be one of `latest` or `stable`, upgrades are not supported for the alpha repository

## Rolling Back a Failed Upgrade
If the upgrade fails, the process for rolling back is to restore from the snapshot taken just prior to the upgrade.
