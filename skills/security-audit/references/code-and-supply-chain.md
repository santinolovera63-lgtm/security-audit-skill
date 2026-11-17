# Code, Secrets & Supply Chain Checklist

Use this guide when auditing repositories, dependency trees, build pipelines, and container configurations.

## 1. Secrets & Credential Hygiene
* **Source & history:** Check source code, `.env` files, config files, and test fixtures for API keys, private keys, database URIs, OAuth secrets, or bearer tokens.
* **Client-side exposure:** Ensure server-only secrets are never bundled into frontend code (e.g., mistakenly prefixed with `NEXT_PUBLIC_`, `VITE_`, or `REACT_APP_`).
* **`.gitignore` coverage:** Confirm local secret files (`.env`, `.env.local`, `*.pem`, `*.key`, service account JSONs) are ignored before commits.
* **Rotation advice:** If a real secret is found in Git history, note that removing it from the latest commit is not enough—the credential must be rotated and revoked at the provider.

## 2. Dependencies & Supply Chain
* **Lockfiles:** Verify a deterministic lockfile (`package-lock.json`, `pnpm-lock.yaml`, `uv.lock`, `poetry.lock`, `Cargo.lock`) is committed and used in CI (`npm ci` instead of `npm install`).
* **Known vulnerabilities:** Run the ecosystem's built-in audit command and focus on **reachable, production** dependencies first (distinguishing dev-only tooling from runtime exposure).
* **Typosquatting & abandoned packages:** Flag newly added or suspiciously named dependencies, unmaintained critical libraries, and unnecessary `postinstall` scripts.

## 3. Static Analysis (SAST)
When local tools are available, run targeted read-only scans and triage findings manually to eliminate false positives:
* **Semgrep:** `semgrep --config auto --quiet` (great for multi-language patterns).
* **Language linters:** `eslint-plugin-security`, `bandit` (Python), `gosec` (Go), `cargo clippy` (Rust).

## 4. CI/CD & Container Security
* **Least-privilege CI tokens:** Check GitHub Actions / CI workflows for overly broad permissions; recommend explicit minimal `permissions:` blocks (e.g., `contents: read`).
* **Pinned actions & base images:** Prefer pinning third-party CI actions to commit SHAs and Docker base images to specific versions/digests rather than `latest`.
* **Non-root containers:** Verify Dockerfiles drop privileges using a dedicated `USER` directive and avoid copying `.git`, `.env`, or dev secrets into the final image (check `.dockerignore`).
* **Untrusted PR triggers:** Flag `pull_request_target` workflows that check out untrusted fork code with access to repository secrets.
