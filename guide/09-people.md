# 9. People

Who is paged, who is told, what the client believes was promised, and whose laptop the whole path
starts on. Read with [SE1 chapter 9](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/09-people.md).

---

### <a id="h1"></a>DO1/H1. Whoever can change it should hear it break, and whoever is woken by it should be able to change it

*Reframes [SE1/H1](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/09-people.md#h1).*

SE1/H1: a system copies the communication structure of the team that built it, so choose the team's
shape on purpose. Split a team into people who build and people who run, and the system grows a seam
exactly there: a handoff document, a release meeting, a queue of tickets in each direction, and
software that is easy to write and hard to operate ([DO1/P6](01-purpose.md#p6)).

The arrangement that removes the seam is ownership that runs the whole length. The people who write
a service deploy it and carry its pager; the people who run the platform treat those teams as their
users. Neither needs to be large. What matters is that the pain of a bad design returns to someone
who can change the design.

A course team of four already has this. The risk there is the opposite one: a single person who
quietly becomes "ops" ([DO1/H2](09-people.md#h2)).

> **Ask:** "Does the person who gets woken have the access, and the right, to fix the cause?"

### <a id="h2"></a>DO1/H2. Operators fail the four ways inputs do: the one who is absent holds the only login

*Reframes [SE1/H2](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/09-people.md#h2).*

SE1/H2 applies absent, late, wrong, and lying to teammates. For operations the sharpest is
**absent**, because access collects in one person. They made the cloud account with their own
email. Their laptop has the only working deploy. The domain is registered to their card. None of it
was a decision.

Ask, for every account, credential, and procedure: *who is the second person?* An account has two
owners. A deploy runs from the pipeline and not from a home directory ([DO1/O2](07-operating.md#o2)).
A recovery code is somewhere two people can reach.

The others apply too. **Late**: the renewal someone meant to do. **Wrong**: the command run in the
environment that was not intended ([DO1/D9](05-design.md#d9)). **Lying**: "the backup is fine," said
in good faith, about a status with no age ([DO1/U6](02-unknowing.md#u6)).

> **Ask:** "Who is the second person who can do this, and have they done it?"

### <a id="h3"></a>DO1/H3. Add two rotating roles: who is on call, and who leads an incident

*Reframes [SE1/H3](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/09-people.md#h3).*

SE1/H3 gives four roles and rotates them each iteration, because the hand-off reveals what lived in
one head. A running system adds two, and they rotate for the same reason.

**On call** is the person a page goes to this week. They have the access, the runbooks, and the
authority to roll back without asking. They also get the quiet job of the week: fix the page that
should not have fired ([DO1/H4](09-people.md#h4)).

**Incident lead** exists only during an incident, and is whoever takes it, not whoever is most
senior or knows the system best; the expert is more useful with their hands free. The lead decides,
assigns, and keeps the timeline going ([DO1/O10](07-operating.md#o10)).

Rotate both through everyone, including the newest person, with the previous holder beside them the
first time. A rotation is the cheapest runbook review there is.

> **Ask:** "Who is on call this week — and could the newest person on the team do it?"

### <a id="h4"></a>DO1/H4. A pager is a queue: one that fires every night has already saturated a person

*Reframes [SE1/H11](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/09-people.md#h11).*

SE1/H11: someone behind is a queue near saturation; treat it as a character flaw and it recurs
silently. On call is a queue with an unusually expensive service time, because each item is paid for
in sleep, and the debt carries into the next day's work.

So measure it like a queue. Count pages per shift, and how many of them needed a person. Set a
budget — say, two a shift that mattered — and when it is exceeded, the alerts are the next piece of
work, ahead of features. Follow SE1's order: redistribute, pair, cut. An alert that fires and
resolves by itself is cut ([DO1/T4](06-testing.md#t4)).

Someone who has been woken for three nights does not need encouragement. They need the queue to be
shorter, and time back.

> **Ask:** "How many times was someone paged last week, and how many of those needed a person?"

### <a id="h5"></a>DO1/H5. The operator who typed the command is the one who found the missing guard

*Reframes [SE1/H12](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/09-people.md#h12).*

SE1/H12: nobody is blamed for K2; a finding says "findable by X; X is now on the list." Operations
tests this rule harder than anything else, because its worst days have a named person at a keyboard.

Hold the line anyway, for a practical reason. The person who typed the command knows exactly what
the screen showed, what they believed, and why it looked right. That account is the most valuable
evidence there is, and it is given only by someone who expects to be thanked for it. A team that
punishes the operator gets, from then on, incidents with no witnesses.

A provider that took down a large part of its own storage service in 2017, with one mistyped
argument to a routine command, drew the useful conclusion in public: the tool had allowed too much
to be removed too fast, so the tool was changed. The question is always what made the wrong action
easy.

> **Ask:** "What made that the easy thing to type?"

### <a id="h6"></a>DO1/H6. Say "we are down" before the users say it to you

*Reframes [SE1/H9](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/09-people.md#h9).*

SE1/H9: bad news early is cheap; within a day of knowing, to the person it affects, with a number
and an option. During an outage the same rule runs on a shorter clock: minutes.

Tell the people affected, in the place they will look, as soon as you know something is wrong and
before you know what. Say what they are seeing, what they can do meanwhile, and **when the next
update will come** — then keep that appointment even if the update is "still working on it." Silence
is read as either ignorance or concealment.

The place has to be somewhere the outage cannot take down with it, and the person writing should not
be the person fixing ([DO1/O10](07-operating.md#o10)). For the stadium, that is a call to the
operations manager, who would otherwise hear it from an attendant.

> **Ask:** "Who needs to hear this from us first — and when did we promise the next word?"

### <a id="h7"></a>DO1/H7. Read the operating assumptions back to the client, in their words

*Reframes [SE1/H5](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/09-people.md#h5) and [SE1/H6](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/09-people.md#h6).*

SE1/H5: the client knows the domain and the pain, and cannot tell you what happens in a case they
never faced. SE1/H6: every assumption is a sentence the team believes and the client was never
asked; read each back as a question. Operating assumptions are the ones least likely to have been
asked, because they sound technical.

They are not. Each is about the client's own day:

- "We're assuming that if this is down on a morning with no event, nobody is harmed. True?"
- "We're assuming a restart during a game, thirty seconds of blank signs, is acceptable if it fixes
  something. True?"
- "We're assuming you would rather it stop than run up a bill past this figure. True?"
- "We're assuming nobody needs to be called at night between events. True?"

Most take two seconds. One gets "well, except the night before a final."

> **Ask:** "Which of our operating numbers has the client actually heard, out loud?"

### <a id="h8"></a>DO1/H8. A drill is a test: someone else follows the runbook, and the author watches their hands

*Reframes [SE1/H8](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/09-people.md#h8).*

SE1/H8: a demo is a test, so run it like one; the client drives while you watch their hands, and you
write down every "huh." A runbook is tested the same way, and for the same reason: its author cannot
see what it leaves out.

Pick an operation — restore the database, roll back the last deploy, rotate a key — and a person who
has not done it. They follow the written steps on the real system, or a full copy. The author
watches, says nothing, and writes down every pause, every question, and every step that was done
from memory because the page did not say. Time it.

Afterward the runbook is fixed, and its header gets today's date and the time it took
([DO1/U6](02-unknowing.md#u6)). Invite the client to one. Watching a restore succeed does more
for their confidence than any number.

> **Ask:** "Who, other than its author, has followed this runbook to the end?"

### <a id="h9"></a>DO1/H9. The laptop is the first environment: setting one up is a deploy, with a written path and a doctor at the end

*New.*

SE1 starts from a working machine. Every project actually starts one step earlier, with a person
and a laptop that cannot yet build anything, and the path from there to a first commit is usually an
oral tradition: install these, in roughly this order, ask someone when it breaks.

Treat that path as the first deploy. It is written, per operating system, as numbered steps. It ends
with a doctor that checks every step and prints one line for each ([DO1/T2](06-testing.md#t2)).
It is run for real on a regular schedule — every new teammate, every semester — and each run fixes
the document, because the tools it names keep moving ([DO1/O1](07-operating.md#o1)).

The laptop also holds real power: signing keys, cloud logins, the ability to push. Its setup is
where disk encryption, key backup, and credential storage are decided, or left to chance
([DO1/R8](04-requirements.md#r8)).

> **Ask:** "How long did the last new person take to get from a bare laptop to a passing check — and
> what did they have to ask?"

---

Next: [10. Agents](10-agents.md) — a fast operator with your credentials and no pager.
