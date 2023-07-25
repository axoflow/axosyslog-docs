---
title: Sending messages to Falcon LogScale
linktitle: Falcon LogScale
---

Starting with version 4.3.0, {{% param "product_name" %}} can send messages to [Falcon LogScale](https://library.humio.com/) using its [Ingest Structured Data API](https://library.humio.com/integrations/api-ingest.html#api-ingest-structured-data).

## Prerequisites

- Create an [Ingest token](https://library.humio.com/falcon-logscale-self-hosted/ingesting-data-tokens.html) for {{% param "product_name" %}} to use in the `token()` option of the destination. <!-- FIXME When creating the token, [set/do not set a parser](https://library.humio.com/data-analysis/parsers-assigning-to-ingest-tokens.html) for the token. -->

## Ingest Structured Data API

The `logscale()` destination feeds LogScale via the [Ingest Structured Data API](https://library.humio.com/integrations/api-ingest.html#api-ingest-structured-data).

Minimal configuration:

```sh
destination d_logscale {
  logscale(
    token("your-logscale-ingest-token")
  );
};
```

## Options

```sh
url()
rawstring()
timestamp()
timezone()
attributes()
extra-headers()
content-type()
```

`attributes()`: A JSON object representing key-value pairs for the LogScale Event, formatted as [{{% param "product_name" %}} value-pairs](https://axoflow.com/docs/axosyslog-core/chapter-concepts/concepts-value-pairs/option-value-pairs/). Default value: `"--scope rfc5424 --exclude MESSAGE --exclude DATE --leave-initial-dot"`

`content-type()`: The content-type of the HTTP request. Default value: `"application/json"`

`extra-headers()`: Extra headers for the HTTP request. Default value: `""`

`rawstring()` accepts template that you can use to format the [LogScale event](https://library.humio.com/integrations/api-ingest.html#api-ingest-more-events). Default value: `${MESSAGE}`

`timestamp()`: The timestamp added to the [LogScale event](https://library.humio.com/integrations/api-ingest.html#api-ingest-more-events). Default value: `${S_ISODATE}"`

`timezone()`: The timezone of the event. Default value: `""`

`url()`: The URL of the LogScale Ingest API. Default value: `"https://cloud.humio.com"`
