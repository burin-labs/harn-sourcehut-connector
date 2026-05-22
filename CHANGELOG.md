# Changelog

## Unreleased

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
