# Upgrading Workloads
You can upgrade a workload by directly editing the YAML and changing the image or other parameters, or by clicking the Edit button and going back through the form to change items to new values.

When you do this, Kubernetes will perform an update according to the update specifications you set in the current workload definition.

Changing the scaling and upgrade policy or changing the number of replicas doesn't result in a redeployment of the running workload, so if you change this at the same time that you change other parameters, Rancher effectively applies the scaling and upgrade policy to the current workload and then performs any additional changes necessary.

# Rolling Back Workloads
All upgrades performed via the Rancher UI are equivalent to upgrades performed with kubectl and --record=true.

If you select Rollback for your workload, you'll be presented with a list of points to which you can roll back. Selecting any of these shows a diff between that workload and the current one, which helps you determine what will change when rolling back.
