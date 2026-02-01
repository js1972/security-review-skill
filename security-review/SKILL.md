---
name: security-review
description: Perform a security review of a local codebase (frontend, backend, or full-stack). Use when asked to audit, review, assess, or harden an application's security, produce a short prioritized list of issues, or enumerate backend endpoints with authentication and rate-limiting details.
---

# Security Review

## Overview
Perform a focused application security review of the project in the current working directory. Identify the attack surface, verify security controls, and report a short prioritized list of issues. If the project includes a backend, also produce an endpoint inventory table with auth, rate limiting, and a 1-5 security rating.

## Workflow

### 1) Identify project type and stack
- Determine whether this is frontend, backend, or full-stack by inspecting top-level manifests and frameworks.
- Note the language, framework, and entry points (server startup, routes, API clients).
- If both frontend and backend exist, review both and still produce the backend endpoint inventory.

### 2) Map the attack surface
- For backends: enumerate HTTP endpoints, GraphQL operations, WebSocket events, and gRPC services.
- For frontends: identify API clients, auth flows, token storage, and security-sensitive UI paths.
- Use `references/endpoint-discovery.md` for framework-specific route discovery patterns.

### 3) Review security controls and risks
- Use `references/security-review-checklist.md` for detailed checks.
- Prioritize: authn/authz, injection, data exposure, secrets, dependency risks, security headers, CSRF/CORS, file uploads, SSRF, and rate limiting.
- Gather concrete evidence (file path + line) for each finding.

### 4) Produce the report
- Provide a short prioritized list of the most important issues (aim for 3-8).
- If backend present, include the endpoint inventory table (required).
- Call out unknowns and suggest follow-up verification when the codebase lacks evidence.

## Required Output Format

### Findings (prioritized)
For each finding include:
- Severity (Critical/High/Medium/Low)
- Title
- Evidence (file path + line)
- Impact
- Recommendation

### Endpoint Inventory (backend only)
Provide a markdown table with these columns:

`Method | Path | Handler/Location | Auth (Yes/No/Partial/Unknown) | Auth Mechanism | Rate Limiting (Yes/No/Unknown) | Security Rating (1-5) | Notes`

Rules:
- List every exposed endpoint you can identify.
- If an endpoint is unauthenticated:
  - Explain why it is acceptable (e.g., health check, public content) OR
  - Flag it as HIGH priority and add a corresponding finding.
- If auth or rate limiting is unclear, mark as Unknown and add a note.

### Security Rating Scale
- 1: Unprotected or clearly exploitable
- 2: Weak controls / significant gaps
- 3: Moderate / mixed or unclear controls
- 4: Strong controls with minor gaps
- 5: Strong controls with defense in depth

## Notes
- Do not run active security scans or make network calls; this is a code review.
- Be explicit about assumptions and unknowns.
