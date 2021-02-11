# Troubleshooting Nginx Proxy
The nginx proxy exists so that non-controlplane nodes can reach the services in the controlplane without knowing which node they’re on or what IP they have.  
If this service isn't running, other nodes won't be able to communicate with the kube-api-server container.

Check the worker and the etcd nodes to verify these containers are running and check their logs with the `docker logs` command

Even if the Kubenetes API-Server is up, connectivity may be interrupted if the nginx proxy is not running to redirect API traffic from non control nodes to the control plane nodes.

Make sure the nginx proxy container is running on all nodes that don't have the control-plane role and also inspect the nginx config for the container to confirm it has been altered and is pointing to all control plane nodes by running:
```bash
docker exec nginx-proxy cat /etc/nginx/nginx.conf
```

Example output is below:
```bash
worker_process auto;
events {
  multi_accept on;
  use epoll;
  worker_connections 1024;
}

stream {
  upstream kube_apiserver {
    server 172.31.43.69:6443;
    server 172.31.32.168:6443;
    server 172.31.33.41:6443;
  }
  server {
    listen        6443;
    proxy_pass    kube_apiserver;
    proxy_timeout 30;
    proxy_connect_timeout 2;
  }
}
```

Finally, as in the case with all our key components not being manaaged as kubernetes API objects, inspect the proxy container logs for any signs of failure or configuration drift by running:

```bash
docker logs nginx-proxy
```
