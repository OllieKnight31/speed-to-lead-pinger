# speed-to-lead-pinger

A scheduler. Nothing else lives here.

Every 5 minutes GitHub Actions makes one authenticated POST to a relay endpoint, which
checks whether any high-intent lead has been sitting unclaimed and posts to Slack if so.

Public purely so the scheduled workflow runs on GitHub's free unlimited-minutes tier for
public repositories. There is no application code, no data and no credentials here — the
endpoint URL and bearer token are encrypted repository secrets:

| Secret | Purpose |
|---|---|
| `RELAY_ESCALATE_URL` | The endpoint to ping |
| `ESCALATE_SECRET` | Bearer token the endpoint checks |

Only `schedule` and `workflow_dispatch` can trigger the workflow, so a fork cannot run it,
and GitHub never exposes secrets to forks regardless.

**Note on timing:** GitHub's scheduled runs drift and are sometimes dropped under load.
The endpoint is idempotent and has no time window, so a late or missed run means a lead is
escalated slightly late — never skipped, never escalated twice.
