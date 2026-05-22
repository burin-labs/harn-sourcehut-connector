# SKILL: harn-sourcehut-connector

Use `harn-sourcehut-connector` when wiring Harn triggers or outbound helpers for SourceHut.

## Connector surface

- Provider id: `sourcehut`
- Trigger kinds: `webhook`
- Supported events: `repo.push`, `repo.update`, `repo.created`, `repo.deleted`, `git.pre_receive`,
  `git.post_receive`, `tracker.created`, `tracker.updated`, `tracker.deleted`,
  `ticket.created`, `ticket.updated`, `ticket.deleted`, `label.created`, `label.updated`,
  `label.deleted`, `ticket.event_created`, `build.created`, `build.updated`,
  `mailing_list.created`, `mailing_list.updated`, `mailing_list.deleted`, `email.received`,
  `patchset.received`
- Webhook verification: `sourcehut_ed25519`
- Outbound helpers: `api.request`, `api.paginate`, `graphql.request`, `graphql.paginate`

## Trigger recipe

```toml
[[triggers]]
id = "sourcehut-events"
kind = "webhook"
provider = "sourcehut"
match = { path = "/hooks/sourcehut", events = ["repo.push"] }
handler = "handlers::on_sourcehut_event"
config = { webhook_public_key_secret = "sourcehut/webhook-public-key" }
```

SourceHut webhook verification uses an Ed25519 public key, not a shared HMAC
secret. Outbound calls can read the `sourcehut/api-token` secret.
