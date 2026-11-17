# Web & API Security Checklist

Use this guide when auditing HTTP servers, backend APIs, single-page applications, SSR apps, or authentication flows.

## 1. Authentication & Session Management
* **Session cookies:** Verify `HttpOnly`, `Secure`, and `SameSite` (`Lax` or `Strict`) flags are set on session/auth cookies.
* **Token handling:** Ensure JWTs verify signature, `exp`, `iss`, and `aud`; never allow `alg: none` or fall back to symmetric verification with a public key.
* **Password storage:** Confirm adaptive hashing (`Argon2id`, `scrypt`, or `bcrypt`)—never plain MD5/SHA1/SHA256 without a dedicated password KDF.
* **Brute-force & abuse protection:** Check for rate limiting on login, password reset, OTP verification, and registration endpoints.

## 2. Authorization & Access Control
* **Server-side enforcement:** Every protected endpoint must verify authentication and resource ownership server-side—never rely on hidden UI controls or client-supplied role flags.
* **Object-level access (IDOR / BOLA):** When an endpoint accepts a record ID (`/orders/:id`, `/files/:id`), verify the authenticated user owns or has explicit permission to access that specific record.
* **Privilege boundaries:** Confirm admin/internal routes enforce explicit role/scope checks and fail closed by default.
* **Mass assignment:** Ensure request payloads are explicitly validated/allowlisted (e.g., Zod, Pydantic, DTOs) before passing objects to ORM `.create()` or `.update()`.

## 3. Input Validation & Injection Prevention
* **Database queries:** Verify parameterized queries or safe ORM builders everywhere. Flag string interpolation or concatenation inside SQL/NoSQL queries.
* **Command & shell execution:** Avoid shell invocation (`exec`, `system`, `shell=True`) with untrusted input. Use argument arrays (`execFile`, `spawn` without shell) and strict allowlists.
* **XSS & template safety:** Check for raw HTML sinks (`dangerouslySetInnerHTML`, `v-html`, `innerHTML`, unescaped template blocks) fed by user-controlled data.
* **SSRF:** If the server fetches user-supplied URLs (webhooks, previews, imports), enforce scheme allowlists (`https`), resolve DNS carefully, and block loopback/link-local/private IP ranges.
* **File uploads:** Validate MIME type and magic bytes, enforce size limits, generate safe random filenames (prevent path traversal), and store outside executable web roots.

## 4. Browser & Transport Hardening
* **CORS:** Flag `Access-Control-Allow-Origin: *` combined with `Access-Control-Allow-Credentials: true`, or blind reflection of the `Origin` header.
* **CSRF:** For cookie-based state-changing requests, verify CSRF tokens or strict `SameSite` + `Origin`/`Referer` checks.
* **Security headers:** Check for `Content-Security-Policy`, `Strict-Transport-Security` (HSTS), `X-Content-Type-Options: nosniff`, and `Referrer-Policy`.
* **Error handling:** Confirm production error responses do not leak stack traces, SQL errors, internal file paths, or environment variables.
