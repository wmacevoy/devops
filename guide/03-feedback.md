# 3. Feedback

What it costs to learn whether a change works once real users are on the other end, and why the size
of the change sets the price. Read with [SE1 chapter 3](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/03-feedback.md).

---

### <a id="f1"></a>DO1/F1. A test of a build costs seconds; a test in production costs users — unless the change is small

*Reframes [SE1/F1](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/03-feedback.md#f1).*

SE1/F1: where a test costs a million dollars, front-load analysis; where it costs a second, loop.
Production is the one environment that is exactly right about everything, and the one where being
wrong is paid for by someone else.

That looks like an argument for testing there rarely and carefully. It is the opposite. The cost of
a production test is set by how much changed and how many users met it. A one-line change shown to
one lot in a hundred, watched for five minutes and withdrawn by one command, is a cheap test of the
most accurate kind. A quarter's work released to everyone on the morning of a game is the
million-dollar test, taken without the analysis ([DO1/O5](07-operating.md#o5)).

> **Ask:** "What does it cost if this is wrong — and what would make that smaller?"

### <a id="f2"></a>DO1/F2. The cost of a deploy is designed, not given

*Reframes [SE1/F2](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/03-feedback.md#f2).*

SE1/F2 lists the toolkit that makes a test cheap. The operations toolkit does the same for a deploy:

- **One command** brings the whole system up on a laptop, and one resets it to a known state.
- **The same artifact** runs everywhere, so what was tested is what ships
  ([DO1/D7](05-design.md#d7)).
- **A second environment costs a name**, not a week: the environment is a parameter of the
  templates, and two can run side by side ([DO1/T5](06-testing.md#t5)).
- **Faults on request**: a sensor that goes silent, a database that refuses connections, a full
  disk.
- **A deploy and its reverse are the same kind of step**, both run often enough to be boring.

Each multiplies against every future change. A deploy that takes a day and a specialist will be done
monthly; one that takes five minutes and anyone will be done whenever something is ready. *Hard to
deploy* is information about the design.

> **Ask:** "How long from a merged change to running in production, and how many people does it
> need?"

### <a id="f3"></a>DO1/F3. Push each class of failure down to the cheapest loop that can catch it

*Reframes [SE1/F3](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/03-feedback.md#f3).*

SE1/F3 orders the loops by cost. The operations ladder, cheapest first:

1. A linter on the configuration: a misspelled key, a malformed template.
2. Building the image.
3. Running it on the laptop, in the same container that will ship.
4. CI, from a clean checkout ([DO1/O2](07-operating.md#o2)).
5. A staging environment.
6. A fraction of production traffic.
7. All of production.
8. A user telling you.

When something reaches a rung, ask which lower rung could have caught it, and put a check there. A
missing environment variable found at rung 7 belonged at rung 3: the service should have refused to
start ([DO1/T2](06-testing.md#t2)). What only rungs 6 and 7 can find — real load, real
data, real networks — is exactly what makes them worth reaching cheaply and often.

> **Ask:** "Which cheaper step could have told us this?"

### <a id="f4"></a>DO1/F4. A disk fills without a sound: every warning a machine gives must be built

*Reframes [SE1/F4](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/03-feedback.md#f4).*

SE1/F4: software does not sag or crack before it fails; every signal a physical structure gives for
free has to be built on purpose. Infrastructure is the same, and its failures are mostly cliffs. A
disk at ninety-nine percent works exactly as well as one at ten. A connection pool with one slot
left serves every request. A certificate is valid until the second it is not.

So the warning is a slope you draw yourself: how full, how fast it is filling, and when the line
crosses the edge at that rate. *Saturation*, how close a resource is to its limit, is the signal
that tells you about tomorrow, where errors and latency tell you only about now.

In the running example, twenty readings a second is 1.7 million rows a day. Somebody should know the
date the volume fills.

> **Ask:** "What is the next thing here to run out, and on what date, at today's rate?"

### <a id="f5"></a>DO1/F5. When uptime, deploy count, or time-to-restore becomes a target, it stops measuring

*Reframes [SE1/F5](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/03-feedback.md#f5).*

SE1/F5: aim at a measure and you get the measure. Aim at availability and outages get reclassified
as "degraded." Aim at deploys per day and changes get split into meaningless pieces. Aim at mean
time to restore and incidents get closed before they are understood, then reopened under a new
number. Aim at zero pages and the alerts get deleted.

The four delivery measures that have held up best under study — how often you deploy, how long a
change takes to reach production, how often a change fails, how long recovery takes — work as a set
because each is the cheapest way to fake another. Keep them together, keep the reason attached, and
look at them for a team rather than at a person.

> **Ask:** "How would we hit this number without getting any better?"

### <a id="f6"></a>DO1/F6. The size of a change is the size of the search when it breaks

*New.*

SE1 asks for the smallest diff so that it can be reviewed. Operations has a second, separate reason
that would hold even if review were free: when production misbehaves after a deploy, the suspects
are everything that changed.

Ship one change and the diagnosis is done before it starts; reverting it costs one feature. Ship
forty and you are searching forty changes and their interactions at three in the morning, and the
revert takes out thirty-nine things that worked. The search grows with the combinations of changes,
not with their count.

Surveys of delivery teams keep finding that those who deploy most often also have fewer failed
changes and faster recoveries ([READING](../READING.md)). Frequency is not the cause. Small is, and
frequent is the only way to stay small.

> **Ask:** "If this deploy breaks, how many changes will we be choosing between?"

### <a id="f7"></a>DO1/F7. Undeployed work is unverified work, and it ages

*New.*

A change that is merged and not running in production has passed every test except the one that
counts. Until it ships, nobody knows whether it works there, and meanwhile three things get worse.
The author forgets it, so a failure next month costs a re-read. Other changes pile on top of it, so
the eventual deploy is large ([DO1/F6](03-feedback.md#f6)). And production moves on without it,
so the thing it was tested against is no longer what it will meet.

Treat the time from merge to production as inventory, the way a warehouse treats unsold stock: it
has a carrying cost and it spoils. Measure it. If the answer is weeks, the fix is rarely to work
faster. It is to find what the change is waiting for — a manual approval, a release train, a person —
and ask whether a check could do that waiting instead.

> **Ask:** "What is the oldest merged change that is not yet in production, and what is it waiting
> for?"

---

Next: [4. Requirements](04-requirements.md) — the promises a running system makes, and their prices.
