# Contributing

How to work on this repository — for a new teammate and for an agent, who both arrive knowing
nothing ([SE1/A3](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/10-agents.md#a3)). Keep it short and true: a change that makes a line here untrue updates it in the
same change.

## Set up a machine

- **Walkthrough:** `_path_` — _numbered steps, per operating system_ ([DO1/H9](https://github.com/wmacevoy/devops/blob/main/guide/09-people.md#h9))
- **The doctor:** `_command_` — _one line per check; exits nonzero on a failure_ ([DO1/T2](https://github.com/wmacevoy/devops/blob/main/guide/06-testing.md#t2))

## Run it

- **Up, locally:** `_command_` — _what it starts, on which ports_ ([DO1/F2](https://github.com/wmacevoy/devops/blob/main/guide/03-feedback.md#f2))
- **Reset to a known state:** `_command_` — _what it deletes_
- **Environments:** _names, and that each name is in every container, network, and volume_
  ([DO1/T5](https://github.com/wmacevoy/devops/blob/main/guide/06-testing.md#t5)) — see `INVENTORY.md`
- **Generated files, never edited or committed:** _which, from which templates_ ([DO1/D6](https://github.com/wmacevoy/devops/blob/main/guide/05-design.md#d6))

## Check it

- **The full check:** `_command_` — the same command the pipeline runs ([DO1/O2](https://github.com/wmacevoy/devops/blob/main/guide/07-operating.md#o2))
- **Success prints:** `_the line that means it passed_`

## Deploy it

- **Who or what deploys:** _the pipeline, on merge to … / a person, with …_ ([DO1/A6](https://github.com/wmacevoy/devops/blob/main/guide/10-agents.md#a6))
- **Deploy:** `_command or trigger_` · **Roll back:** `_command_`, last timed _date_, took _…_
  ([DO1/O5](https://github.com/wmacevoy/devops/blob/main/guide/07-operating.md#o5))
- **What is running now:** `_command that prints the commit_` ([DO1/D13](https://github.com/wmacevoy/devops/blob/main/guide/05-design.md#d13))
- **Freeze:** _when nothing ships_ — see `OPERATIONS_vN.md`

## Rules for this repository

- **What an agent's session may reach:** _which accounts and environments; production is / is not
  among them_ ([DO1/A3](https://github.com/wmacevoy/devops/blob/main/guide/10-agents.md#a3))
- **Commands that need a person's word every time:** _the destructive ones, by name_ ([DO1/A4](https://github.com/wmacevoy/devops/blob/main/guide/10-agents.md#a4))
- **Secrets:** _where they live, how they are referred to; never a value in a commit, log, or
  prompt_ ([DO1/O8](https://github.com/wmacevoy/devops/blob/main/guide/07-operating.md#o8), [DO1/A9](https://github.com/wmacevoy/devops/blob/main/guide/10-agents.md#a9))
- **What may be sent to a hosted model:** _logs, configuration — scrubbed how_ ([DO1/A9](https://github.com/wmacevoy/devops/blob/main/guide/10-agents.md#a9))

## Where things are

- **Working set:** _`docs/`: `REQUIREMENTS_vN`, `OPERATIONS_vN`, `DESIGN_vN`, `INVENTORY`, `QUEUE`,
  `FINDINGS`, and `runbooks/`_ ([DO1/W5](https://github.com/wmacevoy/devops/blob/main/guide/08-work.md#w5))

## AGENTS.md's nouns, here

| The agreements say             | In this repository |
| ------------------------------ | ------------------ |
| the repository guide           | this file          |
| the operating contract         | `_path_`           |
| the inventory                  | `_path_`           |
| the runbooks                   | `_path_`           |
| the doctor, the preflight      | `_command_`        |
| the check command              | `_command_`        |
| the queue, findings            | `_paths_`          |
