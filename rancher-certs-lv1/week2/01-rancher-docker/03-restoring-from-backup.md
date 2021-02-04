# RESTORING RANCHER ON DOCKER FROM A BACKUP
Like for the backup creation, the process for restoring from a backup change depending on how you made the backup.
In both cases you're going to restore the backup tar archive into a directory that will be made available to a new Rancher server container.

## Restoring Rancher from a Docker Volume
For take the backup in case of Docker Volume we made a data container and then created the tar archive from data.
We now want to overwrite the contents of `/var/lib/rancher` in the Rancher container with the contents of the tar archive.

- Move the backup tar archive you want to restore into the Rancher server and place it in /opt/backup
- Get the Rancher container name (sweet_ritchie)
```bash
docker ps
```
- Stop Rancher container
```bash
docker stop sweet_ritchie
```
- Make fresh backup following the backup guide
- Restore with a temporary container
```bash
docker run --volumes-from sweet_ritchie -v /opt/backup:/backup \
busybox sh -c "rm -rf /var/lib/rancher/* && \
tar pzxvf /backup/rancher-data-backup-2.4.13-2021-02-01.tar.gz"
```
- Start Rancher
```bash
docker start sweet_ritchie
```

## Restoring Rancher from a Bind-Mount Volume
If Rancher container runs using a bind-mounted volume such as `/opt/rancher`, then restoring from a backup is even easier.

- Get the Rancher container name (agitated_albattani)
```bash
docker ps
```
- Move the backup archive you want to restore into the Rancher server and
place it in /opt
- Stop the Rancher container
```bash
docker stop agitated_albattani
```
- Move /opt/rancher to /opt/rancher.old
```bash
sudo mv /opt/rancher /opt/rancher.old
```
- Extract the backup tar archive, this will create a new /opt/rancher directory
```bash
sudo tar xzpf rancher-2.4.13-2020-02-01.tar.gz
```
- Start the Rancher container
```bash
docker start agitated_albattani
```
