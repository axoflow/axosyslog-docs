---
title: syslog-ng kubernetes() source
linktitle: Kubernetes
---

The `kubernetes()` source collects logs from the Kubernetes cluster where syslog-ng is running. It reads plain-text and JSON-formatted container logs (as described in the [Container Runtime Interface (CRI) design proposal](https://github.com/kubernetes/design-proposals-archive/blob/main/node/kubelet-cri-logging.md)), for example, from the `/var/log/containers` or `/var/log/pods` files, and enriches them with various metadata retrieved from the Kubernetes API.

```shell
source s_kubernetes {
  kubernetes(
    base-dir("/var/log/containers")  # Optional. Where to look for the containers' log files. Default: "/var/log/containers"
    cluster-name("k8s")              # Optional. The name of the cluster, used in formatting the HOST field. Default: "k8s"
    prefix(".k8s.")                  # Optional. Prefix for the metadata name-value pairs' names. Default: ".k8s."
    key-delimiter(".")               # Optional. Delimiter for multi-depth name-value pairs' names. Default: "."
  );
};
```

<!-- FIXME create a reference section for the options -->

## Kubernetes metadata

Syslog-ng {{< param "version" >}} retrieves the following metadata fields.

| syslog-ng name-value pair | source |
|---------------------------|--------|
| .k8s.namespace_name | Container log file name.|
| .k8s.pod_name | Container log file name.|
| .k8s.pod_uuid | Container log file name or python kubernetes.client.CoreV1Api.|
| .k8s.container_name | Container log file name or python kubernetes.client.CoreV1Api.|
| .k8s.container_id | Container log file name.|
| .k8s.container_image | python kubernetes.client.CoreV1Api.|
| .k8s.container_hash | python kubernetes.client.CoreV1Api.|
| .k8s.docker_id | python kubernetes.client.CoreV1Api.|
| .k8s.labels.* | python kubernetes.client.CoreV1Api.|
| .k8s.annotations.* | python kubernetes.client.CoreV1Api.|

{{< include-headless "disk-buffer-in-container.md" >}}
