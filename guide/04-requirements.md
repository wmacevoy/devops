# 4. Requirements

The promises a running system makes: how often it may be down, how fast it comes back, how much it
may lose, what it may cost, and what nobody thought to ask. Read with
[SE1 chapter 4](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/04-requirements.md); operating requirements go in the same versioned
document as the rest, or in the operating contract beside it
([template](../templates/OPERATIONS_v1.md)).

---

### <a id="r1"></a>DO1/R1. Keep the pain; test the tool

*Reframes [SE1/R1](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/04-requirements.md#r1).*

SE1/R1: a request arrives as a pain plus a guess at its cure, confident enough to trigger no
investigation. Operations receives the guess with a product name on it: "we need Kubernetes," "it has
to be multi-region," "put it behind a CDN," "it can never go down."

Walk back to the pain. *What happened, on which day, and who got the call?* For the stadium it was
one evening: the signs went dark forty minutes before gates and stayed dark, because the one person
who could restart the collector was in the stands. Nothing in that story asks for an orchestrator.
It asks for a restart that needs no particular person, and a signal that reaches someone.

The guess may still be right. Test it against the pain like any other design, and against what it
will cost to carry ([DO1/D8](05-design.md#d8)).

> **Ask:** "What happened the last time this went wrong — and would the tool have changed it?"

### <a id="r2"></a>DO1/R2. Specific and wrong: write the operating numbers down so someone can object

*Reframes [SE1/R2](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/04-requirements.md#r2).*

SE1/R2: draft to be wrong cheaply; "responsive and up to date" gets nodded at. So does "highly
available," "secure," and "scalable." Nobody can object to them, which means nobody has agreed to
anything.

Write the numbers, with invented ones marked as invented:

- *From four hours before gates until an hour after the event ends, drivers see a count no older
  than thirty seconds, for at least 99.9% of requests. (invented)*
- *Outside those windows the service may be down for up to an hour, with notice.*
- *After losing the database, service is back within thirty minutes, missing at most five minutes
  of readings. (invented)*

The client will correct the second line immediately, and that correction is the requirement. Where
what matters is a judgment, write who judges it ([DO1/U9](02-unknowing.md#u9)).

> **Ask:** "Which of these numbers did the client give, and which did we make up?"

### <a id="r3"></a>DO1/R3. Availability is a budget: spend it on change

*Reframes [SE1/R7](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/04-requirements.md#r7).*

SE1/R7: availability is a budget with a period and a beneficiary, and unstated it is 100%, which
forbids deploys. Do the arithmetic. 99.9% of a thirty-day month allows forty-three minutes of
failure. 99% allows seven hours. 99.99% allows four minutes, less than one careful rollback.

The number left over is an **error budget**, and it is there to be spent. Every deploy, migration,
and experiment draws on it. While there is budget, ship. When it is gone, the next thing shipped is
whatever stops the spending. That turns the old argument between shipping and stability into one
number both sides can read.

Say *when* it counts. A stadium does not need four nines on a Tuesday in February; it needs the two
hours before gates, about ten times a month. A budget averaged over the calendar hides exactly the
hours that matter.

> **Ask:** "How much of this month's budget is left, and what are we going to spend it on?"

### <a id="r4"></a>DO1/R4. Recovery is two numbers: how much may be lost, and how long until it is back

*Reframes [SE1/R7](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/04-requirements.md#r7).*

SE1/R7 also asks what happens while it is down, and who gets it back, by which command, in how long:
real only if run and timed. Operations splits that into two numbers that are bought separately.

**How much data may be lost** sets how often you copy it: a nightly backup is a promise to lose up
to a day. **How long until service returns** sets how you restore: from a tape, from a snapshot, from
a replica already running. Tightening either is a purchase ([DO1/R7](04-requirements.md#r7)).

Then give each piece of state its own answer ([DO1/D1](05-design.md#d1)). In the running
example, a few minutes of readings can be lost with a shrug. The device registry cannot be lost at
all: without it no reading, past or future, can be checked.

Neither number is a requirement until someone has restored from the backup and looked at a clock
([DO1/T3](06-testing.md#t3)).

> **Ask:** "If we lost this now, how much would be gone, how long until we are back — and when was
> that last timed?"

### <a id="r5"></a>DO1/R5. Every platform dependency can be absent, late, wrong, or lying

*Reframes [SE1/R6](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/04-requirements.md#r6).*

SE1/R6 asks, per input: what if it is absent, late, wrong, or lying? The system's operational inputs
are easy to forget because they sit under the code: DNS, the container registry, the package mirror,
the certificate authority, the cloud provider's control API, the CI service, the secrets store, the
identity provider that logs your own operators in.

Ask the four questions of each. The registry is **absent**: can you deploy? Can you roll back, or
did that need a pull too? The package mirror is **late**: does the build hang or fail? The base
image tag is **wrong**, pointing at a new build since yesterday ([DO1/O1](07-operating.md#o1)).
The provider's status page is **lying**, green while your region fails. A model the system calls is
one of these inputs, and it fails by answering plausibly: give it a fallback that is not a model, decided
with the client and written in the requirements, and test the switch ([DM1/O5](https://github.com/wmacevoy/data-mining/blob/v1.4/guide/07-operating.md#o5)).

Unstated, the answer is whatever the tool does by default, discovered during an incident that the
same outage caused.

> **Ask:** "Which outside service must be up for us to deploy — and to roll back?"

### <a id="r6"></a>DO1/R6. Ask what nobody volunteers: who is paged, the worst day, the freeze, the ceiling

*Reframes [SE1/R3](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/04-requirements.md#r3).*

SE1/R3 lists what no client volunteers. The operating list, to ask in the first conversation:

- **Who gets the call** when it breaks at night, and are they able to fix it?
- **The worst day**: a sold-out final, in rain, with the cell network saturated.
- **When it must not change**: the hours or weeks in which a deploy is forbidden.
- **When it may be down**: a real maintenance window, or none.
- **The ceiling on the bill**, per month, and who is told when it is near
  ([DO1/R10](04-requirements.md#r10)).
- **Where the data may live**: which country, whose account, who may log in.
- **Who else is touched**: the sign vendor, the network team, whoever owns the DNS.
- **How it ends**: who runs it in two years, and how it is shut down.

Ask first the ones that could invalidate the plan. "It runs on the stadium's network, which is
closed to the internet" is an answer that redesigns the pipeline.

> **Ask:** "Who is paged — and did anyone ask them?"

### <a id="r7"></a>DO1/R7. Every nine is a purchase someone must authorize

*Reframes [SE1/R4](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/04-requirements.md#r4).*

SE1/R4: every sentence in a requirements document is a purchase. Reliability is the clearest case,
because the price list is steep. As a rule of thumb, not a measurement, each added nine costs
several times the one before: a second instance, then a second zone, then a second region, then
someone awake.

So "it must never go down" gets SE1's answer: *yes, and here is what it moves.* Lay out two or three
tiers with a monthly cost and what each one survives — a process crash, a machine, a zone — and let
the person who pays choose. Shown the list, a client can buy less than they first asked for, on
purpose, and know what they bought.

The purchase includes the carry: a standby nobody has failed over to is a cost with no benefit
([DO1/T3](06-testing.md#t3)).

> **Ask:** "What does the next nine cost per month, and who agreed to pay it?"

### <a id="r8"></a>DO1/R8. The pipeline is production: it holds every key

*Reframes [SE1/R8](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/04-requirements.md#r8).*

SE1/R8: security, law, and ethics are requirements on every project, and their absence looks like
presence. Operations adds a target SE1's threat model can miss, because it is not part of the
product: the path itself.

The CI system can push images, read every secret, and deploy to production. A dependency's install
script runs with those rights. So does a third-party pipeline step, pinned to a tag its author can
move. Whoever compromises any of them does not need to attack the service; they are already inside
it, with a legitimate deploy.

So the threat model covers the pipeline, the registry, the base images, and the operators' laptops
([DO1/H9](09-people.md#h9)), and the requirements say who may deploy, from where, with
what review ([DO1/O9](07-operating.md#o9), [DO1/D13](05-design.md#d13)).

> **Ask:** "Who, or what, can put code into production without a second person seeing it?"

### <a id="r9"></a>DO1/R9. Write down what you do not operate

*Reframes [SE1/R9](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/04-requirements.md#r9).*

SE1/R9: four lists, nearly free — scope decisions, non-goals, not yet answered, deferred. The
operating non-goals are the ones that end the most expensive arguments:

- *Not multi-region. A regional outage is an outage.*
- *No zero-downtime deploys outside event windows; a thirty-second blip is accepted.*
- *Nobody is on call between events. Alerts go to email.*
- *Old readings are not kept past two seasons.*
- *We do not run the sign hardware; its vendor does.*

Each is a decision somebody would otherwise assume the other way, and discover during an incident.
Each also dates: beside it, write what would reopen it.

> **Ask:** "What will someone assume we handle, that we have decided not to?"

### <a id="r10"></a>DO1/R10. Cost is a requirement, with a ceiling and an alarm

*New.*

SE1 counts resources among the four constraints and leaves it there, because a program's cost is
mostly the people writing it. A deployed system has a second cost with a property SE1 never needs to
name: **it accrues while nobody is doing anything**, and it responds to inputs you do not control. A
retry loop, a log level left at debug, a forgotten test environment, a crawler, an attacker: each
turns into money, at a rate set by someone else's price list.

So treat the bill as an input that can be wrong or lying ([DO1/R5](04-requirements.md#r5)).
Write the ceiling into the requirements. Set an alarm well below it, and see it fire
([DO1/T3](06-testing.md#t3)). Tag every resource with what it is for, so the bill can be read.
And decide in advance what happens at the ceiling: for most small systems, *it stops* is a better
answer than *it keeps charging*.

> **Ask:** "What is the most this can cost in a month — and what happens when it gets there?"

---

Next: [5. Design](05-design.md) — infrastructure as state, and the repository as its authority.
