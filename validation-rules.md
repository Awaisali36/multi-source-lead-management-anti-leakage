# Validation Rules & Deduplication Logic

## Overview

Every lead record passes through a validation and normalization layer
before being pushed to Real Geeks or Homebot. Records that fail
critical validation are flagged in the source Google Sheet rather
than being pushed with bad data.

---

## Field Validation Rules

### Name Parsing (AI by Zapier)

```
Input:  "John Smith" / "Mary Jane Watson" / "Bob"

Process:
  AI by Zapier prompt:
  "Split this full name into first name and last name.
   Return JSON: {"first": "...", "last": "..."}
   If only one name provided, put it in first and leave last empty."

Output:
  "John Smith"       → first: "John"  / last: "Smith"
  "Mary Jane Watson" → first: "Mary Jane" / last: "Watson"
  "Bob"              → first: "Bob"   / last: "" → FLAG
```

### Address Standardization

```
Required format: "Street Number Street Name, City, State ZIP"
Example:         "123 Main Street, Dallas, TX 75201"

Validation checks:
  ✓ Contains comma separators
  ✓ State is 2-letter abbreviation
  ✓ ZIP is 5 digits
  ✓ Street number present

Failures → Error flag: "ADDRESS_FORMAT_INVALID"
```

### Phone Validation

```
Accepted formats (normalized to 10 digits):
  (214) 555-1234  → 2145551234
  214-555-1234    → 2145551234
  214.555.1234    → 2145551234
  12145551234     → 2145551234 (strip leading 1)

Rejected:
  Less than 10 digits → FLAG: "PHONE_INVALID"
  Non-numeric chars remaining after strip → FLAG: "PHONE_INVALID"
```

### Email Validation

```
Required: TRUE — leads without email cannot enter Homebot pipeline

Checks:
  ✓ Contains @ symbol
  ✓ Contains domain with TLD (.com, .net, etc.)
  ✓ No spaces

Failures → Error flag: "EMAIL_MISSING" or "EMAIL_INVALID"
Real Geeks pipeline: continues without email (email optional)
Homebot pipeline: blocked until email provided
```

---

## Deduplication Logic

```
Window: 15 minutes

On new lead received:
  1. Extract phone number (primary key)
  2. Query recent entries log (last 15 minutes)
  3. Match found?
     YES → Block entry
           Add to Google Sheet with flag: "DUPLICATE_BLOCKED"
           Log: original entry timestamp, matching phone
     NO  → Proceed with validation and push

Secondary check (if phone missing):
  1. Match on email (if available)
  2. Same 15-minute window
  3. Same block/flag logic

After 15 minutes:
  Window resets — same lead can be re-entered
  (Handles legitimate re-adds after correction)
```

---

## Error Flag Reference

| Flag | Meaning | Action Required |
|---|---|---|
| `ADDRESS_FORMAT_INVALID` | Address doesn't match required format | Team corrects in sheet, re-triggers |
| `PHONE_INVALID` | Phone number invalid or missing | Team adds valid phone, re-triggers |
| `EMAIL_MISSING` | Email field empty (Homebot block only) | Team adds email, re-triggers Homebot pipeline |
| `EMAIL_INVALID` | Email format invalid | Team corrects, re-triggers |
| `NAME_INCOMPLETE` | Only first name found | Team adds last name, re-triggers |
| `DUPLICATE_BLOCKED` | Same phone seen in last 15 min | Review — likely double-entry |
| `CRM_PUSH_FAILED` | Real Geeks API error | Auto-retry 3× then alert |
| `NURTURE_PUSH_FAILED` | Homebot API error | Auto-retry 3× then alert |

---

## Lead Source Attribution

Each Google Sheet tab auto-assigns the source tag:

| Tab Name | Source Tag Applied |
|---|---|
| Expired Listings | `Expired` |
| FSBO | `FSBO` |
| Pre-Foreclosure | `Pre-Foreclosure` |
| Probate | `Probate` |

Source tag is pushed to both Real Geeks and Homebot.
Enables filtering, reporting, and source-specific nurture sequences.

---

## Dual Pipeline Flow

```
PIPELINE 1 — Real Geeks CRM
  Trigger:  New validated row in Google Sheet
  Action:   POST /contacts
  Fields:   first_name, last_name, phone, email,
            address, source, assigned_agent
  On fail:  Retry 3× → flag in sheet

PIPELINE 2 — Homebot
  Trigger:  Same validated row (runs in parallel)
  Requires: Valid email (blocks if missing)
  Action:   POST /contacts/enroll
  Fields:   first_name, last_name, email,
            address, source_tag
  On fail:  Retry 3× → flag in sheet
```

Both pipelines run simultaneously — a failure in one
does not block the other.
