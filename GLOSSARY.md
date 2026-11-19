# Glossary

The terms this guide uses precisely, beyond those in [SE1's glossary](https://github.com/wmacevoy/software-engineering/blob/v1.7/GLOSSARY.md) — K0–K4, K3
factory, cache, contract, one-way door, and the rest mean the same here. One thing, one word
([DO1/D9](guide/05-design.md#d9)): if you find the guide using one of these loosely, that is a bug.

| Term                   | Meaning                                                                                       |
| ---------------------- | --------------------------------------------------------------------------------------------- |
| **Apply**              | The moment a description becomes a change to something running. The operational form of *publish* ([DO1/A6](guide/10-agents.md#a6)). |
| **Artifact**           | The built thing that is deployed, named by something that cannot move, such as an image digest ([DO1/D7](guide/05-design.md#d7)). |
| **Cold start**         | Rebuilding the whole system from the repository and the backups into an empty account ([DO1/T11](guide/06-testing.md#t11)). |
| **Deploy**             | Putting a new artifact into an environment. Not the same event as a *release* ([DO1/O6](guide/07-operating.md#o6)).     |
| **Doctor**             | One command that checks a workstation or a repository, one line per check, nonzero on failure ([DO1/T2](guide/06-testing.md#t2)). |
| **Drift**              | A difference between what the repository declares and what is running, made without the repository ([DO1/U3](guide/02-unknowing.md#u3)). |
| **Environment**        | One complete copy of the system, with a name that appears in everything it contains ([DO1/T5](guide/06-testing.md#t5)). |
| **Error budget**       | The failure an availability target allows in a period; spent by change ([DO1/R3](guide/04-requirements.md#r3)).            |
| **Freeze**             | A stated period in which nothing is deployed ([DO1/R6](guide/04-requirements.md#r6)).                                      |
| **Gate**               | A check in the path that can stop a rollout without a person ([DO1/O5](guide/07-operating.md#o5)).                      |
| **Inventory**          | The record of what exists: environments, kept state, secrets by name, identities, dated things ([template](templates/INVENTORY.md)). |
| **Kept state**         | State that cannot be derived from the repository, and so must be backed up ([DO1/D1](guide/05-design.md#d1)).        |
| **Operating contract** | The system's promises while running: availability, recovery, cost, who is told ([template](templates/OPERATIONS_v1.md)). |
| **Page**               | A signal that interrupts a person now. Distinct from a *ticket* and a *record* ([DO1/O3](guide/07-operating.md#o3)).    |
| **Path**               | Everything between a change in someone's head and a running, watched, restorable system: laptop, repository, pipeline, registry, environments ([DO1/P1](guide/01-purpose.md#p1)). |
| **Pin**                | A reference that cannot resolve differently tomorrow: a digest, an exact version, a commit ([DO1/O1](guide/07-operating.md#o1)). |
| **Pipeline**           | The automated part of the path. It is production ([DO1/R8](guide/04-requirements.md#r8)).                                  |
| **Preflight**          | A doctor run at the start of a deploy ([DO1/T2](guide/06-testing.md#t2)).                                             |
| **Preview**            | A tool's statement of what an apply would create, change, and destroy ([DO1/A2](guide/10-agents.md#a2)).             |
| **Rebuildable state**  | State that can be discarded and derived again from the repository ([DO1/D1](guide/05-design.md#d1)).                 |
| **Release**            | Making a change visible to users. May follow its deploy by days ([DO1/O6](guide/07-operating.md#o6)).                   |
| **Repair / replace**   | Changing a running machine in place, or building a new one from the description and discarding the old ([DO1/D14](guide/05-design.md#d14)). |
| **Runbook**            | The written steps for one operation, with the date it was last run and how long it took ([template](templates/RUNBOOK.md)). |
| **Saturation**         | How close a resource is to its limit ([DO1/F4](guide/03-feedback.md#f4)).                                              |
| **Switch**             | Configuration that turns shipped code on or off without a deploy; a feature flag ([DO1/O6](guide/07-operating.md#o6)).  |
| **Toil**               | Operational work that is manual, repetitive, leaves nothing better, and grows with the system ([DO1/W8](guide/08-work.md#w8)). |
| **Torn deploy**        | A deploy that stopped partway and looks finished ([DO1/O4](guide/07-operating.md#o4)).                                  |
