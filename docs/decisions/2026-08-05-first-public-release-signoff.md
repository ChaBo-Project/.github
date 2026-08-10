# First public image release requires technical lead sign-off

**Date:** 2026-08-05
**Status:** Accepted, execution pending

## Context

A release is already a deliberate action (merging the release PR triggers a build),
gated by ordinary PR review. But routine review doesn't guarantee the same scrutiny as a
package's first-ever public exposure — the same pattern as this round's other incidents,
applied to packages instead of repos.

## Decision

The first public release of any package/image requires explicit sign-off from the
technical lead, beyond normal review. Subsequent routine releases follow the ordinary
one-review-approves flow once `CODEOWNERS` is properly scoped to a Team.

## Consequences

One deliberate checkpoint before a package's public debut, without requiring the
technical lead's involvement in every release after that. Needs a manual way to check
"is this the first release" — not yet automated.
