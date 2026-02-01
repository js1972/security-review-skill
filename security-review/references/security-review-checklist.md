# Security Review Checklist

Keep this list short and focus on the highest-risk issues first. Tailor to the tech stack.

## Cross-cutting checks
- Secrets in repo, config, or build output; verify secret loading from env or vault
- Dependency risks: vulnerable or unpinned dependencies; unsafe third-party scripts
- Error handling: stack traces or sensitive data in responses/logs
- Logging/monitoring: auth failures, rate limit hits, suspicious activity
- Configuration: CORS, security headers, TLS, cookie flags, HSTS

## Backend-focused checks
- Authentication: enforced on protected routes; no auth bypass paths
- Authorization: role/tenant checks; object-level access control
- Input validation: schema validation and strict parsing
- Injection: SQL/NoSQL/command/template injection risk
- SSRF and outbound requests: allowlists, DNS pinning, metadata blocking
- File uploads: type/size validation, storage isolation, malware scanning
- Rate limiting: login, signup, password reset, and high-cost endpoints
- Data exposure: sensitive fields filtered, admin endpoints protected
- Deserialization and unsafe eval/exec usage

## Frontend-focused checks
- XSS: unsafe HTML injection, untrusted markdown, DOM sinks
- Token storage: avoid localStorage for sensitive tokens when possible
- CSRF: protections for cookie-based auth
- CSP and security headers for main app routes
- Third-party scripts: SRI, integrity, minimal permissions
- Client-side authz assumptions: enforce on server as source of truth

## Full-stack checks
- Auth flow consistency between client and server
- Least-privilege API design; avoid overly broad scopes
- Account recovery and password reset security
- Multi-tenant isolation and IDOR risks
