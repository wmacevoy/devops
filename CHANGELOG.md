# Changelog

What changed between releases, by principle ID, newest first. A change you cannot spot is a K3
factory ([SE1/U7](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/02-unknowing.md#u7)), so changes are listed here by ID, and copies of AGENTS.md and the templates say
which release they came from.

Within DO1, a minor release may add principles, reword them, or extend what they ask; it never moves
an ID ([principle index](README.md#principle-index)). Work toward a release goes under
`## vX.Y — unreleased`; give it a date when you tag it. `tools/check.py` requires copies to link to
the newest dated release, and to `main` until there is one. Links to SE1 never name `main`: they
name the SE1 release this guide was written against, which each release below states.

## v1.0 — unreleased

· 100 principles

A first draft: 84 principles that reframe SE1 principles for operations, and 16 new ones that SE1
has no counterpart for. Written on 2026-11-19 by Claude Fable 5.2 with Warren MacEvoy, in one
session, against **SE1 v1.7**, on the pattern of the data-mining guide (DM1 v1.4). It stays
unreleased until the author has read it and signs it ([SE1/U9](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/02-unknowing.md#u9)).

The new principles: [DO1/P6](guide/01-purpose.md#p6), [DO1/U3](guide/02-unknowing.md#u3), [DO1/U10](guide/02-unknowing.md#u10), [DO1/F6](guide/03-feedback.md#f6), [DO1/F7](guide/03-feedback.md#f7), [DO1/R10](guide/04-requirements.md#r10), [DO1/D14](guide/05-design.md#d14),
[DO1/T11](guide/06-testing.md#t11), [DO1/O6](guide/07-operating.md#o6), [DO1/O13](guide/07-operating.md#o13), [DO1/W8](guide/08-work.md#w8), [DO1/H9](guide/09-people.md#h9), [DO1/A3](guide/10-agents.md#a3), [DO1/A4](guide/10-agents.md#a4), [DO1/A9](guide/10-agents.md#a9), [DO1/A10](guide/10-agents.md#a10).

With it: AGENTS.md, ADOPT.md, CHECKLISTS, GLOSSARY, READING, GAPS, five templates (OPERATIONS_v1,
INVENTORY, RUNBOOK, POSTMORTEM, CONTRIBUTING), and `tools/check.py` with its tests, ported from
DM1's. SE1 v1.7 is vendored whole at `vendor/se1`.

Not reviewed: every reframing was written from SE1's quick reference, one line per principle, and
not from SE1's chapters. SE1 principles this guide leaves off its map were not each considered and
declined; they were not reached.
