# AGENTS.md

SourceHut connector written in Harn. It verifies Ed25519-signed webhooks and
exposes GraphQL plus raw HTTP helpers.

Shared connector authoring rules live in the Harn guide:

- [Connector authoring guide](https://github.com/burin-labs/harn/blob/main/docs/src/connectors/authoring.md)

Put shared connector guidance in the Harn guide and keep only
provider-specific notes and local hazards here.

`CLAUDE.md` points here. Edit `AGENTS.md` only.

## Provider notes

- Webhook event names use `x-sourcehut-event`; delivery IDs use `x-sourcehut-delivery` or
  `x-srht-delivery`.
- Signed SourceHut webhooks use Ed25519 public-key verification, not an HMAC signing secret.
- Outbound calls default to the SourceHut GraphQL endpoint and accept OAuth2 tokens or PATs through
  call args, `sourcehut/api-token`, `SOURCEHUT_TOKEN`, or `SOURCEHUT_API_TOKEN`.

<!-- BEGIN HARN SHARED AGENT CONTRACT: managed by harn-bump-fleet -->

## Ecosystem working agreement

- Pursue the ambitious product outcome; make the seams boring with small typed
  interfaces, explicit invariants, and deterministic projections.
- Give each behavior one semantic owner. Generate or parity-test other surfaces
  instead of maintaining competing implementations.
- Work autonomously inside approved scope. Pause for destructive, production,
  high-spend, ambiguous, or authority-expanding actions—not routine reversible work.
- Treat stop, wait, stand down, and pivot as control events for long-lived work.
- Match evidence to the claim: exercise the canonical user path, state the
  falsifier, verify liveness and recovery, and record residual blind spots.
- "Ship" means landed on main with required deploy and post-merge checks complete.

<!-- END HARN SHARED AGENT CONTRACT -->
