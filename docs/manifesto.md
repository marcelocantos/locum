# Manifesto

This is not a design document. It is the position locum exists to hold.
Architecture comes later, and only where it serves these claims.

## The deputy

An agent that can see a page can operate that page. Clicks, fills,
redirects, SAML hops, OAuth chrome, “Next” buttons — these are chores.
They are not a reason to put a human in the chair.

Locum stands in for a human on the open web the way a locum tenens
stands in for a practice. The principal does not walk every waiting
room. The principal appears when a proof is required that a deputy
cannot forge.

Headless is the default. A window is a hop, not the product.

## What is not special

The industry has a habit of promoting accidents into rituals. Locum
refuses the promotions.

**Playwright’s exclusive owner is not a law of browsers.** It is
Chromium locking one `user-data-dir`. Named sessions and
`storage-state` copies do not have that lock. A second MCP pointed at
the same profile does. Locum never opens someone else’s profile.

**Chrome is not special.** Blink will render a login form in any
Chromium. What people mean by “real Chrome” is a signed process that
is allowed to open a particular vault and to call a particular
authenticator. That process can be Chrome, Safari, or an entitled app
you write. Playwright’s bundled Chromium is a different principal on
the keychain ACL. The engine is not the point. The ACL is.

**A vault is not special, and Chrome’s is not the best one.** Any
entitled app can hold secrets. 1Password already does, and locum
should ask it — not the model, not a custom sqlite file, not a
chat prompt. `op` can hand a password or a TOTP to a broker. The
broker fills. The model never sees the string.

**SAML is not special.** Neither is OAuth consent HTML. They are
pages and redirects. If the agent can see what you can see, the agent
can finish the dance. “Drive the IdP” is not a human job.

**Judging a login page is not a human job.** “Look at this URL and
decide if you mean it” is the same control as humans reviewing
agent-written PRs. In theory it catches the model. In practice the
human rubber-stamps, and rubber-stamping is how phishing works. A
tired person is more likely to type into `github-login.evil` than a
checker that only allows the exact RP ID, the known IdP host graph,
and a matching cert. Passkeys and OTP already moved the secret off
the phishable form. Keeping the human as the phishing reviewer is the
weaker control.

That checker must not be the same process that chose the URL.
Navigator-reviews-own-destination is author-reviews-own-PR.

## What is load-bearing

A human is involved only when the verifier demands a proof defined to
be unforgeable by a process that can see the page.

That process is what a stolen session, a malicious extension, and the
agent all look like. Sight does not prove who authorized the access.

**Possession, not judgment.**

- The authenticator will not sign a passkey without user verification.
  The private key never reaches the page. `op item get` cannot complete
  the ceremony. 1Password can, as an authenticator (extension or OS
  passkey provider), not as a CLI secret. The human sees a vault /
  passkey UV sheet (Touch ID or 1Password). The RP name on that sheet
  comes from the authenticator, not from the page title.
- An SMS or email OTP lives on a channel locum does not have. The
  interrupt names the site and asks only for the code. The agent
  already filled the form and followed the hops.
- A push ack (a push or a hardware tap — one kind) is “approve the
  prompt on your phone for *site*.” Nothing else.

After the vault is unlocked, username, password, TOTP, SAML, and
consent chrome are the agent’s.

The industry collapsed those atoms into “here is a browser, you are
the user.” That was laziness plus liability, plus bot-defense leaking
into every IdP. It was never entailed by SAML, OAuth, or MFA.

## What we will not pretend is a duty

Scored challenges — reCAPTCHA and friends — are a vendor control aimed
at deputies. Locum is a deputy on purpose. A challenge that honest
automation fails and paid solver farms pass is a tax, not a security
property the principal asked for. Passkeys and OTP already do the
identity job.

Locum does not promote that tax into a human ritual. It also does not
sit the exam badly. A scored challenge grades the body: mouse path,
timing, focus, cookies, IP reputation. A solver that gets the tiles
right and the kinematics wrong is worse than failing. The next step is
often account or IP punishment.

So: do not touch a scored challenge unless the interaction *is* the
principal. If it is not, do not try. Avoid the surface (API, token,
MCP OAuth, a warm session that never trips the score). If a puzzle
still appears, hand a real window or stop. Never wiggle the pointer.
Never call a solver.

That handoff is damage control, not theology.

## One identity is a mistake

Locum does not keep one logged-in person and share it around. Sessions
are per service or per task. Isolation multiplies logins. That is
intended. The cost is more possession interrupts, not a god-profile
that every worker contends for and every leak empties.

Cooperating services should never see a browser. API, PAT, MCP OAuth,
device flow — the page is the fallback for services that did not opt
in.

## The confused deputy

If the vault is unlocked for the session and passkeys auto-assert for
whatever origin the worker opens, a compromised or over-eager agent
*is* you, on every item in that vault. That is the same shape already
accepted for code.

The mitigation is the same as for code: narrow grants, an independent
checker, and a human only for the factor the machine cannot forge.

The residual is real and smaller than “make the human drive Okta.”

## What locum is not

- A better Playwright MCP on the same locked profile
- One shared logged-in identity across all work
- A Chrome monopoly
- A custom password manager
- A deputy that puts vault secrets in model context or tool-call arguments
- A virtual authenticator for real accounts
- A CAPTCHA cracker
- A product that finishes the web

The open web is an allowlist of IdPs you teach, plus an ever-growing
tail of risk scores, device-trust emails, and invisible scoring that
never shows a puzzle. Phase 1 is ordinary password and TOTP sites,
OTP interrupts, and cowardly CAPTCHA. Phase 2 is the headed hop for
passkeys and the SAML chains you actually use. “Whatever I navigate
to” does not converge.

## The human surface, complete

These kinds are the 🎯T1.7 possession atoms (hardware tap is push ack).
Scored-challenge exits match 🎯T1.12: avoid the surface, hand a real
window, or stop.

| Interrupt | What you see |
|---|---|
| Vault / passkey UV | System or 1Password sheet |
| OTP digits | The digits, and the site name |
| Push ack | “Approve the prompt on your phone for *site*” |
| Scored challenge | Avoid the surface, a real window, or a stop |
| Everything else | Nothing |

If locum is showing you Okta, it has already failed.
