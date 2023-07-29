---
---
## `auth()` {#auth}

You can set authentication in the `auth()` option of the `opentelemetry()` driver. By default, authentication is disabled (`auth(insecure())`).

The following authentication methods are available in the `auth()` block:

### `adc()` {#adc}

[Application Default Credentials (ADC)](https://cloud.google.com/docs/authentication/application-default-credentials). This authentication method is only available for destination. It accepts the `target-service-account()` option, where you can list service accounts to match against when authenticating the server.

### `alts()` {#alts}

[Application Layer Transport Security (ALTS)](https://grpc.io/docs/languages/cpp/alts/) is a simple to use authentication, only available within Google's infrastructure.

```shell
source {
    opentelemetry(
      port(12345)
      auth(alts())
    );
  };
```

### `insecure()` {#insecure}

This is the default method, authentication is disabled (`auth(insecure())`).

### `tls()` {#tls}

<!-- FIXME xinclude these from the other tls blocks -->

`tls()` accepts the `key-file()`, `cert-file()`, `ca-file()` and `peer-verify()` (possible values:
`required-trusted`, `required-untrusted`, `optional-trusted` and `optional-untrusted`) options.

```shell
destination {
    opentelemetry(
      url("your-otel-server:12346")
      auth(
        tls(
          ca-file("/path/to/ca.pem")
          key-file("/path/to/key.pem")
          cert-file("/path/to/cert.pem")
        )
      )
    );
  };
```

> Note: `tls(peer-verify())` is not available for the `opentelemetry()` destination.
