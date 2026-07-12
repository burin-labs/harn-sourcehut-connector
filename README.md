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

SourceHut webhooks must be signed. Provide `public_key`,
`webhook_public_key`, or the `sourcehut/webhook-public-key` secret; the
connector verifies the `x-sourcehut-signature`, `x-srht-signature`, or
`x-signature-ed25519` Ed25519 signature against the raw request body and rejects
requests with no configured public key, missing binding id, missing signature,
or invalid signature.

SourceHut GraphQL webhook enum names such as `GIT_POST_RECEIVE` and
`REPO_UPDATE` are accepted and mapped to Harn event kinds such as
`git.post_receive` and `repo.update`. The connector also maps the current
SourceHut todo, builds, and lists webhook enums, including `TICKET_CREATED`,
`JOB_UPDATED`, `EMAIL_RECEIVED`, and `PATCHSET_RECEIVED`.

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

## Typed convenience helpers

Alongside the generic GraphQL escape hatches, the connector exposes a few typed
`call` helpers over the SourceHut GraphQL APIs. Each targets the service-specific
GraphQL endpoint (overridable via `api_base_url`) and wraps the exact upstream
operation from the SourceHut schema:

- `call("ticket.comment", {tracker_id, ticket_id, text, status?, resolution?})` —
  todo.sr.ht `submitComment` mutation.
- `call("ticket.update_status", {tracker_id, ticket_id, status, resolution?})` —
  todo.sr.ht `updateTicketStatus` mutation (`resolution` is required by SourceHut
  when `status` is `RESOLVED`).
- `call("build.get", {job_id})` — builds.sr.ht `job(id)` query returning
  `{ id, status, note }`.
- `call("build.submit", {manifest, note?, tags?, secrets?, execute?, visibility?})` —
  builds.sr.ht `submit` mutation.

For anything else, use `api.request`, `graphql.request`, or `graphql.paginate`
with the exact SourceHut GraphQL operation the workflow needs.

Inbound `JOB_UPDATED` (`build.updated`) events carry a normalized
`payload.build` summary `{status, failed, succeeded}` so workflows can branch on
build failure (`failed` is set for `FAILED` / `TIMEOUT` / `CANCELLED`).

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
