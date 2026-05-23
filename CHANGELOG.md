# Changelog

## Unreleased

- Security sweep 2026-05-23 (hardening, fail-closed defaults):
  - **F1 (CRITICAL) SSRF:** `__api_url` rejects absolute-URL `path` arguments so the
    configured `api_base_url` (and any attached `Authorization` header) cannot be redirected
    to attacker-chosen hosts.
  - **F3 (HIGH) fail-closed:** webhook delivery is rejected with `401 missing_signature` when
    no webhook public key is configured for the binding (previously accepted as `unsigned`).
  - **F4 (HIGH) drop raw.public_key:** `__webhook_public_key` no longer reads
    `raw.public_key` / `raw.webhook_public_key` / `raw.config.public_key` /
    `raw.secrets.public_key` / `raw.metadata.public_key`. The public key resolves only from
    orchestrator `ctx`, the per-binding state seeded via `activate()`, or a host-managed
    secret keyed by `binding_id`.
  - **F6 (HIGH) body source:** signature verification runs against `body_text` when present,
    else over the base64-decoded bytes of `body_base64`. Absent body now rejects
    `400 missing_body`.
  - **F8 (MEDIUM) require binding_id:** rejects `400 missing_binding_id` when inbound
    requests have no binding pointer; implicit single-binding fallback removed.
  - **F9 (MEDIUM) dedupe key:** derived from the host-set `x-sourcehut-delivery` /
    `x-srht-delivery` headers only; falls back to `sha256(body_base64 | received_at |
    binding_id)`.
  - **F13 (LOW) shutdown gating:** documented `@host_only` semantics for `shutdown()`.
  - **F14 (LOW) verify before parse:** signature verification runs ahead of `json_parse`.
  - Deferred: F12 (further outbound dispatch consolidation), F15 (bump-harn workflow
    scoping).
- Added SourceHut event parity for repository create/delete events and Git pre/post receive enums.
- Added SourceHut todo, builds, and lists webhook enum parity with contract fixtures.
- Switched outbound HTTP/env access to current Harn shared helpers to avoid ambient builtin drift.
- Hardened event and Ed25519 signature matching so unsupported prefix events and wrong signature
  schemes reject cleanly.
- Removed advertised GitHub-shaped source-control shortcut methods that SourceHut does not support;
  use raw HTTP or GraphQL helpers for provider-specific operations.
- Added GraphQL request and cursor pagination helpers with shared Harn rate-limit handling.
- Switched webhook setup docs and metadata from shared secrets to SourceHut Ed25519 public keys.
- Added connector setup metadata, a package install smoke test, and the Harn runtime bump workflow.

## v0.1.0

- Initial pre-alpha SourceHut connector package implementing Harn Connector contract v1.
