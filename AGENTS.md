# AGENTS.md

This repository implements `harn-sourcehut-connector`, a pure-Harn connector package for SourceHut.

Run targeted checks with a local Harn CLI from the main Harn repository:

```sh
harn check src/lib.harn
harn fmt --check src/lib.harn tests/*.harn
for test in tests/*.harn; do harn run "$test" || exit 1; done
```
