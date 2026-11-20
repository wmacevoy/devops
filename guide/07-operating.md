# 7. Operating

The path and the platform in motion: pinned tools, a pipeline that runs your command, alerts a person
will believe, deploys that can be undone, secrets with a lifecycle, incidents, capacity, and the
backup a failure cannot reach. [SE1 chapter 7](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/07-operating.md) is this subject at the scale
of one service, and everything in it applies. This chapter is what changes when the path itself is
the thing being engineered.

---

### <a id="o1"></a>DO1/O1. Pin the platform the way you pin the build: images by digest, tools by version, runners by name

*Reframes [SE1/O1](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/07-operating.md#o1) and [SE1/O2](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/07-operating.md#o2).*

SE1/O1 and SE1/O2: an image tag is a family, not a file; say what you are compatible with, record
what you ran, and install with the command that refuses drift. The build is not the only thing that
resolves differently next week. So do the base image, the CI runner, the deploy tool, the provider
plugin, and every third-party pipeline step referred to by a tag its author can move. A hosted model
named by an alias is one more: the provider's update is a retrain that nobody here deployed
([DM1/A9](https://github.com/wmacevoy/data-mining/blob/v1.4/guide/10-agents.md#a9)).

The guides this one is modeled on carry the scar. Their CI asked for `ubuntu-latest`, which one
month became a new release that the Python setup step had no 3.9 for, the oldest version they
support. Nothing in the repository had changed. The fix was to name the older runner.

Pin each to something that cannot move: a digest, an exact version, a commit hash. Then, as SE1
says, schedule the updates, or the pins become the neglect ([DO1/D12](05-design.md#d12)).

> **Ask:** "What in this pipeline could be different tomorrow without anyone here changing it?"

### <a id="o2"></a>DO1/O2. The pipeline runs the command you run; its logic lives in the repository, not in the pipeline's own language

*Reframes [SE1/O3](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/07-operating.md#o3).*

SE1/O3: CI runs the same command you run, with the gate in the script, so no second recipe drifts.
The pipeline's configuration language makes the second recipe very easy to write. Forty lines of
shell inside a workflow file cannot be run on a laptop, cannot be tested, and are debugged by
pushing commits and waiting.

Keep the pipeline file to a list of calls: *check*, *build*, *deploy to staging*, each a script or
make target in the repository that a person can run. Then a red pipeline is reproduced locally in
one line, and moving to another CI service is a day and not a quarter.

The deploy belongs here too. A deploy that works only from one person's laptop is a pipeline with
one runner and no log ([DO1/U5](02-unknowing.md#u5)).

> **Ask:** "Can I run exactly what the pipeline runs, on my machine, with one command?"

### <a id="o3"></a>DO1/O3. Page a person only for what a person must do now; every page names its runbook

*Reframes [SE1/O4](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/07-operating.md#o4).*

SE1/O4: every failure needs a signal a person will see and believe; alert on symptoms users feel;
an alarm that fires when nothing is wrong is broken. Operations adds the question of *which* signal
goes *where*, because there are three destinations and most teams use one.

- **A page** interrupts someone. It is for a symptom users feel, happening now, that a person can do
  something about. It names a runbook ([template](../templates/RUNBOOK.md)).
- **A ticket** waits for working hours: the disk that fills next week, the certificate that expires
  next month ([DO1/U10](02-unknowing.md#u10)).
- **A record** is for nobody until someone asks: logs, with each decision's inputs.

For what to watch on any service, four numbers cover most of it: how much traffic, how many errors,
how slow, and how close to full ([DO1/F4](03-feedback.md#f4)). And treat missing data as its
own alarm ([DO1/U2](02-unknowing.md#u2)).

> **Ask:** "When this fires, what does the person do — and if nothing, why does it page?"

### <a id="o4"></a>DO1/O4. Handle the platform's failures on purpose: time out the deploy, bound the retries, and decide what a half-finished one leaves

*Reframes [SE1/O5](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/07-operating.md#o5).*

SE1/O5: every remote call has a timeout shorter than the caller's deadline; retries are bounded,
jittered, on idempotent operations only, at one layer. A pipeline is a long chain of remote calls —
to the registry, the provider, the cluster — made by scripts that mostly assume each one works.

Give every step a timeout, or one hung pull holds the deploy, and the lock, for six hours. Retry
only steps that are safe to repeat ([DO1/D4](05-design.md#d4)). And decide, for each step, what
the system looks like if the pipeline dies immediately after it: the image pushed and not deployed;
the migration applied and the code not; three of five instances replaced.

A torn deploy that looks finished is the K3 factory SE1/U7 warns of. Make the pipeline record where
it stopped, and make the next run able to carry on or undo.

> **Ask:** "If the pipeline were killed after this step, what state would we be in, and how would we
> know?"

### <a id="o5"></a>DO1/O5. Deploy to a few before all, behind a gate that can say no without a person

*Reframes [SE1/O6](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/07-operating.md#o6).*

SE1/O6: a deploy is a test whose failure is an outage; it is finished when the thing reports healthy
and the system notices, with an automatic revert and a rollback timed before the window opens.
Operations adds the dial that makes the test cheap ([DO1/F1](03-feedback.md#f1)): how many users
meet the new version first.

One instance, or one lot in a hundred, gets the change. A gate compares it with the old version on
the signals users feel, for a stated number of minutes, and either widens the rollout or withdraws
it, with nobody watching. Only then does everyone get it. A scheduled retrain goes through the same
gate, and the same freeze, as a commit does ([DM1/O6](https://github.com/wmacevoy/data-mining/blob/v1.4/guide/07-operating.md#o6)).

Two cautions. The gate is a check, so it has been seen to refuse ([DO1/T3](06-testing.md#t3)).
And "roll back" means *to the previous artifact*, which is still in the registry and can still read
the data ([DO1/O7](07-operating.md#o7)).

> **Ask:** "How many users meet this first, what would stop it, and who has seen that stop work?"

### <a id="o6"></a>DO1/O6. Deploying is not releasing: ship it dark, turn it on separately, and give every switch an owner and an end date

*New.*

SE1 treats a deploy as the moment users get the change. They can be two events. Code can reach
production switched off, be turned on for one lot, then ten, then all, and be turned off again in
seconds without a deploy at all.

Separating them is what lets deploys be small and constant ([DO1/F6](03-feedback.md#f6))
while releases stay a decision someone makes on purpose, at a sensible hour. It also gives incidents
their fastest mitigation: turn it off. For a model, shipping dark is shadow mode: the new one sees
live inputs and logs its answers while the old one still decides ([DM1/O6](https://github.com/wmacevoy/data-mining/blob/v1.4/guide/07-operating.md#o6)).

The price is that every switch is state ([DO1/D1](05-design.md#d1)), and the combinations
multiply. An old switch is a branch nobody tests. In 2012 a trading firm reused the name of a
retired switch for a new feature; on the one server that still had the old code, the name turned the
old behavior back on, and the firm lost over four hundred million dollars in forty-five minutes.

So each switch has an owner, a date by which it is removed, and never a second meaning.

> **Ask:** "How many switches are live right now, and which should already be gone?"

### <a id="o7"></a>DO1/O7. Change infrastructure the way you change a schema: add beside, move, then remove

*Reframes [SE1/O7](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/07-operating.md#o7).*

SE1/O7: expand, then contract; add alongside, write both, read either, switch, then remove, each
step undoable. That is written about data and interfaces, and it is exactly how to change the
platform under a running system.

A new database, a new load balancer, a new region, a new certificate: stand it up beside the old
one. Send it a copy of the traffic, or a small share. Compare. Move the rest. Keep the old one,
stopped, for a stated period. Then delete it. At every step there is a way back, and no step is "the
cutover."

Mind the caches that set the pace ([DO1/D2](05-design.md#d2)): a DNS change takes as long as the
record's lifetime to finish, so shorten the lifetime days ahead. And rehearse anything that touches
kept state on a copy the size of production, timed, as SE1 says.

> **Ask:** "At which step of this change is there no way back?"

### <a id="o8"></a>DO1/O8. A secret has a lifecycle: issued, stored, used, rotated, revoked — and a rotation never performed does not work

*Reframes [SE1/O8](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/07-operating.md#o8).*

SE1/O8: secrets never in the repository, prompts, or logs; refer to them by name; a leaked one is
rotated; a missing one is an exit code. That is a rule about where a secret rests. Operations has to
manage the whole life.

Keep an inventory by **name**, never by value: what it opens, where it is stored, who and what can
read it, when it was last rotated, and the procedure. Then rotate each one on an ordinary day,
because the first rotation always finds a place the old value was copied to
([DO1/T3](06-testing.md#t3)).

Encrypting secrets *into* the repository is a common compromise, and it has costs to state plainly.
Every clone carries every past value forever, so removing a person means rotating everything they
could read. And a path pattern is not proof of encryption: in the course repository this guide grew
from, one file sat in an encrypted directory in plain text, because it was committed before the
pattern covered it. Check for the ciphertext itself.

> **Ask:** "If this secret leaked today, what is the procedure, and when did anyone last run it?"

### <a id="o9"></a>DO1/O9. Give the pipeline its own identity: scoped to one environment, short-lived, and unable to do what it never needs to

*Reframes [SE1/O8](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/07-operating.md#o8).*

SE1/O8 also says least privilege per component and per key. The component most often left out is the
pipeline, which typically runs as an administrator with a key created in the first week and never
changed, because that made the first deploy work ([DO1/R8](04-requirements.md#r8)).

Give it what a careful person would get. **A separate identity per environment**, so that a job for
staging cannot touch production even when its configuration is wrong. **Credentials issued for the
run** and expiring within the hour, in place of a long-lived key stored in the CI service. **Only
the actions the deploy performs**: a pipeline that updates a service has no need to delete a
database, read the backups, or create users.

People get the same treatment: everyday access that cannot destroy, and a separate, logged, rarely
used way to get more.

> **Ask:** "What is the worst thing this pipeline's credentials could do — and does it need to be
> able to?"

### <a id="o10"></a>DO1/O10. In an incident: one leads, the timeline is written as it happens, and "restored" is confirmed by the signal that reported it

*Reframes [SE1/O9](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/07-operating.md#o9).*

SE1/O9: mitigate before diagnosing; one leads, one communicates, status on a schedule; runbooks are
real only if run; write it up without blame and change something. Three additions from running the
path.

**The first question is "what changed?"**, and the deploy log answers it
([DO1/D13](05-design.md#d13)). Most incidents follow a change, and the fastest mitigation is
usually its reverse.

**Write the timeline during the incident**, in one shared place, with times: what was seen, what was
tried, what happened. Memory rewrites all three by morning, and the write-up is only as good as its
timeline ([template](../templates/POSTMORTEM.md)).

**It is over when the signal that showed the harm shows it gone**, for long enough to trust, and not
when the fix was applied ([DO1/P2](01-purpose.md#p2)). Then say so to whoever was told it began
([DO1/H6](09-people.md#h6)).

> **Ask:** "What changed last — and who is writing down what we do?"

### <a id="o11"></a>DO1/O11. Capacity includes limits you do not own: quotas, rate limits, and how fast the platform can add more

*Reframes [SE1/O10](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/07-operating.md#o10).*

SE1/O10: measure before optimizing; judge by the slowest few percent of requests and not the
average; run the load test at the peak the requirements state, on production-sized data. On a
platform, three limits sit outside your code and are found at that peak.

**Quotas.** The account may run so many instances, addresses, or requests per second, and the
default is sized for nobody in particular. **Other people's rate limits.** The registry, the
certificate authority, and the provider's control API each cap how fast you can ask, which decides
how fast you can recover. **The speed of scaling.** Adding capacity automatically takes minutes; a
stadium fills its lots in a burst. Scaling that reacts arrives after the peak it was for.

So for a known peak, add capacity ahead of time, on a schedule, and test the load with the scaling
you will really have.

> **Ask:** "At the stated peak, which limit do we reach first — and is it ours to raise?"

### <a id="o12"></a>DO1/O12. Some operations have no undo: a migration, a deletion, a message sent, a key retired

*Reframes [SE1/O11](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/07-operating.md#o11).*

SE1/O11: a release you cannot roll back is a one-way door; ship it like one. A healthy pipeline makes
most deploys reversible, and that success creates a blind spot, because a few operations travel
through the same pipeline and are not.

A migration that drops a column. A deletion with no backup behind it. Ten thousand emails. A firmware
image accepted by a hundred sensors. A signing key retired while readings signed with it are still
arriving. A model that steered what people did: it rolls back, and the data it shaped does not
([DM1/O11](https://github.com/wmacevoy/data-mining/blob/v1.4/guide/07-operating.md#o11)). The pipeline shows each as a green step like any other.

Mark them. A step with no undo is labeled as such in the pipeline, needs a second person, and has a
written plan for the case where it goes wrong, since "roll back" is not available. Wherever
possible, turn it into a reversible one first: stop using the column for a release before dropping
it ([DO1/O7](07-operating.md#o7)).

> **Ask:** "Which step in this deploy cannot be undone — and does the pipeline treat it
> differently?"

### <a id="o13"></a>DO1/O13. Keep the backup where the failure cannot reach it

*New.*

SE1 asks that a backup be restored before it is trusted. It does not ask where the backup lives,
and on a modern platform that is how backups are lost. Whatever destroys the data — an operator's
mistaken command, a script with the wrong environment name, a stolen credential, ransomware — acts
with some identity's permissions. If that identity can also delete the backups, they go in the same
minute.

Replication is not a defense. A replica faithfully copies the deletion.

So at least one copy sits where the everyday credentials cannot write: another account, another
provider, or storage that refuses deletion until a date has passed. The identity that makes backups
can add and cannot remove. And the copy is far enough away, in every sense, that one mistake cannot
be made to both ([DO1/O9](07-operating.md#o9), [DO1/A4](10-agents.md#a4)).

> **Ask:** "Could the same command, or the same stolen key, delete both the data and its backup?"

---

Next: [8. Work](08-work.md) — the loop that does not end at deploy.
