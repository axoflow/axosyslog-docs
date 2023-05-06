---
title: Install AxoSyslog with Docker
linktitle: Docker
weight: 100
---

{{< include-headless "cloud-ready-images.md" >}}

## Install the syslog-ng images by Axoflow

You can find the list of tagged versions at [https://github.com/axoflow/axosyslog-docker/pkgs/container/syslog-ng](https://github.com/axoflow/axosyslog-docker/pkgs/container/syslog-ng).

To install the latest stable version, run:

```shell
docker pull ghcr.io/axoflow/syslog-ng:latest
```

You can also use it as a base image in your Dockerfile:

```shell
FROM ghcr.io/axoflow/syslog-ng:latest
```

If you want to test a development version, you can use the nightly builds:

```shell
docker pull ghcr.io/axoflow/syslog-ng:nightly
```

> Note: These named packages are automatically updated when a new syslog-ng package is released. To install a specific version, run `docker pull ghcr.io/axoflow/syslog-ng:<version-number>`, for example:
>
> ```shell
> docker pull ghcr.io/axoflow/syslog-ng:4.1.1
> ```

## Contribute

If you have fixed a bug or would like to contribute your improvements to these images, [open a pull request](https://github.com/axoflow/axosyslog-docker/pulls). We truly appreciate your help.
