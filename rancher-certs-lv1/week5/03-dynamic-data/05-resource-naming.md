# Resource Naming

Kubernetes treats Secrets, Certificates, and Registry Credentials as Secrets, and therefore each of these resources must have a unique name within a namespace.

If you try to create any of these resources with a name that already exists in any other resource, Kubernetes will return an error.

A strategy for dealing with this is to name Secrets for what they are,
Certificates for their Common Name (CN), and Registry Credentials for
where they apply.

For example:
- Secret: mysql-admin-password
- Certificate: www-example-com-crt
- Registry Credential: registry-gitlab-com
