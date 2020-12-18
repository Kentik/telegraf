# InfluxDB v1.x Output Plugin

The InfluxDB output plugin writes metrics to the [InfluxDB v1.x] HTTP or UDP service.

### Configuration:

```toml
# Configuration for sending metrics to InfluxDB
[[outputs.influxdb]]
  ## The full HTTP or UDP URL for your InfluxDB instance.
  ##
  ## Multiple URLs can be specified for a single cluster, only ONE of the
  ## urls will be written to each interval.
  # urls = ["unix:///var/run/influxdb.sock"]
  # urls = ["udp://127.0.0.1:8089"]
  # urls = ["http://127.0.0.1:8086"]

  ## The target database for metrics; will be created as needed.
  ## For UDP url endpoint database needs to be configured on server side.
  # database = "telegraf"

  ## The value of this tag will be used to determine the database.  If this
  ## tag is not set the 'database' option is used as the default.
  # database_tag = ""

  ## If true, the 'database_tag' will not be included in the written metric.
  # exclude_database_tag = false

  ## If true, no CREATE DATABASE queries will be sent.  Set to true when using
  ## Telegraf with a user without permissions to create databases or when the
  ## database already exists.
  # skip_database_creation = false

  ## Name of existing retention policy to write to.  Empty string writes to
  ## the default retention policy.  Only takes effect when using HTTP.
  # retention_policy = ""

  ## The value of this tag will be used to determine the retention policy.  If this
  ## tag is not set the 'retention_policy' option is used as the default.
  # retention_policy_tag = ""

  ## If true, the 'retention_policy_tag' will not be included in the written metric.
  # exclude_retention_policy_tag = false

  ## Write consistency (clusters only), can be: "any", "one", "quorum", "all".
  ## Only takes effect when using HTTP.
  # write_consistency = "any"

  ## Timeout for HTTP messages.
  # timeout = "5s"

  ## HTTP Basic Auth
  # username = "telegraf"
  # password = "metricsmetricsmetricsmetrics"

  ## HTTP User-Agent
  # user_agent = "telegraf"

  ## UDP payload size is the maximum packet size to send.
  # udp_payload = "512B"

  ## Optional TLS Config for use on HTTP connections.
  # tls_ca = "/etc/telegraf/ca.pem"
  # tls_cert = "/etc/telegraf/cert.pem"
  # tls_key = "/etc/telegraf/key.pem"
  ## Use TLS but skip chain & host verification
  # insecure_skip_verify = false

  ## HTTP Proxy override, if unset values the standard proxy environment
  ## variables are consulted to determine which proxy, if any, should be used.
  # http_proxy = "http://corporate.proxy:3128"

  ## Optional HTTP headers
  # http_headers = {"X-Special-Header" = "Special-Value"}

  ## Compress each HTTP request payload using GZIP.
  ## Additional HTTP headers
  # http_headers = {"X-Special-Header" = "Special-Value"}

  ## HTTP Content-Encoding for write request body, can be set to "gzip" to
  ## compress body or "identity" to apply no encoding.
  # content_encoding = "gzip"

  ## When true, Telegraf will output unsigned integers as unsigned values,
  ## i.e.: "42u".  You will need a version of InfluxDB supporting unsigned
  ## integer values.  Enabling this option will result in field type errors if
  ## existing data has been written.
  # influx_uint_support = false
```

### Required parameters:

* `urls`: List of strings, this is for InfluxDB clustering
support. On each flush interval, Telegraf will randomly choose one of the urls
to write to. Each URL should start with either `http://` or `udp://`
* `database`: The name of the database to write to.


### Optional parameters:

* `write_consistency`: Write consistency (clusters only), can be: "any", "one", "quorum", "all".
* `retention_policy`:  Name of existing retention policy to write to.  Empty string writes to the default retention policy.
* `timeout`: Write timeout (for the InfluxDB client), formatted as a string. If not provided, will default to 5s. 0s means no timeout (not recommended).
* `username`: Username for influxdb
* `password`: Password for influxdb
* `user_agent`:  Set the user agent for HTTP POSTs (can be useful for log differentiation)
* `udp_payload`: Set UDP payload size, defaults to InfluxDB UDP Client default (512 bytes)
* `ssl_ca`: SSL CA
* `ssl_cert`: SSL CERT
* `ssl_key`: SSL key
* `insecure_skip_verify`: Use SSL but skip chain & host verification (default: false)
* `http_proxy`: HTTP Proxy URI
* `http_headers`: HTTP headers to add to each HTTP request
* `content_encoding`: Compress each HTTP request payload using gzip if set to: "gzip"
### Metrics
￼
Reference the [influx serializer][] for details about metric production.
￼
[InfluxDB v1.x]: https://github.com/influxdata/influxdb
[influx serializer]: /plugins/serializers/influx/README.md#Metrics
