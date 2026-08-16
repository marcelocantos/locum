# locum

A locum tenens for the open web: the agent does the practice; the human
appears only for what a deputy cannot forge.

Locum is a browser deputy with an identity plane. It drives pages headless,
keeps sessions per service rather than one shared person, fills from a real
vault, and interrupts you for possession factors — not for SAML, not for
judging a login page, not for the rest of the site.

This repository is a ledger. There is no implementation yet.

## Thesis

The agent can see and operate the same DOM you can. SAML, OAuth consent
chrome, and ordinary forms are chores, not rituals. Chrome is not special;
it is one signed process that happens to be on a keychain ACL. A vault is
not special; 1Password is a better one. Intent-via-staring-at-the-page is
the same failing control as humans reviewing agent-written PRs.

A human is involved only when the verifier demands a proof defined to be
unforgeable by a process that can see the page:

- Touch ID / vault unlock (OS or 1Password sheet; RP name from the
  authenticator, not the page title)
- An OTP that arrived on a channel locum does not have
- A push/hardware tap on a device locum does not have

Scored challenges (reCAPTCHA and friends) are vendor hostility, not a
human duty. Locum does not solve them. A half-good mouse path is how you
lose the account. Avoid the surface, hand a real window, or stop.

Phishing review belongs to an independent policy box (exact RP ID, known
IdP graph, cert), not the navigator and not the tired human.

## Non-goals

- One shared logged-in identity across all work
- A second Playwright MCP on the same `user-data-dir`
- Virtual WebAuthn authenticators for real accounts
- Putting vault secrets in model context or tool-call arguments
- Kinematic CAPTCHA solvers
- “Done for the whole web” — locum grows an allowlist of IdPs; it does
  not converge on every site

## Human surface

| Interrupt | What you see |
|---|---|
| Vault / passkey UV | System or 1Password sheet |
| SMS / email OTP | “Type the digits that just went to your phone for *site*” |
| Push | “Approve the prompt on your phone for *site*” |
| Scored challenge | Real window, or a stop — never an improvised solver |
| Everything else | Nothing |

## Status

Empty. Desired states live in `bullseye.yaml`. No GitHub remote until
visibility is chosen.
