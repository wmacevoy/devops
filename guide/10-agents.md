# 10. Agents

What changes when the one typing the commands is an AI agent: a fast operator who holds whatever
credentials the shell holds, and carries no pager. Read with
[SE1 chapter 10](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/10-agents.md); the working agreement that follows from this chapter is
[AGENTS.md](../AGENTS.md).

---

### <a id="a1"></a>DO1/A1. Writing infrastructure got cheap much faster than knowing what applying it will do

*Reframes [SE1/A1](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/10-agents.md#a1).*

SE1/A1: producing code got cheap faster than knowing it is right, so everything scarce moved to
specification, verification, review, and ownership. An agent will produce a pipeline, a container
file, and two hundred lines of infrastructure description in a minute, correct in form and plausible
in every line.

What that text will *do* depends on things outside it: what already exists in the account, what
depends on what is about to be replaced, which of two similarly named environments the session is
pointed at. None of that is in the diff, and reading the diff more carefully does not reveal it.

So the scarce work is the part between the text and the apply: the inventory of what is there
([DO1/W9](08-work.md#w9)), the preview of what will change
([DO1/A2](10-agents.md#a2)), and a person deciding.

> **Ask:** "Now that writing this was free, who has checked what it will do to what is already
> running?"

### <a id="a2"></a>DO1/A2. A preview is a claim; an apply is an action. Read the first before the second

*Reframes [SE1/A2](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/10-agents.md#a2).*

SE1/A2: fluent, confident, plausible, and occasionally wrong is the exact shape of K3; run the check.
For infrastructure the check has a specific form, and most tools provide it: a preview, plan, diff,
or dry run that states what would be created, changed, and destroyed.

Run it and read it, every line, and above all the destroyed ones. "One to change" may be a
replacement that deletes a volume. An agent's summary of a preview is a summary
([DO1/A11](10-agents.md#a11)); the person approving reads the preview itself.

Then remember what a preview is not. It is the tool's prediction, made from its own records, and it
is wrong exactly where those records have drifted from reality ([DO1/U3](02-unknowing.md#u3)).
Where no preview exists — a shell script, a console — the equivalent is saying the target and the
effect before running, and running it first somewhere that does not matter.

> **Ask:** "What does the preview say will be destroyed — and has a person read that line?"

### <a id="a3"></a>DO1/A3. An agent's shell holds your credentials: keep production out of its reach

*New.*

An agent working in a terminal is not given access the way a new teammate is. It inherits everything
the session already has: the cloud login from this morning, the cluster context last selected, the
ssh agent with its keys loaded, the tokens in the environment, the decrypted secrets in the working
tree. Nobody granted any of it. It was simply there.

That makes "the agent would not do that" the wrong kind of safeguard, since it depends on a model
correctly judging, every time, which of two similarly named environments a command touches.

Arrange it so the dangerous thing is unavailable and not merely unlikely. Work in a profile or
container that holds credentials for a development account only. Leave production changes to the
pipeline ([DO1/O9](07-operating.md#o9)). Before a session, check what is in reach, and say so.
Where an agent must act on production, it is with a credential issued for that task and logged.

> **Ask:** "What could this session do to production right now, if it tried?"

### <a id="a4"></a>DO1/A4. A destructive command is a one-way door at typing speed: stop, name the target, and ask

*New.*

Operational tools are full of commands that remove things, and of flags that remove the question:
*prune*, *destroy*, *force*, *recursive*, *all*, *yes*. A person learns to slow down at them the way
a driver slows at a blind corner. An agent executing a plan does not feel the corner; the command is
one more step, issued at the same speed as a directory listing.

The scope is usually wider than the name suggests. "Remove unused images" is every image on the
machine and not this project's. "Clean up the volumes" is someone's database.

So this is an absolute rule for agents and a good one for people. Before any command that deletes,
overwrites, or cannot be reversed: state what it will remove and from which environment, prefer the
form that only reports, and wait for a person. An earlier general permission does not cover it.
Recovering from a wrongly refused deletion costs a minute ([DO1/W2](08-work.md#w2),
[DO1/O13](07-operating.md#o13)).

> **Ask:** "Exactly what will this remove, from where — and who said yes to that?"

### <a id="a5"></a>DO1/A5. Spend the speed on the boring half of operations

*Reframes [SE1/A5](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/10-agents.md#a5).*

SE1/A5: spend the saved time on fixtures, harnesses, fault scripts, and the boring second test case,
not on more features. Operations has a long list of work that everyone agrees matters and nobody
gets to, and an agent is well suited to nearly all of it:

- the doctor script and the preflight ([DO1/T2](06-testing.md#t2));
- a runbook drafted from the transcript of someone doing the task by hand
  ([DO1/W8](08-work.md#w8));
- the fault scripts for the laptop stack, and the second environment;
- the smoke test and the statements that must stay true ([DO1/T7](06-testing.md#t7));
- the inventory, drafted from what the account reports, every line marked *(inferred)*
  ([DO1/W9](08-work.md#w9));
- the restore test, written so that it runs on a schedule.

What it should not be spent on is a larger platform than the requirements ask for
([DO1/D8](05-design.md#d8)).

> **Ask:** "Which of the checks we never had time for could be written this afternoon?"

### <a id="a6"></a>DO1/A6. Whoever applies it owns it: an agent proposes the change, a person or a reviewed pipeline makes it

*Reframes [SE1/A7](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/10-agents.md#a7).*

SE1/A7: whoever signs owns it; read the whole diff; the agent does not commit, push, or publish
unless asked. In operations the equivalent of publishing is *applying*: the moment a description
becomes a change to something that is running.

So the default division is this. The agent writes the change and produces the preview. A person
reads both. The apply is done by the pipeline, on merge, with the reviewer's approval as the
signature; or by a person at a terminal, who has thereby signed.

When someone does ask an agent to apply — to a development stack, usually — that request covers that
change to that environment, once. It is not a standing grant, and it does not extend to the next
environment up.

> **Ask:** "Who will have read this before it touches something running — and is that written
> anywhere?"

### <a id="a7"></a>DO1/A7. An agent sees the repository, not the console: what was made by hand does not exist to it

*Reframes [SE1/A3](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/10-agents.md#a3).*

SE1/A3: every session starts from the repository; a constraint said only in chat does not exist next
session, and the agent hands back a clean, tested violation of it. Operations has a second place
where things exist unrecorded: the platform itself.

The firewall rule added by hand, the bucket created in the first week, the DNS record someone's
manager set up: an agent reading the repository cannot know about them. Asked to build the
environment, it writes a complete and correct description of a system that omits them, and applying
it either duplicates what is there or removes it.

This is the best argument there is for [DO1/D6](05-design.md#d6): what is not in the
repository is not known to half the team. Until then, tell the agent what exists outside it, or have
it read the account before it writes anything ([DO1/W9](08-work.md#w9)).

> **Ask:** "What exists in the account that the repository does not mention?"

### <a id="a8"></a>DO1/A8. What to hand over, and what to keep

*Reframes [SE1/A9](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/10-agents.md#a9).*

SE1/A9 lists what to hand an agent and what to keep. For operations:

**Hand over:** container files and pipeline configuration from a written design. The second
environment, once the first exists. Reading ten thousand lines of logs for the three that matter.
Drafting a runbook, a postmortem timeline from the incident channel, an inventory from what the
account reports. "Which of these forty changes could explain this error?" "What in this description
would be destroyed by this change?"

**Keep:** the operating numbers, and the conversation with the client that sets them. Anything on
the one-way list ([DO1/D11](05-design.md#d11)). The apply to production. Every decision
about a secret: who reads it, where it lives, when it rotates. Whether an alert pages a human being.
Leading an incident. Deciding that a resource nobody can explain may be deleted.

The line is the one SE1 draws: an agent is fastest where the work is mechanical and reversible, and
cannot decide where it is a promise to a person.

> **Ask:** "Is this a draft the agent can make, or a promise only we can make?"

### <a id="a9"></a>DO1/A9. Logs, configuration, and environment sent to an agent have left the room

*New.*

Diagnosing an operational problem means showing the agent what the system printed, and operational
output is full of things that were never meant to be shown: connection strings with passwords in
them, tokens in headers, internal hostnames and addresses, customer identifiers in log lines, the
whole environment of a process dumped by a helpful error handler. Where the system holds data about
people, rows of the data are the same, whatever the reason for showing them ([DM1/A8](https://github.com/wmacevoy/data-mining/blob/v1.4/guide/10-agents.md#a8)).

Pasting that into a hosted model sends it to a third party, under that service's terms, and into a
transcript that will outlive the incident. A secret that has been pasted is leaked, by the ordinary
definition, and gets the ordinary treatment: it is rotated ([DO1/O8](07-operating.md#o8)).

So decide in advance what may be sent and to which service. Scrub logs where they are written, not
where they are read, since nobody scrubs carefully at three in the morning. Refer to secrets by name.
And keep decrypted secret files out of any directory an agent works in
([DO1/A3](10-agents.md#a3)).

> **Ask:** "What is in this output that we would not put in a public issue?"

### <a id="a10"></a>DO1/A10. In an incident an agent reads fast and decides nothing

*New.*

An incident is where an agent's speed is most welcome and its usual weakness most expensive. It can
read every log, compare this deploy with the last, list what changed, and draft the status update,
in the time it takes a person to find the dashboard. It also produces a confident cause within
seconds, and under pressure a confident cause is believed.

So fix the roles before the incident. The agent gathers and proposes: *here is what changed, here
are three candidate causes, here is the evidence for each and what would distinguish them.* The lead
decides ([DO1/H3](09-people.md#h3)). No mitigation an agent suggests is run by the agent;
during an incident every action is on the one-way list until shown otherwise
([DO1/A4](10-agents.md#a4)). And each claim it makes about the system's state is checked
against the system before anyone acts on it ([DO1/A11](10-agents.md#a11)).

Afterward it is useful again: the timeline, assembled from the channel and the logs, for a person to
correct.

> **Ask:** "Is this the agent's reading of the evidence, or something we have looked at ourselves?"

### <a id="a11"></a>DO1/A11. After a gap, the state of production is recalled, not observed: ask the system

*Reframes [SE1/A11](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/10-agents.md#a11).*

SE1/A11: a compaction summary or a handoff note is recalled, not observed, and the next context
cannot tell by feel; where summary and source disagree, the source wins. For code the source is the
repository. For operations there are two sources, and the second moves on its own.

"Deployed to staging, healthy" in a summary was true when written. Since then someone else may have
deployed, the instance may have been replaced, the certificate may have expired. A session that
continues from the summary acts on a system that is no longer there.

So after any gap — a compaction, a handoff, a night — re-observe before acting: which commit is
running ([DO1/D13](05-design.md#d13)), what the health signal says now, whether the
half-finished step finished ([DO1/O4](07-operating.md#o4)). Which environment the session is pointed
at is the first thing to re-check and the easiest to get wrong.

> **Ask:** "Did we just look at that, or do we remember it?"

---

Back to the [README](../README.md), or on to [CHECKLISTS.md](../CHECKLISTS.md).
