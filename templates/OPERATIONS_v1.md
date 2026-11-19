# Operating contract — v1

What this system promises while it runs, written so that someone can object to it
([DO1/R2](https://github.com/wmacevoy/devops/blob/main/guide/04-requirements.md#r2)). It sits beside `REQUIREMENTS_vN.md`; a change in a promise's meaning is a new version
([SE1/R10](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/04-requirements.md#r10)). Mark every number nobody gave you *(invented)*, and every line read off a running
system *(inferred)*.

Client: _name_ · Judge of "reliable enough": _name, and on what occasion_ ([DO1/U9](https://github.com/wmacevoy/devops/blob/main/guide/02-unknowing.md#u9)) · Date sent:
_…_ · *Tell us what is wrong with it.*

## When it matters

- **OPS-WINDOW-1** _The hours or events in which the promises below apply in full, in a named time
  zone, and what applies outside them._ ([DO1/R3](https://github.com/wmacevoy/devops/blob/main/guide/04-requirements.md#r3), [DO1/D5](https://github.com/wmacevoy/devops/blob/main/guide/05-design.md#d5))
- **OPS-FREEZE-1** _When nothing may be deployed, and who may make an exception._ ([DO1/R6](https://github.com/wmacevoy/devops/blob/main/guide/04-requirements.md#r6))

## Availability

- **OPS-AVAIL-1** _What a user gets, how fresh, for what share of requests, over what period._
  _Error budget for the period: … minutes._ ([DO1/R3](https://github.com/wmacevoy/devops/blob/main/guide/04-requirements.md#r3))
- **OPS-DOWN-1** _When it may be down on purpose, for how long, with what notice._

## Recovery

One row per piece of kept state in the inventory ([DO1/R4](https://github.com/wmacevoy/devops/blob/main/guide/04-requirements.md#r4), [DO1/D1](https://github.com/wmacevoy/devops/blob/main/guide/05-design.md#d1)).

| Kept state | May lose up to | Back within | Restored from | Last restore timed | Took |
| ---------- | -------------- | ----------- | ------------- | ------------------ | ---- |
| _…_        | _…_            | _…_         | _…_           | _date, or never_   | _…_  |

## Cost

- **OPS-COST-1** _Ceiling per month; who is told at what share of it; what happens at the ceiling
  (stops, degrades, keeps charging)._ ([DO1/R10](https://github.com/wmacevoy/devops/blob/main/guide/04-requirements.md#r10))

## People

- **OPS-CALL-1** _Who is told when it breaks, by what route, during which hours; who is not on
  call, and when._ ([DO1/R6](https://github.com/wmacevoy/devops/blob/main/guide/04-requirements.md#r6), [DO1/H4](https://github.com/wmacevoy/devops/blob/main/guide/09-people.md#h4))
- **OPS-TELL-1** _Who we tell when it is down, where, and how soon._ ([DO1/H6](https://github.com/wmacevoy/devops/blob/main/guide/09-people.md#h6))

## Dependencies we do not run

| Dependency | If absent | If late | If wrong | If lying | Needed to deploy? To roll back? |
| ---------- | --------- | ------- | -------- | -------- | ------------------------------- |
| _…_        | _…_       | _…_     | _…_      | _…_      | _…_                             |

([DO1/R5](https://github.com/wmacevoy/devops/blob/main/guide/04-requirements.md#r5))

## Non-goals

_What we have decided not to operate, each with what would reopen it._ ([DO1/R9](https://github.com/wmacevoy/devops/blob/main/guide/04-requirements.md#r9))

## Assumptions read to the client

_Each as a question, with the answer and the date._ ([DO1/H7](https://github.com/wmacevoy/devops/blob/main/guide/09-people.md#h7))

## Not yet answered
