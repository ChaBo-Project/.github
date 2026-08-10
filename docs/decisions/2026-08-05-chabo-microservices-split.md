# Split chabo-microservices into ChaBo-Channels, ChaBo-Deploy, and chabo-unreviewed

**Date:** 2026-08-05
**Status:** Accepted, execution pending

## Context

`chabo-microservices` was assumed to become "Chabo-Stack," but its real contents —
`whatsapp-channel`, `speech-channel`, `docker-compose/`, an unauthorized
`embedding-service`, and a duplicate of `ChaBo-Orchestrator`.

## Decision

`whatsapp-channel`/`speech-channel` stay; repo renamed to `ChaBo-Channels`.
`docker-compose/` is deleted; deployment topology is taken care of by a new `ChaBo-Deploy`
repo (earlier called Stack). `embedding-service` moves to a new `chabo-unreviewed` repo —
a standing home for code created without discussion or sign-off. The duplicate
orchestrator code is dropped entirely. Maintainers have until **2026-08-14** to do this
themselves; otherwise the org admin executes it.

## Consequences

Three repos replace one, each matching its real content and the Guidelines' intended
boundaries. `GUIDELINES.md`'s A1 table needs updating once published.
