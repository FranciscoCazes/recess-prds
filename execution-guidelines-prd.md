# PRD: How We Create and Send Execution Guidelines to Organizers

**Date:** 2026-02-26
**Status:** Draft v2
**Owner:** Account Support Specialist
**Stakeholders:** Account Managers, Organizers, Brand Partners

---

## 1. Problem Statement

Today, execution guidelines are created by manually customizing a Google Doc template, then copying the content into an email sent to organizers. Once all brand information is available, the actual creation is fast — a matter of minutes. The real bottleneck is **upstream**: brands frequently send required assets (creatives, approved copy, survey links, promo links) past the agreed-upon due date, which delays execution guideline creation and puts organizer activation timelines at risk.

Beyond the timing issue, the current process has three additional gaps:
1. **Inconsistency** — guidelines vary between campaigns because there's no locked structure
2. **No confirmation loop** — we don't know if organizers received, read, or shared the guidelines with their execution team
3. **No per-campaign compliance reference** — recap review is based on general campaign standards rather than the specific execution guideline sent to each organizer. There is no dynamic checklist tied to what was required for a given campaign.

---

## 2. Goals & Success Metrics

| Goal | What Success Looks Like |
|------|------------------------|
| **Brand accountability** | Brands deliver required assets on time — the process surfaces delays early and creates clear ownership |
| **Consistency** | Every organizer gets the right, complete information — zero freeform editing, no missing sections |
| **Scalability** | Works for 5 organizers or 500 with no additional effort from the ASS |
| **Automation** | No manual copy/paste — generated from structured Asana inputs |
| **Confirmation** | We know who received, read, and acknowledged the guideline |
| **Compliance** | Recap reviewers have a checklist tied to the exact requirements in each guideline |

> **North Star:** "The execution guideline is automatically generated and sent based on structured inputs in Asana — no manual copy/paste, less room for error, and it scales across all campaign sizes."

> **Important nuance:** Creation itself is already fast (minutes). The system must also solve for **brand asset delays** — surfacing when brands are late and making it easy to hold them accountable before it impacts organizers.

### KPIs (Proposed — not currently tracked)

No formal KPIs exist today for the execution guideline process. Recap quality currently varies by organizer rather than campaign size — some organizers consistently submit complete, high-quality recaps while others require follow-up due to missing elements or low-quality photos.

| KPI | Description | Target (TBD) |
|-----|-------------|--------------|
| **Recap first-pass approval rate** | % of recaps approved without requiring revision or resubmission | TBD |
| **Recap re-submission rate** | % of recaps sent back due to missing elements or low-quality photos | TBD |
| **Guideline sent on time** | % of execution guidelines sent at least 2 weeks before activation date | 100% |
| **Organizer acknowledgment rate** | % of organizers who confirm receipt within 48hrs (future) | TBD |

---

## 3. Personas

### Account Manager (AM)
- Owns the brand relationship
- Collects all assets and campaign information from the brand
- Enters structured data into Asana (Brand Activations board)
- Signals readiness by marking their Asana task as complete

### Account Support Specialist (ASS)
- Creates and sends execution guidelines
- Monitors Asana for AM task completion (the trigger)
- Tracks sending in the master advance spreadsheet
- Currently: manually edits Google Doc → copies into email

### Organizer
- Receives the execution guideline via email
- Executes the brand activation (or delegates to their team)
- Submits recap photos, feedback, and required deliverables
- May not be the person physically running the activation

---

## 4. Current Workflow (As-Is)

```
Brand provides all assets to AM
        ↓
AM collects: creatives, banners, copy, survey links, promo links
        ↓
AM adds to Asana (Brand Activations board → brand-specific section)
        ↓
AM marks their Asana task complete ← ASS watches for this signal
        ↓
ASS opens Google Doc template
        ↓
ASS manually edits: adds/removes sections, fills in brand info
        ↓
ASS copies content into email
        ↓
ASS checks master advance spreadsheet for accepted organizers
        ↓
ASS sends email manually to each organizer (or batch)
        ↓
ASS marks "SENT" in the master advance spreadsheet (no date — status only)
        ↓
Organizers reply to email to confirm (inconsistent)
        ↓
Organizers forward to their team themselves (no visibility)
```

**Pain Points:**
- ⚠️ **Brand delays are the #1 bottleneck** — brands frequently send assets (creatives, copy, links) past the due date, blocking guideline creation and risking organizer timelines
- No locked structure → every guideline looks and feels different between campaigns
- No confirmation tracking → we don't know if organizers actually read the guidelines
- No compliance checklist → recap reviewers eyeball photos and feedback without a structured reference
- No visibility into whether organizers shared the guidelines with their execution team

---

## 5. Proposed Workflow (To-Be)

```
AM fills structured Asana task (all required fields)
        ↓
AM marks task complete → AUTO TRIGGER
        ↓
System generates execution guideline from Asana inputs
(locked template + brand data, no freeform editing)
        ↓
ASS receives notification + preview for review
        ↓
ASS approves → System sends to all accepted organizers
(pulled automatically from master advance)
        ↓
Delivery + timestamp logged automatically
        ↓
Organizer receives email with guideline + acknowledgment link
        ↓
Organizer clicks "I've read this" → logged to master advance
        ↓
Organizer can forward team view link (read-only, no editing)
        ↓
At recap review: compliance checklist auto-generated from guideline
```

---

## 6. What Information Goes Into an Execution Guideline

### 6A. Information Received from the Brand (AM Collects)

Before the guideline can be created, ALL of the following must be in Asana:

| Asset | Format | Required? |
|-------|--------|-----------|
| Brand name | Text | ✅ Always |
| Campaign name | Text | ✅ Always |
| Activation dates (start / end) | Date fields | ✅ Always |
| 8.5x11 creative file | File attachment | ✅ Always |
| Pull-up banner file | File attachment | ⚠️ If applicable |
| Approved email copy | Text or doc | ✅ Always |
| Approved social media copy | Text or doc | ✅ Always |
| Survey link | URL | ⚠️ If applicable |
| Promo link(s) | URL | ⚠️ If applicable |
| Distribution limits (units per person) | Text | ✅ Always |
| Sampling instructions (e.g. serve cold) | Text | ⚠️ If applicable |
| Sweepstakes or incentive details | Text | ⚠️ If applicable |
| Number of guideline versions (1 or 2) | 1 or 2 | ✅ Always |
| Recap deadline | Date | ✅ Always |

**Trigger:** AM checks all items complete and marks their Asana task as ✅ Done.

---

### 6B. Execution Guideline Email Template Structure

The execution guideline is delivered as a **formatted email** (not an attachment). Every guideline follows the same locked structure. Content marked 🔒 is fixed text identical across all campaigns. Content marked 🔄 is brand-specific, pulled from Asana inputs. Content marked ⚙️ is conditionally included based on campaign flags.

---

**🔒 EMAIL SUBJECT (fixed template):**
> Execution Guidelines for Your Upcoming Activation with [Brand Name]

---

**🔒 OPENING (fixed — identical for all campaigns):**
> Hi [Name],
>
> I'm sharing the execution guidelines for your upcoming activation with **[Brand]**. Please review the full instructions below and ensure your onsite team follows them exactly — this helps maintain consistency across all activations and sets your event up for success.
>
> Feel free to forward this email directly to anyone managing the activation onsite.

---

**🔄 ABOUT THE BRAND**
- 2–3 sentence brand description, written/approved by brand
- Pulled directly from Asana brand description field
- Examples from real campaigns:
  - *Nerds:* "Nerds Candy. A burst of tangy, crunchy fun in every bite — bright, bold, and unapologetically playful."
  - *Just Made:* "Grown by family farmers using traditional, eco-friendly practices. Packed with vitamins, antioxidants, and tropical goodness."
  - *GetJoy:* "GetJoy Fresh, vet-formulated dog food made with USDA meats and superfoods. Support your dog's gut health and wellness with meals delivered to your door."

---

**KEY EXECUTION GUIDELINES** *(section header — fixed)*

**🔒 Distribution Rule (always included):**
- "One (1) Product only per person. Do not distribute in bulk or give away multi-packs."

**⚙️ Cold Serving Requirement (if applicable):**
- "Serve Chilled. Ensure the product is cold at the time of sampling. Use ice or a cooler as needed to maintain temperature."
- Toggle: ON/OFF per campaign in Asana

**⚙️ Promo Cards (if applicable):**
- Instructions to include a promo card with each sample distributed
- Toggle: ON/OFF per campaign in Asana

**⚙️ Call to Action / Promo (if applicable):**
- Brand-specific primary CTA — only included when a promo link is provided:
  - *Nerds:* Scan QR code on signage for brand promo at Dollar General
  - *Just Made:* Scan QR code for B1T1 offer (go.recess.is link)
  - *GetJoy:* Scan QR on creative or promo card → 30% off on Amazon
- Promo link URL (pulled from Asana — toggle ON/OFF)
- Brand-approved social + email copy (verbatim, pulled from Asana)

**🔄 Social Media Promotion:**
- "Encourage social media posts tagging the brand and Recess — don't forget to use **#partnerwithrecess**" *(#partnerwithrecess is fixed across all campaigns)*
- Brand-specific handles (pulled from Asana):

| Platform | Field in Asana |
|----------|----------------|
| Instagram | Brand handle + @recess |
| Facebook | Brand page + recess |
| TikTok | Brand handle |
| Twitter/X | Brand handle + RECESS |

**🔒 Signage (fixed structure, variable link):**
- "Please print (in full color) and display the 8.5x11 creative linked [HERE] and ensure it is clearly visible and placed in your sampling area."
- ⚙️ If pull-up banner provided: "Displayed prominently. Encourage participants to engage with the banner/signage and to scan the QR code for the brand promo."

**🔒 Product Delivery (fixed — identical across all campaigns):**
- "Products should arrive at least 2 days before your event or within your specified receiving window. If they do not arrive on time, notify Recess immediately."

**🔒 Recap Submission (fixed — identical across all campaigns):**
- "After your event, please log into your Recess account and go to the Recaps page to upload your recap photos (at least 5) to the relevant brand activation offer."
- Deadline: Recaps must be submitted within 5 days post-event
- Payment will not be issued for recaps not submitted within deadline
- Let Recess know if you run into any upload issues
- **⚙️ Survey link** — included in recap section only if a survey link is provided (toggle ON/OFF)
- **🔄 Photos requested** (brand-specific, but follows this fixed structure):
  - People holding or interacting with the product ⚙️ and/or promo cards (solo shots + group shots)
  - The setup featuring brand collateral/signages/banners
  - Avoid photos showing multiple or excess products being handed out (seconds are fine, keep extra stock out of frame)

---

**🔒 ACTION ITEMS (fixed — identical across all campaigns):**
1. Confirm once your product shipment arrives
2. If it does not arrive by your receiving date, flag it to Recess right away
3. Make sure your onsite team has this email and follows all the outlined steps

---

## 7. How Execution Guidelines Are Generated

### Step-by-Step: ASS Creates the Guideline

1. **ASS receives notification** — Asana notifies ASS when AM marks task complete
2. **ASS opens generation tool** — pulls structured data directly from the Asana task
3. **Template auto-populated** — all required fields filled from Asana inputs; optional sections toggled on/off based on campaign flags
4. **ASS reviews** — checks for completeness, tone, any campaign-specific nuance
5. **ASS approves** — clicks "Send" — no copy/paste, no manual email composition

### Fields the ASS Fills In (Generation Step)
These are the only fields ASS touches during creation:

| Field | Description |
|-------|-------------|
| Version label | "Version A" / "Version B" if multi-creative campaign |
| Any campaign-specific notes | Edge cases not captured in Asana template |
| Recipient group | Which organizer segment gets which version (if 2 versions) |

Everything else is auto-populated from Asana.

---

## 8. How Guidelines Are Sent to Organizers

### Sending Rules

| Rule | Detail |
|------|--------|
| **Who receives** | Only organizers who have **accepted** the offer — pulled from master advance |
| **When sent** | As soon as AM finalizes assets in Asana — target: at least **2 weeks before activation date** |
| **Internal SLA** | No separate ASS turnaround SLA; the 2-week pre-activation deadline drives timing |
| **Volume** | Supports any scale — 5 to 500+ organizers |
| **Versioning** | Most campaigns: 1 guideline sent to all organizers. When a brand requires different creatives by venue type (e.g., fitness studios vs. universities), separate guidelines are created per venue type and distributed based on each organizer's assigned activation type. Segmentation is handled manually based on campaign setup. |
| **Tracking** | ASS marks "SENT" in the master advance Google Sheet when the guideline is delivered to the organizer |
| **Individual sends** | Each organizer receives their own separate email — guidelines are never sent in bulk to all organizers at once |
| **Re-sends** | Required when brand updates assets after initial send |

### Master Advance (Google Sheet)
The master advance is a Google Sheet tracker maintained by the ASS. It is the source of truth for accepted offers and execution guideline status. Key columns tracked include whether the execution guideline was created and sent for each organizer. *(A dedicated PRD for the master advance will be created separately.)*

### Email Format
- **From:** Recess account email (shared or individual AM — TBD)
- **Subject:** "Execution Guidelines for Your Upcoming Activation with [Brand]"
- **Body:** Fully formatted guideline content (not an attachment — inline for readability)
- **CTA:** "Click here to confirm you've read these guidelines" → acknowledgment link

---

## 9. How Organizers Confirm Receipt & Understanding

### Current State
Organizers reply to the email manually — inconsistent, untracked, hard to follow up at scale.

### Future State
- Every guideline email includes a **one-click acknowledgment link**
- Organizer clicks → logged automatically (timestamp + organizer name) in master advance
- ASS can see at a glance who has/hasn't confirmed
- Automated follow-up reminder sent after 48 hours if no acknowledgment (future phase)

---

## 10. How Organizers Share With Their Teams

### Current State
Organizers forward the email themselves — Recess has zero visibility into whether the right person (the one physically executing) ever sees the guidelines.

### Future State
- Each guideline email includes a **shareable team view link** — read-only, no editing
- Organizer can forward or share this link with any staff member running the activation
- Link is branded and mobile-friendly (easy to reference day-of)
- Optional: organizer can input their team member's email directly so Recess sends a copy

---

## 11. How We Maintain Compliance

### Current State
Recap review is based on general campaign standards — reviewers apply consistent baseline criteria across all recaps (good photos, feedback included, required shots present). There is an SOP that supports this, but it is not actively consulted during every review. There is no dynamic connection between the specific execution guideline sent to an organizer and what the reviewer checks — review criteria are general, not campaign-specific.

### Future State: Campaign-Linked Compliance Checklist

The improvement is making the review criteria **campaign-aware** — surfacing not just general standards, but the specific requirements that were in the execution guideline sent to that organizer. Instead of applying the same generic checklist to every recap, the reviewer sees:
1. **Standard requirements** — from the SOP (apply to all campaigns)
2. **Campaign-specific requirements** — pulled from the execution guideline (e.g., were promo cards required? Was cold serving required? Was a survey link included?)

This doesn't replace how reviewers work today — it simply gives them the right reference at the right moment, reducing reliance on memory and making it easier to catch campaign-specific gaps.

**Example Compliance Checklist (auto-generated):**

| Requirement | Required? | Verified? |
|-------------|-----------|-----------|
| Min. 5 photos submitted | ✅ | ☐ |
| Product display photo included | ✅ | ☐ |
| Branded setup photo included | ✅ | ☐ |
| Attendee interaction photo included | ✅ | ☐ |
| No excess inventory visible in photos | ✅ | ☐ |
| Survey link promoted | ✅ | ☐ |
| Social post submitted (if required) | ✅ | ☐ |
| Distribution count reported | ✅ | ☐ |
| Recap submitted within 5 days | ✅ | ☐ |
| Pull-up banner displayed (if applicable) | ⚠️ | ☐ |

- Checklist items are pulled from the actual guideline fields (not generic)
- Reviewer checks off each item during recap review
- Non-compliant items are flagged → can trigger follow-up before payment is released
- Compliance data is stored per organizer per campaign for tracking over time

---

## 12. Activation Examples

These examples illustrate how the same template adapts across different campaign types. They are **not exhaustive** — campaigns vary significantly depending on the brand, CTA type, product category, and activation format. The template must be flexible enough to support this range.

> **Key Insight from examples:** Only ~30% of the guideline content actually changes between campaigns. The remaining 70% (distribution rules, delivery timeline, recap requirements, #partnerwithrecess, action items) is fixed and identical. The system should lock the fixed content and only expose the variable fields for input.

---

### Example 1 — Nerds (Candy Brand / Retail Promo Drive)

**Category:** Candy / Confectionery
**CTA Type:** Retail promo — Dollar General coupon via QR code
**Special Flags:** Pull-up banner with QR code ✅ | Cold serving ❌ | Promo cards ❌

**Brand Description:** "Nerds Candy. A burst of tangy, crunchy fun in every bite — bright, bold, and unapologetically playful."

**Brand-Specific Guidelines:**
- Social copy: *"Unleash your senses in a bold new way with NERDS Juicy Clusters"*
- Promo link: dollargeneral.com/deals/coupons/save-/X543524
- Social handles: Instagram @nerdscandy + @recess | Facebook: NERDS + recess | TikTok: nerdscandy | Twitter: RECESS
- Pull-up banner required: encourage participants to scan QR code on banner for brand promo
- Photo requirements: people holding/interacting with product and/or promo cards; setup featuring brand collateral/signages; avoid excess inventory in frame

**What's Fixed (same as all campaigns):**
1 product per person · 2-day delivery window · 5-day recap deadline · 5 photos minimum · #partnerwithrecess

---

### Example 2 — Just Made (Juice Brand / B1T1 Offer)

**Category:** Juice / Beverage
**CTA Type:** B1T1 promotional offer via QR code (go.recess.is link)
**Special Flags:** Pull-up banner ❌ | Cold serving ✅ | Promo cards ❌

**Brand Description:** "Grown by family farmers using traditional, eco-friendly practices. Packed with vitamins, antioxidants, and tropical goodness."

**Brand-Specific Guidelines:**
- Cold serving required: *"Serve Chilled. Ensure the product is cold at the time of sampling. Use ice or a cooler as needed to maintain temperature."*
- B1T1 promotional link: go.recess.is/pIXrLA — organizers encourage attendees to scan QR on signage
- Social handles: Instagram @justmadejuice + @recess | Facebook: justmadejuice + recess | TikTok: @justmadejuice | Twitter: RECESS
- Photo requirements: setup featuring brand collateral/signages/banners; avoid photos showing excess products being handed out

**What's Fixed (same as all campaigns):**
1 product per person · 2-day delivery window · 5-day recap deadline · 5 photos minimum · #partnerwithrecess

---

### Example 3 — GetJoy (Pet Food Brand / Amazon Purchase Drive)

**Category:** Pet food / D2C
**CTA Type:** 30% off Amazon coupon — QR code on creative or promo card
**Special Flags:** Pull-up banner ❌ | Cold serving ❌ | Promo cards ✅

**Brand Description:** "GetJoy Fresh, vet-formulated dog food made with USDA meats and superfoods. Support your dog's gut health and wellness with meals delivered to your door."

**Brand-Specific Guidelines:**
- Promo cards: distribute GetJoy pack WITH a promo card — do not give product without card
- CTA: encourage customers to scan QR code on creative or promo card → 30% off on Amazon landing page
- Social handles: @getjoyfood + @recess | #partnerwithrecess
- Photo requirements: people (and their pets!) holding/interacting with product and/or promo cards; solo and group shots; setup featuring brand collateral/promo cards; avoid excess products in frame

**What's Fixed (same as all campaigns):**
1 product per person · 2-day delivery window · 5-day recap deadline · 5 photos minimum · #partnerwithrecess

---


## 13. What Else Is Involved in This Process

Beyond creation and sending, the following are part of the full execution guidelines workflow:

| Step | Detail |
|------|--------|
| **Asset QA** | ASS verifies all Asana attachments are correct format/size before generating |
| **Versioning management** | If brand updates a creative mid-campaign, a v2 guideline must be generated and re-sent |
| **Individual sends** | Every organizer receives their own individual email — there is no bulk/group send. Each EG is sent one-by-one as organizers are ready to receive it. |
| **Deadline management** | Guidelines must be sent at least 2 weeks before activation date — ASS sends as soon as AM finalizes assets in Asana |
| **Follow-up cadence** | Reminder to organizers who haven't acknowledged or haven't submitted recap |
| **Organizer questions** | ASS is the primary point of contact for organizer questions post-send. When questions require brand knowledge or judgment calls, ASS escalates to the AM for guidance before responding. |
| **Product not arriving — escalation** | If an organizer reports that product has not arrived, the ASS contacts the responsible party (brand or logistics partner) to obtain tracking information and clarify the issue. Once resolved, the ASS follows up with the organizer with next steps. This process exists in practice but is not yet formally documented. |

---

## 14. Functional Requirements

| # | Requirement | Priority |
|---|-------------|----------|
| FR-1 | Standardized Asana task template with all required fields per campaign | P0 |
| FR-2 | AM cannot mark task complete unless all required fields are filled | P0 |
| FR-3 | ASS notified automatically when Asana task is marked complete | P0 |
| FR-2b | Asset due date tracked in Asana — AM alerted when brand is approaching/past deadline | P0 |
| FR-2c | AM has a clear checklist of what's still missing from the brand at any point | P0 |
| FR-4 | Guideline auto-generated from Asana inputs using locked template | P0 |
| FR-5 | Support 1 or 2 guideline versions per campaign | P0 |
| FR-6 | Optional sections toggled on/off per campaign (banners, cold sampling, etc.) | P0 |
| FR-7 | ASS review + approval step before any send | P0 |
| FR-8 | Organizer list pulled from master advance (accepted offers only) | P0 |
| FR-9 | Bulk send to all accepted organizers (any volume) | P0 |
| FR-10 | Master advance updated to "SENT" status when guideline is delivered to each organizer | P0 |
| FR-11 | One-click acknowledgment link in every guideline email | P1 |
| FR-12 | Acknowledgment tracked per organizer in master advance | P1 |
| FR-13 | Shareable read-only team view link per guideline | P1 |
| FR-14 | Auto-generated compliance checklist tied to guideline requirements | P1 |
| FR-15 | Compliance checklist visible to recap reviewer per campaign/organizer | P1 |
| FR-16 | Re-send capability for updated guidelines | P2 |
| FR-17 | Automated follow-up reminder for unacknowledged organizers (48hr) | P2 |

---

## 15. Implementation Phases

### Phase 1 — Standardize (Immediate, No Tech Required)
- Create a locked Asana task template with all required fields
- Create a locked Google Doc template — no freeform editing, sections are toggled on/off
- Document the end-to-end process in a Standard Operating Procedure (SOP)
- Add compliance checklist to recap review process manually
- **Outcome:** Eliminates inconsistency immediately

### Phase 2 — Semi-Automate
- Build script that pulls Asana fields → populates Google Doc template automatically
- ASS reviews and sends manually, but no copy/paste
- Acknowledgment link added to email (simple form or typeform)
- **Outcome:** Cut creation time by ~70%, adds confirmation tracking

### Phase 3 — Full Automation
- Auto-generate guideline on Asana task completion
- Auto-send to accepted organizers via master advance integration
- Auto-log delivery + acknowledgment in master advance
- Auto-generate compliance checklist at recap review
- **Outcome:** ASS role shifts to QA and exception handling only

---

*PRD drafted via collaborative brainstorming session — 2026-02-26*
*Based on real execution guideline examples: Get Joy, Just Made, Nerds campaigns*

---

## GitHub Push Plan

**Steps:**
1. Create a new GitHub repo: `recess-prds` (private)
2. Push `execution-guidelines-prd.md` as the filename
3. Return the direct GitHub link to the file
