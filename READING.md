# Reading

The longer arguments behind the guide, one line each, grouped by the chapter they support most. For
software engineering's sources, see [SE1's READING](https://github.com/wmacevoy/software-engineering/blob/v1.7/READING.md).

**None of these was opened during the drafting.** Each line is the drafting model's recollection of
what the source says ([GAPS 3](GAPS.md#3-nothing-cited-was-opened)). Check before you quote.

**Purpose and people** ([1](guide/01-purpose.md), [9](guide/09-people.md))

- **John Allspaw and Paul Hammond, "10+ Deploys per Day: Dev and Ops Cooperation at Flickr"**
  (talk, 2009) — the talk usually credited with starting the conversation the field is named for.
- **Werner Vogels, interviewed in ACM Queue, "A Conversation with Werner Vogels"** (2006) — the
  source of "you build it, you run it."
- **Gene Kim, Kevin Behr, and George Spafford, *The Phoenix Project*** (2013) — a novel about a wall
  between development and operations, and taking it down.
- **Melvin Conway, "How Do Committees Invent?"** (1968) — systems copy the communication structure
  of the organizations that design them.
- **John Allspaw, "Blameless PostMortems and a Just Culture"** (2012) — why the account you need
  comes only from someone who does not expect punishment.
- **Sidney Dekker, *The Field Guide to Understanding 'Human Error'*** (2006) — human error as the
  start of an investigation, not its conclusion.

**Feedback** ([3](guide/03-feedback.md))

- **Nicole Forsgren, Jez Humble, and Gene Kim, *Accelerate*** (2018) — the survey research behind
  the four delivery measures, and the finding that throughput and stability go together.
- **Jez Humble and David Farley, *Continuous Delivery*** (2010) — the deployment pipeline, build
  once, and "if it hurts, do it more often."

**Requirements, operating** ([4](guide/04-requirements.md), [7](guide/07-operating.md))

- **Betsy Beyer, Chris Jones, Jennifer Petoff, and Niall Richard Murphy (eds.), *Site Reliability
  Engineering*** (2016) — service-level objectives, error budgets, toil, the four signals to
  monitor, and incident roles, as one company practiced them.
- **Betsy Beyer and others (eds.), *The Site Reliability Workbook*** (2018) — the same ideas with
  worked examples, including alerting on budget burn.

**Design** ([5](guide/05-design.md))

- **Adam Wiggins, "The Twelve-Factor App"** (2011) — one codebase, configuration in the
  environment, build and run kept apart, disposable processes.
- **Kief Morris, *Infrastructure as Code*** (2016; later editions) — infrastructure described in
  files, applied by tools, and the patterns for doing it without drift.
- **Randy Bias, "The History of Pets vs Cattle"** (2016) — where the pets-and-cattle picture of
  repairing versus replacing machines came from.

**Testing** ([6](guide/06-testing.md))

- **Ali Basiri and others, "Chaos Engineering"** (IEEE Software, 2016) — injecting failure into
  production as an experiment with a stated hypothesis.
- **GitLab, "Postmortem of database outage of January 31"** (2017) — a public account of an
  accidental deletion and of backups that had not been working.

**Operating: cases** ([7](guide/07-operating.md))

- **U.S. Securities and Exchange Commission, order in the matter of Knight Capital Americas LLC**
  (2013) — a deploy that reached some servers and not others, and a reused switch.
- **Amazon Web Services, "Summary of the Amazon S3 Service Disruption in the Northern Virginia
  Region"** (2017) — one mistyped input to a routine command, and the tool changed afterward.

**Work** ([8](guide/08-work.md))

- **Gene Kim, Jez Humble, Patrick Debois, and John Willis, *The DevOps Handbook*** (2016) — the
  practices, organized as flow, feedback, and learning.
- **Thomas Limoncelli, "Manual Work is a Bug"** (ACM Queue, 2018) — the order this guide borrows:
  do it by hand, document it, then automate it.
