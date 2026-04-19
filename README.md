# vuln-copilot

I built this Python automation tool to streamline the Tenable to ticket pipeline for a healthcare environment, eliminating the manual handoff that caused high severity vulnerabilities to miss SLA deadlines.

## The Problem

Tenable Security Center was generating vulnerability findings daily, but getting those findings into remediation tickets required manual triage and data entry. Security operations teams were spending roughly 3 hours a week just creating CVE remediation tickets, and even then the process was inconsistent. High severity CVEs would sit in a queue for days before the right team saw them, causing regular SLA misses and expanding the attack surface.

## What It Does

- Pulls vulnerability management findings from Tenable Security Center via API, filtered by CVSS severity and asset criticality
- Maps each finding to the responsible team based on asset ownership tags
- Automatically creates CVE remediation tickets with pre populated severity, SLA deadline, CVE details, and affected asset info
- Implements risk based prioritization using a criticality weighting model that accounts for asset type and healthcare environment context
- Runs on a schedule via Python automation so nothing sits in a queue overnight
- Supports exposure management workflows by ensuring consistent vulnerability tracking across infrastructure

## Results

- Contributed to an 85% reduction in critical CVEs across 100+ servers and endpoints
- Reduced remediation ticket creation from ~3 hours a week to under 10 minutes via Python automation
- Eliminated manual data entry errors that previously caused ticket misroutes
- Ensured high severity findings reached the right team the same day they were discovered

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
| `tenable.severity_threshold` | Minimum CVSS severity to pull (default: medium) |
| `ticketing.url` | Ticketing system base URL |
| `ticketing.api_key` | API key for ticket creation |
| `sla.critical_days` | SLA window for critical findings (default: 3) |
| `sla.high_days` | SLA window for high findings (default: 14) |

## Built For

Built for a healthcare environment managing critical infrastructure. The asset tagging logic, SLA windows, and criticality weighting reflect that context, adjust those pieces if adapting for a different environment.

## Related Projects

- [Threat-Detection-Hunt-Pack](https://github.com/AJ-Jeffreys/Threat-Detection-Hunt-Pack) — KQL detection queries for Microsoft Defender XDR

## Author

**AJ Jeffreys** — Detection Engineer specializing in vulnerability management, exposure management, and security automation in healthcare/critical infrastructure environments.

[LinkedIn](https://www.linkedin.com/in/ajani-jeffreys/) · [GitHub](https://github.com/AJ-Jeffreys)
