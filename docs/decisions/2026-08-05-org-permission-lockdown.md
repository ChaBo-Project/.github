# Restrict org-wide repo creation, visibility change, and Pages publishing to owners

**Date:** 2026-08-05
**Status:** Accepted

## Context

`ChaBo-Project.github.io` went public and had content published, and `ChaBo-Frontend` was
created with no linked issue and no named owner — both without review. Org settings
allowed any member to create a repo of any visibility and publish Pages, with no gate.

## Decision

Restrict repository creation, visibility changes, and Pages creation to organization
owners only. Require org-wide 2FA (execution pending — affected members to be notified first, to
avoid an involuntary removal).

## Consequences

New repos now require the org admin — slower, but deliberate. Existing repos' current
state is unaffected retroactively (tracked separately).
