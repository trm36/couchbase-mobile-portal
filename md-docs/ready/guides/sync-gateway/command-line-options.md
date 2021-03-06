---
id: command-line-options
title: Command line options
permalink: ready/guides/sync-gateway/command-line-options/index.html
---

To configure Sync Gateway, you can specify command-line options when you start Sync Gateway. Command-line options can only specify a limited set of configuration properties, and cannot be used to configure multiple databases. For more comprehensive configuration, use a JSON configuration file. For information about using a configuration file, see the configuration file guide.

Configuration determines the runtime behavior of Sync Gateway, including server configuration and the database or set of databases with which a Sync Gateway instance can interact.

> **Note:** Note:You can only specify a small subset of configuration properties as command-line options. Two command-line options do not have corresponding configuration properties: `-help` and `-verbose`.

When specifying command-line options, the format of the `sync_gateway` command is:

```bash
sync_gateway [command-line options]
```

## Command-line options

You can prefix command-line options with one hyphen (-) or with two hyphens (--). For command-line options that take an argument, you specify the argument after an equal sign (=), for example, `-bucket=db`, or as a following item on the command line, for example, `-bucket db`. Command-line options are case-insensitive. Here we use lower camel case.

Following are the command-line options that you can specify when starting Sync Gateway.

|Option|Default|Description|
|:-----|:------|:----------|
|`‑adminInterface`|`127.0.0.1:4985`|Port or TCP network address (IP address and the port) that the Admin REST API listens on.|
|`-bucket`|`sync_gateway`|Name of the Couchbase Server bucket.|
|`-dbname`|`sync_gateway`|Name of the Couchbase Server database to serve through the Public REST API.|
|`-help`|N/A|Lists the available options and exits.|
|`-interface`|`:4984`|Port or TCP network address (IP address and the port) that the Public REST API listens on.|
|`-log`|`HTTP`|Comma-separated list of log keywords to enable. The log keyword `HTTP` is enabled by default, which means that HTTP requests and error responses are always logged. Omitting `HTTP` from your list does not disable HTTP logging. HTTP logging can be disabled through the Admin API.|
|`-personaOrigin`|None|URL that clients use to communicate with a Mozilla Persona IdP server.|
|`-pool`|`default`|Name of the Couchbase Server pool in which to find buckets.|
|`-pretty`|`false`|Pretty-print JSON responses to improve readability. This is useful for debugging, but reduces performance.|
|`-url`|`walrus:`|URL of the database server. An HTTP URL implies Couchbase Server. A `walrus:` URL implies the built-in Walrus database. A combination of a Walrus URL and a file-style URI (for example, `walrus:///tmp/walrus`) implies the built-in Walrus database and persisting the database to a file.|
|`-verbose`|Non-verbose logging|Logs more information about requests.|

### Examples

The following command does not include any parameters and just uses the default values. It connects to the bucket named `sync_gateway` in the pool named `default` of the built-in Walrus database. It is served from port 4984, with the Admin interface on port 4985.

```bash
$ sync_gateway
```

The following command creates an ephemeral, in-memory Walrus database, served as `db`, and specifies use of pretty-printed JSON responses. Because Walrus is the default database, the option `-url` could be omitted.

```bash
$ sync_gateway -url=walrus: -bucket=db -pretty
```

The following command starts Sync Gateway and specifies the address of a Couchbase Server instance (instead of using the default database server, which is Walrus):

```bash
$ ./sync_gateway -url http://cbserver:8091
```

The following command accomplishes the same things as the prior command, persists the Walrus database to a file named `/tmp/walrus/db.walrus`, and turns on additional logging for the log keys `HTTP+` and `CRUD`.

```bash
$ sync_gateway -url=walrus:///tmp/walrus -bucket=db -log=HTTP+,CRUD
```