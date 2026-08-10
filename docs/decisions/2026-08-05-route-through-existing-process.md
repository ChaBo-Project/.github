# Public-facing and placement questions route through existing process, not a unilateral call

**Date:** 2026-08-05
**Status:** Accepted

## Context

Two open questions surfaced this round — `ChaBo-Project.github.io`'s content/visibility,
and `technical-architecture`'s place in A1 — both already have an existing path: issue
[#6](https://github.com/ChaBo-Project/ChaBo-Project.github.io/issues/6) for the former,
Discussions for the latter. Deciding either unilaterally here would repeat the same
top-down pattern this round exists to fix.

## Decision

Don't duplicate or override existing mechanisms. `ChaBo-Project.github.io` stays tracked
via issue #6 (only the root page public, rest inaccessible, or the whole repo goes
private; it also flags the doc-aggregation-mismatch risk). `technical-architecture` is
raised as a Discussion first, converting to an issue or A1 entry only if it converges. If
either isn't resolved within a week (by 2026-08-14), the org admin decides directly rather
than leaving it open indefinitely.

## Consequences

Both stay open a little longer, but get resolved through the existing process rather than
a unilateral call — even one made by the org admin — unless a week passes without
resolution, in which case the org admin steps in to avoid an indefinite stall.
