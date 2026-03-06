# PRD: Lindy Bot Outbound Calls for Pending Offers

**Date:** 2026-03-06
**Status:** Draft
**Author:** Recess CS/Ops

---

## 1. Overview & Problem Statement

**Feature:** Lindy Bot Outbound Call Automation for Pending Offers

**Problem:** When offers sit in "submitted" state without organizer action, the CS/Ops team needs to proactively call organizers to drive activations. Currently this requires manual steps across two tools with no standardized logic, making execution inconsistent and time-consuming.

**Goal:** Define a repeatable, rule-based process to populate the Lindy Bot call sheet from the Offer Tracker, ensuring the right organizers are called with the right context.

**Versions:**
- **v1 (now):** Documented manual process — a human executes the steps, but rules are clearly defined so results are consistent every time
- **v2 (future):** Automated pipeline triggered by new offer submissions — eliminates manual data work and optionally auto-uploads to Lindy via webhook

**Out of Scope (v1):** Automation, CRM integration, call outcome tracking

---

## 2. Data Sources

| Resource | Link |
|---|---|
| Offer Tracker (source) | https://docs.google.com/spreadsheets/d/1df2JuxZdmMcUmLpzfecg21DUZpH1CwBiBgy7xELpueI |
| Lindy Bot Sheet (destination) | https://docs.google.com/spreadsheets/d/1kwTEbMNrelhKIN9r3-Bs-uL6AKgaiKjTNfxpxrIHoiM |
| Lindy AI Web App | https://chat.lindy.ai/claires-workspace-8/home |

**Offer Tracker tab:** `R-90_days`

---

## 3. Filtering Logic

Three filters are applied **in order** to produce the final call list:

### Step 1 — Filter by Offer State
Keep only rows where:
```
Offer State = "submitted"
```
These are pending offers that have been sent to the organizer but not yet accepted, denied, or ignored.

### Step 2 — Remove Missing Phone Numbers
Remove any row where `Organizer Phone` is blank.
> Rationale: Lindy Bot requires a phone number to make outbound calls. No phone = cannot be processed.

### Step 3 — Remove Duplicate Organizers
Group remaining rows by `Organizer Email`.
- If an organizer appears in **exactly one** submitted offer → ✅ **include**
- If an organizer appears in **more than one** submitted offer → ❌ **exclude all their rows entirely**

> Rationale: Calling an organizer about multiple offers simultaneously creates confusion. Multi-offer organizers are handled separately outside of Lindy.

**Expected result:** One row per organizer, with a valid phone number, with exactly one pending offer.

---

## 4. Column Mapping

Columns are copied from the Offer Tracker (`R-90_days` tab) into the Lindy Bot sheet in this exact order:

| Col | Lindy Sheet Column | Offer Tracker Column | Notes |
|---|---|---|---|
| A | Organizer Phone | Organizer Phone | Required — rows without this are filtered out in Step 2 |
| B | Expires At | Expires At | |
| C | Product | Brand Name | |
| D | Sms | Sms | |
| E | Item Count | Item Count | |
| F | Payout | Payout | |
| G | Event_Name | Audience Listing | Full name e.g. "barre3 - barre3 Rivertowns" |
| H | Venue_Name | Venue Name | |
| I | Local Start Day | Local Start Day | |
| J | Local End Day | Local End Day | |
| K | Expires_At | Expires At | Duplicate of col B (required by Lindy) |
| L | Organizer_Name | Organizer Name | |
| M | Organizer_Email | Organizer Email | Used for deduplication in Step 3 |
| N | Offer Url | Offer Url | |
| O | Team | Team | |
| P | Sponsor Name | Sponsor Name | |
| Q | Brand_Name | Brand Name | |
| R | Accepted At | Accepted At | Blank for submitted offers |
| S | Denied At | Denied At | Blank for submitted offers |
| T | Ignored At | Ignored At | Blank for submitted offers |
| U | Submitted At | Submitted At | |
| V | Primary Type | Primary Type | |
| W | Types | Types | |
| X | Venue Setting | Venue Setting | |
| Y | Venue Street | Venue Street | |
| Z | Venue City | Venue City | |
| AA | Venue State | Venue State | |
| AB | Venue Zip | Venue Zip | |
| AC | Dma | Dma | |

---

## 5. Manual Process — v1

**Who:** CS/Ops team member
**When:** On demand, as needed to follow up on pending offers

### Steps

1. Open the **[Offer Tracker](https://docs.google.com/spreadsheets/d/1df2JuxZdmMcUmLpzfecg21DUZpH1CwBiBgy7xELpueI)** → navigate to tab `R-90_days`
2. Filter column `Offer State` = `"submitted"`
3. Remove any rows where `Organizer Phone` is blank
4. Group by `Organizer Email` — delete **all rows** from organizers that appear more than once
5. Copy the remaining rows into the **[Lindy Bot Sheet](https://docs.google.com/spreadsheets/d/1kwTEbMNrelhKIN9r3-Bs-uL6AKgaiKjTNfxpxrIHoiM)** following the column mapping in Section 4
6. Export the Lindy Bot Sheet as **CSV**
7. Go to the **[Lindy web app](https://chat.lindy.ai/claires-workspace-8/home)** → upload the CSV → Lindy begins outbound calls

### What Lindy Does
- Makes outbound calls to each organizer's phone number
- If no answer: leaves a voicemail and follows up with an email summarizing the offer details (Product, Event, Payout, Expiration Date, Event Start Date, Offer URL)
- Follow-up email comes **from:** `lindy@recess.is` and is also sent to `events` and `deuce` for visibility
- Organizers may reply directly to that email thread

---

## 6. Automated Process — v2

### Trigger
**Event-based:** The pipeline runs automatically whenever the Offer Tracker (`R-90_days` tab) is updated with a new submitted offer — no manual scheduling needed.

---

### Option A: Automated Sheet Population (Semi-Auto)
A script handles data prep automatically, leaving the Lindy upload as a manual review step.

**How it works:**
1. Google Sheets trigger detects a new row or update in `R-90_days`
2. Script applies all three filter rules (Offer State + Phone + Deduplication)
3. Writes qualifying rows directly into the Lindy Bot Sheet (clears previous batch first)
4. Sends a **Slack notification** to the team: `"📞 Lindy call sheet ready — X organizers queued. Review and upload."`
5. Team member reviews and manually uploads CSV to Lindy web app

**Benefits:** Eliminates all manual data work, ensures consistent filtering, reduces human error

---

### Option B: Fully Automated End-to-End
Extends Option A by also triggering Lindy directly via webhook — zero manual steps.

**How it works:**
1. All steps from Option A
2. Script sends each qualifying organizer to Lindy via **HTTP POST webhook** with `Authorization: Bearer [secret]` header
3. Lindy begins outbound calls automatically
4. Slack notification confirms: `"📞 Lindy calls started — X organizers being contacted"`

**Lindy API:** Lindy supports webhook triggers ([docs](https://docs.lindy.ai/skills/by-lindy/webhooks)). Each Lindy workflow has a unique webhook URL and secret key. The script sends organizer data as JSON in the request body — Lindy references fields directly in the call script.

**Benefits:** Fully hands-off — new offer submitted → organizer called automatically

---

## 7. Call Outcome Tracking

When Lindy completes a call attempt, it sends a **follow-up email** from `lindy@recess.is` to the organizer, CC'ing the Recess team (`events`, `deuce`). The email includes:
- Offer URL
- Event name & brand
- Quantity, payout, expiration date, event start date
- A call to action to accept or request more info

Organizers sometimes reply to this email thread directly.

**v1:** Outcomes tracked manually via email inbox — no automation.
**v2 (future consideration):** Monitor `lindy@recess.is` reply threads and log outcomes (reached / voicemail / replied) back into the Offer Tracker or a dedicated log sheet.

---

## 8. Success Criteria

| Version | Metric | Target |
|---|---|---|
| v1 | Any team member can run the process end-to-end using this doc | 100% |
| v1 | No organizers with missing phones or multiple offers reach Lindy | 0 errors |
| v2A | Time spent on data prep per run | 0 min (fully automated) |
| v2A | Slack alert sent within 5 min of offer tracker update | Automated |
| v2B | Time from offer submission to Lindy call initiated | < 10 min |

---

## 9. Open Questions — Resolved

| # | Question | Answer |
|---|---|---|
| 1 | Does Lindy have an API? | ✅ Yes — webhook trigger with Bearer token auth ([docs](https://docs.lindy.ai/skills/by-lindy/webhooks)) |
| 2 | When should v2 run? | ✅ Event-triggered — when a new offer is submitted and the Offer Tracker updates |
| 3 | How are call outcomes tracked? | ✅ Via email — Lindy sends follow-up email from `lindy@recess.is`; organizers reply to that thread |
