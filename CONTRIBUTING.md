# Contributing

How to work on this repository, for a new teammate and for an agent, who both arrive knowing
nothing ([SE1/A3](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/10-agents.md#a3)). A change that makes a line here untrue updates it in the same change.
`templates/CONTRIBUTING.md` is the template this guide publishes for projects that run something;
this file is about the guide itself.

## Run it

- **Prerequisites:** Python 3.9 or later and git. SE1 is at `vendor/se1`. Nothing to install.
- **Build:** none; the repository is Markdown and scripts.
- **Run:** `./se --doctor` for this repository's state.
- **Reset to a known state:** `git status`, then `git restore` on what you do not want. Nothing is
  generated.

## Check it

- **The full check:** `python3 -I tools/check.py && python3 -I tools/test_check.py`, the same
  commands CI runs.
- **On Windows,** where an installer may give `python` and not `python3`, use `python` in the same
  commands.
- **Success prints:** `0 problems` from the checker, then `OK` from the tests.
- **Instruments:** the checker and its tests. `tools/test_check.py` plants a breakage per checker
  rule in a copy of this repository.

## Rules for this repository

- The root `AGENTS.md` is this guide's published agreement. SE1's is the unedited copy at
  `docs/SE1-AGENTS.md`, and `vendor/se1/`, `se`, `se.ps1`, and `se.py` are unedited copies too;
  change them only by re-copying from a release. `vendor/dm1/` is DM1 v1.4, unedited, except that its
  own `vendor/se1/` is left out: it was the same SE1 v1.7 as ours.
- Bumping SE1 follows *Changing this guide* in the README.
- Python 3.9 or later and git are the only dependencies, as in SE1 and DM1 *(inferred from those
  guides; not yet confirmed for this one)*.
- A principle's ID never moves to a different idea. After adding or rewording one, paste the
  output of `python3 -I tools/check.py --print` into the README.
- Nothing here has been released. Until `v1.0` is dated in the CHANGELOG, copies link to `main`.

## Where things are

- **Working set:** `docs/`: `REQUIREMENTS_vN`, `DESIGN_vN`, `QUEUE`, `FINDINGS`.
- **How a project adopts this guide:** `ADOPT.md`.
- **Agents:** `CLAUDE.md` imports both agreements; read this file before acting.

## AGENTS.md's nouns, here

| AGENTS.md says                 | In this repository                                      |
| ------------------------------ | ------------------------------------------------------- |
| the repository guide           | `CONTRIBUTING.md`                                       |
| the queue                      | `docs/QUEUE.md`                                         |
| findings                       | `docs/FINDINGS.md`                                      |
| the check command              | the full check above; success prints `0 problems`, `OK` |
| the handoff check              | `se --post`, a `SessionStart` hook; not yet seen to fire here |
| `Non-goals`, `Scope decisions` | sections of `docs/REQUIREMENTS_v1.md`                   |
| the operating contract, the inventory, the runbooks | none: a guide runs nothing         |
