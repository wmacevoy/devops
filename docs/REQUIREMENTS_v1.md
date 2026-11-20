# Requirements — v1

What must be observably true of this guide, and what it is not. Written on 2026-11-19 by Claude
Fable 5.2 from one request — "make a devops public repo, look at software-engineering and
data-mining as a template and create a similar set of guidelines" — and from those two repositories
([SE1/W12](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/08-work.md#w12)), so **all of it is *(inferred)* until the client confirms it**. A change in meaning is a
new version.

Client: Warren MacEvoy · Proxy for the users: none yet; the DevOps course's projects are expected to
be · Date sent: 2026-11-19, with the first publication · *Tell us what is wrong with it.*

## Glossary

GLOSSARY.md, at the root, is the glossary; this repository adds no terms to it.

## Mission

- **MISSION-1** *(since v1)* *(judged)* *(inferred)* People and agents who deliver and run software
  keep it running and changeable, by following a written guide that extends SE1 and does not repeat
  it. Judge: Warren MacEvoy, on the guide as it stands, at each release he signs. Veto: a field
  report where following a principle made a system less reliable and the guide does not change.

## Shape

Held by: Warren MacEvoy *(inferred)*

- **SHAPE-1** *(since v1)* *(judged)* *(inferred)* Every principle sits in one of SE1's ten chapters
  under SE1's letter, and says on the line under its heading whether it is new or which SE1
  principles it reframes. Judge: the holder, at each release.
- **SHAPE-2** *(since v1)* *(judged)* *(inferred)* What SE1 already says is cited at a pinned
  release, not restated. Judge: the holder, at each release.
- **Says no to:** a principle that applies to operations unchanged from SE1; a tool with a
  dependency outside the standard library; a tutorial on a particular product.
- **Exceptions:** none yet.

## The pain

*(inferred)* SE1's chapter on operating is written at the scale of one service built by one team. The
DevOps course teaches the path around it — containers, pipelines, secrets, clouds, laptops — from a
repository of examples with no stated principles, and agents working there have no rule for
credentials in reach, destructive commands, or what was made by hand.

## People this touches

*(inferred)* Students in the course, teams adopting the guide, agents working from its agreement, and
the users of whatever those teams run, who meet it only when it is down.

## Requirements

- **CHK-1** *(since v1)* *(inferred)* `tools/check.py` exits nonzero when a link, anchor, DO1 or SE1
  citation, basis line, README index or map, or release label is wrong.
- **PIN-1** *(since v1)* *(inferred)* Every SE1 link, llms.txt, and CI name one SE1 release.
- **PORT-1** *(since v1)* *(inferred)* The full check in `CONTRIBUTING.md` runs on Windows, macOS,
  and Linux with Python 3.9 or later and git, and CI shows it on all three.
- **AGR-1** *(since v1)* *(inferred)* `AGENTS.md` extends SE1's agreement at a named release, and
  each rule cites its principle by versioned ID.
- **HON-1** *(since v1)* *(inferred)* The guide says, wherever a reader would otherwise assume
  otherwise, that it is untried, who drafted it, and what was not checked.

## Non-goals

- *(inferred)* Replacing SE1, or teaching software engineering.
- *(inferred)* Teaching a tool: Docker, a CI service, a cloud. The course repository does that.
- *(inferred)* Running anything; this repository deploys nothing.

## Scope decisions

- 2026-11-19 *(inferred)*: the guide's prefix is `DO1` and its repository `devops`, after `SE1` and
  `DM1`.
- 2026-11-19 *(inferred)*: published before the author read it, at his instruction to make the
  repository public, and marked unreleased for that reason.
- 2026-11-19 *(inferred)*: no doctor of its own in v1.0; DM1's is as large as SE1's, with tests as large again,
  and one written unseen would be a check never seen red.
- 2026-11-19 *(inferred)*: all of SE1 is vendored at `vendor/se1`, as DM1 does, and its copy of the
  agreement lives at `docs/SE1-AGENTS.md`.
- 2026-11-19: DM1 v1.4 is vendored at `vendor/dm1`, at the owner's request: DevOps often includes
  data mining.
- 2026-11-19 *(inferred)*: DM1 is cited where it changes what a principle asks of a system with a
  model in it; SHAPE-2 holds for it as for SE1; and CHK-1's "link" covers DM1 links.
- 2026-11-19: operations is not only software: on premises it is plain engineering, and the cloud is
  engineering rented; the Gantt chart is real there (the owner's words, written as DO1/W10).

## Not yet answered

- Whether GitHub Pages should render this guide, as it does SE1 and DM1. `_config.yml` is here; the
  site is not switched on.
- Whether `DO1` is the prefix the owner wants.
- Which SE1 principles left off the map were meant to be.

## Deferred

- *Not yet:* a doctor; `archive/`, with checked sources; a field report.
