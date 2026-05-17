# CLAUDE.md — Calendar Repository Guide

## Repository Purpose

This repository stores the Saudi Ministry of Education school calendar for the
academic year 1447–1448 (Hijri) / 2025–2026 (Gregorian) as an iCalendar file
that can be imported into any standards-compliant calendar application (Google
Calendar, Apple Calendar, Outlook, etc.).

## Repository Structure

```
Calendar/
├── CLAUDE.md                              # This file
├── README.md                              # Project readme (minimal)
└── Saudi_School_Calendar_1447-1448.ics    # iCalendar data file
```

There is no build system, package manager, or runtime environment — the repo is
pure data.

## The ICS File

**File:** `Saudi_School_Calendar_1447-1448.ics`

The file follows the [iCalendar RFC 5545](https://datatracker.ietf.org/doc/html/rfc5545)
standard. All events are all-day (`VALUE=DATE`) entries. The VEVENT summaries
are written in Arabic (right-to-left text).

### Key Events Defined

| Date (Gregorian) | Arabic Summary | English Translation |
|---|---|---|
| 2025-06-26 | بداية إجازة نهاية العام الدراسي | Start of end-of-year holiday |
| 2025-08-12 | عودة المشرفين والهيئتين التعليمية والإدارية | Supervisors & staff return |
| 2025-08-17 | عودة المعلمين الممارسين للتدريس | Practicing teachers return |
| 2025-08-24 | بداية الدراسة للعام الدراسي 1447-1448 | First day of school year 1447–1448 |
| 2025-09-23 | إجازة اليوم الوطني | National Day holiday |
| 2025-10-12 | إجازة إضافية | Additional holiday |
| 2025-11-21 | إجازة الخريف | Autumn holiday |
| 2025-12-11 | إجازة إضافية | Additional holiday |
| 2026-01-09 | إجازة منتصف العام الدراسي | Mid-year holiday |
| 2026-02-22 | إجازة يوم التأسيس | Foundation Day holiday |
| 2026-03-06 | إجازة عيد الفطر | Eid Al-Fitr holiday |
| 2026-03-11 | يوم العلم السعودي | Saudi Science Day |
| 2026-05-22 | إجازة عيد الأضحى | Eid Al-Adha holiday |

## Conventions for Editing the ICS File

### Required VCALENDAR headers

Every valid file must open and close with:

```
BEGIN:VCALENDAR
VERSION:2.0
CALSCALE:GREGORIAN
...events...
END:VCALENDAR
```

### Adding a new all-day event

```
BEGIN:VEVENT
SUMMARY:<Arabic or English title>
DTSTART;VALUE=DATE:YYYYMMDD
DTEND;VALUE=DATE:YYYYMMDD
END:VEVENT
```

- `DTSTART` and `DTEND` use the format `YYYYMMDD` (no hyphens).
- For a single all-day event, `DTSTART` and `DTEND` should be the **same date**
  (this matches the pattern already used in the file).
- For a multi-day range (e.g., a week-long holiday), set `DTEND` to the day
  **after** the last day of the event (exclusive end, per RFC 5545).
- Keep summaries in Arabic to stay consistent with existing entries. If adding
  bilingual support, use `SUMMARY;LANGUAGE=ar:...` and add a second
  `SUMMARY;LANGUAGE=en:...` line.
- Do not add `UID` or `DTSTAMP` unless you are integrating with a system that
  requires them (they are not present in the existing file).

### Validation

There is no automated validator in the repo. To verify an edited `.ics` file:

```bash
# Quick structural check — look for unmatched BEGIN/END blocks
grep -c "BEGIN:VEVENT" Saudi_School_Calendar_1447-1448.ics
grep -c "END:VEVENT"   Saudi_School_Calendar_1447-1448.ics
# Both counts must match.
```

Import the file into a calendar application to confirm events render correctly
before committing.

## Hijri / Gregorian Calendar Notes

- The academic year label follows the **Hijri (Islamic)** calendar: 1447–1448.
- All dates stored in the file are **Gregorian** (`CALSCALE:GREGORIAN`).
- Islamic holidays (Eid Al-Fitr, Eid Al-Adha) shift each Gregorian year because
  the Hijri calendar is lunar (~11 days shorter). Verify exact dates against an
  authoritative source when updating for a new academic year.

## Git Workflow

- The default branch is `main`.
- Feature branches follow the pattern `claude/<description>-<id>`.
- Commit messages should be descriptive (e.g., `Add Eid Al-Adha holiday for 1448`).
- There is no CI pipeline; all validation is manual.

## Updating for a New Academic Year

1. Create a new file named `Saudi_School_Calendar_<HijriYear>.ics`.
2. Copy the VCALENDAR shell from the existing file.
3. Replace all VEVENT blocks with the new year's official dates from the Saudi
   Ministry of Education.
4. Update this `CLAUDE.md` table and the filename reference accordingly.
5. Keep the old file in the repo for historical reference.
