# 6. Testing

Checks worth trusting, for the path and the platform: preflights and doctors, backups seen to
restore, environments that do not collide, failures injected on a day you chose. Read with
[SE1 chapter 6](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/06-testing.md), which covers the code.

---

### <a id="t1"></a>DO1/T1. Name what each operational check retires: configuration, status, input, output, state, time, space

*Reframes [SE1/T1](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/06-testing.md#t1).*

SE1/T1 sorts self-tests into seven kinds and asks which fear each retires. The same seven sort every
operational check:

| Kind              | The fear                                     | A check that retires it                              |
| ----------------- | -------------------------------------------- | ---------------------------------------------------- |
| **Configuration** | missing or malformed at boot                 | the service refuses to start and says what is missing |
| **Status**        | broken right now                             | a health check that exercises something real         |
| **Input**         | a dependency absent, late, wrong, or lying   | a probe of the registry, the DNS, the upstream       |
| **Output**        | serving something impossible                 | a synthetic request compared with a known answer     |
| **State**         | drifted from what was declared               | a scheduled comparison of declared and running       |
| **Time**          | expired, or about to                         | an alert on every dated thing, well ahead            |
| **Space**         | a disk, a quota, a pool, a budget running out | a projection of when, at today's rate                |

A check that fits no row is worth a second look. So is a row with no check.

> **Ask:** "Which of the seven does this check cover — and which has nothing?"

### <a id="t2"></a>DO1/T2. A reliable path checks itself: a doctor for the laptop, a preflight for the deploy

*Reframes [SE1/T2](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/06-testing.md#t2).*

SE1/T2: a reliable program checks itself, refuses to start if a precondition is missing, and says so
loudly. Apply that to the path as well as the product.

A **doctor** is one command that checks a workstation: the tools and their versions, the container
runtime actually running a container, the identity that will sign commits, the login that will push,
whether the secrets are readable. It prints one line per check and exits nonzero if any fails. A
**preflight** does the same at the start of a deploy: is this the commit that passed CI, is the
target the environment named, is there budget left, is this inside a freeze?

Both replace a page of "make sure that…" with something that cannot be skimmed. Each check has been
seen to fail ([DO1/T3](06-testing.md#t3)), and a check for something optional warns rather than
fails, or people learn to ignore the result.

> **Ask:** "What does a new laptop, or a new deploy, run first — and what does it refuse?"

### <a id="t3"></a>DO1/T3. A backup never restored, an alert never fired, and a rollback never run prove nothing

*Reframes [SE1/T3](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/06-testing.md#t3).*

SE1/T3: a check that has never been seen to fail proves nothing; watch it go red once. Operations
has the same rule for mechanisms that are supposed to *work* on a bad day. Each has to be seen
working before the day comes:

- **The backup:** restored, to a separate place, and the restored data read by the application.
- **The alert:** fired, by injecting its fault, and received by a person on the device they carry.
- **The rollback:** run, in production, on an ordinary day, and timed.
- **The failover:** triggered by turning the primary off, not by a tabletop discussion.
- **The budget alarm:** tripped, with the threshold set low for an afternoon.

The well-known public case is a code-hosting company that deleted a production database in 2017 and
found, during the outage, that none of its five backup and replication mechanisms was working as
intended. It recovered from a copy an engineer happened to have made six hours earlier. Every
mechanism had been configured. None had been watched to work.

> **Ask:** "When did we last watch this work, on purpose?"

### <a id="t4"></a>DO1/T4. A pipeline people re-run until it passes has stopped being a test

*Reframes [SE1/T4](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/06-testing.md#t4).*

SE1/T4: flaky tests train a team to ignore red. A pipeline that fails one run in five for no reason
teaches one habit: press *retry*. After that, a real failure gets retried too, and passes on the
second attempt often enough, because the faults it caught were intermittent.

The same disease has an alerting form. A page that fires nightly and resolves itself teaches whoever
carries it to acknowledge without looking. On the night it is real, they do
([DO1/H4](09-people.md#h4)).

Treat both as SE1 says: fix or quarantine the day it is found. A quarantined step is marked as such
where everyone sees it, with a date and an owner. An alert nobody should act on is deleted or
demoted to a report. And count the retries: a pipeline's re-run rate is one of the cheapest health
measures there is.

> **Ask:** "How often was this re-run last month before it went green?"

### <a id="t5"></a>DO1/T5. Give every run its own environment: put its name in everything it touches

*Reframes [SE1/T5](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/06-testing.md#t5).*

SE1/T5: repeatability belongs to the state a test touches; local is safe, sharded is safe, global
and written is a race. Environments are state. Two pipeline runs sharing one staging database, or a
developer's stack and a test stack fighting over port 5432, are the global-and-written case.

The fix is SE1's, in order. **Make it local:** an environment created for the run and destroyed
after. **Shard it:** one set of templates, with the environment's name in every container, network,
volume, database, and port, so that `development` and `test` run side by side on one laptop and
cannot touch each other. **Serialize it:** a lock on the one shared staging, and a sentence saying
why it could not be the first two.

The name does a second job: nobody can run a destructive command without the target being visible
in it ([DO1/D9](05-design.md#d9)).

> **Ask:** "Could two of these run at the same time without knowing about each other?"

### <a id="t6"></a>DO1/T6. Staging is a mock of production: make it misbehave on request, and check it against the real thing

*Reframes [SE1/T6](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/06-testing.md#t6).*

SE1/T6: a mock is a K4 about the real thing, false the day you write it; make it misbehave on
request and run one conformance check against both. An emulator, a local stack, and a staging
environment are mocks of production, and they fail the way mocks fail: they are politer than the
real thing. They do not throttle. They do not enforce permissions. They answer in a millisecond.

So give them faults you can switch on — slow, down, refusing, out of quota — and use them. And run
one small check against both staging and production, on a schedule, that would notice them
diverging: the same request, the same shape of answer, the same list of configuration keys
([DO1/U4](02-unknowing.md#u4)).

What only production can show, send to production in a small dose ([DO1/F1](03-feedback.md#f1)).

> **Ask:** "How is this environment kinder than production — and can we make it less kind?"

### <a id="t7"></a>DO1/T7. Test the environment the way you test the code: write the check that passes when it is right

*Reframes [SE1/T8](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/06-testing.md#t8).*

SE1/T8: write the tests that would pass when it works; for each attack in the threat model,
demonstrate it first. Infrastructure described in files gets tests like any other code, and the
useful ones are stated from outside.

- **After every deploy, a smoke test**: a real request through the public name, compared with a
  known answer, before the deploy is called finished ([DO1/O5](07-operating.md#o5)).
- **Statements that must stay true**: *nothing but the load balancer accepts traffic from the
  internet; no storage is public; every volume is encrypted*. Run them against the description
  before applying it, and against what is running afterward.
- **Each defense, demonstrated**: try the request without the token, the deploy from the wrong
  branch, the unsigned image. Trust none until you have watched it refuse.

> **Ask:** "What would be true from the outside if this environment were correct — and does anything
> check that?"

### <a id="t8"></a>DO1/T8. A restart destroys the evidence: capture on the way to the fix

*Reframes [SE1/T9](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/06-testing.md#t9).*

SE1/T9: you cannot fix a rumor, only change something and fail to observe the symptom, which feels
like fixing. Operations has a standing source of rumors: the outage that was ended by restarting.
The restart was right ([DO1/O10](07-operating.md#o10)). It also erased the memory, the
connections, the stuck thread, and often the logs, and what is left is a story.

Do not choose between mitigation and evidence. Make the capture part of the mitigation: one command
in the runbook that saves the logs, the process state, and the resource numbers to somewhere that
survives, and takes seconds, run immediately before the restart. Have containers send logs off the
machine as they are written, so that replacing the container does not delete them.

Then the afternoon's question is *why*, with data, and not *did that really happen*.

> **Ask:** "If we restart it now, what will we wish we had saved?"

### <a id="t9"></a>DO1/T9. Break it on purpose, on a day you chose

*Reframes [SE1/T10](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/06-testing.md#t10).*

SE1/T10: examples check the cases you imagined; property tests and fuzzing find the ones you did
not. The operational equivalent is injecting failure into a running system and watching what it
does: stop a container, cut the network between two services, fill a disk, expire a credential, make
the database slow.

Do it first where it is cheap, on the laptop, with the fault controls the system already has
([DO1/F2](03-feedback.md#f2)). Then in staging. Then, when the answers have stopped being
surprising, in production, in daylight, with everyone watching and a stated way to stop.

Say beforehand what you expect to happen. The value is in the difference. A team that has watched
the collector lose its database on a quiet Wednesday afternoon will recognize it on a game night in
seconds.

> **Ask:** "What happens when this dies — and is that a guess or a memory?"

### <a id="t10"></a>DO1/T10. When the platform owns "correct," test against the platform

*Reframes [SE1/T11](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/06-testing.md#t11).*

SE1/T11: when a format or protocol is owned elsewhere, its implementation is the only judge. A cloud
provider's permissions, quotas, network rules, and API behavior are owned elsewhere, and the
documentation, the emulator, and a language model's recollection are all approximations of them.

So anything that depends on the provider's judgment is tested against the provider: a real account,
ideally a separate one, with real permissions. A role that "should" allow the deploy is tried. A
quota that "should" be enough is read from the account. An emulator earns trust for the things you
have seen agree with the real service, and for nothing else ([DO1/T6](06-testing.md#t6)).

Pin the version of every tool and provider plugin that talks to the platform
([DO1/O1](07-operating.md#o1)), and record every disagreement between the documentation and
the platform in FINDINGS. There will be some.

> **Ask:** "Was this tried against the real platform, or against what we believe about it?"

### <a id="t11"></a>DO1/T11. Rebuild from nothing on a schedule: the cold start finds the circle

*New.*

Every other test in this chapter exercises a part. This one exercises the claim the whole guide
leans on: that the system can be rebuilt from the repository and the backups
([DO1/P5](01-purpose.md#p5)). Start from an empty account and a fresh clone, follow the
written procedure, and time it.

A cold start finds what nothing else does, because every running system is leaning on itself in ways
nobody drew. The pipeline pulls its tools from the registry the pipeline publishes to. The secrets
store needs a secret to unlock. The script assumes a bucket that was made by hand in the first week.
The documentation is on the system that is down. Each is a circle, invisible while everything is up,
and each ends a recovery.

Once a quarter is a reasonable start; after any change to the account layout, sooner. What it finds
goes in the procedure, and the time it took is the honest recovery number
([DO1/R4](04-requirements.md#r4)).

> **Ask:** "When did someone last build this from an empty account — and what did they have to ask?"

---

Next: [7. Operating](07-operating.md) — the pipeline, the deploy, secrets, incidents, capacity.
