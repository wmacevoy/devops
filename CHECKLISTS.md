# Checklists

Every checklist in the guide, in the order the work meets them. Each item cites the principle that
explains it; if an item retires no fear in your work, drop it and say why ([DO1/P4](guide/01-purpose.md#p4)). SE1's
[checklists](https://github.com/wmacevoy/software-engineering/blob/v1.7/CHECKLISTS.md) still apply to the service as software; these are the ones a running
system adds.

## Starting

**The first conversation with the client**

- [ ] The pain before the tool: what happened, on which day, who got the call. ([DO1/R1](guide/04-requirements.md#r1))
- [ ] When it matters, and when it does not: the hours, the events, the season. ([DO1/R3](guide/04-requirements.md#r3))
- [ ] Who is told when it breaks at night, and whether they can fix it. ([DO1/R6](guide/04-requirements.md#r6))
- [ ] The worst day, and the days on which nothing may change. ([DO1/R6](guide/04-requirements.md#r6))
- [ ] How much may be lost, and how long it may be gone, for each thing kept. ([DO1/R4](guide/04-requirements.md#r4))
- [ ] The ceiling on the bill, and what happens at it. ([DO1/R10](guide/04-requirements.md#r10))
- [ ] Each assumption read back as a question. ([DO1/H7](guide/09-people.md#h7))

**The first week**

- [ ] An empty page has been through the whole path to production. ([DO1/W3](guide/08-work.md#w3))
- [ ] One command brings the system up on a laptop; one resets it. ([DO1/F2](guide/03-feedback.md#f2))
- [ ] A new laptop's setup is written, and ends in a doctor. ([DO1/H9](guide/09-people.md#h9), [DO1/T2](guide/06-testing.md#t2))
- [ ] The operating contract exists, with invented numbers marked. ([DO1/R2](guide/04-requirements.md#r2))
- [ ] The inventory exists: environments, kept state, secrets by name, dated things. ([DO1/W5](guide/08-work.md#w5))
- [ ] The non-goals are written. ([DO1/R9](guide/04-requirements.md#r9))

## Designing

**A new environment or component**

- [ ] Which written requirement would fail without it? ([DO1/D8](guide/05-design.md#d8))
- [ ] Its state is sorted: rebuildable, or kept and backed up. ([DO1/D1](guide/05-design.md#d1))
- [ ] Its name is the same everywhere, with the environment in it. ([DO1/D9](guide/05-design.md#d9))
- [ ] It is described in the repository and applying the description twice changes nothing.
      ([DO1/D6](guide/05-design.md#d6), [DO1/D4](guide/05-design.md#d4))
- [ ] It says how it starts, when it is ready, what healthy checks, and how it stops. ([DO1/D10](guide/05-design.md#d10))
- [ ] How it differs from production is written beside it. ([DO1/U4](guide/02-unknowing.md#u4))
- [ ] Someone holds it, and it is on the bill under a tag. ([DO1/D12](guide/05-design.md#d12))
- [ ] Any one-way door in it has been named and reviewed. ([DO1/D11](guide/05-design.md#d11))

**A number in configuration**

- [ ] Measured, decided, or guessed, and by whom, is written beside it. ([DO1/U8](guide/02-unknowing.md#u8))

## Building the path

- [ ] The pipeline only calls scripts a person can run. ([DO1/O2](guide/07-operating.md#o2))
- [ ] Images by digest; tools, runners, and third-party steps by exact version. ([DO1/O1](guide/07-operating.md#o1))
- [ ] One artifact, built once, promoted unchanged. ([DO1/D7](guide/05-design.md#d7))
- [ ] The pipeline's identity is scoped to one environment and expires. ([DO1/O9](guide/07-operating.md#o9))
- [ ] Every step has a timeout, and a stated result if the pipeline dies after it. ([DO1/O4](guide/07-operating.md#o4))
- [ ] Steps with no undo are marked and need a second person. ([DO1/O12](guide/07-operating.md#o12))
- [ ] Who or what can deploy without a second person seeing it? ([DO1/R8](guide/04-requirements.md#r8))

## Before a deploy

- [ ] It is one concern. ([DO1/F6](guide/03-feedback.md#f6))
- [ ] The preview has been read, and every destroyed line accounted for. ([DO1/A2](guide/10-agents.md#a2))
- [ ] It is not inside a freeze, and there is budget left. ([DO1/R3](guide/04-requirements.md#r3), [DO1/R6](guide/04-requirements.md#r6))
- [ ] The way back is named, and the previous artifact can still read the data.
      ([DO1/O5](guide/07-operating.md#o5), [DO1/O7](guide/07-operating.md#o7))
- [ ] The signal that would show it broken exists. ([DO1/W6](guide/08-work.md#w6))
- [ ] A few users meet it first, behind a gate seen to refuse. ([DO1/O5](guide/07-operating.md#o5))

## After a deploy

- [ ] A real request through the public name returned the right answer. ([DO1/T7](guide/06-testing.md#t7), [DO1/P2](guide/01-purpose.md#p2))
- [ ] The running commit is the one intended. ([DO1/D13](guide/05-design.md#d13))
- [ ] The inventory, runbooks, and contract it made untrue are updated. ([DO1/W5](guide/08-work.md#w5))

## On a schedule

- [ ] Restore from backup, somewhere separate, timed. ([DO1/T3](guide/06-testing.md#t3), [DO1/R4](guide/04-requirements.md#r4))
- [ ] Compare what is running with what is declared. ([DO1/U3](guide/02-unknowing.md#u3))
- [ ] Fire each paging alert by injecting its fault. ([DO1/T3](guide/06-testing.md#t3))
- [ ] Roll back in production on an ordinary day. ([DO1/T3](guide/06-testing.md#t3))
- [ ] Rotate each secret. ([DO1/O8](guide/07-operating.md#o8))
- [ ] Read the list of dated things. ([DO1/U10](guide/02-unknowing.md#u10))
- [ ] Update the pins. ([DO1/O1](guide/07-operating.md#o1))
- [ ] Look at production with no ticket open; list what is running and what it costs.
      ([DO1/U7](guide/02-unknowing.md#u7), [DO1/D12](guide/05-design.md#d12))
- [ ] Break something on purpose. ([DO1/T9](guide/06-testing.md#t9))
- [ ] Rebuild from an empty account. ([DO1/T11](guide/06-testing.md#t11))
- [ ] Have someone else follow a runbook. ([DO1/H8](guide/09-people.md#h8))
- [ ] Count the pages, the re-runs, and the hours of toil. ([DO1/H4](guide/09-people.md#h4), [DO1/T4](guide/06-testing.md#t4), [DO1/W8](guide/08-work.md#w8))

## In an incident

- [ ] One person leads; another tells the people affected, and says when the next word comes.
      ([DO1/O10](guide/07-operating.md#o10), [DO1/H6](guide/09-people.md#h6))
- [ ] What changed last? ([DO1/O10](guide/07-operating.md#o10))
- [ ] Capture, then mitigate. ([DO1/T8](guide/06-testing.md#t8))
- [ ] The timeline is being written, with times. ([DO1/O10](guide/07-operating.md#o10))
- [ ] It is over when the signal that showed the harm shows it gone. ([DO1/O10](guide/07-operating.md#o10))

## After an incident

- [ ] Written without blame: what made the wrong thing easy. ([DO1/H5](guide/09-people.md#h5))
- [ ] Which cheaper loop could have caught it. ([DO1/F3](guide/03-feedback.md#f3))
- [ ] Each action is a thing that refuses, with an owner and a queue item. ([DO1/W7](guide/08-work.md#w7))

## Handing a task to an agent

- [ ] What the session can reach has been checked, and production is not in it. ([DO1/A3](guide/10-agents.md#a3))
- [ ] What exists outside the repository has been said. ([DO1/A7](guide/10-agents.md#a7))
- [ ] The destructive commands that need a word every time are named. ([DO1/A4](guide/10-agents.md#a4))
- [ ] What may be pasted to a hosted model is decided. ([DO1/A9](guide/10-agents.md#a9))
- [ ] Who applies the change is stated. ([DO1/A6](guide/10-agents.md#a6))
- [ ] The task is on the hand-over side of the line. ([DO1/A8](guide/10-agents.md#a8))
