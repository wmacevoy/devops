# DevOps

A guide to delivering change to a running system and keeping it running — written for humans and
for the AI agents working beside them. It is a companion to
[Software Engineering](https://github.com/wmacevoy/software-engineering) (`SE1`), and assumes it.

Software engineering asks *does the thing work*. Operations asks *is it running, and can we change
it without finding out the hard way*. Most of the discipline is the same, pointed at a different
noun — not the code, but **the path** from a change in someone's head to a system that is running,
watched, and restorable — so this guide does not repeat SE1. Each principle here either **reframes**
an SE1 principle for operations (and links to it), or is **new**: something SE1 does not say because
a program on one machine does not need it. Every principle says which, on the line under its
heading.

It is organized as **numbered principles** with versioned IDs (`DO1/F6`, `DO1/D6`, `DO1/O13`, …), in
the same ten chapters, with the same letters, as SE1. Each states one idea and says why it is true;
each ends with a question you can ask out loud in a meeting or a review: *"if this deploy breaks,
how many changes are we choosing between? (DO1/F6)"*, *"how old is the newest proof that the backup
works? (DO1/U6)"*.

**This is a first draft, v1.0, unreleased.** It was written in one session and nothing in it has
been tried. Read [Why to trust this, and why to doubt it](#why-to-trust-this-and-why-to-doubt-it)
before leaning on any of it.

## The argument in ten lines

1. **Operations engineers a path, not a server.** The deliverable is the running system and the
   means to rebuild it; the honest thing to move is the size of the change
   ([DO1/P1](guide/01-purpose.md#p1), [DO1/P5](guide/01-purpose.md#p5)).
2. **Deployed is not running, and running is not serving.** Every layer can report success while
   the one above it is broken ([DO1/P2](guide/01-purpose.md#p2)).
3. **Production says "fine" by default.** A dashboard with no data is calm, and the repository and
   the machine drift apart without a sound ([DO1/U2](guide/02-unknowing.md#u2), [DO1/U3](guide/02-unknowing.md#u3)).
4. **Everything has an age and an expiry.** A status with no date is a belief; a certificate is
   valid until the second it is not ([DO1/U6](guide/02-unknowing.md#u6), [DO1/U10](guide/02-unknowing.md#u10)).
5. **The size of a change is the size of the search when it breaks.** Small and often is how
   reliability and speed are both had ([DO1/F6](guide/03-feedback.md#f6), [DO1/F7](guide/03-feedback.md#f7)).
6. **Availability is a budget, recovery is two numbers, and cost has a ceiling.** Each is a
   purchase someone authorizes ([DO1/R3](guide/04-requirements.md#r3), [DO1/R4](guide/04-requirements.md#r4), [DO1/R10](guide/04-requirements.md#r10)).
7. **The repository is the authority; what runs is derived from it.** Separate what can be rebuilt
   from what must be kept, and replace instead of repairing ([DO1/D1](guide/05-design.md#d1), [DO1/D6](guide/05-design.md#d6), [DO1/D14](guide/05-design.md#d14)).
8. **A backup never restored, an alert never fired, and a rollback never run prove nothing** — and
   the backup lives where the failure cannot reach it ([DO1/T3](guide/06-testing.md#t3), [DO1/O13](guide/07-operating.md#o13)).
9. **The pipeline is production.** It holds every key, so it gets an identity, a scope, and a
   review of its own ([DO1/R8](guide/04-requirements.md#r8), [DO1/O9](guide/07-operating.md#o9)).
10. **An agent's shell holds your credentials.** It proposes; a person or a reviewed pipeline
    applies; anything destructive waits for a word ([DO1/A3](guide/10-agents.md#a3), [DO1/A4](guide/10-agents.md#a4), [DO1/A6](guide/10-agents.md#a6)).

## How to read it

**People:** if you know SE1, read the chapters in order; each is short, because SE1 carries the long
arguments, and [SE1 chapter 7](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/07-operating.md) is already this subject at the scale of one
service. If you do not, read SE1's first three chapters and its seventh first, then this, following
the *Reframes* link whenever a principle leans on SE1. After that, use the index below and
[CHECKLISTS.md](CHECKLISTS.md). Terms used precisely are in [GLOSSARY.md](GLOSSARY.md).

**Agents:** read [AGENTS.md](AGENTS.md) — the working agreement for operational work — and SE1's
[AGENTS.md](https://github.com/wmacevoy/software-engineering/blob/v1.7/AGENTS.md), which it extends, before acting in a repository that follows this guide.
Then read the chapter that bears on the task (the promises → 4, environments and state → 5, checks →
6, pipelines, deploys, secrets, incidents → 7). Cite principles by ID when you raise a concern or
explain a choice.

**Teams adopting it:** follow [ADOPT.md](ADOPT.md), for a new system or one already running. It
puts production out of an agent's reach first, then vendors both guides: SE1's AGENTS.md as
`AGENTS.md`, this guide's beside it as `AGENTS-DO.md`, both imported from `CLAUDE.md`.

## Contents

| Chapter                                     | Answers                                                          |
| ------------------------------------------- | ---------------------------------------------------------------- |
| [1. Purpose](guide/01-purpose.md)           | what operations is for, and who pays when it fails               |
| [2. Unknowing](guide/02-unknowing.md)       | why production says "fine" by default: silence, drift, and expiry |
| [3. Feedback](guide/03-feedback.md)         | what a test in production costs, and how the size of a change sets the price |
| [4. Requirements](guide/04-requirements.md) | the promises: availability, recovery, dependencies, cost, and what is not operated |
| [5. Design](guide/05-design.md)             | infrastructure as state and caches; the repository as authority; one artifact, many environments |
| [6. Testing](guide/06-testing.md)           | doctors and preflights, restores seen to work, injected failure, the cold start |
| [7. Operating](guide/07-operating.md)       | pins, pipelines, alerts, deploys and releases, secrets, identities, incidents, capacity, backups |
| [8. Work](guide/08-work.md)                 | the loop past deploy, the empty system first, done, toil, adopting on a running system |
| [9. People](guide/09-people.md)             | who is paged, who is told, blame, drills, and the laptop          |
| [10. Agents](guide/10-agents.md)            | a fast operator with your credentials and no pager               |

| Reference                          | For                                                              |
| ---------------------------------- | ---------------------------------------------------------------- |
| [AGENTS.md](AGENTS.md)             | the working agreement for agents doing operational work, portable to any repository |
| [ADOPT.md](ADOPT.md)               | how to adopt the guide, on a new system or a running one         |
| [CHECKLISTS.md](CHECKLISTS.md)     | every checklist, in the order the work meets them                |
| [GLOSSARY.md](GLOSSARY.md)         | the terms used precisely (SE1's glossary covers the rest)        |
| [READING.md](READING.md)           | the longer arguments, one line each                              |
| [CHANGELOG.md](CHANGELOG.md)       | what changed in each release, by principle ID                    |
| `templates/`                       | starting points: [OPERATIONS](templates/OPERATIONS_v1.md), [INVENTORY](templates/INVENTORY.md), [RUNBOOK](templates/RUNBOOK.md), [POSTMORTEM](templates/POSTMORTEM.md), [CONTRIBUTING](templates/CONTRIBUTING.md) — and SE1's [REQUIREMENTS](https://github.com/wmacevoy/software-engineering/blob/v1.7/templates/REQUIREMENTS_v1.md), [DESIGN](https://github.com/wmacevoy/software-engineering/blob/v1.7/templates/DESIGN_v1.md), [QUEUE](https://github.com/wmacevoy/software-engineering/blob/v1.7/templates/QUEUE.md), and [FINDINGS](https://github.com/wmacevoy/software-engineering/blob/v1.7/templates/FINDINGS.md), unchanged |
| [GAPS.md](GAPS.md)                 | where the guide falls short, and what is not yet tested          |

## The running example

SE1's running example is a parking-occupancy service for a stadium's lots: about a hundred lots, one
sensor per lot reporting free spaces every five seconds, a collector that marks each lot's count
`live`, `stale`, or `unreachable`, a device registry of the sensors' public keys, and an app for
drivers and operators. This guide picks it up on the day it has to be run. **The service works; now
it has to be there on game night.**

- **The environments.** A laptop stack with simulated sensors and their fault controls; a test
  stack beside it; a staging copy with ten simulated lots; production, with a hundred real ones.
- **The path.** A merge builds one image, CI checks it, the pipeline pushes it by digest and
  deploys it to staging, then to production behind a gate.
- **What must be kept.** The readings, twenty a second; the device registry, without which no
  reading can be verified; the secrets.
- **The client** is the same operations manager, who now says "it can never go down." Underneath is
  one evening: the signs went dark forty minutes before gates, and the one person who could restart
  the collector was in the stands ([DO1/R1](guide/04-requirements.md#r1)).
- **When it matters.** About ten events a month. The two hours before gates are the whole
  requirement; a Tuesday in February is not ([DO1/R3](guide/04-requirements.md#r3)).
- **Built-in traps.** The nightly backup has never been restored ([DO1/T3](guide/06-testing.md#t3)). A firewall rule was
  changed by hand during the first game and is in no file ([DO1/U3](guide/02-unknowing.md#u3)). Staging has a tenth of the
  lots and none of the reconnect storms ([DO1/U4](guide/02-unknowing.md#u4)). The only working deploy is on one laptop
  ([DO1/U5](guide/02-unknowing.md#u5)). A certificate and a hundred sensor keys each have a date ([DO1/U10](guide/02-unknowing.md#u10)). The readings
  volume fills on a day nobody has computed ([DO1/F4](guide/03-feedback.md#f4)). And the sensors' firmware, once sent, cannot
  be called back ([DO1/O12](guide/07-operating.md#o12)).

The [DevOps course repository](https://github.com/wmacevoy/devops-wmacevoy) this guide grew from
holds real, small versions of several of these: a notes service carried through compose, a CI
container, encrypted secrets, and deploys to three clouds; laptop setup walkthroughs; and a doctor
script that checks them.

## Principle index

Every citation carries the guide's major version: `DO1/F5` is principle 5 of *Feedback* in the first
major version, and `SE1/F5` is a different principle in a different guide. Within DO1, a principle
may be reworded, but its ID never moves to a different idea; a retired principle keeps its ID with a
note saying where its content went, and new principles take the next number in their chapter. The
letters are the chapters, as in SE1: **P**urpose, **U**nknowing, **F**eedback, **R**equirements,
**D**esign, **T**esting, **O**perating, **W**ork, **H**umans (people), **A**gents. (Requirement IDs
in your own repository should use a different form, such as `OPS2/AVAIL-1`, so they are never
confused with these.)

**[1. Purpose](guide/01-purpose.md)**

- [**DO1/P1**](guide/01-purpose.md#p1) Operations delivers change to a running system and keeps it running, on time, within the budget and the people it has, managing discomfort for the team, the client, and whoever is paged
- [**DO1/P2**](guide/01-purpose.md#p2) Deployed is not running, and running is not serving
- [**DO1/P3**](guide/01-purpose.md#p3) Whoever is paged was not in the planning meeting
- [**DO1/P4**](guide/01-purpose.md#p4) A practice that relieves the fear without reducing the risk is worse than nothing, and operations is full of them
- [**DO1/P5**](guide/01-purpose.md#p5) The deliverable is the running system and the path that rebuilds it, not the server
- [**DO1/P6**](guide/01-purpose.md#p6) Building and running are one loop; a wall between them is where the feedback dies

**[2. Unknowing](guide/02-unknowing.md)**

- [**DO1/U1**](guide/02-unknowing.md#u1) Operating is discovering, before production does it for you, which category each part of the system belongs to
- [**DO1/U2**](guide/02-unknowing.md#u2) Production says "fine" by default: silence is not health
- [**DO1/U3**](guide/02-unknowing.md#u3) Drift is the canonical K3: the repository says one thing and the machine runs another
- [**DO1/U4**](guide/02-unknowing.md#u4) Every environment is a chosen falsehood about production; say where it lies
- [**DO1/U5**](guide/02-unknowing.md#u5) What only the last operator knows is K3 for them and K2 for whoever is paged next
- [**DO1/U6**](guide/02-unknowing.md#u6) A status with no age is a K3 factory: last backup, last restore, last deploy, last rotation
- [**DO1/U7**](guide/02-unknowing.md#u7) Look at production when nothing is wrong
- [**DO1/U8**](guide/02-unknowing.md#u8) A threshold, a timeout, and a limit are rules: say who set each, when, and what would show it wrong
- [**DO1/U9**](guide/02-unknowing.md#u9) "Is it reliable enough?" is a judgment; a service-level number may veto, and only the people who depend on it can pass it
- [**DO1/U10**](guide/02-unknowing.md#u10) Everything in production has an expiry date; find it before it finds you

**[3. Feedback](guide/03-feedback.md)**

- [**DO1/F1**](guide/03-feedback.md#f1) A test of a build costs seconds; a test in production costs users — unless the change is small
- [**DO1/F2**](guide/03-feedback.md#f2) The cost of a deploy is designed, not given
- [**DO1/F3**](guide/03-feedback.md#f3) Push each class of failure down to the cheapest loop that can catch it
- [**DO1/F4**](guide/03-feedback.md#f4) A disk fills without a sound: every warning a machine gives must be built
- [**DO1/F5**](guide/03-feedback.md#f5) When uptime, deploy count, or time-to-restore becomes a target, it stops measuring
- [**DO1/F6**](guide/03-feedback.md#f6) The size of a change is the size of the search when it breaks
- [**DO1/F7**](guide/03-feedback.md#f7) Undeployed work is unverified work, and it ages

**[4. Requirements](guide/04-requirements.md)**

- [**DO1/R1**](guide/04-requirements.md#r1) Keep the pain; test the tool
- [**DO1/R2**](guide/04-requirements.md#r2) Specific and wrong: write the operating numbers down so someone can object
- [**DO1/R3**](guide/04-requirements.md#r3) Availability is a budget: spend it on change
- [**DO1/R4**](guide/04-requirements.md#r4) Recovery is two numbers: how much may be lost, and how long until it is back
- [**DO1/R5**](guide/04-requirements.md#r5) Every platform dependency can be absent, late, wrong, or lying
- [**DO1/R6**](guide/04-requirements.md#r6) Ask what nobody volunteers: who is paged, the worst day, the freeze, the ceiling
- [**DO1/R7**](guide/04-requirements.md#r7) Every nine is a purchase someone must authorize
- [**DO1/R8**](guide/04-requirements.md#r8) The pipeline is production: it holds every key
- [**DO1/R9**](guide/04-requirements.md#r9) Write down what you do not operate
- [**DO1/R10**](guide/04-requirements.md#r10) Cost is a requirement, with a ceiling and an alarm

**[5. Design](guide/05-design.md)**

- [**DO1/D1**](guide/05-design.md#d1) Infrastructure is state: separate what you can rebuild from what you must keep
- [**DO1/D2**](guide/05-design.md#d2) An image tag, a build cache, a DNS record, and a deployed config are caches
- [**DO1/D3**](guide/05-design.md#d3) There is no single "now" in a deploy: two versions run at once, and desired is not actual
- [**DO1/D4**](guide/05-design.md#d4) Make every operation idempotent: declare the state, do not script the steps
- [**DO1/D5**](guide/05-design.md#d5) Schedule in UTC, log in UTC, and know which clock expires your credentials
- [**DO1/D6**](guide/05-design.md#d6) The repository is the authority; what runs is derived from it by one command
- [**DO1/D7**](guide/05-design.md#d7) Build once, configure at the edge, and promote the same artifact
- [**DO1/D8**](guide/05-design.md#d8) A platform's complexity must be paid for by a requirement
- [**DO1/D9**](guide/05-design.md#d9) One name for one thing, the same in the repository, the registry, the dashboard, and the alert
- [**DO1/D10**](guide/05-design.md#d10) A service's contract says how it starts, how it stops, and how it reports its health
- [**DO1/D11**](guide/05-design.md#d11) One-way doors in infrastructure: a name, a region, a data store, an account, a published tag
- [**DO1/D12**](guide/05-design.md#d12) Everything running is carried: each service, job, dashboard, and alert has a holder and a bill
- [**DO1/D13**](guide/05-design.md#d13) Every running thing can say which commit it is
- [**DO1/D14**](guide/05-design.md#d14) Replace, do not repair

**[6. Testing](guide/06-testing.md)**

- [**DO1/T1**](guide/06-testing.md#t1) Name what each operational check retires: configuration, status, input, output, state, time, space
- [**DO1/T2**](guide/06-testing.md#t2) A reliable path checks itself: a doctor for the laptop, a preflight for the deploy
- [**DO1/T3**](guide/06-testing.md#t3) A backup never restored, an alert never fired, and a rollback never run prove nothing
- [**DO1/T4**](guide/06-testing.md#t4) A pipeline people re-run until it passes has stopped being a test
- [**DO1/T5**](guide/06-testing.md#t5) Give every run its own environment: put its name in everything it touches
- [**DO1/T6**](guide/06-testing.md#t6) Staging is a mock of production: make it misbehave on request, and check it against the real thing
- [**DO1/T7**](guide/06-testing.md#t7) Test the environment the way you test the code: write the check that passes when it is right
- [**DO1/T8**](guide/06-testing.md#t8) A restart destroys the evidence: capture on the way to the fix
- [**DO1/T9**](guide/06-testing.md#t9) Break it on purpose, on a day you chose
- [**DO1/T10**](guide/06-testing.md#t10) When the platform owns "correct," test against the platform
- [**DO1/T11**](guide/06-testing.md#t11) Rebuild from nothing on a schedule: the cold start finds the circle

**[7. Operating](guide/07-operating.md)**

- [**DO1/O1**](guide/07-operating.md#o1) Pin the platform the way you pin the build: images by digest, tools by version, runners by name
- [**DO1/O2**](guide/07-operating.md#o2) The pipeline runs the command you run; its logic lives in the repository, not in the pipeline's own language
- [**DO1/O3**](guide/07-operating.md#o3) Page a person only for what a person must do now; every page names its runbook
- [**DO1/O4**](guide/07-operating.md#o4) Handle the platform's failures on purpose: time out the deploy, bound the retries, and decide what a half-finished one leaves
- [**DO1/O5**](guide/07-operating.md#o5) Deploy to a few before all, behind a gate that can say no without a person
- [**DO1/O6**](guide/07-operating.md#o6) Deploying is not releasing: ship it dark, turn it on separately, and give every switch an owner and an end date
- [**DO1/O7**](guide/07-operating.md#o7) Change infrastructure the way you change a schema: add beside, move, then remove
- [**DO1/O8**](guide/07-operating.md#o8) A secret has a lifecycle: issued, stored, used, rotated, revoked — and a rotation never performed does not work
- [**DO1/O9**](guide/07-operating.md#o9) Give the pipeline its own identity: scoped to one environment, short-lived, and unable to do what it never needs to
- [**DO1/O10**](guide/07-operating.md#o10) In an incident: one leads, the timeline is written as it happens, and "restored" is confirmed by the signal that reported it
- [**DO1/O11**](guide/07-operating.md#o11) Capacity includes limits you do not own: quotas, rate limits, and how fast the platform can add more
- [**DO1/O12**](guide/07-operating.md#o12) Some operations have no undo: a migration, a deletion, a message sent, a key retired
- [**DO1/O13**](guide/07-operating.md#o13) Keep the backup where the failure cannot reach it

**[8. Work](guide/08-work.md)**

- [**DO1/W1**](guide/08-work.md#w1) The six steps are a loop, and it does not end at deploy
- [**DO1/W2**](guide/08-work.md#w2) Slow down around the operations with no undo: delete, migrate, rotate, rename, anything forced
- [**DO1/W3**](guide/08-work.md#w3) Deploy the empty system first: one page, through the whole pipeline, to production
- [**DO1/W4**](guide/08-work.md#w4) If deploying hurts, do it more often
- [**DO1/W5**](guide/08-work.md#w5) The working set gains an operating contract, an inventory, and runbooks — in the repository
- [**DO1/W6**](guide/08-work.md#w6) Done means running in production, observed, and reversible
- [**DO1/W7**](guide/08-work.md#w7) An incident review that changes nothing will be written again
- [**DO1/W8**](guide/08-work.md#w8) Do it by hand, write it down, then script it — in that order
- [**DO1/W9**](guide/08-work.md#w9) To adopt this on a system that is already running, write down what is there before what should be

**[9. People](guide/09-people.md)**

- [**DO1/H1**](guide/09-people.md#h1) Whoever can change it should hear it break, and whoever is woken by it should be able to change it
- [**DO1/H2**](guide/09-people.md#h2) Operators fail the four ways inputs do: the one who is absent holds the only login
- [**DO1/H3**](guide/09-people.md#h3) Add two rotating roles: who is on call, and who leads an incident
- [**DO1/H4**](guide/09-people.md#h4) A pager is a queue: one that fires every night has already saturated a person
- [**DO1/H5**](guide/09-people.md#h5) The operator who typed the command is the one who found the missing guard
- [**DO1/H6**](guide/09-people.md#h6) Say "we are down" before the users say it to you
- [**DO1/H7**](guide/09-people.md#h7) Read the operating assumptions back to the client, in their words
- [**DO1/H8**](guide/09-people.md#h8) A drill is a test: someone else follows the runbook, and the author watches their hands
- [**DO1/H9**](guide/09-people.md#h9) The laptop is the first environment: setting one up is a deploy, with a written path and a doctor at the end

**[10. Agents](guide/10-agents.md)**

- [**DO1/A1**](guide/10-agents.md#a1) Writing infrastructure got cheap much faster than knowing what applying it will do
- [**DO1/A2**](guide/10-agents.md#a2) A preview is a claim; an apply is an action. Read the first before the second
- [**DO1/A3**](guide/10-agents.md#a3) An agent's shell holds your credentials: keep production out of its reach
- [**DO1/A4**](guide/10-agents.md#a4) A destructive command is a one-way door at typing speed: stop, name the target, and ask
- [**DO1/A5**](guide/10-agents.md#a5) Spend the speed on the boring half of operations
- [**DO1/A6**](guide/10-agents.md#a6) Whoever applies it owns it: an agent proposes the change, a person or a reviewed pipeline makes it
- [**DO1/A7**](guide/10-agents.md#a7) An agent sees the repository, not the console: what was made by hand does not exist to it
- [**DO1/A8**](guide/10-agents.md#a8) What to hand over, and what to keep
- [**DO1/A9**](guide/10-agents.md#a9) Logs, configuration, and environment sent to an agent have left the room
- [**DO1/A10**](guide/10-agents.md#a10) In an incident an agent reads fast and decides nothing
- [**DO1/A11**](guide/10-agents.md#a11) After a gap, the state of production is recalled, not observed: ask the system

## Reading SE1 as an operator

Each SE1 principle below is reframed for operations by the DO1 principles it points to. **SE1
principles not listed apply to operational work unchanged** — a pipeline is software, and everything
SE1 says about software applies to it.

- [SE1/P1](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/01-purpose.md#p1) → [DO1/P1](guide/01-purpose.md#p1)
- [SE1/P2](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/01-purpose.md#p2) → [DO1/P2](guide/01-purpose.md#p2)
- [SE1/P3](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/01-purpose.md#p3) → [DO1/P3](guide/01-purpose.md#p3)
- [SE1/P4](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/01-purpose.md#p4) → [DO1/P4](guide/01-purpose.md#p4)
- [SE1/P5](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/01-purpose.md#p5) → [DO1/P5](guide/01-purpose.md#p5)
- [SE1/U2](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/02-unknowing.md#u2) → [DO1/U1](guide/02-unknowing.md#u1)
- [SE1/U3](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/02-unknowing.md#u3) → [DO1/U2](guide/02-unknowing.md#u2)
- [SE1/U4](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/02-unknowing.md#u4) → [DO1/U4](guide/02-unknowing.md#u4)
- [SE1/U6](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/02-unknowing.md#u6) → [DO1/U5](guide/02-unknowing.md#u5)
- [SE1/U7](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/02-unknowing.md#u7) → [DO1/U6](guide/02-unknowing.md#u6)
- [SE1/U8](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/02-unknowing.md#u8) → [DO1/U7](guide/02-unknowing.md#u7)
- [SE1/U9](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/02-unknowing.md#u9) → [DO1/U8](guide/02-unknowing.md#u8)
- [SE1/U10](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/02-unknowing.md#u10) → [DO1/U9](guide/02-unknowing.md#u9)
- [SE1/F1](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/03-feedback.md#f1) → [DO1/F1](guide/03-feedback.md#f1)
- [SE1/F2](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/03-feedback.md#f2) → [DO1/F2](guide/03-feedback.md#f2)
- [SE1/F3](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/03-feedback.md#f3) → [DO1/F3](guide/03-feedback.md#f3)
- [SE1/F4](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/03-feedback.md#f4) → [DO1/F4](guide/03-feedback.md#f4)
- [SE1/F5](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/03-feedback.md#f5) → [DO1/F5](guide/03-feedback.md#f5)
- [SE1/R1](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/04-requirements.md#r1) → [DO1/R1](guide/04-requirements.md#r1)
- [SE1/R2](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/04-requirements.md#r2) → [DO1/R2](guide/04-requirements.md#r2)
- [SE1/R3](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/04-requirements.md#r3) → [DO1/R6](guide/04-requirements.md#r6)
- [SE1/R4](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/04-requirements.md#r4) → [DO1/R7](guide/04-requirements.md#r7)
- [SE1/R6](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/04-requirements.md#r6) → [DO1/R5](guide/04-requirements.md#r5)
- [SE1/R7](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/04-requirements.md#r7) → [DO1/R3](guide/04-requirements.md#r3), [DO1/R4](guide/04-requirements.md#r4)
- [SE1/R8](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/04-requirements.md#r8) → [DO1/R8](guide/04-requirements.md#r8)
- [SE1/R9](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/04-requirements.md#r9) → [DO1/R9](guide/04-requirements.md#r9)
- [SE1/D1](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/05-design.md#d1) → [DO1/D1](guide/05-design.md#d1)
- [SE1/D2](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/05-design.md#d2) → [DO1/D2](guide/05-design.md#d2)
- [SE1/D3](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/05-design.md#d3) → [DO1/D3](guide/05-design.md#d3)
- [SE1/D4](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/05-design.md#d4) → [DO1/D4](guide/05-design.md#d4)
- [SE1/D5](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/05-design.md#d5) → [DO1/D5](guide/05-design.md#d5)
- [SE1/D6](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/05-design.md#d6) → [DO1/D6](guide/05-design.md#d6)
- [SE1/D7](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/05-design.md#d7) → [DO1/D7](guide/05-design.md#d7)
- [SE1/D8](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/05-design.md#d8) → [DO1/D8](guide/05-design.md#d8)
- [SE1/D9](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/05-design.md#d9) → [DO1/D9](guide/05-design.md#d9)
- [SE1/D10](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/05-design.md#d10) → [DO1/D10](guide/05-design.md#d10)
- [SE1/D11](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/05-design.md#d11) → [DO1/D11](guide/05-design.md#d11)
- [SE1/D12](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/05-design.md#d12) → [DO1/D12](guide/05-design.md#d12)
- [SE1/D13](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/05-design.md#d13) → [DO1/D13](guide/05-design.md#d13)
- [SE1/T1](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/06-testing.md#t1) → [DO1/T1](guide/06-testing.md#t1)
- [SE1/T2](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/06-testing.md#t2) → [DO1/T2](guide/06-testing.md#t2)
- [SE1/T3](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/06-testing.md#t3) → [DO1/T3](guide/06-testing.md#t3)
- [SE1/T4](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/06-testing.md#t4) → [DO1/T4](guide/06-testing.md#t4)
- [SE1/T5](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/06-testing.md#t5) → [DO1/T5](guide/06-testing.md#t5)
- [SE1/T6](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/06-testing.md#t6) → [DO1/T6](guide/06-testing.md#t6)
- [SE1/T8](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/06-testing.md#t8) → [DO1/T7](guide/06-testing.md#t7)
- [SE1/T9](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/06-testing.md#t9) → [DO1/T8](guide/06-testing.md#t8)
- [SE1/T10](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/06-testing.md#t10) → [DO1/T9](guide/06-testing.md#t9)
- [SE1/T11](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/06-testing.md#t11) → [DO1/T10](guide/06-testing.md#t10)
- [SE1/O1](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/07-operating.md#o1) → [DO1/O1](guide/07-operating.md#o1)
- [SE1/O2](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/07-operating.md#o2) → [DO1/O1](guide/07-operating.md#o1)
- [SE1/O3](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/07-operating.md#o3) → [DO1/O2](guide/07-operating.md#o2)
- [SE1/O4](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/07-operating.md#o4) → [DO1/O3](guide/07-operating.md#o3)
- [SE1/O5](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/07-operating.md#o5) → [DO1/O4](guide/07-operating.md#o4)
- [SE1/O6](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/07-operating.md#o6) → [DO1/O5](guide/07-operating.md#o5)
- [SE1/O7](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/07-operating.md#o7) → [DO1/O7](guide/07-operating.md#o7)
- [SE1/O8](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/07-operating.md#o8) → [DO1/O8](guide/07-operating.md#o8), [DO1/O9](guide/07-operating.md#o9)
- [SE1/O9](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/07-operating.md#o9) → [DO1/O10](guide/07-operating.md#o10)
- [SE1/O10](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/07-operating.md#o10) → [DO1/O11](guide/07-operating.md#o11)
- [SE1/O11](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/07-operating.md#o11) → [DO1/O12](guide/07-operating.md#o12)
- [SE1/W1](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/08-work.md#w1) → [DO1/W1](guide/08-work.md#w1)
- [SE1/W4](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/08-work.md#w4) → [DO1/W2](guide/08-work.md#w2)
- [SE1/W6](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/08-work.md#w6) → [DO1/W3](guide/08-work.md#w3)
- [SE1/W7](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/08-work.md#w7) → [DO1/W4](guide/08-work.md#w4)
- [SE1/W10](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/08-work.md#w10) → [DO1/W5](guide/08-work.md#w5)
- [SE1/W11](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/08-work.md#w11) → [DO1/W6](guide/08-work.md#w6)
- [SE1/W12](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/08-work.md#w12) → [DO1/W9](guide/08-work.md#w9)
- [SE1/W13](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/08-work.md#w13) → [DO1/W7](guide/08-work.md#w7)
- [SE1/H1](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/09-people.md#h1) → [DO1/H1](guide/09-people.md#h1)
- [SE1/H2](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/09-people.md#h2) → [DO1/H2](guide/09-people.md#h2)
- [SE1/H3](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/09-people.md#h3) → [DO1/H3](guide/09-people.md#h3)
- [SE1/H5](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/09-people.md#h5) → [DO1/H7](guide/09-people.md#h7)
- [SE1/H6](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/09-people.md#h6) → [DO1/H7](guide/09-people.md#h7)
- [SE1/H8](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/09-people.md#h8) → [DO1/H8](guide/09-people.md#h8)
- [SE1/H9](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/09-people.md#h9) → [DO1/H6](guide/09-people.md#h6)
- [SE1/H11](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/09-people.md#h11) → [DO1/H4](guide/09-people.md#h4)
- [SE1/H12](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/09-people.md#h12) → [DO1/H5](guide/09-people.md#h5)
- [SE1/A1](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/10-agents.md#a1) → [DO1/A1](guide/10-agents.md#a1)
- [SE1/A2](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/10-agents.md#a2) → [DO1/A2](guide/10-agents.md#a2)
- [SE1/A3](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/10-agents.md#a3) → [DO1/A7](guide/10-agents.md#a7)
- [SE1/A5](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/10-agents.md#a5) → [DO1/A5](guide/10-agents.md#a5)
- [SE1/A7](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/10-agents.md#a7) → [DO1/A6](guide/10-agents.md#a6)
- [SE1/A9](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/10-agents.md#a9) → [DO1/A8](guide/10-agents.md#a8)
- [SE1/A11](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/10-agents.md#a11) → [DO1/A11](guide/10-agents.md#a11)

## Twelve questions

If you remember nothing else, each of these can be asked out loud, and each has an answer.

| Ask                                                                    | Checks |
| ---------------------------------------------------------------------- | ------ |
| "When this is deployed and broken, who finds out first — us, or a user?" | [DO1/P2](guide/01-purpose.md#p2) |
| "If this had stopped reporting an hour ago, what would the screen look like?" | [DO1/U2](guide/02-unknowing.md#u2) |
| "When did something last compare what is running with what the repository says?" | [DO1/U3](guide/02-unknowing.md#u3) |
| "How old is the newest proof that this works?"                         | [DO1/U6](guide/02-unknowing.md#u6) |
| "If this deploy breaks, how many changes will we be choosing between?" | [DO1/F6](guide/03-feedback.md#f6) |
| "How much of this month's budget is left, and what are we spending it on?" | [DO1/R3](guide/04-requirements.md#r3) |
| "If we lost this now, how much is gone, how long until we are back — and when was that timed?" | [DO1/R4](guide/04-requirements.md#r4) |
| "What is the most this can cost in a month, and what happens when it gets there?" | [DO1/R10](guide/04-requirements.md#r10) |
| "Is what is running in production the same bytes that passed the tests?" | [DO1/D7](guide/05-design.md#d7) |
| "Which commit is running right now — and how long did it take to find out?" | [DO1/D13](guide/05-design.md#d13) |
| "Could the same command, or the same stolen key, delete both the data and its backup?" | [DO1/O13](guide/07-operating.md#o13) |
| "What could this session do to production right now, if it tried?"     | [DO1/A3](guide/10-agents.md#a3) |

## Changing this guide

The guide follows its own advice. Run `python3 -I tools/check.py` before committing: it fails if a
link or anchor is broken, a principle has no *New* or *Reframes* line, an SE1 citation points at the
wrong place, or the index or the SE1 map above no longer matches the chapters. It also checks that
every SE1 ID and anchor exists in `vendor/se1`, that `vendor/se1` is the release this guide is
pinned to, and that the copies of SE1's files here equal it. `python3 -I tools/check.py --print`
prints the index and the SE1 map as they should read, to paste in after adding or rewording a
principle. Reword freely; never reuse an ID for a different idea. `python3 -I tools/test_check.py`
plants a breakage per rule in a copy of this repository. CI runs both on three platforms and two
Pythons. Python 3.9 or later and git are all any of it needs.

**This guide is pinned to one SE1 release**, named once, by `SE_RELEASE` in `tools/check.py`, and
every SE1 link, llms.txt, and CI name it: a reader lands on the text that was read, not on whatever
SE1's `main` says now. Bumping it is that one edit, a rewrite of the links, a fresh copy of
`vendor/se1`, a walk through SE1's CHANGELOG for every ID this guide reframes, and a CHANGELOG entry
here saying what was reframed, declined, or confirmed, by SE1 ID.

## Why to trust this, and why to doubt it

Who made it, when, and what would show it wrong, as [SE1/U9](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/02-unknowing.md#u9) asks of any rule.

- **Who, and when.** Drafted on 2026-11-19 by Claude Fable 5.2 with Warren MacEvoy, in a single
  session, at his request to make a guide like [Data Mining](https://github.com/wmacevoy/data-mining)
  (`DM1`) for DevOps. The agent read SE1 v1.7's principles in their one-line form, DM1 v1.4's
  README, working agreement, checker, and parts of two chapters, and the DevOps course repository,
  then wrote every principle here. The author had not read the result when it was first published. It is `v1.0 —
  unreleased` until he has, and signs it.
- **What it rests on.** SE1's principles, by reference. The shape of DM1. The course repository's
  examples. And the agent's recollection of the operations literature listed in
  [READING.md](READING.md), **none of which was opened and checked during the drafting**: the public
  incidents the chapters mention are described from memory, and each should be checked against its
  source before it is relied on ([GAPS 3](GAPS.md#3-nothing-cited-was-opened)).
- **What has been seen to fail.** `tools/check.py`, on a breakage planted per rule
  (`tools/test_check.py`). That shows the documents agree with each other and with SE1's IDs. It
  says nothing about whether any principle is true.
- **What has not.** Everything else. No project has adopted this guide. No runbook, inventory, or
  operating contract has been filled in from its templates. No agent has worked with AGENTS.md
  loaded. No person, and no other model family, has reviewed a line. Whether each *Reframes* is
  faithful to the SE1 principle it names was judged by one model from SE1's quick reference, not
  from the chapters. There is no doctor script for operational work, where DM1 has one.
- **What would show it wrong.** A course project where following a principle here made the system
  less reliable, or cost more than it caught. A reframing that contradicts its SE1 source about
  software, which by the section below is a bug here. An incident described here that its source
  describes differently. An agent, of any family, for which the working agreement makes the work
  worse.
- **How to check it yourself.** `python3 -I tools/check.py`, which reads SE1 from `vendor/se1`; the
  SE1 map above, read against SE1's text at v1.7; and [GAPS.md](GAPS.md), which lists what is known
  to be thin.

## Where this came from

Written for the [DevOps course](https://github.com/wmacevoy/devops-wmacevoy), whose repository is a
set of worked examples and laptop setup guides, and organized to follow SE1's chapters and principle
IDs, on the pattern DM1 set. See [READING.md](READING.md) for the sources the arguments lean on.

**This repository is the authority for these principles** ([SE1/D6](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/05-design.md#d6)). **SE1 is the authority for
everything this guide reframes**: where a DO1 principle and the SE1 principle it reframes seem to
disagree about software, SE1 wins, and the DO1 principle has a bug.

Licensed under the [MIT License](LICENSE).
