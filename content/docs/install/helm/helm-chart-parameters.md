---
title: Parameters of the AxoSyslog collector Helm chart
linktitle: Chart parameters
weight: 100
---

The following table lists the configurable parameters of the AxoSyslog collector chart and their default values. For details on installing the chart, see {{% xref "/docs/install/helm/_index.md" %}}.


| Parameter | Description | Default |
| --------- | ----------- | ------- |
|  image.repository  | The container image repository |  ghcr.io/axoflow/axosyslog  |
|  image.pullPolicy  | The container image pull policy |  IfNotPresent  |
|  image.tag  | The container image tag |  {{< param "version" >}}   |
|  image.extraArgs  | Custom arguments applied as the value of spec.container.args |  []  |
|  imagePullSecrets  | The names of secrets containing private registry credentials |  []  |
|  nameOverride  | Override the chart name |  ""  |
|  fullnameOverride  | Override the full chart name |  ""  |
|  daemonset.enabled  | Deploy AxoSyslog as a DaemonSet |  true  |
|  daemonset.labels  | Additional labels to apply to the DaemonSet |  {}  |
|  daemonset.annotations  | Additional annotations to apply to the DaemonSet |  {}  |
|  daemonset.affinity  | Pod affinity |  {}  |
|  daemonset.nodeSelector  | Node labels for pod assignment |  {}  |
|  daemonset.resources  | Resource requests and limits |  {}  |
|  daemonset.tolerations  | Tolerations for pod assignment |  []  |
|  daemonset.hostAliases  | Add host aliases |  []  |
|  daemonset.secretMounts  | Mount additional secrets as volumes |  []  |
|  daemonset.extraVolumes  | Additional volumes to mount |  []  |
|  daemonset.securityContext  | Security context for the pod |  {}  |
|  daemonset.maxUnavailable  | The maximum number of unavailable pods during a rolling update |  1  |
|  daemonset.hostNetworking  | Whether to enable host networking |  false  |
|  rbac.create  | Whether to create RBAC resources |  false  |
|  rbac.extraRules  | Additional RBAC rules |  []  |
|  openShift.enabled  | Whether to deploy on OpenShift |  false  |
|  openShift.securityContextConstraints.create  | Whether to create SecurityContextConstraints on OpenShift |  true  |
|  openShift.securityContextConstraints.annotations  | Annotations to apply to SecurityContextConstraints |  {}  |
|  serviceAccount.create  | Whether to create a service account |  true  |
|  serviceAccount.annotations  | Annotations to apply to the service account |  {}  |
|  namespace  | The Kubernetes namespace to deploy to |  ""  |
|  podAnnotations  | Additional annotations to apply to the pod |  {}  |
|  podSecurityContext  | Security context for the pod |  {}  |
|  securityContext  | Security context for the container |  {}  |
|  resources  | Resource requests and limits for the container. If not set, the values of daemonset.resources are used. |  {}  |
|  nodeSelector  | Node labels for pod assignment |  {}  |
|  tolerations  | Tolerations for pod assignment |  []  |
|  affinity  | Pod affinity |  {}  |
|  updateStrategy  | Update strategy for the DaemonSet |  RollingUpdate  |
|  priorityClassName  | The name of the [PriorityClass](https://kubernetes.io/docs/concepts/scheduling-eviction/pod-priority-preemption/#priorityclass) the pod belongs to |  ""  |
|  dnsConfig  | The [DNS configuration of the pod](https://kubernetes.io/docs/concepts/services-networking/dns-pod-service/#pod-dns-config) |  {}  |
|  hostAliases  | Additional [entries to the pod's hosts file](https://kubernetes.io/docs/tasks/network/customize-hosts-file-for-pods/#adding-additional-entries-with-hostaliases) |  []  |
|  secretMounts  | Additional secrets to mount for the pod. If not set, the values of daemonset.secretMounts are used. |  []  |
|  extraVolumes  | Additional volumes to mount for the pod. If not set, the values of daemonset.extraVolumes are used. |  []  |
|  terminationGracePeriodSeconds  | How many seconds a [pod with a failing probe](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/#configure-probes) has before shut down |  30  |
