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

See [What locum is not](docs/manifesto.md#what-locum-is-not). This README
does not keep a second inventory.

## Human surface

Same kinds as [the manifesto](docs/manifesto.md#the-human-surface-complete)
and 🎯T1.7. Hardware tap is push ack, not a fifth kind.

| Interrupt | What you see |
|---|---|
| Vault / passkey UV | System or 1Password sheet |
| OTP digits | The digits, and the site name |
| Push ack | “Approve the prompt on your phone for *site*” |
| Scored challenge | Avoid the surface, a real window, or a stop |
| Everything else | Nothing |

## Status

Empty. Desired states live in `bullseye.yaml`.
Public: https://github.com/marcelocantos/locum
