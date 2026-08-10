# ChaBo — Repository Guidelines

**Version 1.3 · August 2026 · maintained by the Chabo-Org team (GIZ Data Service Center)**

Binding for all repositories under ChaBo-Project. Principles first, then structure, then
per-change standards. One file for now — split only once a Part has enough real content
to justify its own doc.

---

# Part A — Repository Structure

## A1. Where things live

| Repository | Owns | Status |
|---|---|---|
| **ChaBo-Orchestrator** | The RAG pipeline — rewrite, retrieve, rerank, generate, guardrails. One generic, channel-agnostic API. Publishes one image. | Exists |
| **ChaBo-Channels** | Independently built services: WhatsApp, speech, web widget etc. One repository, several images, path-filtered CI. | Coming soon |
| **ChaBo-ChatUI** | The web chat channel — a vendored fork tracking upstream. Publishes one image. | Exists |
| **ChaBo-Deploy** | Deployment topology only: compose files, vendored third-party wrappers, per-instance configuration. No application source. | Coming soon |
| **ChaBo-Project.github.io** | Public landing page and aggregated documentation, pulled from each repository at build time. | Exists |
| **`instance-<name>`** | One per deployment: review-worthy content (eval Q&A sets, dated run results, compliance documentation, business-facing description) plus full deployment config — every instance, not just partner-operated ones. Corpus data never lives here — see `ChaBo-Deploy`'s README. | Per-instance; none created yet |
| **`.github`** | This document, `ROADMAP.md`, and `docs/decisions/` — decision records per C3. Also GitHub's own community-health-file location: `CONTRIBUTING.md`, `CODE_OF_CONDUCT.md`, issue/PR templates, auto-surfaced on every repo in the org that doesn't have its own copy. Named `.github` specifically because that's the literal name GitHub's fallback mechanism requires — not a stylistic choice. Also the source repository for org-wide Discussions (C3) — moderating discussions (pin, lock, convert to issue) only needs Triage-level access here, deliberately lighter than the Write access needed to actually change this document. | Placeholder — being created now |

Work is tracked on one org-level project with saved views (roadmap, per-team, tech-debt
housekeeping) rather than separate boards — access follows each Team's repo permissions
automatically, no separate RBAC needed.

---

## A2. Access & permissions

- Org base permissions: none. Access comes only through Team membership.
- One Team per repo-owning group, granted Write to exactly the repos it owns.
- Every repo: PR required, one approval from the owning Team (≥2 people, via CODEOWNERS)
  required, status checks passing before merge.
- Merging to `main` is org-admin only — one org-wide Ruleset, not repeated per repo.
- Every instance: its own `instance-<name>` repository, referencing ChaBo-Deploy at a
  pinned version, never a copy.

---

## A3. Principles

**1. No implementation without a backlog ticket.** Every PR references its issue
(`Closes #NN`) — no issue, no merge. Lets contributors from different orgs pull from one
queue instead of duplicating work.

**2. Repository boundaries follow service/functional boundaries, not ownership.**

**3. A repository is created when the code exists and has a named owner.** Never in
advance — empty repos with plausible names attract work that belongs elsewhere.

**4. Components are artifacts, not copies.** Every service publishes a version-tagged
image; nothing consumes another component by copying its source.

**5. A service publishes at least one generic, consumer-agnostic API — versioned, with
its contract documented** (`docs/api/openapi.yaml`). Consumer-specific translation lives
in the consumer's repository, never the service's; a client guessing the response shape
means a missing contract, not a client bug.

**6. Knowledge lives in the repository, not in a person.** Setup, decisions, tech debt go
in `README.md`/`DEVELOPER.md`/`docs/decisions/`; technical questions go in issues or
Discussions.

**7. Public by default.** Private only for a concrete reason — secrets, client data,
personal information or WIP for first time.

---

## A4. Releases, versions, and breaking changes

- **A merge to `main` runs tests; it doesn't publish an image.** A release is a deliberate
  action — merging the release PR is what triggers a build. Never `:latest` in anything
  that runs; `dev-<sha>` is not a version.
- **Deployments pin a tag** and upgrade deliberately.
- **`CHANGELOG.md` updates in the same PR** as any behaviour change, so a deployment owner
  can judge from it whether a bump is safe.
- **Breaking changes**: fail fast naming the missing/changed key, `CHANGELOG` entry marked
  **BREAKING**, upgrade note in `README`, minor version bump at minimum.
- **New behaviour is opt-in, off by default** — no existing deployment changes silently.
- **No implicit fallbacks.** A misconfigured but enabled stage fails to start rather than
  silently borrowing another stage's config; applies to mandatory Tier-2 config too.

**Tagging**: `ghcr.io/chabo-project/<repo-name>:<version>`, one name per published image.
Multi-image repos tag each image independently. Tags are
never deleted while a tracked instance is pinned to them (`list-instances.sh` is the source
of truth) — otherwise keep a rollback buffer and prune unreferenced tags.

---

## A5. Deployment targets

Four shapes, one codebase. Which applies is a property of the use case.

| Shape | Where | Use |
|---|---|---|
| All-in-one container | Hugging Face Space | One-click public demo — not a real deployment, never appears in `list-instances.sh` |
| Single-container backend + remote inference | HF Space or small VM | PoC with a real corpus |
| Compose stack, single VM | VM in an EU region (Azure Sweden Central / Germany West Central) | Production, and the reference for adopters |
| Compose stack, split across VMs | Multiple VMs, one region | Production where a component — typically TEI — needs GPU and the rest doesn't |

Full deployment mechanics for each shape — container/network setup, cross-VM isolation,
public-facing hardening — live in `ChaBo-Deploy`'s own README, not here.

---

## A6. Code and data standards

- **Dependencies are fully pinned.** No floating `>=` on runtime dependencies — two
  installs of the same version must be the same software. New dependencies need a ticket
  and a permissive licence (Apache-2.0, MIT, BSD); copyleft — AGPL in particular, which
  triggers on network/SaaS use, not just distribution — is incompatible with partners
  redistributing this stack.
- **Tests live in `tests/` and are never gitignored.** Test files excluded from the
  repository disappear with the machine they sit on.
- **Tech debt is recorded as an issue, labelled `tech-debt`, in the repository it
  belongs to — never a standalone file or table, local or otherwise.** Visibility across
  every repo comes from the org project's housekeeping view, not a maintained document.
- **Any local working-context notes — for AI tooling or otherwise — belong in
  `README.md`, `DEVELOPER.md`, or `docs/decisions/`, never local-only regardless of what
  produced them.** The mechanism varies; the rule doesn't: knowledge that only exists on
  one person's machine doesn't survive that person leaving.
- **Secrets are never committed.** Templates only.
- **No client documents, no personal data, no real partner corpora in any repository** —
  including as test fixtures. Applies to `instance-<name>`'s eval Q&A sets too: synthetic
  or curated, never real logged user queries. This is stricter than "no secrets" and it
  is not optional in this context.
- **Naming:** `ChaBo-<Responsibility>` for repositories, `instance-<name>` for
  deployments. Existing names stay as they are; the convention applies to new ones.
- **Commits:** `feat:`, `fix:`, `docs:`, `refactor:`, `test:`, `chore:`, `ci:`.

---

# Part B — Contribution & Roles

- **Contributor** — anyone opening an issue or pull request.
- **Module maintainer** — a named person's title and accountability for a defined set
  of paths, not a bypass of CONTRIBUTING.md's review rule. CODEOWNERS for those paths still points at
  their team. This is the shape ChaBo-Channels was built for — `whatsapp/`, `speech/`,
  each with its own maintainer — not a general pattern across every repository. The
  normal route for a partner contributing a module into a shared repo, distinct from a
  partner operating their own instance.
- **Core maintainer (Chabo-Org)** — owns Chabo-Orchestrator, ChaBo-Deploy, and
  `governance`; sets priorities on the org-level project.
- **Repositories owned outright by another team** — ChaBo-Channels and ChaBo-ChatUI
  today — have their own core maintainers from that team, full autonomy for day-to-day
  work. Chabo-Org's co-ownership there is real but scoped, not a general right to step
  in:
  - **Required reviewer wherever a change touches the published, generic API contract**
    (Principle A3.5) another repo depends on or provides — the boundary
    is Chabo-Org's concern even when the repository isn't.
  - **Org-admin access exists for continuity, not routine involvement** — an
    unresponsive maintainer, an abandoned repository, a security incident. GitHub already
    grants organisation owners this access by default; stating it here is about when it
    gets *used*, not about granting something new.

Recognition matters more than merge rights: contributors are credited, and notable
deployments are published as adaptation patterns with author credit.

---

# Part C — Documentation

## C1. README.md vs. DEVELOPER.md

- **README.md is the front door**: what this repo is, its role from A1's table expanded,
  how to get from "landed here" to "using it" quickly.
- **DEVELOPER.md is the workbench**: how to run this locally, the dev environment, how to
  test a change before a PR.
- **Whichever fits links to `docs/decisions/`** for real decisions behind this repo's
  shape, rather than re-explaining inline.

Format, headers, and length are the maintainer's call, repo by repo.

---

## C2. Compliance documentation

Lives in `instance-<name>` — this is what that content needs to look like, and how it
stays current instead of a document written once and never revisited.

- **A dedicated template**, pending legal review — lives in `.github` alongside issue/PR
  templates, one maintained source, not N drifting copies.
- **Every compliance doc carries `last_reviewed:`/`review_due:` dates** in front matter —
  undated the moment anything about the system changes.
- **Review is time-bound *and* change-triggered.** A per-instance scheduled workflow opens
  an issue when `review_due` passes; a Chabo-Orchestrator guardrail/provider/generation
  change is manually checked against affected instances for now, not automated.

---

## C3. Decision records — and where they sit relative to issues and ideas

Three things, easy to conflate:

- **A Discussion** — a raw idea, no commitment or scope. `config.yml` routes here by
  default. Converts to a real issue (or back) via GitHub's own mechanism, history intact.
- **An issue** — scoped, actionable work (feature/bug/tech-debt/research). Lives in the
  repo it affects.
- **A decision record** — the permanent record of a choice already made, in
  `.github/docs/decisions/`, one file per decision, never edited in place — a changed mind
  gets a new record.

**Idea to landed**: raised in a Discussion (async, or at the biweekly review and written
up after). Resolved as discarded, spun into an issue, or written as a decision record if
it's a standing choice rather than a task.

**Format**: context, decision, consequences — no more ceremony than that.

---

## Open decisions

Marked so they are not mistaken for settled: DCO versus CLA (legal); a Code of Conduct
draft now exists at `.github/CODE_OF_CONDUCT.md`, pending adoption sign-off — the open
question is approval, not existence or location anymore; who hosts partner-operated
instances; documentation and aggregation — how `ChaBo-Project.github.io` pulls docs from
each repository at build time, which repos are in scope, and whether `instance-<name>`
compliance documentation should be included or stay reachable only by direct link.

---

*Questions, corrections, disagreement: open an issue. That is also how these guidelines
change.*
