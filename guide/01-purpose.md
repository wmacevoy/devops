# 1. Purpose

What operations is for, what makes a system worth depending on, and who pays when it is not. Read
with [SE1 chapter 1](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/01-purpose.md), which still applies; this chapter says where the noun
changes.

---

### <a id="p1"></a>DO1/P1. Operations delivers change to a running system and keeps it running, on time, within the budget and the people it has, managing discomfort for the team, the client, and whoever is paged

*Reframes [SE1/P1](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/01-purpose.md#p1).*

Software engineering asks *does the thing work*. Operations asks *is it running, and can we change it
without finding out the hard way*. It is mostly the same discipline pointed at a different noun: not
the code, but **the path** from a change in someone's head to a system that is running, watched, and
restorable. Mostly: the path ends in machines, power, and lead times, which are plain engineering
whether you own them or rent them ([DO1/W10](08-work.md#w10)).

The four constraints of SE1/P1 still trade against each other: reliability, time, resources, and
discomfort. SE1 says scope is the fifth, and usually the honest one to move. Here the honest one is
**the size of the change**. Shipping one thing today instead of twelve things on Friday is not a
compromise. It is the commonest way to get both reliability and speed
([DO1/F6](03-feedback.md#f6)).

Resources here are money that is billed by the hour whether or not anyone is looking
([DO1/R10](04-requirements.md#r10)), and the sleep of whoever carries the pager.

> **Ask:** "Which of these is fixed — and could the change be smaller?"

### <a id="p2"></a>DO1/P2. Deployed is not running, and running is not serving

*Reframes [SE1/P2](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/01-purpose.md#p2).*

A green pipeline is a claim about a build. *Running* is a claim about a process. *Serving* is a claim
about a world that contains expired certificates, a full disk, a DNS record still pointing at last
year's address, and a database that accepted the connection and answers nothing.

SE1/P2 says reliable means it works amid failing parts, *and says so when it does not*. The
operations form: every layer between the commit and the user can report success while the layer
above it is broken. The process is up and the port is closed. The port is open and every request
fails. Every request succeeds and the count on the sign is from Tuesday. Where a model answers, there is
one more: every request gets a fresh, confident forecast that is wrong, and the outcome that would
say so arrives tonight, or never ([DM1/O3](https://github.com/wmacevoy/data-mining/blob/v1.4/guide/07-operating.md#o3)).

So the question is never "did the deploy succeed" but "what did a user get, just now, and how do we
know" ([DO1/O5](07-operating.md#o5), [DO1/D10](05-design.md#d10)).

> **Ask:** "When this is deployed and broken, who finds out first — us, or a driver at the gate?"

### <a id="p3"></a>DO1/P3. Whoever is paged was not in the planning meeting

*Reframes [SE1/P3](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/01-purpose.md#p3).*

SE1/P3: fear degrades judgment, and a frightened team drops exactly the practices that would have
saved it. Operations has its own version of the spiral. A deploy goes badly, so the team deploys
less often. Each deploy now carries more changes, so it goes badly more often, and takes longer to
diagnose when it does. The fear was reasonable at every step and the result is a system nobody dares
touch.

The discomfort also lands unevenly. The decision to ship is made in daylight by several people; the
consequence arrives at night for one. A person who has been woken three nights running is making
tomorrow's decisions with what is left ([DO1/H4](09-people.md#h4)).

Watch for the early signs: a deploy that waits for one particular person, a change "saved for after
the season," a runbook step that says *be careful here*.

> **Ask:** "Who is afraid of this deploy, and what exactly are they afraid of?"

### <a id="p4"></a>DO1/P4. A practice that relieves the fear without reducing the risk is worse than nothing, and operations is full of them

*Reframes [SE1/P4](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/01-purpose.md#p4).*

SE1/P4 names the backup never restored. Operations has a shelf of the same thing: a disaster-recovery
plan that is a PDF; a dashboard that is green because it has no data; an alert routed to a channel
everyone muted; a status page updated by hand, by the person busy fixing the outage; a staging
environment that passes because it has a tenth of the lots and none of the traffic; a security scan
whose findings nobody is assigned to read.

Each spends the alarm and leaves the danger in place. The test is the one SE1 gives: name the
specific fear this retires, then ask when it was last seen doing so
([DO1/T3](06-testing.md#t3)).

> **Ask:** "Which fear does this retire — and when did we last watch it work?"

### <a id="p5"></a>DO1/P5. The deliverable is the running system and the path that rebuilds it, not the server

*Reframes [SE1/P5](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/01-purpose.md#p5).*

SE1/P5: the deliverable is the code plus a stranger's ability to run, reset, deploy, and diagnose it.
For operations the stranger's question is blunter: *if this machine were gone in the morning, what
would you need to get the service back, and where is it written?*

A server someone configured by hand over two years is a working system and a poor deliverable: it
records the result and not the steps, and the steps are the program. Keep the result disposable and
the steps in the repository ([DO1/D6](05-design.md#d6), [DO1/D14](05-design.md#d14)).
What cannot be rebuilt — the readings, the device registry, the keys — is named, backed up, and
restored on a schedule ([DO1/D1](05-design.md#d1), [DO1/R4](04-requirements.md#r4)).

> **Ask:** "Could someone who has never met me rebuild this from the repository and a backup, and
> know it was right?"

### <a id="p6"></a>DO1/P6. Building and running are one loop; a wall between them is where the feedback dies

*New.*

SE1 treats operating as the last chapters of one job. It does not say what happens when the job is
split between two groups, because a team of four students has no such split. Most organizations do,
and the split is the problem this field is named after.

When one group writes and another runs, each optimizes its own half. The writers are measured on
shipping, so they ship. The runners are measured on stability, so they resist change. Every signal
production sends — this query is slow at full size, this log line is useless at three in the
morning, this config needs a restart nobody mentioned — stops at the wall, and the people who could
act on it never hear it.

The cure is not a tool or a job title. It is that whoever can change the system also hears it break,
and whoever is woken by it can change it ([DO1/H1](09-people.md#h1)).

> **Ask:** "When this broke last, did the person who wrote it find out — and how long did that
> take?"

---

Next: [2. Unknowing](02-unknowing.md) — why production says "fine" by default.
