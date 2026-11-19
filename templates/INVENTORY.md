# Inventory

What exists, where, and who answers for it ([DO1/D1](https://github.com/wmacevoy/devops/blob/main/guide/05-design.md#d1), [DO1/W5](https://github.com/wmacevoy/devops/blob/main/guide/08-work.md#w5)). A line read off a running system
and not yet confirmed by someone who can decide is marked *(inferred)* ([DO1/W9](https://github.com/wmacevoy/devops/blob/main/guide/08-work.md#w9)). Written by _name
or agent_ on _date_; last compared with what is actually running on _date_, by _command_
([DO1/U3](https://github.com/wmacevoy/devops/blob/main/guide/02-unknowing.md#u3), [SE1/U9](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/02-unknowing.md#u9)).

## Environments

| Name | What it is for | Where | Brought up by | How it differs from production, and what it therefore cannot show |
| ---- | -------------- | ----- | ------------- | ----------------------------------------------------------------- |
| _…_  | _…_            | _…_   | `_command_`   | _…_                                                               |

([DO1/U4](https://github.com/wmacevoy/devops/blob/main/guide/02-unknowing.md#u4), [DO1/T5](https://github.com/wmacevoy/devops/blob/main/guide/06-testing.md#t5))

## Kept state

Everything that cannot be rebuilt from the repository.

| What | Where | Owner | Backed up to | Can everyday credentials delete the backup? | Last restore tested |
| ---- | ----- | ----- | ------------ | ------------------------------------------- | ------------------- |
| _…_  | _…_   | _…_   | _…_          | _yes / no_                                  | _date, or never_    |

([DO1/R4](https://github.com/wmacevoy/devops/blob/main/guide/04-requirements.md#r4), [DO1/O13](https://github.com/wmacevoy/devops/blob/main/guide/07-operating.md#o13))

## Secrets, by name

Never a value in this file.

| Name | Opens | Stored in | Read by (people, jobs) | Last rotated | Rotation runbook |
| ---- | ----- | --------- | ---------------------- | ------------ | ---------------- |
| _…_  | _…_   | _…_       | _…_                    | _date, or never_ | _link_       |

([DO1/O8](https://github.com/wmacevoy/devops/blob/main/guide/07-operating.md#o8))

## Identities

| Identity (person, pipeline, service) | Environment | Can do | Credential lifetime | Second owner |
| ------------------------------------ | ----------- | ------ | ------------------- | ------------ |
| _…_                                  | _…_         | _…_    | _…_                 | _…_          |

([DO1/O9](https://github.com/wmacevoy/devops/blob/main/guide/07-operating.md#o9), [DO1/H2](https://github.com/wmacevoy/devops/blob/main/guide/09-people.md#h2))

## Things with a date

| What | Expires or ends | Owner | Renewed how | Alert set for |
| ---- | --------------- | ----- | ----------- | ------------- |
| _certificate, token, domain, credit, runtime, base image_ | _date_ | _…_ | _…_ | _date_ |

([DO1/U10](https://github.com/wmacevoy/devops/blob/main/guide/02-unknowing.md#u10))

## One-way doors already taken

_Public names, regions, data stores, account layout, distributed keys — each with why._
([DO1/D11](https://github.com/wmacevoy/devops/blob/main/guide/05-design.md#d11))

## Running and unexplained

_Anything found that nobody could account for: stopped, not deleted, since when, who is watching._
([DO1/D12](https://github.com/wmacevoy/devops/blob/main/guide/05-design.md#d12))
