# Entropy audit — locum (2026-08-22)

## Executive summary

- **Snapshot:** `/Users/marcelo/work/github.com/marcelocantos/locum`, branch `master`, commit `ba85cc15d6d1cc9f2e54c3f0d17f274ddd06019f` (`ba85cc1 Note the public GitHub remote in the README (#1)`). Working tree was clean (`git status --porcelain=v1 -b` showed only `## master...origin/master`). Date: 2026-08-22. Remote: `git@github.com:marcelocantos/locum.git`, public, tracking `origin/master` at the same SHA.
- **Scope:** the entire repository (five tracked files, 653 lines). No generated, vendored, fixture, or source trees exist to skip.
- **Headline mechanism:** this is a pre-implementation **intent ledger**. The product position is supposed to live in `docs/manifesto.md` and the desired states in `bullseye.yaml`, but the README still hosts copies of the human-surface table and the non-goal list. Those copies have already drifted from the manifesto and from 🎯T1 / 🎯T1.7. There is no code, no CI, and no `hygiene.yaml`.
- **Highest-consequence findings:** ENT-001 (interrupt taxonomy has four writers and has already disagreed); ENT-002 (non-goal inventories are independent lists). No P0/P1: nothing executes.
- **Unverified residue:** GitHub wiki is enabled with no pages (no wiki ref); whether the owner intends README tables as a public summary or as a leftover from the manifesto split; language/runtime for a future implementation (no manifest); Dependabot/vulnerability-alerts API 404 vs `security_and_analysis.dependabot_security_updates: disabled`.

## Scope and exclusions

Tracked files: `.gitignore`, `LICENSE`, `README.md`, `bullseye.yaml`, `docs/manifesto.md`.

No `AGENTS.md`, `CLAUDE.md`, `Makefile`, language manifest, `.github/workflows`, `hygiene.yaml`, `CODEOWNERS`, `SECURITY.md`, `NOTICE`, or `CONTRIBUTING.md`. GitHub `languages` is `{}`.

**Languages analyzed:** none. Manifests declare no Python, Go, C/C++, Rust, SQL, Bash, or web runtime. Language companions were not applicable and were not used as verdict sources.

**Named exclusions:** none. There is no generated/vendored/fixture tree. `.git/` was not treated as product.

## Commands run

| Command | Version | Exit | Shipped vs auxiliary | Relevant output / limitations |
|---|---|---|---|---|
| `git rev-parse --abbrev-ref HEAD`; `git rev-parse HEAD`; `git status --porcelain=v1 -b` | git 2.55.0 | 0 | shipped (vcs) | `master`; `ba85cc15d6d1cc9f2e54c3f0d17f274ddd06019f`; clean `## master...origin/master` |
| `git log --stat --format=fuller`; `git ls-files`; `find . -not -path './.git/*' -type f` | git 2.55.0 | 0 | shipped | Four commits, all 2026-08-16. Five files. No untracked product files at start. |
| `git show bff1a38:README.md`; `git show 4f32b49 -- README.md` | git 2.55.0 | 0 | history | Manifesto split left Non-goals + Human surface in README after the commit claimed “README now points at it”. |
| `wc -l` on the five files | — | 0 | auxiliary | 9 + 202 + 37 + 246 + 159 = 653 lines |
| `test -f hygiene.yaml` / presence probes for manifests | — | 0 | shipped | `hygiene.yaml` absent; no language/CI/governance files listed above |
| `/Users/marcelo/.claude/skills/hygiene/hygiene_check.py` | uv 0.6.14, Python 3.13.0 | 1 | auxiliary (hygiene validator) | `FileNotFoundError` for `hygiene.yaml`. Validator does not emit a clean “undeclared” report when the file is missing. |
| `gh repo view`; `gh api repos/marcelocantos/locum`; `gh workflow list`; branch protection; rulesets; languages; contents | gh 2.97.0 | 0 (protection/alerts 404) | auxiliary (GitHub API) | Public; license apache-2.0; default `master`; no workflows; **branch not protected**; rulesets `[]`; `secret_scanning` + `push_protection` **enabled**; Dependabot security updates **disabled**; wiki enabled, no wiki ref; one merged PR `#1`. Vulnerability-alerts endpoint 404. |
| `gh issue list`; `gh pr list --state all` | gh 2.97.0 | 0 | auxiliary | No issues. One merged PR. |
| bullseye MCP `bullseye_query` `view=summary` + `view=validate` | bullseye (in-repo `bullseye.yaml`) | 0 | shipped intent ledger | 14 active / 0 achieved. Validate: advisory only (`T1` “active leaf blocked by unfinished deps”). No `checks:` keys. Children `value=0.0` `cost=0.0`. |
| Python parse of `bullseye.yaml` (`yaml.safe_load`, count `checks`) | Python 3.13.0 | 0 | auxiliary | 14 targets, `checks` present on 0. Not a product test. |

Clone detectors, coverage, linters, and architecture tests were **not** run: there is no implementation corpus, and no analyzer is declared. Installing one would have violated the audit boundary.

## Observed architecture

### Deployable units

None. README states the repository is a ledger with no implementation (`README.md:11-12`, `README.md:36`). Manifesto: “Architecture comes later” (`docs/manifesto.md:3-4`). GitHub languages `{}`.

### Declared layers (agree with observation, with one leak)

| Layer | Declared owner | Observed |
|---|---|---|
| Position / theology | `docs/manifesto.md` | Present. README points at it (`README.md:11-12`). |
| Desired states | `bullseye.yaml` | 14 targets, 🎯T1 converging, 13 children identified. |
| Public summary | `README.md` | Still contains a full human-surface table and a non-goal list after the split. |
| Future session/secret residue | `.gitignore` | Ignores `.env*`, keys, `storage-state.json`, Playwright/agent-browser dirs. |
| License | `LICENSE` Apache-2.0 | File present; GitHub `licenseInfo.key=apache-2.0`. |

### Inferred future runtime (from targets + manifesto; no code)

Intended identity plane, as a dependency graph of properties rather than packages:

```text
T1.4 vault-broker (secrets never in model context)
  <- T1.5 1Password fill
  <- T1.6 real passkey assertion
T1.3 gate detector
  <- T1.7 possession-only interrupts
  <- T1.11 headed hop
  <- T1.9 SAML/OAuth chains (also <- T1.2 headless)
T1.1 session isolation / no shared user-data-dir
T1.10 per-service storage-state
T1.8 independent origin/RP-ID checker (not the navigator)
T1.12 scored-challenge refuse
T1.13 API/PAT/MCP OAuth/device-flow first
     \-> T1 thesis
```

Cross-cutting concerns named but unimplemented: vault fill inside a broker, independent phishing/RP-ID policy, possession interrupts, headed hop as exception UI, captcha cowardice.

`.gitignore:6-9` infers two candidate browser drivers (`storage-state.json`, `.playwright-cli/`, `.agent-browser/`) and forbids committing session/auth state. That is consistent with 🎯T1.1 / 🎯T1.10 and is not a second architecture.

### Declared vs observed rules

- **Agree:** ledger-only; manifesto is the position; bullseye holds desired states; non-goals include no shared god-profile, no virtual WebAuthn for real accounts, no CAPTCHA solver, not “the whole web”.
- **Observed, inferred:** future stack will need a broker process distinct from the navigating agent (🎯T1.4, 🎯T1.8, manifesto “confused deputy”).
- **Contradicted:** “README now points at [the manifesto]” (commit `4f32b49`) vs README still owning copies of the two most-copied artefacts (table + non-goals), which have drifted (ENT-001, ENT-002).
- **Unknown intent:** whether README tables are a deliberate public digest (and should be generated) or incomplete extraction.

### Enforcement

None on the shipped path. No tests, no CI jobs, no architecture tests, no `checks:` on targets. Bullseye validate is advisory graph hygiene only.

## Dimension vector

First audit; “change from baseline” is n/a.

| Dimension | State | Evidence summary | Change from baseline |
|---|---|---|---|
| Architecture topology | healthy | No code cycles. Intent graph is directional (vault hub, detector hub, API-first). Ledger/position/targets are named layers. | n/a |
| Redundancy / sources of truth | concern | Human-surface and non-goal lists exist in README, manifesto (table + prose), and bullseye; copies already disagree. | n/a |
| Change amplification | concern | An interrupt-kind or OTP-copy change must be edited in at least four places (README table, manifesto table, manifesto prose, 🎯T1 + 🎯T1.7). | n/a |
| Local code quality | unknown | No production code. `.gitignore` is small and purpose-built. | n/a |
| Correctness / verification | concern | 14 targets, 0 `checks`, 0 tests, 0 CI. Acceptance is prose. Appropriate to a ledger, but the load-bearing vault-secret claim has no future oracle wired. | n/a |
| Security / dependencies | concern | No dependencies. `.gitignore` excludes secrets/session state. GitHub secret scanning + push protection enabled. Default branch unprotected; Dependabot updates disabled; no `SECURITY.md`. | n/a |
| Build / release / operations | unknown | No build, tags, workflows, or release surface. | n/a |
| Documentation / governance | concern | Manifesto is coherent. Competing copies, no `AGENTS.md`, no `hygiene.yaml`, no CODEOWNERS, wiki enabled, LICENSE appendix still has `[yyyy]` / `[name of copyright owner]`. | n/a |

## Findings

### ENT-001: Interrupt taxonomy has four writers and has already drifted

- **Priority:** P2
- **Dimensions:** Redundancy / sources of truth; Change amplification; Documentation / governance
- **Status:** observed fact
- **Evidence:**
  - README table (`README.md:24-32`): kinds `Vault / passkey UV`, `SMS / email OTP`, `Push`, `Scored challenge`. OTP copy: “Type the digits that just went to your phone for *site*”. Scored: “Real window, or a stop — never an improvised solver”.
  - Manifesto table titled “complete” (`docs/manifesto.md:149-157`): `Vault unlock / passkey UV`, `SMS / email OTP`, `Push / hardware tap`, `Scored challenge`. OTP copy: “The digits, and the site name”. Scored: “A real window, or a stop”.
  - Manifesto prose (`docs/manifesto.md:68-79`): OTP example is GitHub-specific “six digits”; push is “a push or a hardware tap” / “approve the GitHub prompt on your phone”.
  - 🎯T1 (`bullseye.yaml:28`): “Touch ID, OTP digits, push ack”.
  - 🎯T1.7 (`bullseye.yaml:204-206`): kinds “vault/passkey UV, OTP digits, push ack, and scored-challenge handoff or stop”; OTP “names the site and asks only for the code”.
  - 🎯T1.12 (`bullseye.yaml:104`): three legal next steps “avoid the surface, hand a real window, or stop” — the tables omit “avoid”.
  - History: `4f32b49` moved the thesis into the manifesto and said README now points at it; the table was left behind and not subsequently reconciled (`ba85cc1` only added the public URL).
- **Mechanism:** the closed human surface is the product. Four texts define the enum and the copy. They already disagree on (1) whether hardware tap is a kind, (2) Touch ID vs vault/passkey UV vs vault unlock, (3) OTP wording, (4) whether “avoid the surface” is a scored-challenge exit. The next implementation agent will pick a file and ship that enum.
- **Blast radius:** 🎯T1.7, 🎯T1.11 headed hop, 🎯T1.12, any interrupt UX, and every later IdP playbook. Drift is cheap today (markdown) and expensive once a broker exists.
- **Counterevidence checked:** README does point at the manifesto (`README.md:11-12`). Manifesto claims its table is “complete” (`docs/manifesto.md:149`). Bullseye is the SoT for *desired states*, not necessarily for interrupt copy. Those roles do not justify four *different* enums. No test or generator locks them. Deliberate public-summary duplication was not documented.
- **Smallest coherent remediation:** pick one enum owner (manifesto table, or 🎯T1.7 acceptance). Make README a pointer or a generated digest. Align 🎯T1’s “Touch ID” wording with 🎯T1.7. State whether “hardware tap” is the same atom as “push ack”.
- **Verification:** a check that the README interrupt rows, manifesto table rows, and 🎯T1.7 kind list are the same ordered set of names (and that scored-challenge exits match 🎯T1.12).
- **Ratchet candidate:** once any implementation exists, a CI/text fixture that diffs those three lists. Until then, a bullseye `checks` command on 🎯T1.7, or a later `hygiene.yaml` `command:` evidence — not now; do not ratchet in this audit.

### ENT-002: Non-goal inventories are independent lists

- **Priority:** P2
- **Dimensions:** Redundancy / sources of truth; Change amplification
- **Status:** observed fact
- **Evidence:**
  - README `## Non-goals` (`README.md:14-22`): shared identity; second Playwright MCP on the same `user-data-dir`; virtual WebAuthn for real accounts; vault secrets in model context / tool-call arguments; kinematic CAPTCHA solvers; “done for the whole web”.
  - Manifesto `## What locum is not` (`docs/manifesto.md:133-140`): better Playwright MCP on the same locked profile; Chrome monopoly; custom password manager; virtual authenticator for real accounts; CAPTCHA cracker; a product that finishes the web.
  - Overlap is partial. README-only: vault-secret leak, shared identity (the latter is a manifesto *section*, `docs/manifesto.md:110-115`, not an “is not” bullet). Manifesto-only: Chrome monopoly, custom password manager.
- **Mechanism:** two “do not build this” lists. A later reader can satisfy one list and violate the other (e.g. a Chrome-only vault integration is a README-legal non-goal miss, a manifesto violation).
- **Blast radius:** 🎯T1.4, 🎯T1.5, 🎯T1.6, 🎯T1.10, 🎯T1.12, and any “why not Chrome’s password manager” debate.
- **Counterevidence checked:** wording is not required to be identical; manifesto expands “Chrome is not special” in prose (`docs/manifesto.md:29-34`). The README list is a reasonable public subset. The defect is the *unlabeled* dual inventory after the split that was supposed to leave position in the manifesto.
- **Smallest coherent remediation:** README non-goals become a one-line pointer, or a strict subset generated from the manifesto list. Add the two manifesto-only bullets to README only if they are public commitments.
- **Verification:** same as ENT-001: one list, the other links or is generated.
- **Ratchet candidate:** optional later markdown fixture. Low value until code exists.

### ENT-003: Load-bearing claims have acceptance prose and no oracle seam

- **Priority:** P3
- **Dimensions:** Correctness / verification
- **Status:** observed fact (no checks); inference (this will matter at first implementation)
- **Evidence:** `checks:` is absent from all 14 targets (YAML parse; ripgrep `checks:` in `bullseye.yaml` is empty). 🎯T1.4 (`bullseye.yaml:150-164`) is the vault-secret invariant (“A test that asks the model to echo the last fill payload cannot recover a real secret”) but that test is not wired. Children are `value: 0.0` / `cost: 0.0`. No Makefile, no CI, no tests. Validate is advisory only.
- **Mechanism:** bullseye can track intent, but nothing will fail if the first broker logs `op` output into a tool argument. The most important safety property is currently a sentence.
- **Blast radius:** 🎯T1.4, 🎯T1.5, 🎯T1.6, and any agent transcript. Not a current runtime failure: there is no broker.
- **Counterevidence checked:** README “Status: Empty” (`README.md:36`) and manifesto “architecture comes later” make missing tests *expected*. Hygiene is undeclared, so this is not a broken ratchet; it is an absent one. Scoring this P0/P1 would invent a shipped path.
- **Smallest coherent remediation:** when the first broker/test harness lands, put a `checks` entry on 🎯T1.4 that greps transcripts/logs for vault material, and a fixture that tries to echo the last fill.
- **Verification:** 🎯T1.4 `checks` non-empty and executed on the shipped path; a planted secret in a tool log fails CI.
- **Ratchet candidate:** bullseye `checks` + CI job, later `hygiene.yaml` `correctness.*` item. Do not add in this audit.

### ENT-004: Public default branch has no protection or CI; agent/governance files are absent

- **Priority:** P3
- **Dimensions:** Security / dependencies; Build / release / operations; Documentation / governance
- **Status:** observed fact
- **Evidence:**
  - `gh api .../branches/master/protection` → 404 “Branch not protected”; rulesets `[]`.
  - No `.github/workflows`; `gh workflow list` empty.
  - No `AGENTS.md` / `CLAUDE.md` / `CODEOWNERS` / `SECURITY.md` / `CONTRIBUTING.md` / `hygiene.yaml`.
  - Repo `hasWikiEnabled: true` with no wiki git ref (empty competing-doc surface).
  - Dependabot security updates disabled; vulnerability-alerts API 404.
  - Countervailing: `secret_scanning` and `secret_scanning_push_protection` **enabled**; `.gitignore` already drops `.env*`, `*.pem`, `*.key`, `auth-state.json`, `**/storage-state.json`.
- **Mechanism:** anyone with write access (or a compromised token) can push to `master` with no required check. For a five-file ledger the blast radius is documentation. It becomes a secret-leak path the day a vault integration is prototyped on `master`.
- **Blast radius:** default branch, future `.env` / storage-state mistakes, agent-implemented first commit without repo-local rails.
- **Counterevidence checked:** secret scanning + push protection are on; `.gitignore` is forward-looking; `delete_branch_on_merge: true`. No issues tracker debt. Ledger-only status is explicit. This is not a current P0 leak.
- **Smallest coherent remediation:** when implementation starts: `AGENTS.md` (ledger vs code, vault-secret rule, manifesto as position SoT), branch protection requiring CI, `hygiene.yaml` declaring the actual floor. Optionally disable the unused wiki.
- **Verification:** `gh api` protection exists; a workflow file exists; `hygiene_check.py` exits 0 against a real `hygiene.yaml`.
- **Ratchet candidate:** `hygiene.yaml` items for LICENSE/README (already true), CI, and branch protection — after the owner onboards hygiene.

### ENT-005: Apache LICENSE appendix still has the placeholder copyright line; the Work has no NOTICE

- **Priority:** P3
- **Dimensions:** Documentation / governance
- **Status:** observed fact
- **Evidence:** `LICENSE:190` is `Copyright [yyyy] [name of copyright owner]`. Ripgrep `Copyright` in the work hits only the Apache text and that placeholder. No `NOTICE`. GitHub still classifies `licenseInfo.key=apache-2.0`.
- **Mechanism:** the license *file* is the stock Apache 2.0 text (placeholders in the appendix are normal). The Work itself never states year/owner. Downstream redistributors lack the notice Apache §4(c) expects.
- **Blast radius:** legal metadata only; no runtime.
- **Counterevidence checked:** this is the GitHub license-picker shape; many Apache repos leave the appendix intact and put copyright in a NOTICE or file headers. GitHub UI already shows Apache-2.0.
- **Smallest coherent remediation:** add a one-line copyright (README or NOTICE) with year and owner; leave the LICENSE body alone.
- **Verification:** a file outside `LICENSE` matches `Copyright 2026 Marcelo Cantos` (or the owner’s chosen notice).
- **Ratchet candidate:** hygiene `file:` evidence on NOTICE or README copyright line, if the owner wants it.

## Redundancy and competing-source-of-truth inventory

| Concept | Writers | Drift already? | Intended owner (declared / inferred) |
|---|---|---|---|
| Product position / theology | `docs/manifesto.md` | n/a (canonical) | manifesto (declared) |
| Human-surface interrupt enum + copy | README table; manifesto table; manifesto prose; 🎯T1; 🎯T1.7; 🎯T1.12 | **yes** (ENT-001) | manifesto table *or* 🎯T1.7 — **unresolved** |
| Non-goals / “what locum is not” | README list; manifesto list; scattered manifesto sections | **yes** (ENT-002) | manifesto (inferred from split commit) |
| Desired states / acceptance | `bullseye.yaml` only | no second YAML | bullseye (declared) |
| Session/profile isolation | 🎯T1.1, 🎯T1.10, manifesto “one identity”, README non-goal, `.gitignore` | wording differs, not contradictory | bullseye for tests; manifesto for why |
| Vault-secret isolation | 🎯T1, 🎯T1.4, README non-goal, manifesto broker prose | aligned in intent, not in an oracle | 🎯T1.4 (inferred) |
| GitHub repo description | matches README opening sentence | no | README |

Deliberate duplication that the audit failed to invalidate: repeating “headless is the default; a window is a hop” in manifesto and 🎯T1.2 is slogan-level, not a competing enum.

## Healthy structure worth retaining

- **Ledger vs implementation split is honest.** README `README.md:11-12` and `README.md:36` match the tree. Do not invent a code architecture to fill the vacuum.
- **Manifesto is a position, not a design doc** (`docs/manifesto.md:1-4`). That boundary is the right one for this repo.
- **🎯T1 decomposition is coherent.** Vault secrets (🎯T1.4) gate fill and passkeys. Gate detection (🎯T1.3) gates interrupts, headed hop, and SAML. API-first (🎯T1.13) and captcha refuse (🎯T1.12) are first-class, not afterthoughts. Bullseye validate is clean except an advisory on 🎯T1 as a blocked parent.
- **Independent policy box** (🎯T1.8; manifesto “navigator-reviews-own-destination”) is a real boundary, not a slogan: checker ≠ navigator.
- **`.gitignore` already excludes the future secret/session artefacts** (`.env*`, keys, `storage-state.json`) before any code can commit them.
- **GitHub secret scanning and push protection are on** for a public repo.
- **Apache-2.0 is declared** and recognized by GitHub.
- **Public remote is explicit** (`README.md:37`).

## Hygiene posture

**Hygiene posture not declared.** `hygiene.yaml` is absent. It was not initialized.

Validator invocation (mandatory even on absence):

```text
$ /Users/marcelo/.claude/skills/hygiene/hygiene_check.py
FileNotFoundError: .../locum/hygiene.yaml
exit 1
```

No per-dimension held tiers or floors exist to report. No drift vs a declared posture. No planned/skipped gaps.

Overlap with entropy: ENT-003 and ENT-004 are the items a later `hygiene.yaml` would cover (tests/CI, LICENSE/README already true as files, secret scan already true on GitHub, branch protection not true). Do not treat this audit’s finding list as a hygiene floor.

Entropy findings suitable for future hygiene enforcement: ENT-001 consistency check; ENT-003 🎯T1.4 oracle; ENT-004 CI + protection; ENT-005 copyright/NOTICE.

## Oracle coverage and residue

| Property | Decided by |
|---|---|
| Repo is ledger-only / no implementation | Shipped tree + README (manual inspection of `git ls-files`) |
| 🎯T1 graph well-formed | Auxiliary: bullseye validate (advisory warning only) |
| Interrupt enum consistency | **Nothing** (ENT-001) |
| Vault secrets never in model context | Prose acceptance only; **no shipped oracle** (ENT-003) |
| Session isolation / no shared user-data-dir | Prose only |
| Origin/RP-ID checker independence | Prose only |
| Scored challenges refused | Prose only |
| License is Apache-2.0 | File + GitHub license API |
| Secrets not committed | `.gitignore` (convention) + GitHub secret scanning / push protection (platform). No gitleaks/trufflehog in CI. |
| Branch protection | **Absent** (API 404) |
| Tests pass | **No tests** |
| Hygiene floors | **Undeclared** |
| Future language/runtime | **Unknown** |

Failed/skipped checks: `hygiene_check.py` exit 1 (missing file); branch protection 404; vulnerability-alerts 404; no workflows.

### Owner residue (intent, not mechanical work)

- Is the README human-surface table a public digest that should stay (and be generated), or leftover from `4f32b49`?
- Is “hardware tap” the same interrupt kind as “push ack”, or a fifth kind?
- Should “avoid the surface” appear on the human-surface table, or only in 🎯T1.12 as a non-interrupt exit?
- When should `hygiene.yaml` / `AGENTS.md` / CI appear — at first code, or while still a ledger?
- Which runtime is intended (Playwright CLI vs agent-browser vs something else)? `.gitignore` names two.

## Remediation sequence

1. **Do not add code, CI, or `hygiene.yaml` in the name of this audit.** The ledger is the product today.
2. **Converge the human-surface enum (ENT-001) and the non-goal list (ENT-002)** onto one owner. Point the others. This is the only change that removes demonstrated drift before implementation multiplies it.
3. **When the first broker or browser driver lands:** `AGENTS.md` stating manifesto = position, bullseye = desired states, vault secrets never in tool args; 🎯T1.4 `checks` on the shipped path; `.gitignore` remains; branch protection + a single CI workflow.
4. **Onboard `hygiene.yaml` from that reality** (do not invent floors above LICENSE/README/secret-scan). Optionally add NOTICE/copyright (ENT-005).
5. **Re-run this audit** against the same definitions (interrupt enum, non-goal list, presence/absence of `checks` on 🎯T1.4, `hygiene.yaml` presence).

No architectural rewrite is indicated. There is no architecture in code to rewrite.
