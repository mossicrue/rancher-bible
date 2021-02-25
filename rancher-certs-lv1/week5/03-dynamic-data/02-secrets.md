# Secrets

Just like a ConfigMap, Secrets store arbitrary key/value pairs.  
These hold sensitive data like passwords or API keys and will get special treatment within your Kubernetes environment.  
They are base64-encoded to obfuscate their plaintext values when viewed by humans, but this provides a minimal amount of security.

Kubernetes does not encrypt secrets by default. Doing so requires additional configuration of the kube-apiserver component.  
The [Rancher Hardening Guide](https://rancher.com/docs/rancher/v2.x/en/security/) shows how to encrypt secrets at rest, and detailed information about how to do it is included in the next level of certification.

Unlike ConfigMaps, Secrets can be assigned to a namespace or to a project. If assigned to a project, they are available to all namespaces within that project.

## Create a Secret in Cluster Explorer

- Click the dropdown menu on the top left of the page and select "Clusetr Explorer"

- In the "Storage" section choose the "Secrets" entry

- Now click on the "Create" button the top right and choose the secret type:
  - Opaque: arbitrary user-defined `key=value` pairs
  - Registry (kubernetes.io/dockerconfigjson): serialized `~/.docker/config.json` file
  - Certificate (kubernetes.io/tls): data for a TLS client or server
  - SSH (kubernetes.io/ssh-auth): credentials for SSH authentication
  - Basic Auth (kubernetes.io/basic-auth): credentials for basic authentication

- Fill the forms with the requested data and click "Create" when it's all ready

> **NOTE:** In Cluster Explorer it's impossible to reveal a secret value, but it's still copyable by clicking on the "Copy" button of the `key=value` pair

## Create a Secret in Cluster Manager

- Click on the dropdown cluster / project menu on the top and select the project where create the ConfigMap

- In the new page click "Resources" on the dropdown menu and select the "Secrets" entry

- Now click on the "Create" button in the top right and fill the forms with the requested data

- Fill the "Secret Values" sections with the `key=value` pair you need to add

- Once it's all ready, click on the "Save" button

> **NOTE:** To reveal secret value just long click on the value of one key, it's also possible to copy this value when long clicking on it.
