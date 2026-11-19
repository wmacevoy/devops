# 2. Unknowing

Why a running system manufactures erroneous certainty: green is its default color, the repository
and the machine drift apart, and things expire on dates nobody chose to remember. Read with
[SE1 chapter 2](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/02-unknowing.md), which defines K0 to K4.

---

### <a id="u1"></a>DO1/U1. Operating is discovering, before production does it for you, which category each part of the system belongs to

*Reframes [SE1/U2](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/02-unknowing.md#u2).*

SE1/U2: every practice moves something toward K0 before it charges full price. For a running system
the categories look like this:

| Category | In operations                                                    | In the running example                                   |
| -------- | ---------------------------------------------------------------- | -------------------------------------------------------- |
| **K0**   | what a check observed, recently, and could have failed           | the restore ran last Tuesday and took eleven minutes     |
| **K1**   | what you know to look up before it bites                         | the cloud account's quota for load balancers             |
| **K2**   | what production does that no other environment has shown you     | a hundred real sensors reconnecting at once after a power dip |
| **K3**   | what everyone believes because nothing has said otherwise        | "the nightly backup works"                               |
| **K4**   | a falsehood you chose in order to build at all                   | "the region will be up"                                  |

Most operational practice is one of two moves: converting K3 to K0 by making a thing fail on purpose
([DO1/T3](06-testing.md#t3)), and meeting K2 early, in small pieces, by sending
real traffic to small changes ([DO1/F6](03-feedback.md#f6)).

> **Ask:** "Which of these do we know because we watched it, and which because nobody has
> complained?"

### <a id="u2"></a>DO1/U2. Production says "fine" by default: silence is not health

*Reframes [SE1/U3](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/02-unknowing.md#u3).*

SE1/U3: K3 costs nothing at the time, because no search starts. Operations is where that is built
into the tools. A dashboard with no data draws a flat, calm line. An alert whose query matches
nothing never fires. A health check that was never wired to the load balancer is never red. A log
pipeline that fell over stops delivering errors, which reads as *no errors*.

In every case the absence of a signal is displayed as the presence of health, and the more broken
the monitoring, the better things look.

So a signal is trusted only after it has been seen to go red, and "no data" is a state of its own,
shown differently from "zero" and alerted on ([DO1/O3](07-operating.md#o3)).

> **Ask:** "If this had stopped reporting an hour ago, what would the screen look like right now?"

### <a id="u3"></a>DO1/U3. Drift is the canonical K3: the repository says one thing and the machine runs another

*New.*

Every K3 factory in SE1 produces a belief that is wrong. Drift produces one that *was right*, which
is why it is so well defended. The repository describes the firewall, the instance size, the
environment variables. Someone changed one by hand during the first game, in the console, to stop
the bleeding. It worked, so nobody wrote it down.

From then on every reader of the repository is wrong with complete confidence, and the next rebuild
quietly reverts the fix. Nothing fails at the moment of drift. It fails on the day you most need the
description to be true: a restore, a second region, a new teammate.

Drift is found only by comparing what is declared with what is running, by machine, on a schedule
([DO1/D6](05-design.md#d6)), and it is prevented by making the by-hand change
harder than the right one ([DO1/D14](05-design.md#d14)).

> **Ask:** "When did something last compare what is running with what the repository says?"

### <a id="u4"></a>DO1/U4. Every environment is a chosen falsehood about production; say where it lies

*Reframes [SE1/U4](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/02-unknowing.md#u4).*

SE1/U4: nothing gets built without falsehoods; choose them deliberately and write where each is
load-bearing. A laptop stack and a staging environment are K4 about production: smaller data, fewer
nodes, simulated sensors, no real traffic, a different network, credentials with fewer limits.

That is fine and necessary. What is not fine is "it passed in staging" said as if staging were
production. For each environment write the list of ways it differs, and beside each difference, the
class of failure it therefore cannot show you ([DO1/T6](06-testing.md#t6)). Ten
simulated lots will never show what a hundred real ones do to the connection pool.

> **Ask:** "What can break in production that this environment is unable to show us?"

### <a id="u5"></a>DO1/U5. What only the last operator knows is K3 for them and K2 for whoever is paged next

*Reframes [SE1/U6](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/02-unknowing.md#u6).*

SE1/U6: a constraint in one head is K3 for its holder and K2 for everyone else. Operations
concentrates this more than code does, because operating happens outside the repository: in a
console, in a terminal on one laptop, in the memory of what worked last time.

The signs: one person who "does the deploys"; a step everybody knows you have to do first; a
credential that exists in one home directory; a cloud resource nobody can explain but nobody will
delete. Each is fine until that person is on a plane.

Put it where the next operator starts: the runbook, the inventory, a script
([DO1/W5](08-work.md#w5)). The cheap test is to have someone else do it while the
expert watches and says nothing ([DO1/H3](09-people.md#h3)).

> **Ask:** "If the person who usually does this were unreachable, what would we be missing?"

### <a id="u6"></a>DO1/U6. A status with no age is a K3 factory: last backup, last restore, last deploy, last rotation

*Reframes [SE1/U7](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/02-unknowing.md#u7).*

SE1/U7: a K3 factory keeps saying "fine" with no way to say "wrong," and one of its forms is a status
with no *since*. "Backups: enabled" is such a status. So are "certificate: valid," "secrets:
rotated," and "runbook: exists."

Give each an age and make the age visible where the status is: the last backup *completed* at, the
last restore *tested* at, the certificate valid *until*, the secret last rotated *on*, the runbook
last *run* on. Then give each age a limit, and alert when it is passed. An enabled backup whose last
success was nineteen days ago is a different fact from the one on the settings page.

> **Ask:** "How old is the newest proof that this works?"

### <a id="u7"></a>DO1/U7. Look at production when nothing is wrong

*Reframes [SE1/U8](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/02-unknowing.md#u8).*

SE1/U8: an agenda finds only what it looks for; spend time with no task on purpose. During an
incident you look at production through a keyhole, hunting one thing. The rest of the time nobody
looks at all, because nothing is paging.

Schedule the unhurried look: list everything that is running and what each costs; read a day of logs
with no question in mind; open the dashboards nobody opens; trace one ordinary request from the
sign to the database. This is where the forgotten test instance, the retry storm that always
recovers, and the disk at eighty percent are found, while they are still cheap.

Unlike in data work, looking here spends nothing. What it finds goes in FINDINGS or the queue.

> **Ask:** "When did someone last look at this system with no ticket open?"

### <a id="u8"></a>DO1/U8. A threshold, a timeout, and a limit are rules: say who set each, when, and what would show it wrong

*Reframes [SE1/U9](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/02-unknowing.md#u9).*

SE1/U9: a rule reads the same whether it was measured, decided, or guessed. Configuration is made of
such rules. `timeout: 30`. `replicas: 3`. Alert at eighty percent. Keep logs fourteen days. Some were
measured under load. Some came from a tutorial. One was typed during an outage and never revisited.
In the file they are indistinguishable.

Beside any number the system leans on, write where it came from — measured on which date, required
by which requirement, or *(guessed)* — and what observation would show it wrong. Doubt the newest
first, and the copied ones always.

> **Ask:** "Was this number measured, decided, or inherited — and from whom?"

### <a id="u9"></a>DO1/U9. "Is it reliable enough?" is a judgment; a service-level number may veto, and only the people who depend on it can pass it

*Reframes [SE1/U10](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/02-unknowing.md#u10).*

SE1/U10: some questions never convert to K0; a measurement near them is a proxy, and treating the
proxy as the answer is K3. "99.9% of requests succeeded" is such a proxy. It can be met in a month
when the service was down for the forty minutes before every gate opened, and missed in a month
nobody noticed.

Keep the number: it can say *no* cheaply and early. But whether the service was good enough is
decided by someone who was depending on it, on a named occasion: the operations manager, after each
event, asked one question ([DO1/R3](04-requirements.md#r3)).

> **Ask:** "Who decides this was good enough — and when were they last asked?"

### <a id="u10"></a>DO1/U10. Everything in production has an expiry date; find it before it finds you

*New.*

Code does not stop working on a calendar date. Operations does, constantly: TLS certificates, API
tokens, signing keys, domain registrations, trial credits, a free tier, a base image's security
support, a language runtime's end of life, a cloud provider's deprecated API version.

These are the purest K2 in the field, because the system works perfectly until the hour it does not,
and nothing about the day before looks different. They are also the cheapest to convert, because
every one of them is knowable in advance.

Keep one list of everything with a date on it, with the date, the owner, and the renewal procedure
([DO1/W5](08-work.md#w5)), and alert well ahead. Where a renewal can be automated,
automate it, and then alert on the *age*, because automated renewals fail silently too
([DO1/U6](02-unknowing.md#u6)).

> **Ask:** "What stops working on a date — and is that date written anywhere but in the thing
> itself?"

---

Next: [3. Feedback](03-feedback.md) — what a test in production costs, and how to make it cheap.
