# DAY 2 OPERATION FOR RANCHER

## Secure KUBECONFIG file
After Rancher is successfully deployed 2 new file are generated:
- kube_config_cluster.yaml
- cluster.rkestates

The `kube_config_cluster.yaml` file is the kubeconfig file that you can put in the `~/.kube` directory or being setted in the `KUBECONFIG` env to log in into the server and work with them

For my test, the command to run for exporting the KUBECONFIG value was:
```
export KUBECONFIG=/Users/mossicrue/Documents/GitHub/rancher-bible/kube_config_cluster.yml
```

## RKE Backup && Disaster Recovery

By default `backup_config` in the etcd nodes of the `cluster.yml` is set to null, to configure the backup add this snippet

```
  backup_config:
    interval_hours: 6
    retention: 8
```

The two key says to run a etcd backup every 6 hours, `interval_hours`, and to keep the last 8 of them `retention`.
This will keep a backup available for 48 hours, 6 hours x 8 backups
To apply the new configuration run `rke up`

### Manual Backup
If you want to launch the command `rke etcd snapshot-save --name <backup-name> --config <cluster.yml path>`
If the 2 arguments aren't specified the default value will be:
- `rke_etcd_snapshot_<pretty-print timestamp>.zip` for the backup name
- `$PWD/cluster.yml` or `cluster.yml` for the cluster config file path

The snapshot are saved directly on the ETCD host in the path `/opt/rke/etcd-snapshots`

### Restore from a snapshot
To restore Rancher from a previous snapshot run the command `rke etcd snapshot-restore --name <snapshot name>`
Run the command from the path that contain both cluster.yml and cluster.rkestates
Have the snapshot to restore at `/opt/rke/etcd-snapshots` on one etcd node

## Upgrading Kubernetes
Start by upgrading local copy of `rke` following the rke installation guide, then run  `rke config --list-version --all` to see all the supported kubernetes versions, example output is:

```
MacBookPro:rancher-bible mossicrue$ rke config --list-version --all
v1.19.7-rancher1-1
v1.18.15-rancher1-1
v1.16.15-rancher1-4
v1.17.17-rancher1-1
```

From the example output the most recent kubernetes version is the `v1.19.7-rancher1-1`
After taking notes the new version to upgrade, perform a one-time etcd snapshot that will be moved in a secure place, there are no rollbacks in kubernetes upgrade, if something wrong happen you must restore the cluster at the pre-update state.

Now, set the kubernetes desired version that we found before in the `kubernetes_version` key in `cluster.yml` and **DON'T** set it under `system images`!
Then, delete the `system_images` key with all its entries, it wil be repopulated with a newer versions of that images.

Finally run `rke up` to upgrade the cluster and go to take a coffee while it is upgrading.

## Certificates rotations
You can rotate certificate by using the `rke cert rotate` command

Doing a certificate rotations restarts the services where the certificates are used, causing a short interruption of the service

You can rotate certificate for this scenario:
- All the certificates
- Certificates for a specific service
- All certs and CA

### All the certificates
To rotate all the certificates while using the same CA run
```
rke cert rotate
```

### Certificate for a single service
To rotate all the certificates of a particular services, for example `kubelet`, while using the same CA run
```
rke cert rotate --service kubelet
```

The Services you can rotate the certificates are:
- etcd
- kubelet
- kube-apiserver
- kube-proxy
- kube-scheduler
- kube-controller-manager

### All certificates and CA
To rotate all the certificates and also the CA, run
```
rke cert rotate --rotate-ca
```

Rotating the CA will also restarts other system pods that will use the new CA,like:
- Networking pods (canal, calico, flannel and weave)
- Ingress Controller pods
- KubeDNS pods

This is the most comprehensive certificates rotate and is the nuclear options in case of some certificates problem

## Adding and removing nodes
To add or remove a nodes, you can add or remove a nodes entry in the cluster.yml file
You can also move, add or remove a roles from a node, keeping in mind that the minimum number of nodes for every role is:
- 3 etcd
- 2 control plane
- all the worker you need

### Cluster Update Note
The safest way to update the cluster (edit of cluster.yml and `rke up` command) is to do 1 update at time.
For example, control-plane functionalities are on node-a and node-b and we want to move the control-plane role from node-b to node-c, what we have to do?
- Step 1: add node-c with control-plane role
- Step 2: remove control-plane role from node-b
