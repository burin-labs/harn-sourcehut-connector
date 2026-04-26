# SKILL: harn-sourcehut-connector

Use `harn-sourcehut-connector` when wiring Harn triggers or outbound helpers for SourceHut.

## What you get

- Provider id: `sourcehut`
- Trigger kinds: `webhook`
- Supported events: `repo.push, repo.update, todo.created, todo.updated, build.started, build.completed, mail.received`
- Webhook verification: `sourcehut_ed25519`
- Outbound helpers: `api.request`, `pull_requests.comment`, `pull_requests.update`, `issues.comment`, `commit_status.set`, `repository_file.get`

## Trigger recipe

```toml
[[triggers]]
id = "sourcehut-events"
kind = "webhook"
provider = "sourcehut"
match = { path = "/hooks/sourcehut", events = ["repo.push"] }
handler = "handlers::on_sourcehut_event"
secrets = { signing_secret = "sourcehut/signing-secret" }
```

For SourceHut, use a configured `public_key` secret instead of `signing_secret`.
For SVN polling, add a `poll` trigger and run `harn connector check . --provider svn --run-poll-tick`.
