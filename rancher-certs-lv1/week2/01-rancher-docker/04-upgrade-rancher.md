# UPGRADE RANCHER
The upgrade procedure for Rancher running in a Docker container is similar to the procedure for making and restoring a backup.
The only difference is that instead of starting the existing Rancher container, a new container with the new version of Rancher will start.
Rancher will perform any upgrades on the data itself when it starts.

## Upgrade Rancher with Docker Volume
- Stop the Rancher container: infallible_shaw, v2.4.11
```bash
docker stop infallible_shaw
```
- Create a data container that uses the same volume
```bash
docker create --volume-from infallible_shaw \
--name rancher-data rancher/rancher:v2.4.11
```
- Launch a temporary container that extracts `/var/lib/rancher` to a local tar archive
```bash
docker run --volumes-from rancher-data -v $PWD:/backup \
busybox tar zcvf /backup/rancher-data-backup-2.4.11-2021-02-01.tar.gz /var/lib/rancher
```
- Pull the latest or desired version of the Rancher container container image
```bash
docker pull rancher/rancher:v2.4.13
```
- Start a new container with the same certificate options as the original container, adding a `--volumes-from` that points to the data container.
```bash
docker run -d --volumes-from rancher-data --restart=unless-stopped \
-p 80:80 -p 443:443 rancher/rancher:v2.4.13
```
- Verify the upgrade by logging into the new Rancher container and confirming that it is operating correctly.
- Delete the stopped Rancher container so that it doesn't restart if the host is rebooted.
```bash
docker container rm infallible_shaw
```

## Upgrading Rancher with Mount-Binded Volume
- Stop the Rancher container: adoring_haslett, v2.4.11
```bash
docker stop adoring_haslett
```
- Create a tar archive from the bind-mount directory or copy the `/opt/rancher` to `/opt/rancher.bkp`
```bash
sudo cp -Rp /opt/rancher /opt/rancehr.bk
```
- Pull the latest or desired version of the Rancher container container image
```bash
docker pull rancher/rancher:v2.4.13
```
- Start a new container with the same certificate options as the original container, mounting the bind-mount host directory to `/var/lib/rancher`
```bash
docker run -d --restart=unless-stopped -p 80:80 -p 443:443 \
-v /opt/rancher:/var/lib/rancher rancher/rancher:v2.3.5
```
- Verify the upgrade by logging into the new Rancher container and confirming that it is operating correctly.
- Delete the stopped Rancher container so that it doesn't restart if the host is rebooted.
```bash
docker container rm adoring_haslett
```

If the upgraded Rancher container doesn't operate as expected, stop it, follow the steps to restore the backup, and start the old container to return to the previous state.
Once Rancher is back up and running, delete the failed upgrade container from the host.
