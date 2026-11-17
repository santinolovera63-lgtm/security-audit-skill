# Host Hardening & Detection Readiness

Use this guide when reviewing server configurations, cloud/deployment setups, or defensive posture on authorized infrastructure.

## 1. Principle of Least Privilege
* **Service accounts:** Applications and background workers should run under dedicated low-privilege accounts—never `root` or `SYSTEM`/`Administrator` unless strictly required.
* **Database & API scopes:** Verify database users have only the grants required for runtime (`SELECT`, `INSERT`, `UPDATE`, `DELETE`), not schema-admin or superuser roles.
* **File permissions:** Restrict read access on private keys, TLS certificates, `.env` files, and backup archives to the owning service user.

## 2. Attack Surface Reduction
* **Network exposure:** Confirm internal services (databases, Redis, admin panels, metrics endpoints, debug ports) bind to `127.0.0.1` or private networks rather than `0.0.0.0`.
* **Debug & development flags:** Ensure `DEBUG=False`, dev endpoints, Swagger/GraphQL playgrounds (if internal), and verbose profiling routes are disabled in production.
* **Unused services & ports:** Identify open listening ports or enabled services that serve no business purpose and recommend disabling or firewalling them.

## 3. Logging, Auditing & Detection Readiness
* **Security-relevant events:** Verify the application logs authentication successes/failures, privilege changes, password resets, API key creation, and administrative actions with timestamps and actor IDs.
* **No sensitive data in logs:** Confirm passwords, session tokens, full credit card numbers, and secret keys are redacted before logging.
* **Structured & tamper-resistant logs:** Prefer structured logs (JSON) forwarded to centralized storage so anomalies can be alerted on and investigated cleanly.

## 4. Defensive Threat Modeling (High-Level)
When documenting risks for Blue/Purple Team alignment, frame findings around high-level defensive domains:
* **Initial Access & Exposure** (public endpoints, misconfigured auth, exposed credentials).
* **Privilege Escalation & Lateral Movement** (over-scoped tokens, missing authorization checks, flat internal networks).
* **Persistence & Data Exposure** (unmonitored admin changes, unencrypted backups, overly permissive storage buckets).

Pair every identified risk with both a **preventive control** (fix the root cause) and a **detective control** (how to know if someone attempts it).
