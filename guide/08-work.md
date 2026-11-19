# 8. Work

How the work is arranged when the product is never finished: the loop that keeps going after deploy,
the first slice all the way to production, what done means for a running system, and toil. Read with
[SE1 chapter 8](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/08-work.md).

---

### <a id="w1"></a>DO1/W1. The six steps are a loop, and it does not end at deploy

*Reframes [SE1/W1](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/08-work.md#w1).*

SE1/W1: requirements, design, develop, test, deploy, maintain; the order is right in every method,
and methods differ in how much goes through at once. Drawn as a line, the last step looks like an
afterthought. For a running system it is most of the system's life, and it is where the next
requirement comes from.

So draw the arrow back. What operating teaches — this query is slow at full size, nobody uses this
screen, this alert fires every game — is input to requirements, and a team that does not carry it
back has a loop with one end open ([DO1/P6](01-purpose.md#p6)).

Operations shortens the loop by making the trip around it small and routine: a change goes through
all six steps in a day, many times, instead of once a quarter in bulk
([DO1/F6](03-feedback.md#f6)).

> **Ask:** "What did running it last month tell us that has not yet changed a requirement?"

### <a id="w2"></a>DO1/W2. Slow down around the operations with no undo: delete, migrate, rotate, rename, anything forced

*Reframes [SE1/W4](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/08-work.md#w4).*

SE1/W4: a route handler gets the loop; a schema with real data gets a small waterfall inside it,
designed on paper, reviewed by someone who does not share your assumption, done once. Most of
operations is the loop, and should feel like it. A short list is not
([DO1/O12](07-operating.md#o12), [DO1/D11](05-design.md#d11)):

- deleting anything that holds kept state, or its backups;
- a migration that removes or rewrites data;
- rotating a key that others hold;
- changing a public name or where it points;
- any command with a flag whose meaning is *do not ask me*.

For these, write the plan and the way back before touching anything, have a second person read it,
and say the target out loud. The cost is ten minutes. The saving is the incident where the command
was right and the environment was wrong.

> **Ask:** "Is this one of the few that cannot be undone — and has someone else read the plan?"

### <a id="w3"></a>DO1/W3. Deploy the empty system first: one page, through the whole pipeline, to production

*Reframes [SE1/W6](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/08-work.md#w6).*

SE1/W6: build a thin vertical slice first, carrying the load-bearing idea, then widen it. For the
path, the thinnest slice has no features at all. A page that says *hello*, built into an image by
CI, pushed to the registry, deployed to the real production environment by the real pipeline, served
at the real name over TLS, with one health check and one alert.

Do this in the first week. Everything that makes deployment hard is in that slice — accounts,
permissions, networking, certificates, DNS, secrets — and none of it depends on the product. Found in
week one, each is an afternoon. Found in the last week, behind a finished product that has never
left a laptop, each is the deadline.

After that, every feature arrives in a system that already ships.

> **Ask:** "Has anything at all been through the whole path to production yet?"

### <a id="w4"></a>DO1/W4. If deploying hurts, do it more often

*Reframes [SE1/W7](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/08-work.md#w7).*

SE1/W7: fear rises as a deadline nears and produces exactly the shortcuts that hurt; change the
conditions, not the willpower, by making deploys routine and reversible and crossing the gap early.

The operations pathology is the big release. Deploying is painful, so it is done rarely; rare
deploys are large; large deploys fail; failure confirms that deploying is painful. Its calendar
forms are the release held for Friday afternoon and the freeze that lasts so long that the thaw is
the most dangerous day of the year.

The way out is the opposite of the instinct. Do the painful thing so often that it is forced to
become cheap: every merge, on ordinary mornings, by whoever wrote the change
([DO1/F2](03-feedback.md#f2)). A real freeze — before the gates open — stays short because
everything else is already out.

> **Ask:** "What would have to be true for us to deploy this on an ordinary Tuesday morning?"

### <a id="w5"></a>DO1/W5. The working set gains an operating contract, an inventory, and runbooks — in the repository

*Reframes [SE1/W10](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/08-work.md#w10).*

SE1/W10: requirements, design, queue, and findings live in the repository, versioned with the code,
not in a wiki or a chat. Operating knowledge is the most likely of all to live somewhere else: a
wiki page, a pinned message, a console bookmark, one person's shell history.

Three documents join the working set, each from a template:

- **The operating contract** ([OPERATIONS](../templates/OPERATIONS_v1.md)): the promises of chapter
  4, versioned like requirements.
- **The inventory** ([INVENTORY](../templates/INVENTORY.md)): environments, kept state, secrets by
  name, dependencies, everything with a date on it.
- **Runbooks** ([RUNBOOK](../templates/RUNBOOK.md)): one per operation that matters — deploy, roll
  back, restore, rotate — each with the date it was last run and how long it took.

They change in the same commit as what they describe, and a runbook must be readable when the system
it describes is down ([DO1/T11](06-testing.md#t11)).

> **Ask:** "Where would a stranger look for how to restore this — and is that place in the
> repository?"

### <a id="w6"></a>DO1/W6. Done means running in production, observed, and reversible

*Reframes [SE1/W11](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/08-work.md#w11).*

SE1/W11: done is the whole checklist, ending with a person who read all of it. For a change to a
running system, add four lines, because *merged* is where the risk starts
([DO1/F7](03-feedback.md#f7)):

- It is **running in production**, as the artifact that was tested
  ([DO1/D7](05-design.md#d7)).
- **The signal that would show it broken exists**, and someone has looked at it since the deploy.
- **The way back is known**: the previous artifact, the switch, or a written statement that there is
  none ([DO1/O12](07-operating.md#o12)).
- **The documents it made untrue are updated**: the inventory, the runbook, the contract.

"It is merged and the pipeline is green" is a status. It is not this.

> **Ask:** "Is it in production, how would we know if it were broken, and how do we take it back?"

### <a id="w7"></a>DO1/W7. An incident review that changes nothing will be written again

*Reframes [SE1/W13](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/08-work.md#w13).*

SE1/W13: a finding retires "we already learned this" only if it changes something that outlives the
people who learned it; end every finding with what it changed, or why nothing. Incident reviews are
where this rule is most often broken, because writing one feels like the change.

A review ends with actions, and an action is a thing that will refuse next time: a check in the
pipeline, an alert with a runbook, a permission removed, a default altered. "Be more careful," "add
to the wiki," and "raise awareness" are not actions; they rely on the same attention that was
missing.

Give each action an owner and a place in the queue, and look at old reviews when a new incident
arrives. A second incident with the same cause is not bad luck. It is an action that was written
down and not done.

> **Ask:** "What will stop this next time without anyone having to remember?"

### <a id="w8"></a>DO1/W8. Do it by hand, write it down, then script it — in that order

*New.*

SE1 says to spend an agent's speed on harnesses and fixtures, and says nothing about a category of
work that barely exists in a course project: **toil**. Toil is operational work that is manual,
repetitive, brings no lasting improvement, and grows with the system. Restarting the collector.
Adding a lot by editing four files. Renewing a certificate from a calendar reminder. A team can
spend all its time on it and look busy.

The cure is automation, and the order matters. **By hand first**, because you cannot script what you
do not understand. **Written down second**, as a runbook, because the second person to do it finds
what the first assumed. **Scripted third**, when the written steps have stopped changing.

Automating before understanding gives a script that does the wrong thing quickly and that nobody
dares read. And keep a count of the hours: toil that is not measured is defended as "just part of
the job."

> **Ask:** "How many times have we done this by hand — and is it written down yet?"

### <a id="w9"></a>DO1/W9. To adopt this on a system that is already running, write down what is there before what should be

*Reframes [SE1/W12](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/08-work.md#w12).*

SE1/W12: rebuilding a working set from code invents reasons; write what the code observably does,
marked *(inferred)*, until someone who can decide confirms it. A running system has more to
reconstruct than its code, and most of it is not in the repository at all.

Start with the inventory, from observation: list what is actually running in the account, what it
costs, who can log in, what has a date on it. Mark every line *(inferred)*. Do **not** start by
writing the tidy description and applying it; a description applied over a system you have not
inventoried deletes the hand-made thing that was holding it up ([DO1/U3](02-unknowing.md#u3)).

Then bring one piece at a time under the repository: import it, show that applying the description
changes nothing, and only then change it. Resources nobody can explain are stopped, not deleted, and
watched ([DO1/D12](05-design.md#d12)).

> **Ask:** "Is this a description of what is running, or of what we wish were?"

---

Next: [9. People](09-people.md) — who is paged, who is told, and whose laptop it runs on.
