---
title: syslog-ng Helm charts
weight: 300
---

We provide [Helm charts for syslog-ng](https://github.com/axoflow/axosyslog-charts/). You can use these charts to install the [cloud-ready syslog-ng images](https://github.com/axoflow/axosyslog-docker) created and maintained by [Axoflow](https://axoflow.com).

## Usage

> Note: You must have [Helm 3.0 or newer](https://helm.sh) installed to use these charts. Refer to the [official Helm documentation](https://helm.sh/docs/intro/install/) for details.

You can install the charts by running the following commands:

```bash
git clone git@github.com:axoflow/axosyslog-charts.git
cd axosyslog-charts
helm install --generate-name charts/syslog-ng-collector
```

> **Tip**: List all installed releases using `helm list`.

To uninstall a chart release, run:

```bash
helm delete <name-of-the-release-to-delete>
```

## Contribute

If you have fixed a bug or would like to contribute your improvements to these charts, [open a pull request](https://github.com/axoflow/axosyslog-charts/pulls). We truly appreciate your help.
