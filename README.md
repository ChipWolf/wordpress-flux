# Wordpress GitOps

Leverages [Flux](https://github.com/fluxcd/flux) to automate cluster state using config in this repo

## Setup

See [setup/README.md](setup/README.md) for more details.

## Workloads

### Frontend

* [wordpress](wordpress/) - content management platform

### System

* [cert-manager](system/cert-manager/) - automates TLS certificates (https)
* [external-dns](system/external-dns/) - automated DNS records
* [flux](system/flux/) - syncs this repo with the cluster
* [ingress-nginx](system/ingress-nginx/) - point-of-entry for all web requests
* [sealed-secrets](system/sealed-secrets/) - allows secrets to be encrypted and added to this public repo
