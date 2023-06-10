---
title: Sending Kubernetes logs to OpenSearch
weight: 100
---

The following tutorial shows you how to send Kubernetes logs to OpenSearch.

## Prerequisites

You need a Kubernetes cluster. We used [minikube](https://minikube.sigs.k8s.io/docs/) with docker driver and Helm. We used a Ubuntu 22.04 (amd64) machine, but it should work on any system that can run minikube (2 CPUs, 2GB of free memory, 20GB of free disk space).

The OpenSearch service needs a large mmap count setting, so set it to at least 262144, for example:

```bash
sysctl -w vm.max_map_count=262144
```

## Generate logs

1. Install kube-logging/log-generator to generate logs. Run the following commands.

    ```bash
    helm repo add kube-logging https://kube-logging.github.io/helm-charts
    helm repo update
    helm install --generate-name --wait kube-logging/log-generator
    ```

1. Check that the log-generator is running:

    ```bash
    kubectl get pods
    ```

    The output should look like:

    ```bash
    NAME                                        READY   STATUS        RESTARTS       AGE
    log-generator-1681984863-5946c559b9-ftrrn   1/1     Running       0              8s
    ```

## Set up OpenSearch

1. Install an OpenSearch cluster with Helm:

    ```bash
    helm repo add opensearch https://opensearch-project.github.io/helm-charts/
    helm repo update
    helm install --generate-name --wait opensearch/opensearch
    helm install --generate-name --wait opensearch/opensearch-dashboards
    ```

1. Now you should have 5 pods. Check that they exist:

    ```bash
    kubectl get pods
    ```

    The output should look like:

    ```bash
    NAME                                                READY   STATUS    RESTARTS   AGE
    log-generator-1681984863-5946c559b9-ftrrn           1/1     Running   0          3m39s
    opensearch-cluster-master-0                         1/1     Running   0          81s
    opensearch-cluster-master-1                         1/1     Running   0          81s
    opensearch-cluster-master-2                         1/1     Running   0          81s
    opensearch-dashboards-1681999620-59f64f98f7-bjwwh   1/1     Running   0          44s
    ```

1. Forward the 5601 port of the OpenSearch Dashboards service (replace the name of the pod with your pod).

    ```bash
    kubectl port-forward opensearch-dashboards-1681999620-59f64f98f7-bjwwh 8080:5601
    ```

    The output should look like:

    ```bash
    Forwarding from 127.0.0.1:8080 -> 5601
    Forwarding from [::1]:8080 -> 5601
    ```

1. Log in to the dashboard at `http://localhost:8080` with admin/admin. You will soon create an Index Pattern here, but first you have to send some logs from syslog-ng.

## Set up axosyslog-collector

1. Add the AxoSyslog Helm repository:

    ```bash
    helm repo add axosyslog https://axoflow.github.io/axosyslog-charts
    helm repo update
    ```

1. Create a YAML file (called `axoflow-demo.yaml` in the examples) to configure the collector.

    ```yaml
    config:
      sources:
        kubernetes:
          # Collect kubernetes logs
          enabled: true
      destinations:
        # Send logs to OpenSearch
        opensearch:
          - address: "opensearch-cluster-master"
            index: "test-axoflow-index"
            user: "admin"
            password: "admin"
            tls:
              # Do not validate the server's TLS certificate.
              peerVerify: false
            # Send the syslog fields + the metadata from .k8s.* in JSON format
            template: "$(format-json --scope rfc5424 --exclude DATE --key ISODATE @timestamp=${ISODATE} k8s=$(format-json .k8s.* --shift-levels 2 --exclude .k8s.log))"
    ```

1. Check how the syslog-ng.conf file looks with your custom values:

    ```bash
    helm template -f axoflow-demo.yaml -s templates/config.yaml axosyslog/axosyslog-collector
    ```

    The output should look like:

    ```yaml
    # Source: axosyslog-collector/templates/config.yaml
    apiVersion: v1
    kind: ConfigMap
    metadata:
      labels:
        helm.sh/chart: axosyslog-collector-0.3.0
        app.kubernetes.io/name: axosyslog-collector
        app.kubernetes.io/instance: release-name
        app.kubernetes.io/version: "4.2.0"
        app.kubernetes.io/managed-by: Helm
      name: release-name-axosyslog-collector
    data:
      syslog-ng.conf: |
        @version: current
        @include "scl.conf"

        options {
          stats(
            level(1)
          );
        };

        log {
          source { kubernetes(); };
          destination {
            elasticsearch-http(
              url("https://opensearch-cluster-master:9200/_bulk")
              index("test-axoflow-index")
              type("")
              template("$(format-json --scope rfc5424 --exclude DATE --key ISODATE @timestamp=${ISODATE} k8s=$(format-json .k8s.* --shift-levels 2 --exclude .k8s.log))")
              user("admin")
              password("admin")
              tls(
                peer-verify(no)
              )
            );
          };
        };
    ```

1. Install the `axosyslog-collector` chart:

    ```bash
    helm install --generate-name --wait -f axoflow-demo.yaml axosyslog/axosyslog-collector
    ```

    The output should look like:

    ```bash
    NAME: axosyslog-collector-1682002179
    LAST DEPLOYED: Thu Apr 20 16:49:39 2023
    NAMESPACE: default
    STATUS: deployed
    REVISION: 1
    TEST SUITE: None
    NOTES:
    1. Watch the axosyslog-collector-1682002179 container start.
    ```

1. Check your pods:

    ```bash
    kubectl get pods --namespace=default -l app=axosyslog-collector-1682002179 -w
    kubectl get pods
    ```

    The output should look like:

    ```bash
    NAME                                                READY   STATUS    RESTARTS   AGE
    log-generator-1681984863-5946c559b9-ftrrn           1/1     Running   0          13m
    opensearch-cluster-master-0                         1/1     Running   0          11m
    opensearch-cluster-master-1                         1/1     Running   0          11m
    opensearch-cluster-master-2                         1/1     Running   0          11m
    opensearch-dashboards-1681999620-59f64f98f7-bjwwh   1/1     Running   0          10m
    axosyslog-collector-1682002179-pjlkn                1/1     Running   0          6s
    ```

## Check the logs in OpenSearch

1. Open OpenSearch dashboard at `http://localhost:8080/app/management/opensearch-dashboards/`.
1. Create an Index Pattern called `test-axoflow-index`: `http://localhost:8080/app/management/opensearch-dashboards/indexPatterns`. At Step 2, set the **Time** field to `@timestamp`.

    ![OpenSearch create index pattern for syslog messages screenshot](opensearch-create-index.webp)

1. Now you can see your logs on the Discover view at `http://localhost:8080/app/discover`. Opening the detailed view for a log entry shows you the fields sent to OpenSearch.

    ![OpenSearch Kubernetes log messages collected with the AxoSyslog syslog-ng distribution](opensearch-syslog-messages.webp)
    ![Kubernetes metadata collected with the AxoSyslog syslog-ng distribution](opensearch-syslog-message-details.webp)
