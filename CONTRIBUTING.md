# Contributing to ChaBo

Thanks for your interest in contributing. If you need more info on Org repos then either go through
repos readme or[Repository Guidelines](./GUIDELINES.md).

- **Finding work**: issues labelled `good-first-issue` are ready to pick up. For anything
  else, comment on the issue first to avoid duplicate work.
- Please read the [Code of Conduct](./CODE_OF_CONDUCT.md).

## Flow

1. **Find or open an issue** — every branch and PR should trace back to one (`Closes #NN`).
2. **Branch or fork**: `feat/…`, `fix/…`, `docs/…`, `research/…`, `ci/…` — a naming
   convention for humans, doesn't drive any automation on its own.
3. **No direct commits to `main`.** Every change lands through a pull request, never a
   direct push. Commit as often as you like on your own branch — there's no restriction on
   how many commits a PR contains.
4. **Keep it single-purpose** — one concern per PR, especially in the retrieval pipeline.
5. **Open the PR**, link the issue, state what changed and how it was tested. Give the PR
   itself a Conventional Commit-style title (`feat: …`, `fix: …`, `docs: …`) — where a repo
   has release automation configured, that title is what becomes the commit that actually
   lands on `main`, and what `release-please` reads to generate `CHANGELOG.md`. Your
   individual work-in-progress commits stay fully visible on the closed PR itself; they
   don't need to follow any format. Where a repo doesn't have release automation yet,
   update `CHANGELOG.md` by hand in the same PR instead.
6. **One review approves, the org admin merges.** `CODEOWNERS` points at a per-repo
   reviewer team (2+ people, so the PR author is never the only possible approver) that
   actually holds Write access on the repo — a team without it satisfies nothing.

Every issue and PR gets a first response within a week, even if it's "not now, and here's
why."

Have an idea that isn't scoped into an issue yet? Post it in
[Discussions](https://github.com/orgs/ChaBo-Project/discussions) instead of opening a repo
or issue directly.
