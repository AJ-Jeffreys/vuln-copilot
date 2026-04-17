# vuln-copilot

I built this to automate the Tenable-to-ticket pipeline for a healthcare environment, eliminating the manual handoff that caused high-severity vulns to miss SLA deadlines.

## The Problem

Tenable Security Center was generating findings daily, but getting those findings into tickets required manual triage and data entry. Engineers were spending roughly 3 hours a week just creating remediation tickets, and even then the process was inconsistent. High-severity CVEs would sit in a queue for days before the right team saw them, causing regular SLA misses.

## What It Does

- Pulls vulnerability findings from Tenable Security Center via API, filtered by severity and asset criticality
- Maps each finding to the responsible team based on asset ownership tags
- Automatically creates remediation tickets with pre-populated severity, SLA deadline, CVE details, and affected asset info
- Prioritizes findings using a criticality weighting model that accounts for asset type and healthcare environment context
- Runs on a schedule so nothing sits in a queue overnight

## Architecture

```
Tenable Security Center API
        ↓
   vuln-copilot (Python)
   - Pull new/changed findings
   - Enrich with asset context
   - Calculate SLA deadline
   - Deduplicate against open tickets
        ↓
   Ticketing System API
   - Create ticket with full context
   - Assign to responsible team
        ↓
   Summary report (optional)
```

## Results

- Contributed to a 65% reduction in critical CVEs over 6 months
- Reduced remediation ticket creation from roughly 3 hours per week to under 10 minutes
- Eliminated manual data entry errors that previously caused ticket misroutes
- Ensured high-severity findings reached the right team the same day they were discovered

## Setup

```bash
pip install -r requirements.txt
cp config.example.yaml config.yaml
# Fill in your Tenable SC credentials and ticketing system URL
python vuln_copilot.py --config config.yaml
```

## Configuration

| Key | Description |
|-----|-------------|
| `tenable.host` | Tenable Security Center hostname |
| `tenable.api_key` | API key (access + secret) |
| `tenable.severity_threshold` | Minimum severity to pull (default: medium) |
| `ticketing.url` | Ticketing system base URL |
| `ticketing.api_key` | API key for ticket creation |
| `sla.critical_days` | SLA window for critical findings (default: 3) |
| `sla.high_days` | SLA window for high findings (default: 14) |

## Sample Output

`Screenshot or sample output coming soon`

## Built For

I built this for a healthcare environment managing critical infrastructure. The asset tagging logic, SLA windows, and criticality weighting all reflect that context. If you adapt this for a different environment, those pieces are the ones to adjust.

## Related Projects

- [Threat-Detection-Hunt-Pack](https://github.com/AJ-Jeffreys/Threat-Detection-Hunt-Pack) — KQL detection queries for Microsoft Defender XDR
