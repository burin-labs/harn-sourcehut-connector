# harn-sourcehut-connector

Pure-Harn SourceHut connector: Ed25519-signed webhooks plus GraphQL/REST passthrough helpers.

This package implements the Harn Connector interface contract v1 for `sourcehut`.
It normalizes inbound webhook payloads to the tagged `NormalizeResult` envelope,
verifies SourceHut Ed25519 webhook signatures, and exposes outbound raw
GraphQL/API helpers plus common PR/comment/status method aliases.

## Install

```sh
harn add github.com/burin-labs/harn-sourcehut-connector@v0.1.0
```

Until a version is tagged, depend on a path checkout:

```toml
[dependencies]
harn-sourcehut-connector = { path = "../harn-sourcehut-connector" }
```

## Webhook verification

The connector accepts unsigned events only when no verification material is
configured. Configure `public_key`, `webhook_public_key`, or a
`sourcehut/webhook-public-key` secret reference for SourceHut Ed25519
verification.

## Authentication

Outbound calls use OAuth2 token or PAT for SourceHut APIs. Pass `access_token`, `token`,
`personal_access_token`, or `app_password` in the call args, or set the
`SOURCEHUT_TOKEN` environment variable.

Outbound calls use the shared Harn connector token bucket before each request.
Pass `rate_limit` or `rate_limit_config` to tune the bucket, and set
`include_meta: true` to return parsed rate-limit response headers. Cursor-style
SourceHut GraphQL lists can be drained with `call("graphql.paginate", args)`;
the helper follows SourceHut `{ results, cursor }` pages and returns
`{ items, pages, next_cursor, complete }`.

## Development

```sh
harn check src/lib.harn
harn fmt --check src/lib.harn tests/*.harn
for test in tests/*.harn; do harn run "$test" || exit 1; done
```

## License

Dual-licensed under MIT and Apache-2.0.
