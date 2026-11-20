# Gaps

Where the guide falls short. In SE1 this file holds field reports — what happened when the guide met
a real project — and each entry is closed by changing the guide, or by a sentence saying why not
([SE1/P4](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/01-purpose.md#p4)).

**There are no field reports.** Nothing here has met a project. This file lists where the guide is
known to be thin, so that nobody mistakes silence for coverage.

## 1. Nothing has been tried

Every principle, the working agreement, ADOPT.md, the checklists, and the templates were written in
one session and used by nobody. The first reports are expected from the DevOps course. Until one
arrives, read each principle as a proposal.

## 2. The author had not read it when it was published

The guide was drafted by an agent and published at the author's request before he had read it. It is
unreleased for that reason. A principle he disagrees with is a bug to fix before v1.0 is dated.

## 3. Nothing cited was opened

The sources in READING.md, and the public incidents mentioned in [DO1/T3](guide/06-testing.md#t3), [DO1/O6](guide/07-operating.md#o6), and [DO1/H5](guide/09-people.md#h5),
were described from the drafting model's memory. None was opened and checked. SE1 keeps an
`archive/` of sources with checksums and a note of whether each was read; this guide has none. Each
incident should be checked against its published account, and corrected or removed.

## 4. The reframings were made from one line each

SE1's principles were read in `QUICKREF.md`, one line apiece, plus one chapter's headings. A
reframing may lean on a reading of the SE1 principle that its full text does not support. Each
*Reframes* line should be read against the chapter it links to.

## 5. There is no doctor

DM1 has `dm --doctor`, which checks a repository against its guide. This guide asks for a doctor in
adopting repositories ([DO1/T2](guide/06-testing.md#t2)) and has none of its own. The rows it would need — what a session
can reach, kept state listed, each secret's last rotation, each runbook's last run, every dated
thing — are in the inventory template, checked by nobody.

## 6. One system, one scale

The running example and the course repository are a handful of containers run by a handful of
people. The principles were written with that in mind. What changes with dozens of services and
several teams — platform teams, shared clusters, change management, compliance regimes — is mostly
absent, and [DO1/D8](guide/05-design.md#d8) argues against reaching for it early, which may read as dismissing it.

## 7. Thin by topic

- **Observability** gets parts of [DO1/O3](guide/07-operating.md#o3) and [DO1/F4](guide/03-feedback.md#f4). Tracing, sampling, and what to do with a
  volume of logs too large to read are not covered.
- **Security** is treated as it touches the path: [DO1/R8](guide/04-requirements.md#r8), [DO1/O8](guide/07-operating.md#o8), [DO1/O9](guide/07-operating.md#o9), [DO1/O13](guide/07-operating.md#o13). Network
  design, vulnerability management, and compliance are not.
- **Containers and orchestration** are assumed, not taught. The course repository teaches them.
- **Cost** has one principle, [DO1/R10](guide/04-requirements.md#r10), and no method for reading a bill.
- **Databases** as a thing to operate — replication, failover, upgrades — are named and not
  explained.
- **On-call** has [DO1/H3](guide/09-people.md#h3) and [DO1/H4](guide/09-people.md#h4), written by something that has never been paged.

## 8. The agents chapter is the least grounded

[DO1/A3](guide/10-agents.md#a3), [DO1/A4](guide/10-agents.md#a4), [DO1/A9](guide/10-agents.md#a9), and [DO1/A10](guide/10-agents.md#a10) are new, and rest on no trial. SE1's agent principles
came from trials with agents loaded with its rules. These came from reasoning about what a shell
contains. They are the first candidates for a trial, and for being wrong.

## 9. DM1 is cited in part

DM1 v1.4 was read against this guide once, by one model, and a second model's review named DM1
principles with operational content that no principle here cites yet: [DM1/U6](https://github.com/wmacevoy/data-mining/blob/v1.4/guide/02-unknowing.md#u6) (an interrupted
retrain's checkpoint answers as confidently as a finished model: a torn deploy, [DO1/O4](guide/07-operating.md#o4)),
[DM1/O7](https://github.com/wmacevoy/data-mining/blob/v1.4/guide/07-operating.md#o7) (the way back needs the old model loadable and its feature columns still there, [DO1/O7](guide/07-operating.md#o7)),
[DM1/D9](https://github.com/wmacevoy/data-mining/blob/v1.4/guide/05-design.md#d9) (one feature definition for training and serving is build-once for feature code, [DO1/D7](guide/05-design.md#d7)),
[DM1/O8](https://github.com/wmacevoy/data-mining/blob/v1.4/guide/07-operating.md#o8) (a model trained on personal data is personal data; every copy is in the inventory),
[DM1/A4](https://github.com/wmacevoy/data-mining/blob/v1.4/guide/10-agents.md#a4) (the holdout out of reach of agents and the pipeline, [DO1/A3](guide/10-agents.md#a3)),
[DM1/O3](https://github.com/wmacevoy/data-mining/blob/v1.4/guide/07-operating.md#o3) (whether distribution monitors page anyone, [DO1/O3](guide/07-operating.md#o3)),
[DM1/O11](https://github.com/wmacevoy/data-mining/blob/v1.4/guide/07-operating.md#o11) (the decision log and the random slice are kept state, [DO1/D1](guide/05-design.md#d1)), and retraining time as part of
recovery time ([DO1/R4](guide/04-requirements.md#r4)). The README's DM1 section is kept by hand; the checker does not
derive it.
