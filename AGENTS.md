# Agent notes for this repo

Guidance for AI coding agents (Claude Code, Cursor, Copilot CLI, etc.) working in this repository.

## GitHub account

This is a **One Punch Technology** repo — the git remote is under `onepunchtechnology`.

- Run `gh auth switch --user onepunchtechnology` once at the start of a session and **stay on it**
  for all `gh` and `git push` work.
- Do **not** switch back to a personal account mid-session — later `gh` read commands (e.g.
  `gh pr checks`) fail with "Could not resolve to a Repository" because the other account can't
  see the private repo.

## Manifests

The plugin ships three per-host manifests that must stay in sync on `name`, `version`,
`description`, `author`, `homepage`, and `repository`:

| File | Host |
|---|---|
| `.claude-plugin/plugin.json` | Claude Code |
| `.cursor-plugin/plugin.json` | Cursor |
| `plugin.json` (repo root) | GitHub Copilot CLI |

`.claude-plugin/marketplace.json` lets this repo be added as a one-plugin marketplace; Claude
Code and Copilot CLI both read it. Run `claude plugin validate .` after touching any manifest —
CI runs the same check.

## Pricing

Do **not** put pricing, tier names, or credit-limit numbers anywhere in this repo. They drift.
Point users to the live site instead. Credits are described only as "free daily credits for
guests and signed-in users."
