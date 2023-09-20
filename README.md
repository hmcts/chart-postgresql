# chart-postgresql
[![Build Status](https://dev.azure.com/hmcts/PlatformOperations/_apis/build/status/chart-postgresql)](https://dev.azure.com/hmcts/PlatformOperations/_build?definitionId=856)
This chart is intended for adding the postgres sql databases.

## Prerequisites

- To use this chart, you need a postgres Flexible server created beforehand.
- Please see [cnp-flux-config](https://github.com/hmcts/cnp-flux-config/pull/24968) PR link where the Flexible server is deployed using ASO.

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
