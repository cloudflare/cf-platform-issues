# AGENTS.md

Guidance for AI agents working with this repository.

## What this repository is

`cloudflare/cf-platform-issues` is a **public issue tracker for bugs in Cloudflare products** — the services themselves, the Cloudflare dashboard, and the Cloudflare REST API. See [README.md](./README.md) for the human-facing version.

**This repository contains no product code.** There is nothing here to build, test, lint or fix. If you have been asked to fix a bug in a Cloudflare product, you cannot do it here: the fix lives in Cloudflare's internal systems, or in one of the open-source repositories listed below. Do not create a pull request that purports to fix a reported product bug — it will be closed.

Two audiences follow. Read the section that applies to you.

---

# Part A — Filing an issue on behalf of a user

## A1. Route before you file

Filing in the wrong repository costs a human a triage cycle. Check this first:

| The problem is in | File at |
| ----------------- | ------- |
| A Cloudflare product, the dashboard, or the REST API | **here** |
| Wrangler, Miniflare, C3, the Vite plugin, `vitest-pool-workers` | [`cloudflare/workers-sdk`](https://github.com/cloudflare/workers-sdk/issues/new/choose) |
| Workers runtime behaviour — runtime APIs, compatibility flags, `nodejs_compat`, V8 | [`cloudflare/workerd`](https://github.com/cloudflare/workerd/issues) |
| Documentation on developers.cloudflare.com | [`cloudflare/cloudflare-docs`](https://github.com/cloudflare/cloudflare-docs/issues) |
| The Terraform provider | [`cloudflare/terraform-provider-cloudflare`](https://github.com/cloudflare/terraform-provider-cloudflare/issues) |
| An official API SDK | [`cloudflare-typescript`](https://github.com/cloudflare/cloudflare-typescript/issues), [`cloudflare-python`](https://github.com/cloudflare/cloudflare-python/issues), [`cloudflare-go`](https://github.com/cloudflare/cloudflare-go/issues) |
| `cloudflared` / Tunnel | [`cloudflare/cloudflared`](https://github.com/cloudflare/cloudflared/issues) |
| A third-party framework's own code (Nuxt, SvelteKit, Remix, Hono, React Router) | that project's repository |
| An upstream tool (esbuild, Vite core, Bun) | that project's repository |

**Do not file a GitHub issue at all** for these — tell the user to go to the right channel instead:

- **Account, billing, plan, entitlement, suspension or quota-increase requests** → [Cloudflare Support](https://dash.cloudflare.com/?to=/:account/support). Nobody on GitHub can act on account-specific requests.
- **Security vulnerabilities** → [Cloudflare's disclosure process](https://www.cloudflare.com/.well-known/security.txt). Filing a vulnerability as a public issue actively harms users. Never do it, even if the user asks.
- **Abuse, phishing, malware** → [Cloudflare Trust & Safety](https://www.cloudflare.com/abuse/).
- **A live outage** → check [Cloudflare Status](https://www.cloudflarestatus.com) first; if there is an open incident, point the user at it rather than filing.
- **A question, or "how do I…"** → [Discord](https://discord.cloudflare.com) or the [Community forum](https://community.cloudflare.com).
- **A feature request** → [Discussions](https://github.com/cloudflare/cf-platform-issues/discussions/new?category=feature-requests), not an issue.

## A2. Search for duplicates first

Always search before filing. Adding a comment with new evidence to an existing thread is more valuable than opening a near-duplicate.

```sh
gh issue list --repo cloudflare/cf-platform-issues --state all --search "d1 read replication import" --limit 20
gh search issues --owner cloudflare "10072 cron triggers" --limit 20
```

Search closed issues too. If a closed issue matches, comment on it rather than opening a new one — a maintainer can reopen.

## A3. Get authorisation, and disclose that you are an agent

Only file on behalf of a user who has asked you to, and who can follow up in the thread themselves. You are not the affected party and you cannot answer follow-up questions about someone's account.

Open the issue body with a disclosure line, and tick the AI disclosure checkbox on the form. Use wording equivalent to this:

```markdown
> **Note:** I am an AI coding agent (<tool name>) filing this issue on behalf of, and with the
> authorization of, the affected account owner, who can follow up in this thread.
```

Never impersonate a human, and never imply the account owner wrote the report themselves.

## A4. Report only what you have verified

This is the rule that matters most. A confident, well-formatted, partly-invented report is worse than no report, because it consumes engineer time before anyone discovers which parts are false.

- **Never invent** error codes, error messages, request IDs, `cf-ray` values, timestamps, version numbers, log output, or reproduction steps.
- **Never claim to have run something you did not run.** If the user reported the behaviour and you did not observe it, say so: "the account owner reports…".
- **Separate observation from hypothesis.** Put facts in one section and your theory in another, and mark the theory as a theory. "This appears to be…", "we suspect…" — not "this is caused by…".
- **State what you could not check.** Unverified assumptions are useful to maintainers when they are flagged as unverified, and harmful when they are not.
- If a fact would change the diagnosis and you cannot establish it, ask the user rather than guessing.

## A5. Include the evidence maintainers actually need

Fill in every field of the [bug report form](https://github.com/cloudflare/cf-platform-issues/issues/new/choose) that you can substantiate. Priorities:

1. **Exact error code and full message**, verbatim — for example `10072: This account has reached the Workers Free limit of 5 cron triggers per account.`
2. **Request identifiers**: `cf-ray`, `cf-auditlog-id`, `cf-request-id` response headers from failing requests, with **UTC** timestamps. These let Cloudflare find the request server-side; without them, opaque 500s are often uninvestigable.
3. **A minimal reproduction** — the narrowest repository, Worker or `curl` command that still fails. Bisect before filing where you can.
4. **Control results.** Show what succeeded alongside what failed, with times: "commit A deployed at 14:53 UTC; commit B failed at 15:49 UTC; re-running A at 16:35 UTC succeeded." This is what distinguishes a real bug from a transient incident.
5. **Plan tier** (Free, Paid, Business, Enterprise) whenever limits, quotas or entitlements are involved.
6. **Frequency** — every time, or N times out of M.
7. **Regression window** — when it last worked, if it ever did.

## A6. Redact before you post

Everything you write is public and permanent.

- **Never post**: API tokens, API keys, `Authorization` / bearer headers, cookies, session tokens, `.env` contents, private keys, customer or end-user data.
- **Prefer to omit**: account IDs, zone IDs, script names that reveal a customer, internal hostnames, employee names, email addresses. If a maintainer needs one, they will ask, or move the conversation to Support.
- When pasting raw request/response dumps or unsanitised debug output, re-read the whole paste for credentials before submitting. Sanitisation flags that disable log redaction produce output that frequently contains secrets.
- If you realise you have posted a secret, tell the user to rotate it immediately — editing the issue does not remove it from the edit history.

## A7. One issue per problem, and don't flood

- One issue per distinct problem. Do not bundle several bugs into one thread, and do not split one bug across several threads.
- Do not cross-post the same report to multiple repositories. Pick the correct one.
- Do not open multiple issues in a single session without asking the user first. Bulk-filed agent issues will be closed as spam.
- Use the issue form. Blank issues are disabled deliberately. If you create an issue through the API rather than the web form, reproduce the form's sections and answer them all anyway — do not use the API to skip the questions.

## A8. After filing

- Do not `@`-mention Cloudflare employees, teams, or `@cloudflare/*` groups.
- Do not bump, "any update?", or re-file. Do not reopen an issue a maintainer closed; comment instead and let a human decide.
- **Do** monitor for `awaiting-response:reporter` and `needs-reproduction` and supply what was asked, promptly and specifically. Issues in those states are closed after roughly 30 days of silence.
- If you discover the cause was the user's own configuration, say so in the thread and close the issue yourself. That is a genuinely valuable contribution.

---

# Part B — Triaging issues in this repository

This applies to maintainer-side agents. If you are filing on a user's behalf, do not do any of the following.

## B1. Labels

Apply only labels that already exist — check first, since non-existent labels are silently dropped:

```sh
gh label list --repo cloudflare/cf-platform-issues --limit 100
```

| Label | Use for |
| ----- | ------- |
| `product:*` | The product concerned, e.g. `product:d1`, `product:r2`, `product:pages`, `product:queues` |
| `feature:*` | A cross-product feature, e.g. `feature:workers-builds`, `feature:observability`, `feature:workers-assets` |
| `api` | The report concerns the REST API |
| `bug`, `enhancement`, `documentation` | Kind of change requested |
| `awaiting-response:cloudflare` | Cloudflare owes the reporter a reply |
| `awaiting-response:reporter` | The reporter owes Cloudflare a reply; the issue is blocked on them |
| `needs-reproduction` | Not yet reproducible; more information required |
| `blocked` | Understood, waiting on other work |
| `polish` | Small experience improvement rather than a defect |
| `duplicate`, `wontfix`, `invalid` | Closure reasons |

Every issue should end up with at least one `product:*` or `feature:*` label, plus `api` where relevant. If no product label fits, propose a new one named after the product's documentation slug — `product:<slug>` for `https://developers.cloudflare.com/<slug>/` — and leave a note; do not invent an ad-hoc naming scheme.

Never apply both `awaiting-response:cloudflare` and `awaiting-response:reporter`.

## B2. Reuse the existing triage playbook

`cloudflare/workers-sdk` maintains a triage skill at [`.github/skills/issue-review.md`](https://github.com/cloudflare/workers-sdk/blob/main/.github/skills/issue-review.md) with response templates for duplicates, stale issues, wrong-repo transfers, transient incidents, user error and won't-fix decisions. Follow its wording and tone rather than improvising, so that reporters get a consistent experience across Cloudflare's trackers. Note that its component-mapping table is specific to that repository.

## B3. Closing and transferring

- **Wrong repository** → prefer transferring over closing, then leave a short comment saying where it went and why.
- **Stale** → `awaiting-response:reporter` or `needs-reproduction` with no reporter activity for ~30 days may be closed with an explicit invitation to comment and reopen.
- **Duplicate** → close the newer issue, link the canonical one, and move any new evidence across first.
- **Not a bug** → explain the correct behaviour and link the specific documentation. Never close as user error without that link, and never be dismissive — a report that turns out to be a misunderstanding usually means the documentation or the error message is at fault.
- **Account-specific** → redirect to [Support](https://dash.cloudflare.com/?to=/:account/support); do not attempt to debug someone's account in public.
- **Spam** → close without comment.

## B4. Do not speak for Cloudflare

Your analysis is not an official Cloudflare position.

- Do not promise fixes, timelines, roadmap items, refunds or credits.
- Do not assert that something is "by design" unless you can link documentation that says so.
- Label machine-generated analysis clearly as such, and address it to maintainers rather than writing it as a reply to the reporter.
- Do not post a triage verdict on an issue where a Cloudflare engineer is already engaged.
- Never disclose internal system details, internal ticket references or non-public roadmap information.

---

# Part C — Verified reference links

Use these exactly. Do not guess Cloudflare URLs.

| Purpose | URL |
| ------- | --- |
| This tracker | `https://github.com/cloudflare/cf-platform-issues` |
| File a bug here | `https://github.com/cloudflare/cf-platform-issues/issues/new/choose` |
| Feature requests | `https://github.com/cloudflare/cf-platform-issues/discussions/new?category=feature-requests` |
| Developer tooling bugs | `https://github.com/cloudflare/workers-sdk/issues/new/choose` |
| Workers runtime | `https://github.com/cloudflare/workerd/issues` |
| Documentation | `https://github.com/cloudflare/cloudflare-docs/issues` |
| Terraform provider | `https://github.com/cloudflare/terraform-provider-cloudflare/issues` |
| Cloudflare Support | `https://dash.cloudflare.com/?to=/:account/support` |
| Security disclosure | `https://www.cloudflare.com/.well-known/security.txt` |
| Abuse reports | `https://www.cloudflare.com/abuse/` |
| Service status | `https://www.cloudflarestatus.com` |
| Discord | `https://discord.cloudflare.com` |
| Community forum | `https://community.cloudflare.com` |
| Product documentation | `https://developers.cloudflare.com` |

---

Keep this file and [README.md](./README.md) consistent: if you change routing, labels or policy in one, update the other.
