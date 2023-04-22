---
title: Cloud-ready syslog-ng images
linktitle: syslog-ng images
weight: 100
---

We provide [cloud-ready syslog-ng images](https://github.com/axoflow/syslog-ng-docker/). Our images are different from the [upstream syslog-ng images](https://hub.docker.com/r/balabit/syslog-ng/) in a number of ways:

- They are based on Alpine Linux, instead of Debian testing for reliability and smaller size (thus smaller attack surface).
- They incorporate cloud-native features and settings (such as the Kubernetes source).
- They incorporate container-level optimizations (like the use of an alternative malloc library) for better performance and improved security.
- They support the ARM architecture.

Our images are available for the following architectures:

- amd64
- arm/v7
- arm64

## Install the syslog-ng images by Axoflow

You can find the list of tagged versions at [https://github.com/axoflow/syslog-ng-docker/pkgs/container/syslog-ng](https://github.com/axoflow/syslog-ng-docker/pkgs/container/syslog-ng).

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

If you have fixed a bug or would like to contribute your improvements to these images, [open a pull request](https://github.com/axoflow/syslog-ng-docker/pulls). We truly appreciate your help.
