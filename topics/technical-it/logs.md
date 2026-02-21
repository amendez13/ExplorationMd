# Technical IT - Source Log

*Chronological record of sources for this topic*

---

### [What is OAuth?](https://leaflet.pub/p/did:plc:3vdrgzr2zybocs45yfhcr6ur/3mfd2oxx5v22b)
*2026-02-21T00:00:00Z* | Tags: oauth, oidc, authentication, authorization, web-standards

Blaine (original OAuth specification author) explains the historical cascade of requirements that led to OAuth's design. Covers the Twitter/OpenID origins, how OIDC is functionally equivalent to magic link authentication, and why the standard's complexity obscures its simple core purpose.

**Key Points:**
- OIDC is functionally equivalent to magic link authentication - sending a secret to a place only the user can access
- OAuth emerged from Twitter's need to support OpenID without passwords for desktop clients
- At its core, OAuth sends a multi-use secret to a known delegate with consent, then details how to use that secret for subsequent requests
- The IETF standards function more as a framework than strict standard
- Understanding the "why" before the "how" is key to implementing OAuth correctly

---
