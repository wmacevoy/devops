# Adopting the guide

How to make a repository that builds, deploys, or runs something follow this guide, whether the
system starts today or is already running. This file extends [SE1's ADOPT.md](https://github.com/wmacevoy/software-engineering/blob/v1.7/ADOPT.md), which
governs everything about the code: the repository guide, the queue, findings, definition of done,
CI. It adds what is different about a running system, and one thing changes the order: **SE1
rebuilds requirements from the code, which is safe. Rebuilding a description of infrastructure and
applying it is not**, because whatever the description leaves out, the apply may remove
([DO1/W9](guide/08-work.md#w9)). So production goes out of reach first, and the inventory is written from observation
before anything is changed.

**Not yet tried.** Written 2026-11-19 by Claude Fable 5.2 with Warren MacEvoy, in the session that
drafted the guide. Nobody has followed it. The vendoring commands in step 2 were not run against a
real project. Wrong if a repository that followed it still has a deploy only one person can do, or
spends more on this file than it catches.

## For the owner, new system or old

1. **Put production out of the agent's reach before the first session.** A session can use whatever
   the user's shell can, from its first command: the cloud login, the cluster context, the loaded
   ssh keys, decrypted secret files ([DO1/A3](guide/10-agents.md#a3)). Decide which account and environment an agent may
   touch, arrange for that to be all it *can* touch — a separate profile, a container, a development
   account — and write it in the repository guide. Decide what may be sent to a hosted model
   ([DO1/A9](guide/10-agents.md#a9)). If no such separation exists yet, say so there, and name the commands that need a
   person's word every time ([DO1/A4](guide/10-agents.md#a4)).

2. **Vendor both guides, and check that they load.** From the repository's root, with clones of
   both guides beside it:

   ```sh
   SE=../software-engineering; DO=../devops
   for f in se se.ps1 se.py; do git -C "$SE" show v1.7:"$f" > "$f"; done
   git -C "$SE" show v1.7:AGENTS.md > AGENTS.md
   git -C "$SE" show v1.7:QUICKREF.md > QUICKREF.md
   git -C "$DO" show main:AGENTS.md > AGENTS-DO.md
   chmod +x se
   printf '@AGENTS.md\n@AGENTS-DO.md\n@QUICKREF.md\n' > CLAUDE.md
   ./se --hook --write
   ./se --doctor
   ```

   This guide has no release yet, so the copy comes from `main` and says so; pin to a tag once there
   is one. An existing `AGENTS.md` or `CLAUDE.md` is not overwritten: keep SE1's copy under another
   name (`docs/SE1-AGENTS.md`), and add the three imports to the `CLAUDE.md` you have. Do not edit
   the copies. Then start a fresh session, ask it to quote a line of each file without opening
   them, and look for `se --post`'s report ([SE1/A11](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/10-agents.md#a11)).

3. **Add a template when you reach its stage, not all at once.** A runbook for an operation nobody
   has performed is fiction. *None yet* is a true answer on day one.

## Starting a new system

In order, each from its template in [templates/](templates/):

4. **The operating contract,** with invented numbers marked, read to the client
   ([DO1/R2](guide/04-requirements.md#r2), [DO1/H7](guide/09-people.md#h7)).
5. **The empty system through the whole path to production,** in the first week: one page, one
   image, the real pipeline, the real name, one health check, one alert seen to fire
   ([DO1/W3](guide/08-work.md#w3), [DO1/T3](guide/06-testing.md#t3)).
6. **The inventory,** started with that slice: its environments, its kept state (none yet, and say
   so), its secrets by name, its dated things ([DO1/W5](guide/08-work.md#w5)).
7. **One command up and one to reset, locally,** with the environment's name in everything
   ([DO1/F2](guide/03-feedback.md#f2), [DO1/T5](guide/06-testing.md#t5)), and a doctor for a new laptop ([DO1/H9](guide/09-people.md#h9)).
8. **A runbook for each operation the first time it is done by hand,** then run by someone else
   ([DO1/W8](guide/08-work.md#w8), [DO1/H8](guide/09-people.md#h8)).
9. **A restore, timed, as soon as there is anything to keep** ([DO1/R4](guide/04-requirements.md#r4)).

## Bringing a running system under the guide

Each step keeps what is running from being broken by its own tidying. In this order:

4. **Inventory by observation.** List what is actually running, what it costs, who can log in, and
   everything with a date on it. Every line is *(inferred)* until someone who can decide confirms
   it ([DO1/W9](guide/08-work.md#w9), [DO1/U10](guide/02-unknowing.md#u10)).
5. **Find what must be kept, and restore it once,** somewhere separate, timed. Until that has been
   done, change nothing that holds state ([DO1/D1](guide/05-design.md#d1), [DO1/T3](guide/06-testing.md#t3)).
6. **Check where the backups live.** If the everyday credentials can delete them, fix that before
   anything else ([DO1/O13](guide/07-operating.md#o13)).
7. **Write the operating contract as the system observably behaves,** marked *(inferred)*, and read
   it to the client. The gap between what it does and what they thought was promised is the
   migration's first finding ([DO1/H7](guide/09-people.md#h7)).
8. **Move the deploy off the laptop:** the same steps, in a script the pipeline calls, with its own
   scoped identity ([DO1/O2](guide/07-operating.md#o2), [DO1/O9](guide/07-operating.md#o9)).
9. **Bring one resource at a time under the repository.** Import it, show that applying the
   description changes nothing, then change it. Resources nobody can explain are stopped, watched,
   and only then deleted ([DO1/D6](guide/05-design.md#d6), [DO1/D12](guide/05-design.md#d12)).
10. **Run each mechanism once on purpose:** the rollback, each alert, the budget alarm. What does
    not work goes in FINDINGS with what it changed ([DO1/T3](guide/06-testing.md#t3), [DO1/W7](guide/08-work.md#w7)).

Do not rewrite history to look planned. A record that says when the description arrived is worth
more than one that looks tidy.

## For the agent, in a repository that is only partly adopted

SE1's section holds, and where one of these is missing, this replaces the rule that assumed it. Say
each absence once.

- **No statement of what the session may reach.** Report what you can see is in reach, and treat
  every environment but a local stack as production until told otherwise ([DO1/A3](guide/10-agents.md#a3)).
- **No inventory.** Draft one as a proposal from the repository alone, every line *(inferred)*. Do
  not query a cloud account to fill it in unless asked ([DO1/W9](guide/08-work.md#w9)).
- **No operating contract.** Treat the task's text as the requirement, marked *(inferred)*, and
  invent no numbers without marking them ([DO1/R2](guide/04-requirements.md#r2)).
- **No runbook for what you are asked to do.** Say so, propose the steps, and wait for a person to
  run them the first time ([DO1/W8](guide/08-work.md#w8)).
- **No way to preview a change.** Say the target and the effect before any command, and stop at
  anything destructive ([DO1/A2](guide/10-agents.md#a2), [DO1/A4](guide/10-agents.md#a4)).

## Adoption checklist

- [ ] What an agent's session can reach is written down, and production is outside it or the
      exceptions are named. ([DO1/A3](guide/10-agents.md#a3))
- [ ] `AGENTS.md` and `AGENTS-DO.md` are unedited copies that name where they came from, and a
      fresh session quoted a line of each. ([SE1/A11](https://github.com/wmacevoy/software-engineering/blob/v1.7/guide/10-agents.md#a11))
- [ ] The operating contract's numbers have been read to the client, and the invented ones are
      marked. ([DO1/R2](guide/04-requirements.md#r2))
- [ ] The inventory lists every piece of kept state, every secret by name, and everything with a
      date. ([DO1/W5](guide/08-work.md#w5))
- [ ] One command brings the system up locally, and one resets it. ([DO1/F2](guide/03-feedback.md#f2))
- [ ] The deploy runs from the pipeline, with an identity scoped to one environment. ([DO1/O9](guide/07-operating.md#o9))
- [ ] A restore has been run and timed, and the backup is out of the everyday credentials' reach.
      ([DO1/O13](guide/07-operating.md#o13))
- [ ] The rollback and each paging alert have been seen to work. ([DO1/T3](guide/06-testing.md#t3))
- [ ] Everything in SE1's adoption checklist that applies.
