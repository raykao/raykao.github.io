---
layout: post
title:  "Consul ACL Tokens in Practice"
date:   2019-10-25 14:25:00 -0400
tags: one two three
---
# Setting Up Consul TLS in Practice

## Seeding TLS Certificates

- Every agent needs the `consul-agent-ca.pem` file.

## Generate CA Certificates
```bash
consul tls create ca
```

## Store Certificates in Azure Key Vault

```bash
az keyvault secret set --name <secret-name> --vault-name <vault-name> --value $(cat consul-agent-ca.pem | base64)
```

Storing the certificates in AKV allows us to inject the certs in an automation pipeline (Azure Pipelines).  I would suggest to do this at [Packer](https://packer.io) Image build time.  This will allow the certificate key to be rotated and made avaiable as the latest value during a new build when needed.