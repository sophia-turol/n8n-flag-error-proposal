# “Flag an Error” Button Proposal


## Goals

Deliver a one‑click way for <code>flagging inconsistencies</code> across n8n's community-facing content and doc hubs. When <b>flagged directly</b> from the page,

- typos
- broken instructions/images
- configuration errors, etc.

can be surfaced, triaged, and improved <b>within hours instead of days</b>. This simple mechanism contributes to maintaining quality/reputation when proofreading capacities are limited, as well as meaningfully engages the n8n's community.


## Existing feedback landscape (quick audit, Jun 2025)

| Channel                                   | Current mechanism                                                    | Gaps                                                                                       |
| ----------------------------------------- | -------------------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| <a href="https://docs.n8n.io/" target="_blank">docs.n8n.io</a>                           | “Was this page helpful? 👍 / 👎” → leads to a GitHub issue submit     | No screenshot; users must leave the page; slug labelling inconsistent                      |
|  <a href="https://n8n.io/" target="_blank">Workflow templates</a>  | None (only a security‑focused “Report a vulnerability” footer link) | No path for typos or broken instructions—the highest‑traffic marketing pages lack a QA loop |
| <a href="https://n8n.io/" target="_blank">Blog</a>                    | None (no comments, no feedback CTA)                                | Same gap as templates                                                                     |

➡️ **Opportunity:** Extend a single, unified “report an error” widget to *all* customer‑facing content, so feedback UX is consistent.


## Current painpoints

<ol>
<li style="padding-bottom: 6px;">Minor copy issues linger until randomly noticed or a power user opens a forum post. For instance, <a href="https://n8n.io/workflows/2342-handling-appointment-leads-and-follow-up-with-twilio-calcom-and-ai/" target="_blank">this workflow template</a> features some typos and visual impairments, [jump to real issues found](#real-issues-found)</li>
<li>Docs have a partial fix loop; templates + blog have none—creates an uneven brand experience</li>
<li>Fragmentation makes it hard to quantify content quality across platforms</li>
</ol>


## Proposition

Implement a lightweight **floating flag button**, labelled simply as **“flag an error”** that appears in the bottom‑right corner of every public content page. (In terms of microcopy for the button, iterations are necessary to comply with the n8n's tone of voice and maintained user personas/spectrums.) 


### Sample UI wireframe (text)

```
┌───────────────────────────────────────────────┐
│  Page header                     […]         │
│───────────────────────────────────────────────│
│  (Page body)                                 │
│                                               │
│                        ⧉ Flag an error  ⟶     │  ← fixed bottom‑right
└───────────────────────────────────────────────┘
```


### Click → Modal

| Field           | Autofill               | Notes                    |
| --------------- | ---------------------- | ------------------------ |
| URL         | current page           | hidden input             |
| Screenshot  | auto snapshot          | small preview            |
| Description | —                      | 280‑char textarea        |
| Category    | Copy / Config / Other  | dropdown                 |
| Platform    | docs / template / blog | auto‑detected, read‑only |
| CTA         | **Submit & copy**      | copies ticket URL        |

Sample toast: “Nice catch! We're on it.” (In terms of microcopy for the button, iterations are necessary to comply with the n8n's tone of voice and maintained user personas/spectrums.)


## Back-end flow

1. A widget POSTs `/api/report-error` with {url, platform, category, description, screenshot}.
2. A server creates an issue in **n8n-io/n8n-docs** (same repo currently used by docs widget).
3. **Labels**: `content-bug`, `platform:<docs|template|blog>`, `slug:<url‑slug>`.
4. A Slack webhook notifies `#content‑qa`.
5. A triage bot routes to correct the owner based on the label.

> *Note:* Leverages the **existing GitHub flow** already trusted for docs—no new repository governance needed.


## Benefits

* **Consistency:** a unified feedback UX across docs, templates, blog, etc.
* **Faster improvement cycles:** a screenshot + slug automate reproduction for authors; delivery time cut from days to hours
* **Quantifiable quality:** a single label schema enables dashboards (most‑flagged slugs, time‑to‑close)
* **Community trust:** visible mechanic signals attentiveness to detail


## Real issues found

| Page | URL slug                                                           | Error                           | Fix        |
| -------- | ------------------------------------------------------------------ | ------------------------------- | -------------------- |
| <a href="https://n8n.io/workflows/2342-handling-appointment-leads-and-follow-up-with-twilio-calcom-and-ai/" target="_blank">workflow template</a> | handling-appointment-leads-and-follow-up-with-twilio-calcom-and-ai | “…track all our **enquries**.”  | “enquiries”          |
| <a href="https://n8n.io/workflows/2342-handling-appointment-leads-and-follow-up-with-twilio-calcom-and-ai/" target="_blank">workflow template</a> | handling-appointment-leads-and-follow-up-with-twilio-calcom-and-ai | “agent is able **reschedule**.” | “able to reschedule” |
| <a href="https://n8n.io/workflows/2342-handling-appointment-leads-and-follow-up-with-twilio-calcom-and-ai/" target="_blank">workflow template</a> | handling-appointment-leads-and-follow-up-with-twilio-calcom-and-ai | <img width="705" alt="Screenshot 2025-06-26 at 18 33 54" src="https://github.com/user-attachments/assets/6a87313e-f7e5-422e-9173-56847e18fff3" /></img>  | fix the wireframe elements          |


## Next steps

1. **Design:** finalize icon style & placement (½ day)
2. **Dev:** build a widget + `/api/report-error` + GitHub/Slack integration (1 dev‑day)
3. **Pilot:** enable on 50 % of template pages and one blog post for 2 weeks; capture click‑through & issue volume
4. **Rollout:** extend to all public pages; add dashboards



> *Authored by*: Sophia Turol
> *Date*: 26 Jun 2025
> *License*: CC BY 4.0
