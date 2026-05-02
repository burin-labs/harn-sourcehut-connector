# CLAUDE.md - harn-sourcehut-connector

Pure-Harn SourceHut connector package for Ed25519-signed webhooks plus GraphQL/REST passthrough
helpers.

Shared Harn connector authoring rules live in the canonical guide:

- https://github.com/burin-labs/harn/blob/main/docs/src/connectors/authoring.md

Keep this file limited to provider-specific notes and local hazards. Add shared connector guidance
to the Harn guide first.

## Provider Notes

- Webhook event names use `x-sourcehut-event`; delivery ids use `x-sourcehut-delivery` or `x-srht-
  delivery`.
- Signed SourceHut webhooks use Ed25519 public-key verification, not an HMAC signing secret.
- Outbound calls default to the SourceHut GraphQL endpoint and accept OAuth2 tokens or PATs through
  call args or `SOURCEHUT_TOKEN`.
