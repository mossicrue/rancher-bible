# Using Certificates with Rancher

Certificates are the backbone of secure communication on the Internet, and in recent years we've seen the number of websites defaulting to HTTPS grow dramatically.

Kubernetes secures internal communication with TLS certificates, but you can also upload certificates and use them to secure Ingress traffic.

## Static Certificates
Rancher doesn't require that certificates be signed by a recognized certificate authority, but if your certificate requires a chain of certificates that lead back to a recognized CA, include those with the certificate when adding it to Rancher.

After you've added a certificate, you can use it with the Ingress configuration for inbound traffic.

When editing a certificate entry, you will not be shown the private key.  
You'll have to add it again when changing the certificate to show that you have the authority to make the change.

### Create Certificate Secret in Cluster Explorer

- Click the dropdown menu on the top left of the page and select "Clusetr Explorer"

- In the "Storage" section choose the "Secrets" entry

- Now click on the "Create" button the top right and choose the "Certificate" secret type

- Fill the forms with the requested data and upload or copy the private file and all the needed certificates in the 2 dedicated forms

- When it's all ready click on the "Create" button

### Create Certificate Secret in Cluster Manager

- Click on the dropdown cluster / project menu on the top and select the project where create the ConfigMap

- In the new page click "Resources" on the dropdown menu and select the "Secrets" entry

- Click on the "Certificates" tab link to switch the using resources from "Secret" to "Certificates"

- Now click on the "Add Certificate" button and fill the forms with the requested data

- Upload or copy the private key and all the needed certificates in the 2 dedicated forms

- When all it's ready click on the "Save" button

## Dynamic Certificates
Cert Manager from JetStack is a controller that runs in the cluster and dynamically generates certificates on demand.  
These certificates can come from an internal CA, an internal service like Hashicorp Vault, or from LetsEncrypt.

You can request that Cert Manager create a certificate when you create the Ingress that uses it, or you can create Certificate resources that request a certificate from a particular issuer.

In either case, certificates created by Cert Manager are automatically renewed before they expire.

These certificates will appear in the Certificates list and can be used with Ingress resources without having to manually edit the Ingress to add the annotations for generating the cert as part of the Ingress creation.
