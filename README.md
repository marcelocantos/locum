# locum

A locum tenens for the open web: the agent does the practice; the human
appears only for what a deputy cannot forge.

Locum is a browser deputy with an identity plane. It drives pages headless,
keeps sessions per service rather than one shared person, fills from a real
vault, and interrupts you for possession factors — not for SAML, not for
judging a login page, not for the rest of the site.

This repository is a ledger. There is no implementation yet. The
position is in [docs/manifesto.md](docs/manifesto.md).

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

Empty. Desired states live in `bullseye.yaml`.
Public: https://github.com/marcelocantos/locum
