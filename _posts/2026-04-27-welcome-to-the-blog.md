---
layout: post
title: "Welcome to the Blog"
date: 2026-04-27
description: "What this blog is, who it's for, and what I plan to write about — detection engineering, Splunk SPL, MITRE ATT&CK, and the occasional career piece."
tags:
  - Detection Engineering
  - Career
---

This site has existed in placeholder form for longer than I'd like to admit. That ends now.

## What this is

A place to write about detection engineering — the actual technical work, not the vendor marketing version of it. I spend my days building Splunk correlation searches, mapping coverage to MITRE ATT&CK, and hunting threats across federal infrastructure. Most of that work stays classified or proprietary. The parts that don't, I'll write about here.

Topics I plan to cover:

- **Splunk SPL walkthroughs** — real detection logic, explained step by step, with tuning notes
- **MITRE ATT&CK coverage analysis** — where most SOC environments have gaps, and what log sources actually fill them
- **Threat hunting methodology** — how I structure a hunt, from hypothesis to documentation
- **Incident response automation** — PowerShell patterns that have cut triage time in practice
- **Detection engineering career** — what the job actually looks like, how to break in from a SOC analyst background, what certifications are worth it
- **VetSec / veterans in cyber** — occasional posts for military folks navigating the industry

## What this is not

A vendor blog. A thought leadership exercise. A rehash of existing SIEM documentation.

If I write about a Splunk search, it's going to have real SPL in it that you can adapt. If I write about ATT&CK coverage, it's going to have specific techniques, not hand-wavy frameworks. If I write about career stuff, it's going to be practical, not aspirational.

## A quick example — what a detection post will look like

Here's a minimal Splunk search that detects potential PowerShell download cradle activity. This isn't a full walkthrough (that's its own post), but it gives you a sense of the format:

```spl
index=windows sourcetype=WinEventLog:Microsoft-Windows-PowerShell/Operational EventCode=4104
| where match(ScriptBlockText, "(?i)(Net\.WebClient|Invoke-WebRequest|IEX|Invoke-Expression|DownloadString|DownloadFile)")
| stats count by _time, ComputerName, UserName, ScriptBlockText
| where count < 5
| eval risk = case(
    match(ScriptBlockText, "(?i)IEX|Invoke-Expression"), "HIGH",
    match(ScriptBlockText, "(?i)DownloadString|DownloadFile"), "MEDIUM",
    true(), "LOW"
  )
| sort - risk
```

**Log source required:** Windows PowerShell Operational log (Event ID 4104 — Script Block Logging must be enabled)

**ATT&CK mapping:** T1059.001 (Command and Scripting Interpreter: PowerShell), T1105 (Ingress Tool Transfer)

**Tuning note:** Script Block Logging generates significant volume. Baseline your environment first and add exclusions for known-good admin scripts before this goes into production.

Full post on this detection — with log source requirements, common FP patterns, and tuning methodology — is coming soon.

---

If you're a detection engineer, SOC analyst, or veteran transitioning into security and you find something useful here, that's the whole point. LinkedIn is the best place to reach me if you want to talk shop.
