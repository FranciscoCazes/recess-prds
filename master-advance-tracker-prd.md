# PRD: Master Advance Tracker
**Version:** 1.0 | **Date:** Feb 26, 2026 | **Owner:** ASS Team (Francisco + Claire)

---

## Overview

The Master Advance Tracker is a Google Sheet used by the ASS Team to track accepted offers, execution guidelines (EG) status, and shipping information. It has two active tabs: **Sampling_Tracking** and **Photographers**.

This PRD covers:
- **Phase 1:** Automate accepted offer sync into Sampling_Tracking
- **Phase 2:** Document and improve the Photographer tracker workflow

---

# PHASE 1: Sampling Tracker Automation

## Problem Statement

Francisco and Claire manually monitor the Offer Tracker (`R-90_days` tab) for newly accepted offers, then copy columns B→BF into the Master Advance Tracker (`Sampling_Tracking` tab). This process is:
- Error-prone (wrong rows, missed offers, paste misalignment)
- Time-consuming and repetitive
- Dependent on team members remembering to check daily
- Has no duplicate protection

## Goal

Auto-sync accepted offers from the Offer Tracker into Master Advance daily at 9:00 AM. The team retains full manual control of:
- ✏️ Execution Guideline (EG) link and status
- ✏️ Shipping information and status
- ✏️ Notes and ship-to-doc flag

## Out of Scope (Phase 1)
- Photographer tracker automation
- EG creation or sending automation
- Shipping address lookup automation
- WeWork or On-Site tabs

---

## Data Sources

| | Detail |
|---|---|
| **Source Sheet** | Offer Tracker |
| **Source Sheet ID** | `1df2JuxZdmMcUmLpzfecg21DUZpH1CwBiBgy7xELpueI` |
| **Source Tab** | `R-90_days` |
| **Filter** | `Offer State` (col D) = `"accepted"` |
| **Destination Sheet** | Master Advance Tracker |
| **Destination Sheet ID** | `1dIHKiEZ65KN1_pIvYCM4SnbTKQ5oSKg9qCkVjOZy1zY` |
| **Destination Tab** | `Sampling_Tracking` |

---

## Column Mapping

**Rule:** Source cols **B→BF** (57 cols) paste 1:1 into destination cols **H→BL**. Col A (Denial Reasons) is never copied.

| Source Col | Source Field | Destination Col | Notes |
|---|---|---|---|
| A | Denial Reasons | — | ❌ NOT copied |
| **B** | Item Count | **H** | ← Paste starts here |
| C | Activation Type | I | |
| D | Offer State | J | Filter: must = `"accepted"` |
| E | Accepted At | K | |
| I | Sponsor Name | O | |
| J | Brand Name | P | |
| K | Local Start Day | Q | |
| L | Local End Day | R | |
| M | Organizer Email | S | |
| … | *(remaining fields)* | … | Straight 1:1 paste |
| **AA** | **Offer ID** | **AG** | ← 🔑 **Dedup key** |
| … | *(addon/venue/sign fields)* | … | Straight 1:1 paste |
| **BF** | 8.5x11 Can Print | **BL** | ← Paste ends here |

### Formula Columns (Auto-Extended)

| Column | Field | Behavior |
|---|---|---|
| F | Event Start Date | Formula referencing pasted data — copied from last row for each new row |
| G | Event Name | Formula referencing pasted data — copied from last row for each new row |

### Manual Columns (Always Left Blank by Automation)

| Column | Field | Owner |
|---|---|---|
| A | Execution Guidelines LINK | Francisco / Claire |
| B | Execution Guideline Status | Francisco / Claire |
| C | Shipping Status | Francisco / Claire |
| D | Notes | Francisco / Claire |
| E | Ship-to-doc flag | Francisco / Claire |

---

## Sync Logic

```
Schedule: Daily at 9:00 AM (Pacific Time)

1. READ: All rows from R-90_days where col D (Offer State) = "accepted"
2. READ: All existing Offer IDs from col AG in Sampling_Tracking
3. FOR EACH accepted offer:
     IF Offer ID (source col AA) NOT IN existing Master Advance IDs:
       a. Append new row at the bottom of Sampling_Tracking
       b. Copy source cols B→BF into destination cols H→BL
       c. Leave cols A–E blank (manual team input)
       d. Copy formula from the previous row into cols F and G
     ELSE:
       Skip (already exists)
4. LOG: Write entry to hidden `Sync Log` tab in Master Advance Tracker:
     `"[Date] — X new offers added"` or `"[Date] — 0 new offers (nothing new)"`
```

---

## Duplicate Prevention

| | Detail |
|---|---|
| **Dedup Key** | Offer ID — source col AA → destination col AG |
| **Logic** | If Offer ID already exists in col AG → skip entirely |
| **Why Offer ID** | More reliable than visual Event Name + Brand Name check |
| **Fallback** | None needed — Offer ID is guaranteed unique per the Recess platform |

---

## Success Criteria

| Metric | Target |
|---|---|
| New accepted offers appear in Master Advance | Within 24 hours of acceptance |
| Duplicate offers added | 0 |
| Manual copy sessions eliminated | 100% |
| Cols A–E always left blank for team | ✅ Always |
| Formulas in F & G working on new rows | ✅ Always |
| Daily sync runs without intervention | ✅ Always |

---

## Implementation Approach

**Google Apps Script** — a script built directly into the Master Advance Tracker spreadsheet.
- Runs automatically every morning at 9 AM
- No extra tools, accounts, or infrastructure needed
- Lives inside the sheet — the team doesn't need to manage anything separately
- Claude will build and install this script

---

## Open Dependencies

| Item | Owner | Notes |
|---|---|---|
| How does R-90_days refresh daily? | Product team | Needs clarification — sync timing relative to 9 AM matters |
| Google OAuth / Apps Script access | Francisco | Needs edit access to both sheets |
| Confirm formula structure in cols F & G | Francisco / Claire | Script must copy exact formula, not hardcoded values |

---

# PHASE 2: Photographer Tracker

## Context
The `Photographers` tab logs photographers who performed exceptionally well at events. No automation needed — tracker works well today. Goal is to document the process clearly.

## Workflow
1. Find photographer on **Thumbtack** → negotiate → send formal briefing email
2. Event happens → organizer sends feedback via email
3. If feedback is positive + photos met shot list → add to tracker *(judgment call by whoever hired them)*

## What Qualifies as "GREAT"
- Organizer praised them via email **AND** photos followed the shot list

## How to Add an Entry
1. Open **Master Advance Tracker → Photographers tab**
2. Pull Event, Date, Location from the **sent briefing email**
3. Pull Name, Email, Phone from **Thumbtack**
4. Paste organizer's key feedback phrase in **Notes**
5. Update **Pay Status** once payment is confirmed

---

# Summary

| Phase | Scope | Effort | Impact |
|---|---|---|---|
| Phase 1 | Auto-sync accepted offers → Sampling_Tracking | Medium (Apps Script) | High — eliminates daily manual work |
| Phase 2 | SOP documentation + data entry guidance | Low | Medium — consistency + onboarding |
