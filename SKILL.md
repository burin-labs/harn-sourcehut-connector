# SKILL: harn-sourcehut-connector

Use `harn-sourcehut-connector` when wiring Harn triggers or outbound helpers for SourceHut.

## What you get

- Provider id: `sourcehut`
- Trigger kinds: `webhook`
- Supported events: `repo.push, repo.update, repo.created, repo.deleted, git.pre_receive, git.post_receive, todo.created, todo.updated, build.started, build.completed, mail.received`
- Webhook verification: `sourcehut_ed25519`
- Outbound helpers: `api.request`, `api.paginate`, `graphql.request`, `graphql.paginate`, `pull_requests.comment`, `pull_requests.update`, `issues.comment`, `commit_status.set`, `repository_file.get`

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
