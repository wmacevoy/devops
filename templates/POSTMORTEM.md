# Incident review — _date, one line_

Blameless: what made the wrong thing easy, not who did it ([DO1/H5](https://github.com/wmacevoy/devops/blob/main/guide/09-people.md#h5)). Written from the timeline kept
during the incident ([DO1/O10](https://github.com/wmacevoy/devops/blob/main/guide/07-operating.md#o10)); it becomes an entry in `FINDINGS.md`, and its actions become queue
items.

- **Harm:** _what users saw, how many, for how long_ · **Budget spent:** _minutes of …_
  ([DO1/R3](https://github.com/wmacevoy/devops/blob/main/guide/04-requirements.md#r3))
- **Detected by:** _an alert, a person, a user_ · **After:** _how long from start to someone
  knowing_ ([DO1/P2](https://github.com/wmacevoy/devops/blob/main/guide/01-purpose.md#p2))
- **Lead:** _name_ · **Told:** _who, when, where_ ([DO1/H3](https://github.com/wmacevoy/devops/blob/main/guide/09-people.md#h3), [DO1/H6](https://github.com/wmacevoy/devops/blob/main/guide/09-people.md#h6))

## Timeline

Times in UTC ([DO1/D5](https://github.com/wmacevoy/devops/blob/main/guide/05-design.md#d5)). What was seen, what was tried, what happened.

| Time | Observed / done | By |
| ---- | --------------- | -- |
| _…_  | _…_             | _…_ |

## What changed beforehand

_The deploy, switch, configuration, or outside event that preceded it; which commit was running._
([DO1/D13](https://github.com/wmacevoy/devops/blob/main/guide/05-design.md#d13))

## What stopped the harm

_The mitigation, and the signal that confirmed it._

## Why it was possible

_Each condition that had to hold. Which was K2 (nobody knew), which K3 (everybody "knew"
otherwise)?_ ([DO1/U1](https://github.com/wmacevoy/devops/blob/main/guide/02-unknowing.md#u1))

## Which cheaper loop could have caught it

([DO1/F3](https://github.com/wmacevoy/devops/blob/main/guide/03-feedback.md#f3))

## What it changes

Each action is something that will refuse next time without anyone remembering: a check, an alert
with a runbook, a permission removed, a default changed ([DO1/W7](https://github.com/wmacevoy/devops/blob/main/guide/08-work.md#w7)).

| Action | Owner | Queue item | Seen to refuse? |
| ------ | ----- | ---------- | --------------- |
| _…_    | _…_   | _…_        | _date, or not yet_ |

_If nothing changes, say why._ ([SE1/W13](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/08-work.md#w13))
