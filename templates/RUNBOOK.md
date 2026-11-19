# Runbook — _the operation_

One operation, written so that someone who has never done it can, when the system it describes may
be down ([DO1/W5](https://github.com/wmacevoy/devops/blob/main/guide/08-work.md#w5), [DO1/U5](https://github.com/wmacevoy/devops/blob/main/guide/02-unknowing.md#u5)). Keep it where an outage cannot take it away ([DO1/T11](https://github.com/wmacevoy/devops/blob/main/guide/06-testing.md#t11)).

- **Last run for real:** _date_, by _someone other than the author, if possible_ · **Took:** _…_
  ([DO1/U6](https://github.com/wmacevoy/devops/blob/main/guide/02-unknowing.md#u6), [DO1/H8](https://github.com/wmacevoy/devops/blob/main/guide/09-people.md#h8))
- **Run when:** _the alert, symptom, or schedule that calls for this_ ([DO1/O3](https://github.com/wmacevoy/devops/blob/main/guide/07-operating.md#o3))
- **Who may run it:** _role_ · **Needs:** _access, tools, and the second person if one is required_
- **Can it be undone?** _yes, by … / no_ ([DO1/O12](https://github.com/wmacevoy/devops/blob/main/guide/07-operating.md#o12), [DO1/W2](https://github.com/wmacevoy/devops/blob/main/guide/08-work.md#w2))

## Before

1. _Say which environment, and how you checked._ ([DO1/D9](https://github.com/wmacevoy/devops/blob/main/guide/05-design.md#d9))
2. _What to capture first, with the command._ ([DO1/T8](https://github.com/wmacevoy/devops/blob/main/guide/06-testing.md#t8))

## Steps

Each step says what to run, what you should see, and what to do if you do not.

1. `_command_` — _expect: …_ — _if not: …_

## Confirm

_The signal that shows it worked, and how long to watch it._ ([DO1/O10](https://github.com/wmacevoy/devops/blob/main/guide/07-operating.md#o10))

## Way back

_How to undo each step that can be undone; where the point of no return is._

## What this run found

_Every pause, question, and step done from memory; fix the page, then update the date above._
