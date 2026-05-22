# harn-sourcehut-connector

SourceHut connector written in Harn: Ed25519 webhook verification plus GraphQL
and raw HTTP helpers.

This package implements Harn Connector contract v1 for `sourcehut`. It verifies
SourceHut Ed25519 webhook signatures, normalizes inbound webhook payloads to the
tagged `NormalizeResult` envelope, and exposes GraphQL and raw HTTP helpers.

## Install

Use the latest tagged release when you want a stable package ref:

```sh
harn add github.com/burin-labs/harn-sourcehut-connector@v0.1.0
```

Use a path checkout for unreleased `main` or local multi-repo development:

```toml
[dependencies]
harn-sourcehut-connector = { path = "../harn-sourcehut-connector" }
```

## Webhook verification

The connector accepts unsigned events only when no verification material is set.
Provide `public_key`, `webhook_public_key`, or the
`sourcehut/webhook-public-key` secret for SourceHut Ed25519 verification.

SourceHut GraphQL webhook enum names such as `GIT_POST_RECEIVE` and
`REPO_UPDATE` are accepted and mapped to Harn event kinds such as
`git.post_receive` and `repo.update`.

## Authentication

Outbound calls use a SourceHut OAuth2 token or PAT. Pass `access_token`,
`token`, `personal_access_token`, `api_token`, or `app_password` in call args;
store `sourcehut/api-token`; or set `SOURCEHUT_TOKEN` /
`SOURCEHUT_API_TOKEN`.

Outbound calls use the shared Harn connector token bucket before each request.
Pass `rate_limit` or `rate_limit_config` to tune the bucket, and set
`include_meta: true` to return parsed rate-limit response headers. Cursor-style
SourceHut GraphQL lists can be drained with `call("graphql.paginate", args)`;
the helper follows SourceHut `{ results, cursor }` pages and returns
`{ items, pages, next_cursor, complete }`.

## Development

```sh
harn install
harn check src
harn lint src
harn fmt --check src tests
for test in tests/*.harn; do harn run "$test" || exit 1; done
harn connector check .
```

## License

Dual-licensed under MIT and Apache-2.0.
