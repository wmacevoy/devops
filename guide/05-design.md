# 5. Design

Infrastructure as state and caches; the repository as the authority and what runs as derived; one
artifact through every environment; what each one-way door looks like in a cloud account. Read with
[SE1 chapter 5](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/05-design.md).

---

### <a id="d1"></a>DO1/D1. Infrastructure is state: separate what you can rebuild from what you must keep

*Reframes [SE1/D1](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/05-design.md#d1).*

SE1/D1: the code is a description; what runs is state plus the rules that change it, and difficulty
is how much state a change can reach. In operations nearly everything is state: machines, volumes,
DNS records, firewall rules, queues, the contents of a registry.

Sort it into two piles. **Rebuildable** state can be thrown away and derived again from the
repository: containers, images, most configuration. **Kept** state cannot: the readings, the device
registry, the secrets, the certificate's private key. The whole design effort is to make the first
pile as large as possible and the second as small and as clearly marked as possible.

Write the second pile down ([inventory template](../templates/INVENTORY.md)): where each piece
lives, who owns it, how it is backed up, and what the user sees while it is being restored
([DO1/R4](04-requirements.md#r4)). Anything in neither pile on paper is in the second in practice.

> **Ask:** "If we deleted everything we can rebuild, what would be left — and is that list written
> down?"

### <a id="d2"></a>DO1/D2. An image tag, a build cache, a DNS record, and a deployed config are caches

*Reframes [SE1/D2](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/05-design.md#d2).*

SE1/D2: a cache is any copy that might not match the original; naming it one asks the right
questions. Operations runs on copies. An image is a cache of a build. A tag is a cache of which image
is meant. A CI cache is a copy of last week's dependencies. A DNS record is cached by resolvers you
will never meet, for as long as its lifetime says. The configuration inside a running container is a
copy of what the repository said at deploy time.

Ask of each: how stale can it get, how would anyone know, and what happens meanwhile? `latest` is
the standard answer of "arbitrarily, nobody, and something different on each machine"
([DO1/O1](07-operating.md#o1)). A DNS lifetime of a day means a day in which you cannot
finish moving.

> **Ask:** "What is this a copy of, and how would we know it had gone stale?"

### <a id="d3"></a>DO1/D3. There is no single "now" in a deploy: two versions run at once, and desired is not actual

*Reframes [SE1/D3](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/05-design.md#d3).*

SE1/D3: once state lives in two places, each copy is locally correct and they disagree. A deploy is
that condition on purpose. For its duration, old and new code serve side by side, against one
database, behind one name ([DO1/O7](07-operating.md#o7)).

There is a second pair that disagrees all the time: what you **declared** and what is **running**.
An orchestrator narrows that gap continuously and never closes it. "The deploy returned" means the
request was accepted, not that the new version is serving ([DO1/P2](01-purpose.md#p2)).

So ask SE1's question of both pairs: not *how do we make them equal* but *how different may they be,
for how long, and who is told*. And remember what a half-finished deploy leaves behind. A release
that reached seven of eight servers is not seven-eighths of a release. It is a new system nobody
designed.

> **Ask:** "While this is rolling out, what do the two versions disagree about?"

### <a id="d4"></a>DO1/D4. Make every operation idempotent: declare the state, do not script the steps

*Reframes [SE1/D4](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/05-design.md#d4).*

SE1/D4: make doing it twice equal doing it once, so "did that go through?" stops mattering. Every
operational script is run twice sooner or later: after a timeout, after a dropped connection, by a
second person who was not sure the first had finished.

A script of steps — *create the bucket, add the rule, start the service* — fails on the second run,
or worse, half-succeeds. A statement of the end state — *this bucket exists, with this rule* — can be
applied any number of times, and applying it is also how you repair drift
([DO1/U3](02-unknowing.md#u3)). Where a tool cannot converge, write the check before the
action: *if it is not there, create it*.

The test costs nothing. Run the setup twice in a row. The second run should change nothing and say
so.

> **Ask:** "What happens if this runs twice? Has anyone tried?"

### <a id="d5"></a>DO1/D5. Schedule in UTC, log in UTC, and know which clock expires your credentials

*Reframes [SE1/D5](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/05-design.md#d5).*

SE1/D5: wall-clock says when; a monotonic counter says how long; record past moments in UTC and
decide what a day is. Operations meets clocks in four places.

**Scheduled jobs** run in some zone. A nightly job set for 02:30 local time runs twice one night a
year and not at all on another. **Logs** from several machines can be put in order only if every
line carries UTC and the machines' clocks are kept in step. **Credentials** expire by a clock: a
host whose clock has drifted rejects valid tokens, or accepts expired ones. And **windows** — the
freeze before gates, the maintenance hour — are local times in a named zone, which is the one case
where local is right.

Stadium events are scheduled in local time and watched from machines that think in UTC. Write the
zone beside every time a human typed.

> **Ask:** "In which time zone does this job run, and what happens on the two odd days?"

### <a id="d6"></a>DO1/D6. The repository is the authority; what runs is derived from it by one command

*Reframes [SE1/D6](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/05-design.md#d6).*

SE1/D6: anything computed cannot go stale; a value that exists twice needs a sentence saying which
wins and that a mismatch is a fault. For operations the sentence is: **the repository wins.** The
machines, the pipeline, the dashboards, and the alerts are computed from it, by a command anyone can
run.

That has consequences people resist. A fix made in the console is not a fix until it is in the
repository; until then it is drift with a countdown ([DO1/U3](02-unknowing.md#u3)). A
generated file — a compose file expanded from a template, a rendered manifest — is never edited, and
ideally never committed. And the mismatch between declared and running is checked by machine, and
treated as a fault to explain rather than a state to tolerate.

What cannot live in the repository in the clear, the secrets, is referred to there by name
([DO1/O8](07-operating.md#o8)).

> **Ask:** "If the repository and the running system disagree, which one is wrong — and how would we
> find out that they do?"

### <a id="d7"></a>DO1/D7. Build once, configure at the edge, and promote the same artifact

*Reframes [SE1/D7](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/05-design.md#d7).*

SE1/D7: keep the code that touches the world to a few thin lines at the edges, and the logic in
functions that take values and return values. Lift that one level. The **artifact**, an image named
by its digest, is the pure part: the same bytes on a laptop, in CI, in staging, in production. The
**configuration** is the edge: the handful of values that differ between environments, passed in
from outside when the container starts.

The rule that follows is that nothing is rebuilt between environments. An image rebuilt "for
production" is a different program that has never been tested. And nothing environment-specific is
baked in: no production hostname in the image, no `if staging` in the code.

Keep the edge thin. Every value that differs between environments is a way for them to behave
differently ([DO1/U4](02-unknowing.md#u4)), so the list should be short enough to read aloud.

> **Ask:** "Is the thing running in production the same bytes that passed the tests?"

### <a id="d8"></a>DO1/D8. A platform's complexity must be paid for by a requirement

*Reframes [SE1/D8](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/05-design.md#d8).*

SE1/D8: essential complexity is in the problem; accidental complexity is yours, and it must justify
its carry. Ask SE1's question of each piece of the platform: *what requirement would have to
disappear for this to go away?*

For an orchestrator, a service mesh, a message bus, a second region, the answer is often "none we
have written." They arrive because they are what serious systems are assumed to use, and each brings
its own upgrades, its own failure modes, and its own on-call knowledge
([DO1/D12](05-design.md#d12)).

A single machine running containers under a process supervisor, rebuilt from the repository in ten
minutes, meets the written requirements of many small systems, and can be understood in an afternoon
by whoever is paged. Start there, and add a layer on the day a written requirement cannot be met
without it ([DO1/R1](04-requirements.md#r1)).

> **Ask:** "Which written requirement would fail if we removed this?"

### <a id="d9"></a>DO1/D9. One name for one thing, the same in the repository, the registry, the dashboard, and the alert

*Reframes [SE1/D9](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/05-design.md#d9).*

SE1/D9: one thing, one word; one word, one thing; a new noun is a decision. During an incident a
person moves between six tools in a minute, and the names are the only thread. If the repository
says `collector`, the image is `ingest`, the dashboard says `sensor-svc`, and the alert says
`backend-2`, each hop costs a translation, made under stress, sometimes wrongly.

Pick the names once: services, environments, regions. Use them everywhere, and build the environment
into every resource name — container, network, volume, database — so that two environments can run
side by side and so that nobody can ever be unsure which one a command is about to touch
([DO1/T5](06-testing.md#t5), [DO1/A4](10-agents.md#a4)).

> **Ask:** "If the alert names a service, can I find it in the repository by searching for that
> word?"

### <a id="d10"></a>DO1/D10. A service's contract says how it starts, how it stops, and how it reports its health

*Reframes [SE1/D10](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/05-design.md#d10).*

SE1/D10: a contract that does not say how it fails has said nothing; a 200 is not "current." The
platform and a service have a contract too, and most outages during deploys are breaches of it.

- **Starting:** it refuses to start if its configuration is incomplete, with an exit code and a line
  saying what is missing ([DO1/T2](06-testing.md#t2)).
- **Ready:** it says when it can take traffic, which is later than when the process exists.
- **Healthy:** the check exercises something real. A health endpoint that returns 200 from a process
  that cannot reach its database is lying on the service's behalf.
- **Stopping:** asked to stop, it finishes what it is doing, within a stated time, and does not
  accept more.

Each has a failure worth a sentence: a liveness check that depends on the database turns one
database blip into every instance being restarted at once.

> **Ask:** "What does 'healthy' check here — and can it be true while users are failing?"

### <a id="d11"></a>DO1/D11. One-way doors in infrastructure: a name, a region, a data store, an account, a published tag

*Reframes [SE1/D11](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/05-design.md#d11).*

SE1/D11: renaming a variable is free; a format with data in it and a published URL are one-way
doors. The infrastructure list:

- **A public name**: a domain, an API host, a URL printed on a sign.
- **Where the data lives**: region, provider, the kind of database. Moving is a migration with the
  system running.
- **The account and network layout**, which everything else is addressed inside.
- **A published image tag or package version** that someone else has pulled.
- **A key that has been distributed**: to a hundred sensors in a hundred parking lots.
- **Anything deleted** that had no backup.

These get the small waterfall SE1 prescribes: design on paper, a reviewer who does not share your
assumptions, done once ([DO1/W2](08-work.md#w2)). Everything else in the platform should be
arranged so that it is *not* on this list, and can be changed on a Tuesday.

> **Ask:** "Which of these choices could we reverse next month, and what would it cost?"

### <a id="d12"></a>DO1/D12. Everything running is carried: each service, job, dashboard, and alert has a holder and a bill

*Reframes [SE1/D12](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/05-design.md#d12).*

SE1/D12: cost is build plus carry times lifetime, and deletion is the highest-return work nobody
schedules. In operations the carry is unusually visible, if anyone looks: it is on the invoice, in
the list of unpatched vulnerabilities, and in the alert channel.

Every running thing needs a name beside it. A cron job nobody owns still runs. A dashboard nobody
reads still costs someone the minute it takes to decide it is irrelevant, during an incident. An
alert nobody acts on trains people to ignore the ones that matter ([DO1/T4](06-testing.md#t4)).
A dependency in a base image is patched by you, forever.

So ask each iteration what can be turned off, and make turning off safe: stop it before deleting it,
and wait to see who notices.

> **Ask:** "Who would notice if we turned this off — and if nobody, why is it running?"

### <a id="d13"></a>DO1/D13. Every running thing can say which commit it is

*Reframes [SE1/D13](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/05-design.md#d13).*

SE1/D13: evidence is "signed by a key in the registry, here is the check," not a feeling. For a
running system the first piece of evidence is identity. *What exactly is serving traffic right now?*
should have an answer that takes one command: this commit, built by this pipeline run, as this image
digest, deployed at this time, by this person or merge.

Carry it in three places: stamped into the artifact, reported by the running service at an endpoint,
and written to a deploy log that nobody can edit. Then diagnosis starts from "what changed" as a
query instead of a discussion, and a rollback has a named destination.

A signature on the artifact adds what SE1 says a signature adds: who built it and that it was not
altered. It does not say the build was good, and it is worth only as much as the check that refuses
an unsigned one, which someone should have watched refuse ([DO1/R8](04-requirements.md#r8)).

> **Ask:** "Which commit is running in production right now — and how long did it take to find
> out?"

### <a id="d14"></a>DO1/D14. Replace, do not repair

*New.*

SE1 has no machines in it, so it does not say how to change one. There are two ways. You can log in
and alter what is there: install the patch, edit the file, restart the process. Or you can build a
new one from the description and discard the old.

Repair is faster the first time and it is how drift is made ([DO1/U3](02-unknowing.md#u3)):
every repair is a change that exists only on that machine, and after enough of them no two machines
match and none matches the repository. Replacement forces the change through the description, so the
fix is recorded as a side effect of being applied. It also exercises the rebuild path constantly,
which is the path a disaster will need ([DO1/T11](06-testing.md#t11)).

When repair is the only thing quick enough, during an incident, do it, and write the queue item for
the replacement before closing the incident.

> **Ask:** "Was this fixed in the repository, or only on the machine?"

---

Next: [6. Testing](06-testing.md) — checks worth trusting, for the path and the platform.
