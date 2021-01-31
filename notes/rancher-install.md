# INSTALL RANCHER WITH RKE
How to install a single node Rancher cluster with RKE written while studying for Rancher Certified Operator Level 1
Bastion Host is my MacBook Pro, Rancher Node is a Parallels VM running Ubuntu Server 20.04
User to login to the nodes is root and the ssh-key is defined in my notes

## Prepare the Rancher Node
Step to do in the server that will become the Rancher Node

### Install Docker on the target host
Just follow the docker guides of the os you choose, in my case, ubuntu: https://docs.docker.com/engine/install/ubuntu/. Remember to install a Rancher compatible version of Docker, in my case I have to run
```
apt-get install docker-ce=5:19.03.14~3-0~ubuntu-focal docker-ce-cli=5:19.03.14~3-0~ubuntu-focal containerd.io
```

### Set root login for ssh
- Start to edit the ssh configuration file launching `vi /etc/ssh/sshd_config`
- Add the row `PermitRootLogin yes`
- Restart ssh daemon, eg: `systemctl restart ssh`

### Add login user to docker group
Simply run `usermod -a -G docker root`

## Prepare the Bastion host
Step to do in the client or server that will become the Bastion Host

### Install RKE binary
- Download RKE binary from https://github.com/rancher/rke/releases
- chmod +x the binary just downloaded
- Move it to /usr/local/bin or any directory included in $PATH
- Test installation by issueing `rke --version`

### Create the ssh-key to access the Rancher Nodes without password
- On the Bastion host run `ssh-keygen -t rsa -f rancher-id_rsa`
- Copy the public key on the Rancher Node
- Add the key in the `~/.ssh/authorized_keys` file of the node
- Restart ssh service

### Create rke configuration file
Run `rke-config` to create the `cluster.yaml` file and follow the steps prompted. Here an example output of the configuration I build

```
MacBookPro:rancher-bible mossicrue$ rke config
[+] Cluster Level SSH Private Key Path [~/.ssh/id_rsa]: /Users/mossicrue/Documents/GitHub/rancher-bible/rancher-id_rsa
[+] Number of Hosts [1]:
[+] SSH Address of host (1) [none]: 10.211.55.5
[+] SSH Port of host (1) [22]:
[+] SSH Private Key Path of host (10.211.55.5) [none]:
[-] You have entered empty SSH key path, trying fetch from SSH key parameter
[+] SSH Private Key of host (10.211.55.5) [none]:
[-] You have entered empty SSH key, defaulting to cluster level SSH key: /Users/mossicrue/Documents/GitHub/rancher-bible/rancher-id_rsa
[+] SSH User of host (10.211.55.5) [ubuntu]: root
[+] Is host (10.211.55.5) a Control Plane host (y/n)? [y]:
[+] Is host (10.211.55.5) a Worker host (y/n)? [n]: y
[+] Is host (10.211.55.5) an etcd host (y/n)? [n]: y
```

## Deploy Kubernetes Cluster
Run the command `rke up` in the path where you put the `cluster.yaml` file.
In case of error for an unsupported version of docker, delete the `cluster.rkestate` file and reissue `rke up`

## Install kubectl
First of all, we need to know which version of kubernetes is running to install a compatible version of kubectl. To find them run `grep kubernetesVersion cluster.rkestate` the output will be more like this:

```
MacBookPro:rancher-bible mossicrue$ grep kubernetesVersion cluster.rkestate
      "kubernetesVersion": "v1.19.7-rancher1-1",
      "kubernetesVersion": "v1.19.7-rancher1-1",
```

The kubectl client to download will be the v1.19.7, to download them run `curl -LO https://dl.k8s.io/release/v1.19.7/bin/linux/amd64/kubectl` or `curl -LO https://dl.k8s.io/release/v1.19.7/bin/darwin/amd64/kubectl` if you are using macOS
