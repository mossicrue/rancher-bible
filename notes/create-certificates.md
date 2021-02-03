# CERTIFICATES CREATION

## Create a self-signed root CA
The follow procedure shows how to create a self-signed root CA that can be used to sign all the server certificate you need.
For the root CA, the csr passage is optional since we don't need to share the csr with a trusted CA like GoDaddy or Let'sEncrypt, we will simply self-signed it by ourself.

First, create the root CA key to sign the RootCA. Save the key in a secure place and don't share it!
```bash
openssl genrsa -out ca.key 4096
```

Then, create and self sign the root certificates using the command below and following the command prompt
```bash
openssl req -x509 -new -nodes -key ca.key -sha256 -days 730 -out ca.crt
```
Note: if no -days is specified, the root certificate will be valid only for 2 days

## Create a server certificate
This procedure is to repeat for all the server you want use the certificates

First, create the certificate key, this time must be 2048 bit key long
```bash
openssl genrsa -out server.key 2048
```

Then, create the certificate signing request (csr) in which you will specify all the details of the certificate
**NOTE:** Remember to specify the Common Name using an IP Adress or a domain name for the service, otherwise the certificate cannot be verified

```bash
openssl req -new -key server.key -out server.csr
```

Finally, generate the certificate using the `server.csr` that will be signed by the root CA certificate and key

```bash
openssl x509 -req -in server.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out server.crt -days 365 -sha256
```
