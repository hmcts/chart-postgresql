# chart-postgresql
[![Build Status](https://dev.azure.com/hmcts/PlatformOperations/_apis/build/status/chart-postgresql)](https://dev.azure.com/hmcts/PlatformOperations/_build?definitionId=856)
This chart is intended for adding the postgres sql databases.

## Prerequisites

- To use this chart, you need a postgres Flexible server created beforehand.
- Please see [cnp-flux-config](https://github.com/hmcts/cnp-flux-config/pull/25165) PR link where the Flexible server is deployed using ASO.
- Please follow this documentation for generating [ASO secret](https://github.com/hmcts/cnp-flux-config/blob/master/docs/secrets-sops-encryption.md)
- Simple Example to generate ASO secret and encrypt
```
brew install sops
```

```bash
NAMESPACE=probate # adjust this for your Kubernetes namespace
PASSWORD=$(LC_ALL=C tr -dc 'A-Za-z0-9' </dev/urandom | head -c 16 ; echo)
kubectl create secret generic postgres -n $NAMESPACE --from-literal=PASSWORD=${PASSWORD} --type=Opaque -o yaml --dry-run=client > postgres.enc.yaml


sops --encrypt --azure-kv https://dcdcftappsdemokv.vault.azure.net/keys/sops-key/7a5cc0c79b02466c86bc594c431e00f7 --encrypted-regex "^(data|stringData)$" --in-place postgres.enc.yaml
```
## Example configuration

```yaml
flexibleserver: "<flexible server name>"
location: uksouth
setup:
  databases:
   - name: "<name of the database>"
```

Please see example PR [chart-ccd](https://github.com/hmcts/chart-ccd/pull/278)
### Pull Request Validation

A build is triggered when pull requests are created. This build will run `helm lint`, deploy the chart using `ci-values.yaml` and run `helm test`.

### Release Build

Triggered when the repository is tagged (e.g. when a release is created). Also performs linting and testing, and will publish the chart to ACR on success.
