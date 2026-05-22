# CLAUDE.md

SourceHut connector written in Harn. It verifies Ed25519-signed webhooks and
exposes GraphQL plus raw HTTP helpers.

Shared Harn connector authoring rules are in the canonical guide:

- https://github.com/burin-labs/harn/blob/main/docs/src/connectors/authoring.md

Keep this file provider-specific. Add shared connector guidance to the Harn
guide first.

## Provider notes

- Webhook event names use `x-sourcehut-event`; delivery IDs use `x-sourcehut-delivery` or
  `x-srht-delivery`.
- Signed SourceHut webhooks use Ed25519 public-key verification, not an HMAC signing secret.
- Outbound calls default to the SourceHut GraphQL endpoint and accept OAuth2 tokens or PATs through
  call args, `sourcehut/api-token`, `SOURCEHUT_TOKEN`, or `SOURCEHUT_API_TOKEN`.
