# DAY 2 OPERATION FOR RANCHER

## Secure KUBECONFIG file
After Rancher is successfully deployed 2 new file are generated:
- kube_config_cluster.yaml
- cluster.rkestates

The `kube_config_cluster.yaml` file is the kubeconfig file that you can put in the `~/.kube` directory or being setted in the `KUBECONFIG` env to log in into the server and work with them

For my test, the command to run for exporting the KUBECONFIG value was: `export KUBECONFIG=/Users/mossicrue/Documents/GitHub/rancher-bible/kube_config_cluster.yml`

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
- Start by upgrading local copy of `rke`
- Use `rke config --list-version --all` to see all the supported kubernetes versions
- Take a one-time etcd snapshot and move it in a secure place, there are no rollbacks in kubernetes upgrade, only disaster recovery
- Set the kubernetes desired version in the `kubernetes_version` key in `cluster.yml`, DON'T set it under `system images`
