# Design — v1

How the repository satisfies its requirements, written on 2026-11-19 with the repository; not yet
confirmed by the owner ([SE1/W12](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/08-work.md#w12)).

## Components

| Component | Contract (one line) | Holds state? |
| --------- | ------------------- | ------------ |
| `guide/`, `README.md`, the root documents | the principles, their index, and the map to SE1 | the text |
| `AGENTS.md`, `templates/` | what adopters copy; they name where they came from | the text |
| `tools/check.py`, `tools/test_check.py` | the documents agree with each other and with SE1 at `SE_RELEASE`; REQ1/CHK-1, REQ1/PIN-1 | none |
| `se`, `se.ps1`, `se.py`, `docs/SE1-AGENTS.md`, `vendor/se1/` | unedited copies from SE1 v1.7 | none |
| `.claude/settings.json` | runs `se --post` at startup, resume, and compaction | none |
| `_config.yml` | renders `main` with the named theme at https://wmacevoy.github.io/devops/ | none |

## One-way doors

- **Principle IDs and anchors**, once anyone cites them; an ID is never reused. Nobody has yet.
- **The prefix `DO1` and the repository name**, once published.
- **The root `AGENTS.md` path.**

## Decisions

- **The checker is DM1's, with the names changed** — REQ1/CHK-1 — it already enforces this shape,
  and has tests — rejected: writing one, which would be a check never seen red — source: DM1 v1.4.
- **The two agreements are both imported from `CLAUDE.md`** — REQ1/AGR-1 — a file to open is a rule
  not followed — source: DM1's design, and SE1's ADOPT.md.
- **Chapters were drafted with a shorthand for citations and expanded by a script** that is not in
  the repository; the Markdown is the source — REQ1/CHK-1 — a hundred long links typed by hand are a
  hundred chances to mistype one — source: the drafting session.

## Deferred

- *Not yet:* `archive/`; a doctor.
