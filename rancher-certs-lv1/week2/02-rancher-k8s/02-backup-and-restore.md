# BACKING UP RANCHER ON KUBERNETES
When Rancher is installed into an RKE cluster, it uses the etcd datastore to hold its configuration.
If you're backing up the RKE cluster, you're backing up Rancher.
Be sure to move backups offsite, either manually or by having RKE copy them to an S3-compatible datastore after making a snapshot.

## Manual Backup
If you want to make a manual backup, launch the command
```bash
rke etcd snapshot-save --name <snaphost-name> --config <cluster.yml-path>
```
If the 2 arguments aren't specified the default value will be:
- `rke_etcd_snapshot_<pretty-print timestamp>.zip` for the backup name
- `$PWD/cluster.yml` or `cluster.yml` for the cluster config file path

# RESTORING RANCHER BACKUP ON KUBERNETES
These instructions assume that you're running a three-node HA RKE cluster for Rancher. If you're running a single-node cluster, skip the step where you add the additional two nodes and continue with repointing the load balancer or DNS entry.

## Shut Down old cluster
When you shut down the Rancher cluster that you're replacing, all of the downstream clusters will continue to operate.
Users who connect through the Rancher server to manage clusters will be unable to do so, until the Rancher cluster is restored, but all operational workloads will continue to run. Kubernetes will continue to maintain state.
The downstream servers can still be accessible through ssh connection or similar.

We shut down the old cluster nodes to cleanly disconnect the downstream clusters from Rancher.

## Prepare New Nodes
First, prepare three new nodes for the new cluster. These can be the same size or larger than the existing Rancher server nodes.
Then, choose one of these nodes to be the initial "target node" for the restore.
We will bring this node up first and then add the other two once the cluster is online.

## Configure RKE
Now, make a backup copy of the RKE cluster.yml and cluster.rkestate files you used to build the original cluster. Store these in a safe place until the new cluster is online.
Then, edit cluster.yml and make the following changes:
- Remove or comment out the entire addons section. The information about the Rancher deployment is already in etcd backup.
- Change the nodes section to point to the new nodes
- Comment out all the nodes section except the chosen target node.

## Restore the Database
How you perform the initial restore of the database depends on where the data is stored: locally or on a S3 bucket.

### Snapshot Stored Locally
First, move the snapshot you want to restore on the bastion host from an old cluster node.
Then, copy it into /opt/rke/etcd-snapshots on the target node of the new cluster
Finally, restore the snapshot with rke etcd snapshot-restore, passing it the name of the snapshot and pointing to the cluster.yml file
```bash
rke etcd snapshot-restore --name restore-lab-v2.4.13-2021-02-04 --config cluster.yml
```

**NOTE**: In case of `Error: snapshot missing hash but --skip-hash-check=false` remember to remove the .zip file extension form the --name options parameter

### Snapshot Stored on S3
To restore a cluster from a snapshot with on Amazon S3 Bucket, still use the rke etcd snapshot-restore adding it the parameters needed to access S3.
```bash
rke etcd snapshot-restore --s3 --name restore-lab-v2.4.13-2021-02-04.zip --config config.yml \
--s3-endpoint s3.amazonaws.com --bucket-name rancher-certification-nhdlhy \
--access-key ${AWS_ACCESS_KEY_ID} --secret-key ${AWS_SECRET_KEY}
```

## Bring Up the Cluster
Bring up the cluster on the target node by running rke up, pointing to the cluster config file.
When the cluster is ready, RKE will write a credentials file to the local directory.
Configure kubectl to use this file and then check on the status of the cluster. Wait for the target node to change to Ready. The three old nodes will be in a NotReady state.

## Complete the Transition to the New Cluster
- When the target node is Ready, remove the old nodes with `kubectl delete node`.
- Reboot the target node to ensure cluster networking and services are in a clean state
- Wait until all pods in kube-system, ingress-nginx, and the rancher pod in cattle-system return to a Running state
- The cattle-cluster-agent and cattle-node-agent pods will be in an Error or CrashLoopBackOff state until the Rancher server is up and DNS has been pointed to the new cluster.

## Add the New Nodes
Skip this step if you're restoring a single-node cluster.
- Edit cluster.yml and uncomment the additional nodes
- Run `rke up` to add the new nodes to the cluster
- Wait for all nodes to show Ready in the output of `kubectl get nodes`
- Reboot the new nodes to ensure they are in a consistent state

## Reconfigure Inbound Access
Once the cluster is up and all three nodes are Ready, complete any final DNS or load balancer change necessary to point the external URL to the new cluster.
This might be a DNS change to point to a new load balancer, or it might mean that you need to configure the existing load balancer to point to the new nodes.
After making this change the agents on the downstream clusters will automatically reconnect.
Because of backoff timers on the clusters, they may take up to 15 minutes to reconnect.

## Techincal Util Fixes

### Reset the admin password
If for any reason the admin user can't log in after the restore or you need to reset its password for any reasons, like No LDAP and you don't know the password, run this command

```bash
KUBECONFIG=./kube_config_cluster.yml
$ kubectl --kubeconfig $KUBECONFIG -n cattle-system exec $(kubectl --kubeconfig $KUBECONFIG -n cattle-system get pods -l app=rancher | grep '1/1' | head -1 | awk '{ print $1 }') -- reset-password
```

Below a sample output where it print the new administrator password, change it from the UI then

```bash
New password for default admin user (user-w65n6):
O4g-w2HiUEBKFww5i4CN
```
