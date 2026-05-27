# supabase-keepalive

Scheduled GitHub Actions workflow that pings a Supabase free-tier project every 4 days to prevent automatic pausing.

Supabase pauses free-tier projects after 7 days of inactivity. This workflow hits the project's `/health` endpoint on a 4-day cadence, keeping it active without requiring a paid plan upgrade.

---

## Setup

**1. Fork or clone this repo.**

**2. Add a repository secret:**

Go to **Settings → Secrets and variables → Actions → New repository secret**.

| Name | Value |
|------|-------|
| `SUPABASE_URL` | Your project URL, e.g. `https://abcdefghijklmn.supabase.co` |

Your project URL is found in the Supabase dashboard under **Project Settings → API**.

**3. Enable Actions** if not already enabled under the **Actions** tab.

That's it. The workflow runs automatically on schedule.

---

## Manual trigger

Go to **Actions → Supabase Keepalive → Run workflow** to fire it immediately.

---

## Schedule

Runs every 4 days at 08:00 UTC. Adjust the cron expression in `.github/workflows/keepalive.yml` if needed.

```
0 8 */4 * *
```

---

## What it does

- Calls `GET /health` on your Supabase project (no auth required, no data accessed)
- Validates the response status is `ok`
- Exits with a non-zero code and fails the run if the project is unreachable or unhealthy — so you get a GitHub notification if something is wrong

---

## Notes

- No Supabase credentials are stored or transmitted beyond the project URL
- RLS is unaffected — the health endpoint is public and reads no table data
- If you upgrade to a paid Supabase plan, you can safely delete this repo or disable the workflow
