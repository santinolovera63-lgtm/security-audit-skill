---
name: security-audit
description: >-
  Authorized defensive security auditing, threat modeling, and hardening workflow for coding agents.
  Use when the user asks to audit a codebase, review an application or API for vulnerabilities,
  harden a web app or server configuration, check for exposed secrets or risky dependencies,
  build a threat model, or produce a structured security review report with concrete remediations.
---

# Security Audit & Defensive Hardening

Perform structured, defensive security reviews on authorized codebases, applications, and configurations. Focus on identifying real vulnerabilities, explaining concrete risk, and delivering clean, verifiable remediations.

## Scope & Guardrails

* **Authorized defensive use only.** Audit only the workspace, codebases, or configurations the user owns or is explicitly authorized to assess.
* **No offensive payloads or evasion.** Do not generate working exploits, phishing material, credential-harvesting tools, or antivirus/EDR evasion techniques. Demonstrate risks through code analysis, architectural reasoning, and safe defensive tests.
* **Evidence over noise.** Report only verified findings backed by specific files, lines, or configuration keys. Never pad reports with speculative or generic filler.

## Audit Workflow

### Step 1: Map the Attack Surface
Before reading every file, quickly identify:
1. **Stack & runtime** (framework, language, database, auth provider, deployment target).
2. **Entry points** (HTTP routes, APIs, webhooks, file uploads, CLI args, background jobs).
3. **Trust boundaries** (unauthenticated vs. authenticated, user vs. admin, client vs. server, external APIs).
4. **Sensitive assets** (credentials, tokens, PII, payment data, encryption keys).

### Step 2: Run Non-Destructive Local Checks (When Available)
Check what standard read-only audit tools are already installed in the environment rather than installing unnecessary packages:
* **Dependency audits:** `npm audit`, `pnpm audit`, `pip-audit`, `cargo audit`, or `govulncheck` depending on the stack.
* **Static analysis (SAST):** `semgrep --config auto` if Semgrep is installed.
* **Secrets check:** Scan tracked files and config templates (`.env.example`, CI workflows, frontend bundles) for hardcoded keys, tokens, or connection strings.

### Step 3: Targeted Deep Review (Progressive Disclosure)
Load **only** the reference guide(s) relevant to the user's request to keep context lean:

* **Web apps, APIs & auth flows** -> Read [web-api-checklist.md](./references/web-api-checklist.md)
* **Source code, secrets, dependencies & CI/CD** -> Read [code-and-supply-chain.md](./references/code-and-supply-chain.md)
* **Infrastructure, host hardening & detection readiness** -> Read [hardening-and-detection.md](./references/hardening-and-detection.md)
* **Formal audit report output** -> Read [reporting-template.md](./references/reporting-template.md)

### Step 4: Prioritize & Remediate
Rank findings by real-world exploitability and impact (`Critical`, `High`, `Medium`, `Low`, `Info`):
1. Explain **what** is wrong and **where** (exact file and line range).
2. Explain **why** it matters in this specific application (not textbook definitions).
3. Provide a **minimal, drop-in remediation** that fits the project's existing architecture.
4. When fixing code directly, prioritize the highest-severity issues first and keep diffs clean and reversible.

### Step 5: Verify the Fix
Never mark a security fix complete without verification:
* Re-run the relevant test suite or add a targeted regression test (e.g., verifying unauthorized requests return `401`/`403`, invalid input is rejected, or security headers are present).
* Re-run any static/dependency check that flagged the issue and confirm the finding is resolved.
