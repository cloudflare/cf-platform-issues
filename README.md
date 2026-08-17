# Cloudflare Platform Issues

A public issue tracker for bugs in **Cloudflare products** — the services themselves, the Cloudflare dashboard, and the Cloudflare REST API.

There is no code in this repository. It exists purely so that people outside Cloudflare have somewhere public to report product bugs, and so that those reports reach the teams that own the products.

- 🐛 [**Report a bug**](https://github.com/cloudflare/cf-platform-issues/issues/new/choose)
- 💡 [**Request a feature**](https://github.com/cloudflare/cf-platform-issues/discussions/new?category=feature-requests)
- 💬 [Ask a question on Discord](https://discord.cloudflare.com) or the [Community forum](https://community.cloudflare.com)

## Why this repository exists

For a long time, [`cloudflare/workers-sdk`](https://github.com/cloudflare/workers-sdk) was the only public place to report anything Cloudflare-related. That repository contains Cloudflare's open-source developer tooling — Wrangler, Miniflare, C3, the Vite plugin — so reports that were really about a *product* rather than about that tooling had nowhere sensible to live.

Many of the issues you see here were transferred out of `workers-sdk` for exactly that reason. **If your issue was moved here, nothing was lost**: the thread, its comments and its subscribers came with it, and you can keep commenting as normal.

## Is this the right place?

### Yes — file here

- A product does not behave the way its documentation says it does.
- The REST API returns the wrong result, an opaque error, or a 500.
- Something in the Cloudflare dashboard is broken, or disagrees with the API.
- A limit or quota is enforced incorrectly for your plan.
- The API is missing a capability that the product clearly needs (for example, no way to delete a resource that can be created).

### No — these go elsewhere

| What | Where |
| ---- | ----- |
| Bugs in Wrangler, Miniflare, C3, the Vite plugin, `vitest-pool-workers` | [`cloudflare/workers-sdk`](https://github.com/cloudflare/workers-sdk/issues/new/choose) |
| Workers runtime behaviour — runtime APIs, compatibility flags, Node.js compatibility, V8 | [`cloudflare/workerd`](https://github.com/cloudflare/workerd/issues) |
| Documentation on developers.cloudflare.com | [`cloudflare/cloudflare-docs`](https://github.com/cloudflare/cloudflare-docs/issues) |
| Terraform provider | [`cloudflare/terraform-provider-cloudflare`](https://github.com/cloudflare/terraform-provider-cloudflare/issues) |
| Official API SDKs | [`cloudflare-typescript`](https://github.com/cloudflare/cloudflare-typescript/issues), [`cloudflare-python`](https://github.com/cloudflare/cloudflare-python/issues), [`cloudflare-go`](https://github.com/cloudflare/cloudflare-go/issues) |
| `cloudflared` / Tunnel client | [`cloudflare/cloudflared`](https://github.com/cloudflare/cloudflared/issues) |
| Rust Workers | [`cloudflare/workers-rs`](https://github.com/cloudflare/workers-rs/issues) |
| Anything specific to **your account** — billing, plans, entitlements, suspensions, quota increases | [Cloudflare Support](https://dash.cloudflare.com/?to=/:account/support) |
| **Security vulnerabilities** — never file these publicly | [Cloudflare's disclosure process](https://www.cloudflare.com/.well-known/security.txt), see [SECURITY.md](./SECURITY.md) |
| Abuse, phishing or malware | [Cloudflare Trust & Safety](https://www.cloudflare.com/abuse/) |
| A live outage | [Cloudflare Status](https://www.cloudflarestatus.com), then [Support](https://dash.cloudflare.com/?to=/:account/support) |
| Questions, "how do I…", debugging help | [Discord](https://discord.cloudflare.com), [Community forum](https://community.cloudflare.com) |
| Feature requests and ideas | [Discussions](https://github.com/cloudflare/cf-platform-issues/discussions/new?category=feature-requests) |

If you file in the wrong place, we will usually transfer or redirect the issue rather than just close it.

## Writing a report we can act on

The [bug report form](https://github.com/cloudflare/cf-platform-issues/issues/new/choose) asks for most of this, but in short:

- **One problem per issue.** Two bugs in one thread means one of them gets forgotten.
- **Include a reproduction.** The smallest repository, Worker or `curl` command that shows the problem. If you cannot share one, give exact steps instead.
- **Separate expectation from observation.** State what you expected, what actually happened, and what made you expect otherwise — ideally a link to the docs.
- **Give us identifiers.** The exact error code and message, plus `cf-ray` / `cf-auditlog-id` response headers and **UTC** timestamps from failing requests. These let us find your traffic in our own logs, and they are frequently the difference between a report we can fix and one we cannot.
- **Say what *did* work.** "Deploy A succeeded at 14:53, deploy B failed at 15:49, re-running A at 16:35 succeeded" is far more useful than "deploys are broken".
- **Mention your plan** (Free, Paid, Business, Enterprise) for anything involving limits, quotas or entitlements.
- **Say whether it is intermittent**, and roughly how often it reproduces.

### Everything here is public

Redact API tokens, keys, credentials, cookies, customer data and private hostnames before you post. Prefer leaving account, zone and script IDs out; if a specific ID is needed we will ask, or we will move the conversation to [Support](https://dash.cloudflare.com/?to=/:account/support). Do not paste anything you would not want indexed by a search engine.

## AI-assisted reports

Reports written with the help of an AI agent are welcome. A well-evidenced agent-written report is more useful than a vague human-written one. Two conditions:

1. **Say so.** Disclose it at the top of the issue, and tick the disclosure checkbox on the form.
2. **Make sure it is true.** Invented error codes, invented reproduction steps and confidently-worded speculation waste more of everyone's time than filing nothing at all.

If you *are* an agent, read [AGENTS.md](./AGENTS.md) first.

## How triage works

Issues are labelled to route them and to make the state of the conversation obvious.

| Label | Meaning |
| ----- | ------- |
| `product:*` | The product the report concerns, for example `product:d1`, `product:r2`, `product:pages` |
| `feature:*` | A cross-product feature, for example `feature:workers-builds`, `feature:observability` |
| `api` | Concerns the Cloudflare REST API |
| `bug`, `enhancement`, `documentation` | What kind of change is being asked for |
| `awaiting-response:cloudflare` | **We** owe you a reply |
| `awaiting-response:reporter` | **You** owe us a reply — the issue is blocked until you answer |
| `needs-reproduction` | We cannot reproduce it yet and need more from you |
| `blocked` | Understood, but waiting on other work |
| `polish` | A small improvement to the experience rather than a defect |
| `duplicate`, `wontfix`, `invalid` | Why an issue was closed |

New `product:*` labels get created as new products show up here, so do not worry if there is no obvious label for yours.

### What to expect

- **This is not a support channel and there is no SLA.** Issues here are read and routed by the teams that own each product, but response times vary widely, and an unanswered issue is not an escalation path. If something is breaking your business right now, go to [Support](https://dash.cloudflare.com/?to=/:account/support).
- **Issues labelled `awaiting-response:reporter` or `needs-reproduction` are closed after roughly 30 days of silence.** That is housekeeping, not a verdict — comment and we will reopen.
- **Closed does not always mean fixed.** We try to say why, and to link the relevant documentation, release or canonical issue.
- Please do not bump threads or @-mention individual Cloudflare employees. Adding a 👍 reaction to the first post is the useful signal; it is what we sort by.

## Pull requests

There is nothing here to patch — no product code lives in this repository. The only pull requests we can accept are fixes to the files that run the tracker itself: this README, [AGENTS.md](./AGENTS.md), the issue templates, and the policy documents.

To contribute to Cloudflare's open-source projects, see [`workers-sdk`](https://github.com/cloudflare/workers-sdk/blob/main/CONTRIBUTING.md), [`workerd`](https://github.com/cloudflare/workerd) or [`cloudflare-docs`](https://github.com/cloudflare/cloudflare-docs).

## Also in this repository

- [AGENTS.md](./AGENTS.md) — instructions for AI agents filing or triaging issues here
- [SECURITY.md](./SECURITY.md) — how to report a security vulnerability
- [CODE_OF_CONDUCT.md](./CODE_OF_CONDUCT.md) — how we expect people to behave here
