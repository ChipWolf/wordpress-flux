# Setup

## Table of Contents

- [Setup](#setup)
  - [Table of Contents](#table-of-contents)
  - [Installation](#installation)
  - [Sealing Secrets](#sealing-secrets)
    - [Examples](#examples)
      - [Sealing a single value](#sealing-a-single-value)
      - [Sealing a file](#sealing-a-file)
  - [Development](#development)

## Installation

```shell
./bootstrap-cluster.sh
```

The [script](bootstrap-cluster.sh) does the following:

1. Installs [flux](https://github.com/fluxcd/flux)
2. Retrieves the new flux public key and saves it to the GitHub repo as a deploy key _(see [add-repo-key.sh](add-repo-key.sh))_

## Sealing Secrets

There are two secrets required for this cluster: one for [`cert-manager`](../system/cert-manager), the other for [`external-dns`](../system/external-dns).

Secrets are encrypted using [sealed-secrets](https://github.com/bitnami-labs/sealed-secrets); this makes secrets safe to store in this public repository.
Once encrypted, secrets can only be decrypted by the controller in the cluster.

Once [_installation_](#installation) is completed, you can seal the required secrets by following [the usage documentation](https://github.com/bitnami-labs/sealed-secrets#usage) or the [examples](#example) below.

_Note: if a workload's folder contains a `secret.txt`([**example**](../system/external-dns/secret.txt)), it represents what is stored in the sealed secret. In these files, any actual secrets are substituted with placeholders. Otherwise it should be self-explanatory given the name of the secret._

### Examples

The output of the commands in these examples can be used to replace the content of a workload's `sealed-secrets.yaml`([**example**](../system/external-dns/sealed-secret.yaml)).

#### Sealing a single value

The following example demonstrates **how to seal a single secret value**. In this scenario you are sealing the `aws-secret-access-key` in a secret named `route53-credentials` for the `cert-manager` namespace.

_Note: `YOUR_SECRET_HERE` should be substituted for an `aws-secret-access-key` in this example._

```shell
echo -n YOUR_SECRET_HERE | kubectl create secret generic mysecret --dry-run=client --from-file=aws-secret-access-key=/dev/stdin -o json | kubeseal --controller-name kube-system-sealed-secrets --controller-namespace kube-system --name route53-credentials --namespace cert-manager -o yaml
```

#### Sealing a file

The following example demonstrates **how to seal a file**. In this scenario you are sealing the `values.yaml` in a secret named `external-dns-helm-values` for the `external-dns` namespace.

_Note: for this example, ensure you substitute any placeholders in the `secret.txt` before continuing._

```shell
cat secret.txt | kubectl create secret generic mysecret --dry-run=client --from-file=values.yaml=/dev/stdin -o json | kubeseal --controller-name kube-system-sealed-secrets --controller-namespace kube-system --name external-dns-helm-values --namespace external-dns -o yaml
```

## Development

For local testing and development:

- Download and install [minikube](https://minikube.sigs.k8s.io/).
- Follow the [_installation_](#installation) above

_Note: by design, the DNS and certificate provider do not function with a local deployment._

Run this command, then visit http://localhost to access the Wordpress site:

```shell
kubectl port-forward service/wordpress -n wordpress 80:80
```
