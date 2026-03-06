# PRD: How We Create and Send Execution Guidelines to Organizers

**Date:** 2026-03-06
**Status:** Draft v2
**Owner:** Account Support Specialist
**Stakeholders:** Account Managers, Organizers, Brand Partners

---

## 1. Problem Statement

1. **Manual Creation** — Execution guidelines are created by manually customizing a Google Doc template, then copying the content into an email sent to organizers. Once all brand information is available, the actual creation is fast — a matter of minutes.
2. **Timing** — The real bottleneck is **upstream**: brands frequently send required assets (creatives, approved copy, survey links, promo links) past the agreed-upon due date, which delays execution guideline creation and puts organizer activation timelines at risk. Goal is to send at least 2 weeks ahead of activation. If the brand doesn't provide assets on time, we send a **default template** as a fallback.
3. **Inconsistency** — Guidelines vary between campaigns because there's no locked structure.
4. **Discovery** — It's difficult for Event Organizers to find their guidelines because they are delivered via email, not linked to offers in the platform, and not easy to search or reference later.
5. **No confirmation loop** — We don't know if organizers received, read, or shared the guidelines with their execution team.
6. **No per-campaign compliance reference** — Recap review is based on general campaign standards rather than the specific execution guideline sent to each organizer. There is no dynamic checklist tied to what was required for a given campaign when reviewing recaps.
7. **Pre-acceptance visibility** — Some organizers want to see the execution guidelines and creatives before committing to an offer, so they can assess what's required of them.

---

## 2. Goals & Success Metrics

| Goal | What Success Looks Like |
|------|------------------------|
| **Brand accountability** | Brands deliver required assets on time — the process surfaces delays early and creates clear ownership |
| **Consistency** | Every organizer gets the right, complete information — zero freeform editing, no missing sections |
| **Scalability** | Works for 5 organizers or 500 with no additional effort from the team |
| **Automation** | No manual copy/paste — generated from structured inputs |
| **Confirmation** | We know who received, read, and acknowledged the guideline |
| **Compliance** | Recap reviewers have a checklist tied to the exact requirements in each guideline |
| **Timing** | 80% of execution guidelines sent 14 days before start date; 100% sent 7 days before start (default template used if brand assets are late) |
| **Discovery** | Organizers know exactly where to find their guidelines — same location every time, linked to their offer in the platform |

> **North Star:** "The execution guideline is automatically generated and sent based on structured inputs — no manual copy/paste, less room for error, and it scales across all campaign sizes."

> **Important nuance:** Creation itself is already fast (minutes). The system must also solve for **brand asset delays** — surfacing when brands are late and making it easy to hold them accountable before it impacts organizers.

### KPIs (Proposed — not currently tracked)

No formal KPIs exist today for the execution guideline process. Recap quality currently varies by organizer rather than campaign size — some organizers consistently submit complete, high-quality recaps while others require follow-up due to missing elements or low-quality photos.

| KPI | Description | Target |
|-----|-------------|--------|
| **Guideline sent on time (primary)** | % of execution guidelines sent at least 14 days before activation date | 80% |
| **Guideline sent on time (minimum)** | % of execution guidelines sent at least 7 days before activation date | 100% |
| **Recap first-pass approval rate** | % of recaps approved without requiring revision or resubmission | TBD |
| **Recap re-submission rate** | % of recaps sent back due to missing elements or low-quality photos | TBD |
| **Organizer acknowledgment rate** | % of organizers who confirm receipt (future) | TBD |

---

## 3. Personas

### Account Manager (AM)
- Owns the brand relationship
- Collects all assets and campaign information from the brand
- Enters structured data if the brand doesn't do it directly
- Selects the creative partner kits for the campaign

### Account Support Specialist (ASS)
- Responds to organizer questions post-send
- Monitors guidelines workflow and escalates to AM when needed

### Organizer
- Receives the execution guideline
- Acknowledges confirmation they have read/received it
- Executes the brand activation (or delegates to their team)
- Submits recap photos, feedback, and required deliverables
- May not be the person physically running the activation

### Organizer Recap Contact
- Receives the execution guidelines
- Acknowledges confirmation they have read/received it
- Is the specific person responsible for submitting the recap (may differ from the main organizer contact)

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
(makes a copy and creates one per creative partner kit)
        ↓
ASS manually edits: adds/removes sections, fills in brand info
        ↓
ASS copies content into email
        ↓
ASS checks master advance spreadsheet for accepted organizers
        ↓
ASS sends email individually to each organizer
        ↓
ASS marks "SENT" in the master advance spreadsheet (status only, no date)
        ↓
Organizers reply to email to confirm (inconsistent)
        ↓
Organizers forward to their team themselves (no visibility)
```

**Pain Points:**
- ⚠️ **Brand delays are the #1 bottleneck** — brands frequently send assets past the due date, blocking guideline creation and risking organizer timelines
- No locked structure → every guideline looks and feels different between campaigns
- No confirmation tracking → we don't know if organizers actually read the guidelines
- Guidelines live only in email → hard for organizers to find them later
- No compliance checklist → recap reviewers apply general standards without a campaign-specific reference
- No visibility into whether organizers shared the guidelines with their execution team

**Open Question:**
> How can we group and send the execution guidelines confirmation, notification, etc. so that it's based on 1 creative kit approval for all their linked offers? (Possibly grouped by purchase order as the trigger)

---

## 5. Proposed Workflow (To-Be)

```
AM or brand fills in structured data (all required fields)
        ↓
AM or brand submits creative assets → verified by system
        ↓
System auto-generates execution guideline as a public landing page
(locked template + brand data, grouped by creative partner kit,
linked to all associated offers — no freeform editing)
        ↓
System sends execution guideline email by the due date
(auto-triggered, no manual send required)
        ↓
Delivery + timestamp logged automatically
        ↓
Organizer receives email with link to guideline landing page
+ acknowledgment CTA ("I've read and agree")
        ↓
Organizer clicks "I've read and agree" → logged automatically
        ↓
Organizer can share the public landing page link with their team
(read-only, always up to date, mobile-friendly)
        ↓
At recap review: compliance checklist auto-generated
from the specific guideline linked to that organizer's offer
```

---

## 6. What Information Goes Into an Execution Guideline

### 6A. Information Received from the Brand (AM Collects)

Before the guideline can be generated, the following must be submitted (by AM or brand directly):

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

**Default template fallback:** If brand assets are not submitted by the deadline, a default execution guideline template is sent with the standard fixed content only (no brand-specific CTA, copy, or creative). This ensures the 7-day minimum SLA is always met.

**Trigger:** Assets submitted and verified → system auto-generates the guideline.

---

### 6B. Execution Guideline Content Structure

The execution guideline is delivered as a **public landing page** (accessible via link) and as an **email** with a link to that page. Every guideline follows the same locked structure.

Content marked 🔒 is fixed — identical across all campaigns.
Content marked 🔄 is brand-specific — pulled from submitted inputs.
Content marked ⚙️ is conditional — toggled on/off per campaign.

---

**🔒 EMAIL SUBJECT (fixed template):**
> Execution Guidelines for Your Upcoming Activation with [Brand Name]

---

**🔒 OPENING (fixed — identical for all campaigns):**
> Hi [Name],
>
> I'm sharing the execution guidelines for your upcoming activation with **[Brand]**. Please review the full instructions and ensure your onsite team follows them exactly — this helps maintain consistency across all activations and sets your event up for success.
>
> You can also share this page directly with anyone managing the activation onsite: **[Landing Page Link]**

---

**🔄 ABOUT THE BRAND**
- 2–3 sentence brand description, written/approved by brand
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

**⚙️ Promo Cards (if applicable):**
- Instructions to include a promo card with each sample distributed

**⚙️ Call to Action / Promo (if applicable):**
- Brand-specific primary CTA — only included when a promo link is provided:
  - *Nerds:* Scan QR code on signage for brand promo at Dollar General
  - *Just Made:* Scan QR code for B1T1 offer (go.recess.is link)
  - *GetJoy:* Scan QR on creative or promo card → 30% off on Amazon
- Promo link URL

**🔄 Social Media Promotion:**
- "Encourage social media posts tagging the brand and Recess — don't forget to use **#partnerwithrecess**" *(fixed across all campaigns)*
- Brand-specific handles:

| Platform | Example |
|----------|---------|
| Instagram | Brand handle + @recess |
| Facebook | Brand page + recess |
| TikTok | Brand handle |
| Twitter/X | Brand handle + RECESS |

- **Posting timeline (🔒 fixed guidance):**
  - **3–7 days before event:** 2–3 teaser/hype posts
  - **Day-of (first 30 min):** 1 live Story minimum
  - **Day-of (midpoint):** 1 CTA reminder if activation goal is survey responses or redemptions
  - **1–3 days after:** Recap post within 48 hours of event

- **Activation goal determines what you post.** Look to your Recess Omni Channel offer for which platforms are required. Default goal if unsure: Brand Awareness.
  - Survey responses → CTA-driven content with survey link in every post; Stories with link sticker
  - Redemptions / rebates → Clear redemption instructions; save-able posts with offer details
  - Brand awareness → Lifestyle content; crowd/energy shots; branded moments
  - Social engagement → Interactive content (polls, Q&A); real reactions; repostable moments
  - Foot traffic → Location-tagged pre-event hype; specific venue info

**🔒 Signage (fixed structure, variable link):**
- "Please print (in full color) and display the 8.5x11 creative linked [HERE] and ensure it is clearly visible and placed in your sampling area."
- ⚙️ If pull-up banner provided: "Display prominently. Encourage participants to engage with the banner/signage and scan the QR code."

**🔒 Product Delivery (fixed — identical across all campaigns):**
- "Products should arrive at least 2 days before your event. If they do not arrive on time, notify Recess immediately."

**🔒 Recap Submission (fixed — identical across all campaigns):**
- "After your event, log into your Recess account and go to the Recaps page to upload your recap photos (at least 5) to the relevant brand activation offer."
- Deadline: Recaps must be submitted within 5 days post-event
- Payment will not be issued for recaps not submitted within the deadline
- ⚙️ Survey link — included only if provided by brand

**🔒 Photo Requirements — capture all six shot types:**
1. Wide venue/setup shot (before crowds arrive)
2. Product display close-up (branding visible)
3. Crowd energy shot (candid, not posed)
4. Genuine reaction moment (someone trying the product for the first time)
5. Branded touchpoint (signage, banner, merch)
6. Behind-the-scenes (team in action)
⚙️ If promo cards: people interacting with product AND promo cards

**🔒 Photo standards (fixed):**
- Use natural lighting — skip dark or blurry shots
- Shoot vertical for Stories/Reels (9:16)
- Recess branding must be visible in at least one image per post

**🔒 Avoid:**
- Posed shots of someone holding the product and smiling at camera
- Dark, blurry, or poorly lit content
- Content with no Recess branding visible
- Photos showing excess inventory being handed out

---

**🔒 Pre-Publish Social Media QA (fixed — organizer runs before posting):**
- All links work and go to the correct destination
- Copy has no typos and has correct event details (date, time, location)
- Recess branding is visible in the content
- Hashtags and tags are correct
- FTC disclosure included if this is sponsored content
- Post scheduled for the right date/time

**🔒 ACTION ITEMS (fixed — identical across all campaigns):**
1. Confirm once your product shipment arrives
2. If it does not arrive by your receiving date, flag it to Recess right away
3. Make sure your onsite team has this link and follows all the outlined steps
4. Click **"I've read and agree"** to confirm you've received and reviewed these guidelines

---

## 7. How Execution Guidelines Are Generated

Guidelines are **auto-generated by the system** once creative assets are submitted and verified — no manual creation step required.

- System pulls all structured inputs (brand data, creative files, conditional flags)
- Populates the locked template
- Generates a **public landing page** linked to the associated offer(s)
- Groups by creative partner kit — one guideline page covers all linked offers under the same kit
- If brand assets are not ready by the deadline, a **default template** is generated and sent automatically

---

## 8. How Guidelines Are Sent to Organizers

### Sending Rules

| Rule | Detail |
|------|--------|
| **Who receives** | Only organizers who have **accepted** the offer |
| **When sent** | Automatically by the system — target: at least **14 days before activation date** |
| **Fallback** | Default template sent at **7 days before** if brand assets not yet submitted |
| **Format** | Email with link to public landing page (not inline copy) |
| **Versioning** | 1 guideline per creative kit; separate kits created when brand requires different creatives by venue type |
| **Tracking** | Delivery timestamp + acknowledgment status logged automatically |
| **Individual sends** | Each organizer receives their own email |
| **Re-sends** | Auto-triggered when brand updates assets after initial send |

### Master Advance (Google Sheet)
The master advance is a Google Sheet tracker maintained by the ASS. It is the source of truth for accepted offers and execution guideline status. Tracked columns include EG created and SENT status per organizer. *(A dedicated PRD for the master advance will be created separately.)*

---

## 9. How Organizers Confirm Receipt & Understanding

### Current State
Organizers reply to the email manually — inconsistent, untracked, hard to follow up at scale.

### Future State
- Every guideline email and landing page includes a **"I've read and agree"** acknowledgment button
- Organizer clicks → logged automatically (timestamp + organizer name)
- ASS can see at a glance who has/hasn't confirmed
- Automated follow-up reminder after 48 hours if no acknowledgment

---

## 10. How Organizers Find & Share Their Guidelines

### Current State
Guidelines live only in the organizer's email inbox — hard to find later, impossible to search, and forwarding to the execution team is entirely manual with no visibility.

### Future State
- Each guideline is a **permanent public landing page** linked directly to the organizer's offer in the platform
- Organizer can find it anytime via their offer page — no searching through email
- Shareable link works for any staff member running the activation (mobile-friendly, always up to date)
- Organizer Recap Contact can be added separately to receive their own copy

---

## 11. How We Maintain Compliance

### Current State
Recap review is based on general campaign standards — reviewers apply consistent baseline criteria (good photos, feedback included, required shots present). An SOP exists but is not actively consulted. There is no connection between the specific execution guideline sent and what the reviewer checks at recap time.

### Future State: Campaign-Linked Compliance Checklist

At recap review, the reviewer sees a checklist generated from:
1. **Standard requirements** — from the SOP (apply to all campaigns)
2. **Campaign-specific requirements** — pulled from the execution guideline for that organizer (e.g., were promo cards required? Cold serving? Survey link?)

This gives reviewers the right reference at the right moment without replacing how they work today.

**Example Compliance Checklist (auto-generated):**

| Requirement | Required? | Verified? |
|-------------|-----------|-----------|
| Min. 5 photos submitted (6-shot list) | ✅ | ☐ |
| Wide venue/setup shot included | ✅ | ☐ |
| Product display close-up included | ✅ | ☐ |
| Crowd energy shot included (candid) | ✅ | ☐ |
| Genuine reaction moment captured | ✅ | ☐ |
| Branded touchpoint photo included | ✅ | ☐ |
| No posed product-holding shots | ✅ | ☐ |
| No excess inventory visible in photos | ✅ | ☐ |
| Recess branding visible in at least one photo | ✅ | ☐ |
| Social post submitted with correct tags/hashtags | ⚙️ If required | ☐ |
| Survey link promoted | ⚙️ If required | ☐ |
| Promo card distributed with product | ⚙️ If required | ☐ |
| Pull-up banner displayed | ⚙️ If required | ☐ |
| Recap submitted within 5 days | ✅ | ☐ |

- Non-compliant items flagged → can trigger follow-up before payment is released
- Compliance data stored per organizer per campaign for tracking over time

---

## 12. Activation Examples

These examples illustrate how the same template adapts across different campaign types. Not exhaustive — campaigns vary by brand, CTA type, product, and format.

> **Key Insight:** ~70% of the guideline is fixed and identical across all campaigns. Only ~30% changes per brand. The system locks the fixed content and exposes only the variable fields for input.

---

### Example 1 — Nerds (Candy / Retail Promo)
**Special Flags:** Pull-up banner ✅ | Cold serving ❌ | Promo cards ❌ | Promo link ✅

- Social copy: *"Unleash your senses in a bold new way with NERDS Juicy Clusters"*
- Promo: Scan QR on banner for Dollar General coupon
- Handles: @nerdscandy + @recess | TikTok: nerdscandy | RECESS on Twitter
- Photos: people interacting with product, branded setup, avoid excess inventory

---

### Example 2 — Just Made (Juice / B1T1 Offer)
**Special Flags:** Pull-up banner ❌ | Cold serving ✅ | Promo cards ❌ | Promo link ✅

- Serve chilled — use ice or cooler
- Promo: Scan QR for B1T1 offer (go.recess.is/pIXrLA)
- Handles: @justmadejuice + @recess | TikTok: @justmadejuice
- Photos: branded collateral/signage, avoid excess products being handed out

---

### Example 3 — GetJoy (Pet Food / Amazon Purchase Drive)
**Special Flags:** Pull-up banner ❌ | Cold serving ❌ | Promo cards ✅ | Promo link ✅

- Distribute product WITH promo card — never without
- Promo: Scan QR on creative or promo card → 30% off on Amazon
- Handles: @getjoyfood + @recess
- Photos: people (and pets!) with product and promo cards, branded setup

---

## 13. What Else Is Involved in This Process

| Step | Detail |
|------|--------|
| **Asset QA** | System verifies all attachments are correct format/size before generating |
| **Default template fallback** | If brand misses the deadline, a default guideline is auto-sent at the 7-day mark |
| **Versioning management** | If brand updates assets after send, a v2 landing page is generated and organizers are notified |
| **Deadline management** | System auto-sends — no manual trigger needed; 14-day and 7-day SLAs enforced automatically |
| **Organizer questions** | ASS is the primary contact for post-send questions; escalates to AM for brand-specific judgment calls |
| **Product not arriving — escalation** | ASS contacts brand or logistics partner to get tracking info, then follows up with organizer. Not yet formally documented. |
| **Follow-up cadence** | Automated reminder to organizers who haven't acknowledged within 48 hours |
| **Pre-acceptance preview** | Execution guideline landing page is accessible to organizers before accepting the offer, so they can assess requirements |

---

## 14. Functional Requirements

| # | Requirement | Priority |
|---|-------------|----------|
| FR-1 | Structured input form for brand assets (AM or brand can submit directly) | P0 |
| FR-2 | System validates all required fields before generating guideline | P0 |
| FR-3 | Guideline auto-generated as public landing page once assets are verified | P0 |
| FR-4 | Landing page grouped by creative partner kit, linked to all associated offers | P0 |
| FR-5 | Support 1 or 2 guideline versions per campaign (by venue type) | P0 |
| FR-6 | Optional sections toggled on/off per campaign (banners, cold serving, promo cards, etc.) | P0 |
| FR-7 | Auto-send email to accepted organizers at least 14 days before activation | P0 |
| FR-8 | Default template auto-sent at 7 days if brand assets not yet submitted | P0 |
| FR-9 | Delivery timestamp + acknowledgment status logged automatically | P0 |
| FR-10 | "I've read and agree" acknowledgment button on email and landing page | P0 |
| FR-11 | Acknowledgment tracked per organizer | P0 |
| FR-12 | Landing page accessible pre-acceptance from the offer page | P1 |
| FR-13 | Organizer Recap Contact can be added to receive their own copy | P1 |
| FR-14 | Auto-generated compliance checklist tied to campaign-specific guideline at recap review | P1 |
| FR-15 | Automated 48-hour follow-up if no acknowledgment | P1 |
| FR-16 | Landing page updated + organizers notified when brand updates assets post-send | P2 |
| FR-17 | AM alerted when brand is approaching or past asset submission deadline | P2 |

---

## 15. Implementation Phases

### Phase 1 — Standardize (Immediate, No Tech Required)
- Create locked input template for AM/brand (Asana or form)
- Create locked Google Doc template — sections toggled on/off, no freeform editing
- Document end-to-end process as an SOP
- Add compliance checklist to recap review process manually
- Define and communicate the 14-day / 7-day SLAs internally
- **Outcome:** Eliminates inconsistency and establishes timing discipline immediately

### Phase 2 — Semi-Automate
- Build generation tool: pulls inputs → populates template → produces formatted output
- Add acknowledgment link to email (simple form or Typeform)
- Introduce default template fallback at 7-day mark
- **Outcome:** Eliminates manual copy/paste, adds confirmation tracking, enforces SLAs

### Phase 3 — Full Automation (Platform-Native)
- Execution guideline generated as platform landing page, linked to offer
- Auto-send triggered by asset verification, not manual action
- Organizer acknowledgment tracked natively in platform
- Compliance checklist auto-generated at recap review from linked guideline
- Pre-acceptance guideline preview available on offer page
- **Outcome:** Fully automated, discoverable, and compliance-ready — ASS role shifts to QA and exception handling

---

---

## 16. Related Resources

| Resource | Purpose |
|----------|---------|
| **Recess Digital Playbook** | Full content creation reference for organizers — social media templates, posting timelines, platform specs, shot guidance, brand voice standards, and internal review process |
| **Master Advance (Google Sheet)** | Source of truth for accepted offers and EG sent status — dedicated PRD coming separately |
| **Recap Review SOP** | General standards for reviewing submitted recaps — to be linked to campaign-specific compliance checklist in Phase 3 |

---

*PRD v2 — updated 2026-03-06*
*Based on real execution guideline examples: Get Joy, Just Made, Nerds campaigns*
*Incorporating Notion V2 edits, session feedback, and Recess Digital Playbook (Deuce)*
