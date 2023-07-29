---
title: Receive logs, metrics, and traces from OpenTelemetry
linktitle: OpenTelemetry
---

Starting with version 4.3.0, {{% param "product_name" %}} can receive logs, metrics, and traces from [OpenTelemetry](https://opentelemetry.io/) clients over the [OpenTelemetry Protocol (OTLP/gRPC)](https://opentelemetry.io/docs/specs/otlp/).

## Example: Receiving OpenTelemetry data

The following example receives OpenTelemetry data and forwards it to an OpenTelemetry receiver. Note that by default, {{% param "product_name" %}} doesn't parse the fields of the incoming messages into name-value pairs, but are only available for forwarding using the `opentelemetry()` destination. To parse the fields into name-value pairs, use the [`opentelemetry()` parser]({{< relref "/docs/parsers/opentelemetry/_index.md" >}}).

```shell
log otel_forward_mode_alts {
  source {
    opentelemetry(
      port(12345)
      auth(alts())
    );
  };

  destination {
    opentelemetry(
      url("my-otel-server:12345")
      auth(alts())
    );
  };
};
```

{{< include-headless "opentelemetry-authentication.md" >}}

## `port()` {#port}

The port number to receive incoming connections. Default value: 4317

<!-- FIXME xinclude other common options
 threaded_source_driver_option -->