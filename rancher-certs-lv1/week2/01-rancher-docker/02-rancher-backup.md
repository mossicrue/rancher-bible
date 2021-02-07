# Make backup of Rancher on Docker
Making a backup of Rancher when running as a container involves backing up the persistent data located at `/var/lib/rancher` inside of the container.
Depending on how you started the container, this might be a Docker volume, or it might be a bind-mounted volume on the host.
When creating backups, use an identifier that includes both the date and the rancher version, like `rancher-data-backup-v2.4.13-2021-02-01.tar.gz`.

## Backup with Docker Volume
If the container uses a Docker volume, the process for making a backup is the following:
- Get the Rancher container name, `sweet_ritchie`
```bash
docker ps
```
- Stop the container
```bash
docker stop sweet_ritchie
```
- Create a data container that uses the same volume
```bash
docker create --volumes-from sweet_ritchie --name rancher-data-2021-02-01 rancher/rancher:v2.4.13
```
- Launch a temporary container that extracts `/var/lib/rancher` to a local tar archive
```bash
docker run --volumes-from rancher-data-2020-04-06 -v $PWD:/backup busybox tar pzcvf /backup/rancher-data-backup-2.4.13-2020-02-01.tar.gz /var/lib/rancher
```
- Start the container
```bash
docker start sweet_ritchie
```
- Move the backup off from the host

When creating the tar archive, it's important to use the -p parameter to maintain file and directory permissions.

## Backup with Bind-Mounted Volume
If the container uses a Bind-Mounted volume, the process for making a backup is the following:
- Get the Rancher container name (agitated_albattani)
```bash
docker ps
```
- Navigate to the /opt directory
```bash
cd /opt
```
- Stop Rancher container
```bash
docker stop agitated_albattani
```
- Tar the local directory
```bash
sudo tar -czpf rancher.2.4.13-2020-02-01.tgz rancher
```
- Start Rancher
```bash
docker start agitated_albattani
```
- Move backups offsite

Also in this case, when creating the tar archive, it's important to use the -p parameter to maintain file and directory permissions.
