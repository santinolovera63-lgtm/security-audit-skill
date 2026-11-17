# security-audit-skill

An open-source **Agent Skill** for authorized defensive security auditing, threat modeling, and application/infrastructure hardening.

Built following the open Agent Skills specification with **progressive disclosure**, so your coding agent (Antigravity, Claude Code, Codex, Cursor, etc.) stays fast and token-efficient—loading only the specific security checklist needed for the task at hand.

## What It Covers

* **Structured 5-Step Audit Workflow:** Attack surface mapping, read-only local checks, targeted deep review, minimal remediations, and fix verification.
* **Web & API Security:** Authentication, session/JWT safety, authorization/IDOR checks, injection prevention, SSRF, CORS/CSRF, and security headers.
* **Code, Secrets & Supply Chain:** Credential hygiene, frontend secret leaks, lockfiles, dependency audits, SAST triage (`semgrep`, linters), CI/CD, and container hardening.
* **Host Hardening & Detection Readiness:** Least privilege, network exposure reduction, structured security logging, and preventive + detective controls.
* **Standardized Audit Reporting:** Clean, evidence-backed report template ranked by real-world impact.

## Repository Structure

```text
security-audit-skill/
├── README.md
├── LICENSE
└── skills/
    └── security-audit/
        ├── SKILL.md
        └── references/
            ├── web-api-checklist.md
            ├── code-and-supply-chain.md
            ├── hardening-and-detection.md
            └── reporting-template.md
```

## Installation

### Using `npx skills` (Recommended once published on GitHub)

```bash
npx skills add santinolovera63-lgtm/security-audit-skill
```

### Manual / Local Symlink Installation

Clone the repository and link `skills/security-audit` into your agent's global skills folder so `git pull` keeps it updated automatically:

* **Antigravity / Gemini:** `~/.gemini/config/skills/security-audit`
* **Claude Code:** `~/.claude/skills/security-audit`
* **Codex / Project-level:** `.agents/skills/security-audit`

#### Windows (PowerShell / CMD)

```cmd
mklink /J "%USERPROFILE%\.gemini\config\skills\security-audit" "C:\path\to\security-audit-skill\skills\security-audit"
mklink /J "%USERPROFILE%\.claude\skills\security-audit" "C:\path\to\security-audit-skill\skills\security-audit"
```

#### macOS / Linux

```bash
ln -s /path/to/security-audit-skill/skills/security-audit ~/.gemini/config/skills/security-audit
ln -s /path/to/security-audit-skill/skills/security-audit ~/.claude/skills/security-audit
```

## Example Prompts

* *"Audit this Next.js API for auth, IDOR, and injection issues."*
* *"Check this repository for exposed secrets, risky dependencies, and insecure GitHub Actions workflows."*
* *"Review my Dockerfile and server configuration for production hardening."*
* *"Generate a security audit report for the `src/auth` module with concrete fixes."*

## Ethics & Authorized Use

This skill is strictly designed for **defensive security, code review, and authorized assessments** on systems you own or have explicit permission to test. It does not generate exploits, phishing material, or evasion payloads.

## License

MIT — see [LICENSE](./LICENSE).
