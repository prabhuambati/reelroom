# Security boundaries

Reelroom is a loopback-only local MVP. Do not expose its HTTP server to a LAN or the public internet. Public-hosting readiness has not been established.

## Implemented protections

- Separate account database; no default account/password.
- One-use first-admin setup code, generated locally and valid for 15 minutes.
- Salted scrypt password hashing; expiring, hashed server-side session tokens.
- HttpOnly, SameSite=Strict session cookies; current-session invalidation on logout.
- Same-origin JSON and rate limiting for login/signup/setup; per-session CSRF tokens for authenticated mutations.
- Allowed Host validation, no-store responses, MIME-sniffing and referrer protections.
- Backend role gates for reviews/import/export/admin operations, and login gates for private media/range requests.
- Listener responses omit filesystem paths, signed source URLs and internal review details.
- Private playlists/favorites use account-scoped lookups; foreign playlist IDs are denied.
- Source downloads and non-indexed files cannot be accessed through the playback route.
- Creator invitations require explicit human-admin verification, configured official contact and an existing target account. Tokens are hashed, account/page-bound, expiring, revocable and consumed atomically once.
- Replays, wrong recipients, conflicting ownership and captured stable-ID changes fail safely. Revocation of redeemed access invalidates the affected account's sessions.
- Request logging omits query strings, cookies, passwords and raw invitation tokens.

These controls are not equivalent to a third-party security audit or a production security guarantee.

## Development exceptions and missing features

Cookies lack Secure only because this server runs on loopback HTTP. A public environment requires HTTPS and Secure cookies. Legacy inline scripts currently require `unsafe-inline` in the CSP; that needs hardening before public hosting. There is no email verification, password-reset service, multi-factor admin authentication, recovery workflow or automated abuse-monitoring service.

SQLite databases, media, exports, model evidence and account/session data must remain private. OS file permissions are part of the local threat model; account storage is created with owner-only permissions on supported platforms. A user with control of the host can manipulate local files, so localhost is not a sandbox against a compromised computer.

## Creator verification is a human process

A supplied username, blue badge, screenshot, biography or matching photo is not proof of control. The admin must inspect the actual Instagram conversation and source identity, confirm the intended local account and record a minimal private reference. Do not request Instagram passwords, verification codes or cookies. A creator binding does not establish copyright permission or the roles of coauthors.

Do not put credentials, session cookies, setup/invitation tokens, signed media URLs or private DM evidence into public issues. Use an appropriate private owner-contact channel for sensitive reports; no reporting inbox is configured by this source release.
