# Lindy Bot v2 — Automated Pending Offers Pipeline

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Automate the process of reading submitted offers from the Offer Tracker, filtering them, populating the Lindy Bot Sheet, and triggering outbound calls — replacing the current manual 7-step process.

**Architecture:** A Python script reads the Offer Tracker Google Sheet, applies 3 filter rules (submitted state + phone required + one offer per organizer), writes qualifying rows to the Lindy Bot Sheet, and sends a Slack alert. Option B extends this by also firing a Lindy webhook to start calls automatically. The trigger is an Apps Script `onEdit` listener on the Offer Tracker.

**Tech Stack:** Python 3, Google Sheets API (via existing `scripts/google_sheets_helper.py`), Slack API (via existing `scripts/slack_helper.py`), Lindy webhook (HTTP POST), Google Apps Script (trigger), Google Cloud Functions (deployment).

---

## Column Index Reference (Offer Tracker → Lindy Sheet)

| Lindy Col | Lindy Header | Offer Tracker Index | Offer Tracker Header |
|---|---|---|---|
| A | Organizer Phone | 14 | Organizer Phone |
| B | Expires At | 78 | Expires At |
| C | Product | 9 | Brand Name |
| D | Sms | 63 | Sms |
| E | Item Count | 1 | Item Count |
| F | Payout | 32 | Payout |
| G | Event_Name | 29 | Audience Listing |
| H | Venue_Name | 46 | Venue Name |
| I | Local Start Day | 10 | Local Start Day |
| J | Local End Day | 11 | Local End Day |
| K | Expires_At | 78 | Expires At (duplicate) |
| L | Organizer_Name | 13 | Organizer Name |
| M | Organizer_Email | 12 | Organizer Email |
| N | Offer Url | 19 | Offer Url |
| O | Team | 16 | Team |
| P | Sponsor Name | 8 | Sponsor Name |
| Q | Brand_Name | 9 | Brand Name |
| R | Accepted At | 4 | Accepted At |
| S | Denied At | 5 | Denied At |
| T | Ignored At | 6 | Ignored At |
| U | Submitted At | 7 | Submitted At |
| V | Primary Type | 43 | Primary Type |
| W | Types | 44 | Types |
| X | Venue Setting | 45 | Venue Setting |
| Y | Venue Street | 47 | Venue Street |
| Z | Venue City | 48 | Venue City |
| AA | Venue State | 49 | Venue State |
| AB | Venue Zip | 50 | Venue Zip |
| AC | Dma | 51 | Dma |

---

## Task 1: Create the Filter Script

**Files:**
- Create: `scripts/lindy_bot_pipeline.py`

**Step 1: Create the file with constants and imports**

```python
#!/usr/bin/env python3
"""
Lindy Bot Pipeline — Automates pending offer call list population.

Usage:
    python scripts/lindy_bot_pipeline.py              # Option A: populate sheet + Slack
    python scripts/lindy_bot_pipeline.py --trigger    # Option B: also fires Lindy webhook
    python scripts/lindy_bot_pipeline.py --dry-run    # Preview only, no writes
"""

import sys
import os
import json
import requests
from pathlib import Path

# ── Sheet IDs ────────────────────────────────────────────────────────────────
OFFER_TRACKER_ID  = "1df2JuxZdmMcUmLpzfecg21DUZpH1CwBiBgy7xELpueI"
LINDY_SHEET_ID    = "1kwTEbMNrelhKIN9r3-Bs-uL6AKgaiKjTNfxpxrIHoiM"
OFFER_TRACKER_TAB = "R-90_days"
LINDY_TAB         = "Sheet1"

# ── Offer Tracker column indices (0-based) ────────────────────────────────────
COL_OFFER_STATE    = 3
COL_ACCEPTED_AT    = 4
COL_DENIED_AT      = 5
COL_IGNORED_AT     = 6
COL_SUBMITTED_AT   = 7
COL_SPONSOR_NAME   = 8
COL_BRAND_NAME     = 9
COL_LOCAL_START    = 10
COL_LOCAL_END      = 11
COL_ORG_EMAIL      = 12
COL_ORG_NAME       = 13
COL_ORG_PHONE      = 14
COL_TEAM           = 16
COL_OFFER_URL      = 19
COL_AUDIENCE_LIST  = 29
COL_PAYOUT         = 32
COL_PRIMARY_TYPE   = 43
COL_TYPES          = 44
COL_VENUE_SETTING  = 45
COL_VENUE_NAME     = 46
COL_VENUE_STREET   = 47
COL_VENUE_CITY     = 48
COL_VENUE_STATE    = 49
COL_VENUE_ZIP      = 50
COL_DMA            = 51
COL_SMS            = 63
COL_EXPIRES_AT     = 78

# ── Lindy column order (maps each Lindy col to offer tracker index) ───────────
LINDY_COLUMN_MAP = [
    COL_ORG_PHONE,    # A - Organizer Phone
    COL_EXPIRES_AT,   # B - Expires At
    COL_BRAND_NAME,   # C - Product
    COL_SMS,          # D - Sms
    COL_ITEM_COUNT,   # E - Item Count  (index 1)
    COL_PAYOUT,       # F - Payout
    COL_AUDIENCE_LIST,# G - Event_Name
    COL_VENUE_NAME,   # H - Venue_Name
    COL_LOCAL_START,  # I - Local Start Day
    COL_LOCAL_END,    # J - Local End Day
    COL_EXPIRES_AT,   # K - Expires_At (duplicate)
    COL_ORG_NAME,     # L - Organizer_Name
    COL_ORG_EMAIL,    # M - Organizer_Email
    COL_OFFER_URL,    # N - Offer Url
    COL_TEAM,         # O - Team
    COL_SPONSOR_NAME, # P - Sponsor Name
    COL_BRAND_NAME,   # Q - Brand_Name
    COL_ACCEPTED_AT,  # R - Accepted At
    COL_DENIED_AT,    # S - Denied At
    COL_IGNORED_AT,   # T - Ignored At
    COL_SUBMITTED_AT, # U - Submitted At
    COL_PRIMARY_TYPE, # V - Primary Type
    COL_TYPES,        # W - Types
    COL_VENUE_SETTING,# X - Venue Setting
    COL_VENUE_STREET, # Y - Venue Street
    COL_VENUE_CITY,   # Z - Venue City
    COL_VENUE_STATE,  # AA - Venue State
    COL_VENUE_ZIP,    # AB - Venue Zip
    COL_DMA,          # AC - Dma
]

# Add missing constant
COL_ITEM_COUNT = 1
```

**Step 2: Verify the file was created**
```bash
cat scripts/lindy_bot_pipeline.py | head -20
```
Expected: Shows the module docstring and imports.

---

## Task 2: Implement the Filter Logic

**Files:**
- Modify: `scripts/lindy_bot_pipeline.py`

**Step 1: Add the `read_offer_tracker()` function**

```python
def read_offer_tracker():
    """Read all rows from the Offer Tracker R-90_days tab."""
    # Import inline to reuse existing auth pattern
    sys.path.insert(0, str(Path(__file__).parent))
    import google_sheets_helper as gsh

    result = gsh.read_sheet(OFFER_TRACKER_ID, f"{OFFER_TRACKER_TAB}!A1:CF")
    if not result.get("success"):
        raise RuntimeError(f"Failed to read Offer Tracker: {result}")

    rows = result["values"]
    header = rows[0]
    data = rows[1:]  # skip header row
    print(f"✅ Read {len(data)} rows from Offer Tracker")
    return data
```

**Step 2: Add the `apply_filters()` function**

```python
def apply_filters(rows):
    """
    Apply 3 filter rules in order:
    1. Keep only Offer State = 'submitted'
    2. Remove rows with blank Organizer Phone
    3. Remove organizers with more than one submitted offer
    Returns list of qualifying rows.
    """
    # Step 1: Filter by Offer State
    submitted = [r for r in rows if _get(r, COL_OFFER_STATE).lower() == "submitted"]
    print(f"  Step 1 (submitted): {len(rows)} → {len(submitted)} rows")

    # Step 2: Remove blank phones
    with_phone = [r for r in submitted if _get(r, COL_ORG_PHONE).strip()]
    print(f"  Step 2 (has phone): {len(submitted)} → {len(with_phone)} rows")

    # Step 3: Keep organizers with exactly ONE submitted offer
    from collections import Counter
    email_counts = Counter(_get(r, COL_ORG_EMAIL).lower() for r in with_phone)
    unique = [r for r in with_phone if email_counts[_get(r, COL_ORG_EMAIL).lower()] == 1]
    print(f"  Step 3 (one offer): {len(with_phone)} → {len(unique)} rows")

    return unique


def _get(row, index):
    """Safely get a value from a row by index, returning '' if missing."""
    try:
        return str(row[index])
    except IndexError:
        return ""
```

**Step 3: Add a quick manual test**
```python
if __name__ == "__main__":
    rows = read_offer_tracker()
    filtered = apply_filters(rows)
    print(f"\n🎯 Final qualifying organizers: {len(filtered)}")
    for r in filtered[:3]:
        print(f"  - {_get(r, COL_ORG_NAME)} | {_get(r, COL_ORG_PHONE)} | {_get(r, COL_BRAND_NAME)}")
```

**Step 4: Run to verify filtering works**
```bash
python3 scripts/lindy_bot_pipeline.py
```
Expected output:
```
✅ Read XXX rows from Offer Tracker
  Step 1 (submitted): XXX → XX rows
  Step 2 (has phone): XX → XX rows
  Step 3 (one offer): XX → XX rows

🎯 Final qualifying organizers: XX
  - [Name] | [Phone] | [Brand]
```

**Step 5: Commit**
```bash
git add scripts/lindy_bot_pipeline.py
git commit -m "feat: add lindy bot filter logic (step 1-3)"
```

---

## Task 3: Implement the Lindy Sheet Writer

**Files:**
- Modify: `scripts/lindy_bot_pipeline.py`

**Step 1: Add `build_lindy_rows()` function**

```python
def build_lindy_rows(filtered_rows):
    """Map offer tracker rows to Lindy sheet column order (29 columns)."""
    lindy_rows = []
    for row in filtered_rows:
        lindy_row = [_get(row, col_idx) for col_idx in LINDY_COLUMN_MAP]
        lindy_rows.append(lindy_row)
    return lindy_rows
```

**Step 2: Add `write_lindy_sheet()` function**

```python
def write_lindy_sheet(lindy_rows, dry_run=False):
    """Clear the Lindy sheet (data only, keep header) and write new rows."""
    import google_sheets_helper as gsh

    if dry_run:
        print(f"  [DRY RUN] Would write {len(lindy_rows)} rows to Lindy sheet")
        for r in lindy_rows[:3]:
            print(f"    {r[0]} | {r[11]} | {r[2]}")  # Phone | Name | Product
        return

    # Clear existing data rows (row 2 onward), keep header row 1
    gsh.clear_sheet(LINDY_SHEET_ID, f"{LINDY_TAB}!A2:AC")

    if not lindy_rows:
        print("⚠️  No qualifying rows — Lindy sheet cleared, nothing written")
        return

    # Write new rows starting at A2
    result = gsh.write_sheet(
        LINDY_SHEET_ID,
        f"{LINDY_TAB}!A2",
        lindy_rows
    )
    if result.get("success"):
        print(f"✅ Written {len(lindy_rows)} rows to Lindy sheet")
    else:
        raise RuntimeError(f"Failed to write Lindy sheet: {result}")
```

**Step 3: Update `__main__` to include write step**

```python
if __name__ == "__main__":
    dry_run = "--dry-run" in sys.argv
    rows = read_offer_tracker()
    filtered = apply_filters(rows)
    lindy_rows = build_lindy_rows(filtered)
    write_lindy_sheet(lindy_rows, dry_run=dry_run)
```

**Step 4: Run dry-run to verify mapping**
```bash
python3 scripts/lindy_bot_pipeline.py --dry-run
```
Expected:
```
✅ Read XXX rows from Offer Tracker
  Step 1 (submitted): ...
  ...
  [DRY RUN] Would write XX rows to Lindy sheet
    [Phone] | [Name] | [Product]
```

**Step 5: Run live and verify Lindy sheet**
```bash
python3 scripts/lindy_bot_pipeline.py
```
Then open the [Lindy Bot Sheet](https://docs.google.com/spreadsheets/d/1kwTEbMNrelhKIN9r3-Bs-uL6AKgaiKjTNfxpxrIHoiM) and confirm:
- Header row is intact
- Data rows are populated correctly
- 29 columns filled per mapping

**Step 6: Commit**
```bash
git add scripts/lindy_bot_pipeline.py
git commit -m "feat: write filtered rows to lindy bot sheet"
```

---

## Task 4: Add Slack Notification (Option A Complete)

**Files:**
- Modify: `scripts/lindy_bot_pipeline.py`

**Step 1: Add `send_slack_alert()` function**

```python
def send_slack_alert(count, dry_run=False):
    """Send Slack notification that Lindy sheet is ready."""
    import subprocess

    if count == 0:
        msg = "📞 Lindy Bot: No qualifying organizers found. Sheet cleared."
    else:
        msg = (
            f"📞 *Lindy Bot sheet ready* — *{count} organizer(s)* queued for outbound calls.\n"
            f"👉 Review and upload CSV to Lindy: https://chat.lindy.ai/claires-workspace-8/home\n"
            f"📊 Sheet: https://docs.google.com/spreadsheets/d/1kwTEbMNrelhKIN9r3-Bs-uL6AKgaiKjTNfxpxrIHoiM"
        )

    if dry_run:
        print(f"  [DRY RUN] Would send Slack: {msg[:80]}...")
        return

    result = subprocess.run(
        ["python3", "scripts/slack_helper.py", "send", "#cs-ops", msg],
        capture_output=True, text=True, cwd=Path(__file__).parent.parent
    )
    if result.returncode == 0:
        print("✅ Slack notification sent to #cs-ops")
    else:
        print(f"⚠️  Slack notification failed: {result.stderr}")
```

**Step 2: Update `__main__` to include Slack**

```python
if __name__ == "__main__":
    dry_run = "--dry-run" in sys.argv
    rows = read_offer_tracker()
    filtered = apply_filters(rows)
    lindy_rows = build_lindy_rows(filtered)
    write_lindy_sheet(lindy_rows, dry_run=dry_run)
    send_slack_alert(len(lindy_rows), dry_run=dry_run)
    print("\n✅ Option A complete — Lindy sheet populated, team notified.")
```

**Step 3: Run end-to-end dry-run**
```bash
python3 scripts/lindy_bot_pipeline.py --dry-run
```
Expected: All steps print, no actual writes.

**Step 4: Commit**
```bash
git add scripts/lindy_bot_pipeline.py
git commit -m "feat: add slack notification — option A complete"
```

---

## Task 5: Add Lindy Webhook Trigger (Option B)

**Files:**
- Modify: `scripts/lindy_bot_pipeline.py`
- Modify: `.env` (add `LINDY_WEBHOOK_URL` and `LINDY_WEBHOOK_SECRET`)

**Step 1: Add env vars to `.env`**
```
LINDY_WEBHOOK_URL=https://webhooks.lindy.ai/YOUR_WEBHOOK_ID
LINDY_WEBHOOK_SECRET=your_secret_key_here
```
> Get these from the Lindy web app: Settings → Webhook → Copy URL + Secret

**Step 2: Add `trigger_lindy_webhook()` function**

```python
def trigger_lindy_webhook(lindy_rows, dry_run=False):
    """
    Fire Lindy webhook for each qualifying organizer.
    Lindy expects one POST per organizer with organizer data as JSON.
    """
    webhook_url = os.environ.get("LINDY_WEBHOOK_URL") or _load_env("LINDY_WEBHOOK_URL")
    secret = os.environ.get("LINDY_WEBHOOK_SECRET") or _load_env("LINDY_WEBHOOK_SECRET")

    if not webhook_url or not secret:
        raise RuntimeError("Missing LINDY_WEBHOOK_URL or LINDY_WEBHOOK_SECRET in .env")

    headers = {
        "Authorization": f"Bearer {secret}",
        "Content-Type": "application/json"
    }

    success, failed = 0, 0
    for row in lindy_rows:
        payload = {
            "organizer_phone":  row[0],   # A
            "expires_at":       row[1],   # B
            "product":          row[2],   # C
            "item_count":       row[4],   # E
            "payout":           row[5],   # F
            "event_name":       row[6],   # G
            "venue_name":       row[7],   # H
            "local_start_day":  row[8],   # I
            "local_end_day":    row[9],   # J
            "organizer_name":   row[11],  # L
            "organizer_email":  row[12],  # M
            "offer_url":        row[13],  # N
            "sponsor_name":     row[15],  # P
        }

        if dry_run:
            print(f"  [DRY RUN] Would POST to Lindy for: {row[11]} ({row[0]})")
            success += 1
            continue

        resp = requests.post(webhook_url, json=payload, headers=headers, timeout=10)
        if resp.status_code in (200, 201, 202):
            success += 1
        else:
            print(f"  ⚠️ Failed for {row[11]}: {resp.status_code} {resp.text[:100]}")
            failed += 1

    print(f"✅ Lindy webhook: {success} sent, {failed} failed")
    return success, failed


def _load_env(key):
    """Load a value from .env file."""
    env_path = Path(__file__).parent.parent / ".env"
    if env_path.exists():
        for line in env_path.read_text().splitlines():
            if line.startswith(f"{key}="):
                return line.split("=", 1)[1].strip()
    return None
```

**Step 3: Update `__main__` to support `--trigger` flag**

```python
if __name__ == "__main__":
    dry_run  = "--dry-run" in sys.argv
    trigger  = "--trigger" in sys.argv

    rows = read_offer_tracker()
    filtered = apply_filters(rows)
    lindy_rows = build_lindy_rows(filtered)
    write_lindy_sheet(lindy_rows, dry_run=dry_run)

    if trigger:
        trigger_lindy_webhook(lindy_rows, dry_run=dry_run)
        send_slack_alert(len(lindy_rows), dry_run=dry_run, mode="B")
        print("\n✅ Option B complete — Lindy calls triggered automatically.")
    else:
        send_slack_alert(len(lindy_rows), dry_run=dry_run, mode="A")
        print("\n✅ Option A complete — Lindy sheet populated, team notified.")
```

**Step 4: Update `send_slack_alert` to support mode B**

Add `mode="A"` parameter and update message for Option B:
```python
def send_slack_alert(count, dry_run=False, mode="A"):
    if mode == "B":
        msg = (
            f"📞 *Lindy Bot calls started* — *{count} organizer(s)* are being contacted now.\n"
            f"🤖 Calls triggered automatically via Lindy webhook."
        )
    else:
        # existing Option A message
        ...
```

**Step 5: Test dry-run with trigger**
```bash
python3 scripts/lindy_bot_pipeline.py --trigger --dry-run
```
Expected: Shows all dry-run steps including webhook POSTs.

**Step 6: Commit**
```bash
git add scripts/lindy_bot_pipeline.py .env
git commit -m "feat: add lindy webhook trigger — option B complete"
```

---

## Task 6: Deploy the Trigger (Apps Script)

**Files:**
- Create: `scripts/lindy_trigger.gs` (Google Apps Script — deployed manually)

**Step 1: Create the Apps Script file**

```javascript
// lindy_trigger.gs
// Deploy this in the Offer Tracker Google Sheet:
// Extensions → Apps Script → paste this → save → add trigger

const CLOUD_FUNCTION_URL = "https://YOUR_CLOUD_FUNCTION_URL";  // set after deploy
const SECRET = "YOUR_PIPELINE_SECRET";  // set a shared secret for security

/**
 * onEdit trigger — fires when any cell in R-90_days is edited.
 * Only acts when Offer State column (col D = index 3) is set to "submitted".
 */
function onOfferSubmitted(e) {
  const sheet = e.source.getActiveSheet();
  if (sheet.getName() !== "R-90_days") return;

  const col = e.range.getColumn();
  const val = e.value;

  // Column D (index 4 in Apps Script, 1-based) = Offer State
  if (col === 4 && val === "submitted") {
    triggerPipeline();
  }
}

function triggerPipeline() {
  const options = {
    method: "post",
    contentType: "application/json",
    payload: JSON.stringify({ secret: SECRET, mode: "A" }),
    muteHttpExceptions: true
  };
  const response = UrlFetchApp.fetch(CLOUD_FUNCTION_URL, options);
  Logger.log("Pipeline triggered: " + response.getContentText());
}
```

**Step 2: Install the trigger in Google Sheets**
1. Open the [Offer Tracker](https://docs.google.com/spreadsheets/d/1df2JuxZdmMcUmLpzfecg21DUZpH1CwBiBgy7xELpueI)
2. Extensions → Apps Script
3. Paste the script above
4. Save (Ctrl+S)
5. Triggers → Add Trigger → `onOfferSubmitted` → From spreadsheet → On edit
6. Authorize when prompted

**Step 3: Deploy Python script as Cloud Function**
```bash
# Package the script
cd /Users/f.cazes/Downloads/recess-tools
gcloud functions deploy lindy-bot-pipeline \
  --runtime python311 \
  --trigger-http \
  --allow-unauthenticated \
  --entry-point run_pipeline \
  --source scripts/ \
  --region us-central1
```

**Step 4: Add `run_pipeline()` Cloud Function entry point to script**

```python
def run_pipeline(request):
    """Cloud Function HTTP entry point."""
    data = request.get_json(silent=True) or {}
    mode = data.get("mode", "A")
    dry_run = data.get("dry_run", False)

    rows = read_offer_tracker()
    filtered = apply_filters(rows)
    lindy_rows = build_lindy_rows(filtered)
    write_lindy_sheet(lindy_rows, dry_run=dry_run)

    if mode == "B":
        trigger_lindy_webhook(lindy_rows, dry_run=dry_run)
        send_slack_alert(len(lindy_rows), dry_run=dry_run, mode="B")
    else:
        send_slack_alert(len(lindy_rows), dry_run=dry_run, mode="A")

    return {"status": "ok", "organizers": len(lindy_rows), "mode": mode}
```

**Step 5: Test the deployed function**
```bash
curl -X POST https://YOUR_CLOUD_FUNCTION_URL \
  -H "Content-Type: application/json" \
  -d '{"mode": "A", "dry_run": true}'
```
Expected:
```json
{"status": "ok", "organizers": XX, "mode": "A"}
```

**Step 6: Commit**
```bash
git add scripts/lindy_bot_pipeline.py scripts/lindy_trigger.gs
git commit -m "feat: add cloud function entry point and apps script trigger"
```

---

## Summary

| Step | What it delivers |
|---|---|
| Task 1 | Script skeleton + constants |
| Task 2 | Filter logic (3 rules) — tested manually |
| Task 3 | Lindy sheet writer — Option A working locally |
| Task 4 | Slack notification — Option A **complete** |
| Task 5 | Lindy webhook — Option B **complete** |
| Task 6 | Deployed trigger — fully automated end-to-end |

**Run locally anytime:**
```bash
python3 scripts/lindy_bot_pipeline.py              # Option A
python3 scripts/lindy_bot_pipeline.py --trigger    # Option B
python3 scripts/lindy_bot_pipeline.py --dry-run    # Preview only
```
