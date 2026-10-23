# rzv-prd-smoke

Public reliability checks for [Runzivo](https://runzivo.dev) GitHub Actions runners
in production. Each run starts a fresh Runzivo runner, checks that Docker works,
holds the runner for a set time and records the result. The run history in
[Actions](https://github.com/Runzivo/rzv-prd-smoke/actions) is a public record of
how the runners behave over time.

## Why this repository is public

We want customers and anyone evaluating Runzivo to be able to see real runs:
how long jobs wait for a runner, whether they start and how long they run.
Nothing in this repository is secret.

## What runs

| Workflow                | Trigger                                 | What it does                                                                                                                       |
| ----------------------- | --------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| `prd reliability smoke` | Every hour at :17 UTC, until 2026-11-23 | One job on each runner size below. The hold time follows a 12-hour cycle: 2, 5, 10, 15, 20, 25, 30, 35, 40, 45, 50 and 55 minutes. |
| `prd smoke`             | Manual only                             | One Docker check on a chosen runner size, with an optional hold.                                                                   |

Runner sizes:

- `rzv-ubuntu-24.04-x64-2vcpu-8gb`
- `rzv-ubuntu-24.04-x64-4vcpu-16gb`

Each job writes a summary with the runner label, hold time, start and end time
and result. Queue wait is measured from GitHub's run and job timestamps.

## Safety

- The workflows run only on a schedule or a manual dispatch by maintainers.
  They never run on pull requests.
- Workflow permissions are `{}`. The jobs use no secrets and no checkout.
- Each job runs in its own short-lived microVM, which is discarded afterwards.

Workflow runs from fork pull requests need a maintainer's approval, and
maintainers do not approve them. To report a problem with the runners, contact
support through [runzivo.dev](https://runzivo.dev).

## Use the runners yourself

See the [Runzivo documentation](https://runzivo.dev/docs) and the
[runner templates](https://runzivo.dev/docs/runner-templates).
