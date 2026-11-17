# Security Audit Report Template

Use this structure when the user requests a formal audit summary or review report. Keep findings concise, evidence-backed, and sorted from highest to lowest severity.

```markdown
# Security Audit Report — <Project / Component Name>

**Date:** YYYY-MM-DD  
**Scope:** <Files, directories, services, or endpoints reviewed>  
**Checks Run:** <Manual code review, dependency audit, SAST, config review>

## Executive Summary
- **Overall Risk Posture:** <Low / Moderate / Elevated / Critical>
- **Findings Count:** Critical: X | High: X | Medium: X | Low: X | Info: X
- **Top Priorities:**
  1. <Most urgent fix>
  2. <Second priority>

---

## Findings

### [CRITICAL/HIGH/MEDIUM/LOW] <Short Descriptive Title>
- **Location:** `<path/to/file#L10-L25>`
- **Category:** <e.g., Broken Access Control, Injection, Secrets Exposure, Misconfiguration>
- **Observation:** <Factual description of what the code/config currently does.>
- **Impact:** <Concrete risk in this specific system.>
- **Remediation:** <Minimal, concrete code or configuration fix.>
- **Verification:** <How to confirm the fix works (unit test, header check, re-scan).>

---

## Hardening & Positive Observations
- <Brief note on good security practices already in place so the team preserves them.>
- <Optional defense-in-depth recommendations.>
```
