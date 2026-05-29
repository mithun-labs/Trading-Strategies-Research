# Trading Strategy Research Agent

An autonomous, skeptical quantitative research analyst that runs inside **Claude Code Desktop** to discover, dissect, and evaluate publicly documented trading strategies across Forex, Crypto, Stocks, Gold/XAUUSD, and major indices (NAS100, US30, etc.), persisting every finding as a structured markdown archive in this GitHub repository. It exists to build a durable, version-controlled research database whose defining value is honesty: a well-reasoned *"this is unverifiable / probably has no edge"* is treated as a more valuable outcome than an exciting but unfounded write-up.

---

## Overview

The system has no memory between runs. Every bit of continuity comes from files in this repository, which is the **primary persistent storage and canonical long-term memory** (research database, version control, historical archive, recovery system, audit trail, and checkpointing mechanism). Local filesystem storage is **temporary** — a working directory only, never assumed to persist between runs.

Four components interact each run:

- **Claude Code Desktop** — the runtime. Provides native filesystem access plus web search/fetch tools, and runs the agent either on a schedule or interactively.
- **The research agent** — operating under `CLAUDE.md`, it reads prior state, researches one strategy end-to-end, scores it, and persists it before moving on.
- **The GitHub repository** — canonical memory. State is read from it at the start of each run and committed/pushed back before the run finishes.
- **The `research/` archive** — the structured markdown database the agent reads and writes: strategy files, rankings, a master index, a candidate queue, source archives, and daily reports.

A run is therefore a loop of *load state from GitHub → research → persist back to GitHub*, with each completed strategy landing on `main` before the next one begins.

---

## Key Features & Design Principles

- **GitHub as canonical memory** — no cross-run state is held in memory; the repository is the single source of truth.
- **One strategy at a time, atomic** — each strategy is researched and saved end-to-end as a self-contained transaction; no batching.
- **Skeptic-first evaluation** — the strongest case that a strategy *is nothing* is written before any score is assigned.
- **Anti-fabrication hard rules** — no invented statistics, sources, URLs, or quotes; unstated values are recorded as `NOT REPORTED`.
- **Honesty-capped confidence scoring** — scores are capped by the auditability of the evidence, so unverifiable strategies cannot score highly.
- **Repository integrity over quantity** — a smaller, fully recoverable, consistent database outranks a larger but corrupted one.
- **Concurrency safety** — a local lock file prevents two runs from operating on the repository simultaneously.

The priority order, highest first: (1) repository integrity, (2) successful persistence, (3) recoverability, (4) research accuracy, (5) evidence quality, (6) research quantity.

---

## Run Modes

| Mode | How | When |
|---|---|---|
| **Headless** | `claude -p "Run today's research pass"` | Scheduled task / cron / CI |
| **Remote Control** | Via claude.ai/code, iOS/Android app | Async oversight during a run |
| **Interactive** | Terminal session | Debugging, recovery, first-time setup |

---

## Repository Architecture

Information flows in a single loop, anchored on GitHub:

```
                ┌─────────────────────────────┐
                │     Claude Code Desktop     │
                │  (scheduled task / CLI /    │
                │   remote control)           │
                └──────────────┬──────────────┘
                               │ runs
                       ┌───────▼────────┐
                       │ Research Agent │
                       │  (CLAUDE.md)   │
                       └───┬────────┬───┘
            reads state at │        │ commits + pushes
            start of run   │        │ each completed strategy
                       ┌───▼────────▼───┐
                       │     GitHub     │  ← canonical long-term memory
                       │  (main branch) │
                       └───────┬────────┘
                               │ working copy
                       ┌───────▼────────┐
                       │   research/    │
                       │  archive tree  │
                       └────────────────┘
```

- **`research/`** — the working archive, synced from `main` at the start of every run.
- **`master_index.md`** — fingerprint registry used for duplicate detection, plus each strategy's capped score and last-updated date.
- **`queue.md`** — the candidate backlog and per-strategy state (Pending / Researching / Completed / Rejected), so runs never lose the backlog or duplicate work.
- **`source_archive/`** — per-run record of every URL consulted with its retrieval timestamp.
- **Scheduled tasks** — fire the headless run automatically (Claude Code Desktop scheduler, or Linux cron via a wrapper script).
- **Recovery mechanisms** — the run lock, stash/recovery-branch handling at start-of-run, per-strategy push checkpoints, and the safe-reset policy together make interruption safe.

---

## Repository Structure

```
research/
├── daily/                      # YYYY-MM-DD.md, one report per run
├── strategies/                 # <slug>.md, one file per strategy
├── rankings/                   # strategies ranked by capped confidence
├── concepts/                   # recurring concepts/patterns
├── scams/                      # scam / rejected-strategy archive
├── implementation_candidates/  # strategies with codeable, inspectable logic
├── source_archive/             # one .md per run: raw URLs + retrieval timestamp
├── queue.md                    # candidate queue (Pending/Researching/Completed/Rejected)
├── .run.lock                   # concurrency lock; local-only, gitignored, present only during a run
└── master_index.md             # fingerprints + capped scores + last-updated dates
```

| Path | Purpose |
|---|---|
| `daily/YYYY-MM-DD.md` | Per-run report: executive summary, strategies investigated, emerging patterns, red flags, implementation candidates. |
| `strategies/<slug>.md` | Full per-strategy write-up (logic, rationale, costs, evidence, metrics table, skeptic steelman, confidence breakdown). |
| `rankings/` | Strategies sorted by the **capped** confidence score. |
| `concepts/` | Cross-strategy concepts and recurring mechanisms. |
| `scams/` | Strategies flagged as scams or rejected. |
| `implementation_candidates/` | Strategies with `latent_score ≥ 60` **and** inspectable/codeable logic. |
| `source_archive/YYYY-MM-DD.md` | URL · retrieval timestamp · which strategy file it was used for. |
| `queue.md` | Candidate strategy backlog and state tracking. |
| `master_index.md` | Fingerprints for duplicate detection + capped scores + dates. |
| `.run.lock` | Local-only concurrency lock; gitignored; never committed. |

---

## Setup / Quick Start

> Assumes the repo is initialized with **at least one commit** (auth verification and `git push` both require one). Replace `<org>/<repo>` with your repository.

### 1. Authentication

Headless `git push` requires credentials. Choose one method.

**Option A — SSH deploy key (recommended for scheduled/CI runs):**

```bash
# Generate key pair
ssh-keygen -t ed25519 -C "claude-research-agent" -f ~/.ssh/claude_research_agent

# Add the public key as a deploy key with Write access:
# GitHub repo → Settings → Deploy keys → Add deploy key
cat ~/.ssh/claude_research_agent.pub

# Register the custom key name so git uses it automatically
cat >> ~/.ssh/config << 'EOF'
Host github.com
  IdentityFile ~/.ssh/claude_research_agent
  IdentitiesOnly yes
EOF
chmod 600 ~/.ssh/config

git remote set-url origin git@github.com:<org>/<repo>.git
ssh -T git@github.com   # verify
```

**Option B — Personal Access Token (PAT):**

```bash
# Fine-grained PAT with Contents: read+write
git remote set-url origin https://<PAT>@github.com/<org>/<repo>.git
git ls-remote origin   # verify
```

Never store credentials in a file tracked by git. Confirm auth before the first run:

```bash
git push --dry-run origin HEAD   # expect "Everything up-to-date" or a push plan, not an auth error
```

### 2. Git Identity

```bash
git config user.name "Claude Research Agent"
git config user.email "claude-agent@your-org.com"
```

### 3. `.gitignore`

Ensure at minimum the OS/editor noise, credentials (`.env`, `*.pem`, `*.key`), Claude Code working files (`.claude/`, `*.tmp`, `*.log`), and the local-only lock (`research/.run.lock`) are ignored.

### 4. Scheduling

**Claude Code Desktop (macOS/Windows):** use the Scheduled Tasks sidebar to create a recurring job with the prompt:

```
Run today's research pass
```

> When running unattended, configure auto-approve in Claude Code settings or pass `--dangerously-skip-permissions` — only in a controlled environment you trust.

**Linux (cron):** cron runs with a bare environment, so use a wrapper script that loads `PATH` and `ANTHROPIC_API_KEY`, then schedule the script (not an inline command, which would not propagate the env var past `&&`):

```bash
#!/bin/bash
# /usr/local/bin/run-research.sh  (chmod +x)
for f in ~/.bash_profile ~/.profile; do [ -f "$f" ] && source "$f" && break; done
cd /path/to/repo
claude -p "Run today's research pass" --max-turns 50 2>&1 | tee /var/log/research-agent.log
```

```
# /etc/cron.d/research-agent
0 7 * * * youruser /usr/local/bin/run-research.sh
```

> **`--max-turns` sizing:** one strategy end-to-end (search, fetch, score, update files, commit, push, verify) consumes many tool calls. Raise the limit if runs regularly stop mid-strategy — truncating mid-strategy is the worst outcome.

---

## How a Research Run Works

Every run executes the same ordered procedure, built around the per-strategy loop:

**`Research → Validate → Save → Commit → Push → Continue`**

1. **Acquire the run lock.** If `research/.run.lock` exists and is fresh, stop. A lock older than 6 hours is treated as stale, logged, overridden, and overwritten.
2. **Synchronize from GitHub.** Check repo state; stash a dirty working tree (never `git reset --hard`, which destroys unfinished research); fetch, checkout `main`, `pull --rebase`; restore the stash. Begin research only after sync succeeds.
3. **Determine the date** from the environment (`date +%Y-%m-%d`) for filenames.
4. **Load state.** Read `master_index.md`, `queue.md`, recent daily reports, and existing strategy/ranking/concept files; detect fingerprints, recent work, and duplicates. Create the directory structure on first run if it is missing.
5. **Select one strategy** from `queue.md`, moving it Pending → Researching.
6. **Research it** within the per-strategy budget (max **10 source documents OR 30 minutes**).
7. **Validate evidence** — assign the Evidence auditability tier honestly; apply the falsifiability test and the transaction-cost/capacity check.
8. **Steelman the skeptic** — write the *"Why This Might Be Nothing"* section *before* scoring.
9. **Score** — compute `latent_score`, apply the verification cap, record both.
10. **Duplicate detection** against `master_index.md` fingerprints.
11. **Persist** — update the strategy file, master index, rankings, concepts, implementation candidates (if eligible), queue, source archive, and daily report.
12. **Validate consistency**, then **commit, push, and land on `main`**, and **verify** it reached `main` before continuing.

A run researches strategies continuously until context, token budget, or time budget is exhausted, or a repository error occurs — always finishing and persisting the current strategy first. Finding nothing worth recording is an acceptable outcome; padding is not.

---

## Confidence Scoring System

Each strategy is scored on eight weighted dimensions (0–10 each, weights summing to **15**):

| Dimension | Weight |
|---|---|
| Logical / economic soundness (falsifiable) | 3 |
| Transparency & reproducibility | 2 |
| Evidence auditability *(not result truth)* | 2 |
| Risk management quality | 2 |
| Cost & capacity realism | 2 |
| Honest treatment of drawdowns / failure | 1.5 |
| Robustness evidence (OOS / walk-forward / multi-market) | 1.5 |
| Survived independent scrutiny | 1 |

`max_possible = 15 × 10 = 150` → `latent_score = round(weighted_sum / 150 × 100)`.

### Verification cap

The cap enforces *"never overstate"* and is derived from the **Evidence auditability** dimension:

| Evidence auditability | Result | Tops out at |
|---|---|---|
| ≤ 4 (performance is `CLAIMED, UNVERIFIED` — the common case) | `confidence = min(latent_score, 59)` | Experimental |
| 5–7 (reproducible methodology, not independently confirmed) | `confidence = min(latent_score, 74)` | Worth researching |
| ≥ 8 (peer-reviewed methodology, or open code read line-by-line) | no cap; `confidence = latent_score` | High potential / Exceptional |

### Bands (applied to the capped `confidence`)

| Band | Range |
|---|---|
| Exceptional | 90–100 |
| High potential | 75–89 |
| Worth researching | 60–74 |
| Experimental | 40–59 |
| Low confidence | 0–39 |

### Which number is canonical

- **`confidence`** (the capped number) is the **only** score that appears in `master_index.md`, `rankings/`, and the daily report. Rankings are always by the capped score.
- **`latent_score`** lives **only** inside the strategy file's reasoning, recorded as `latent N (capped to M pending verification: <reason>)`. It preserves the quality ordering among unverified ideas — it is never surfaced as a headline and never used to rank. If verification later arrives, the cap lifts and `confidence` rises on its own.

A score above ~75 is structurally rare: it requires auditable evidence to clear the cap at all.

---

## Research Standards

- **No fabrication.** No invented statistics, sources, URLs, or quotes. A value not stated in a real page actually fetched is `NOT REPORTED`. Win rate, Sharpe, drawdown, and other metrics are never inferred, calculated, or derived into the metrics table.
- **Live results cannot be verified.** Equity curves and broker statements cannot be confirmed and doctored screenshots cannot be detected; all performance numbers are labeled `CLAIMED, UNVERIFIED` unless from an independently auditable source.
- **Separate FACTS / ANALYSIS / ASSUMPTIONS** in every write-up; labeled analytical derivations are allowed outside the metrics table, but never inside it.
- **Evidence tiers.** Each metric is tagged exactly once: `NOT REPORTED`, `CLAIMED, UNVERIFIED`, or `AUDITABLE`. The `AUDITABLE` tier is reserved for peer-reviewed reproducible methodology or open code read line-by-line **and** an Evidence-auditability dimension ≥ 8. A thorough-looking write-up does not promote a claim.
- **Falsifiability gate.** A real economic rationale states the conditions under which the edge should disappear. If you cannot state when the mechanism should fail, the rationale scores 0–2 (pattern-only).
- **Cost & capacity realism.** Spread, slippage, commission, and swap/financing must be checked; a zero-cost backtest on a high-turnover or short-timeframe strategy is presumptively invalid. Reported metrics are sanity-checked against asset-class benchmarks, with automatic scrutiny triggers (e.g. Sharpe > 3, Win Rate > 80%, Max Drawdown < 5%) requiring explicit discussion.
- **Scam / red-flag detection.** Strategies are tagged `LOW CONFIDENCE` / `POSSIBLE SCAM` / `INSUFFICIENT VERIFICATION` for martingale or uncontrolled grids, "no-loss"/"100% win" claims, binary options, screenshot-only proof, paywalled-only evidence, no risk management, extreme-leverage returns, signal-selling funnels, curve-fitting, omitted costs on high-turnover strategies, or unfalsifiable rationales.
- **Trusted sources, in rough order:** research papers (SSRN, arXiv q-fin) → open-source code with history → transparent practitioner write-ups → forum *criticism* threads → TradingView public scripts → YouTube (only for concrete testable logic). Discord, Telegram, private/paid groups, and paywalled content cannot be accessed and are never claimed as sourced.

---

## GitHub Persistence & Recovery Model

GitHub is the canonical long-term memory; `main` is the single source of truth and the permanent record.

- **Auto-merge to `main`, no human review gate.** Two environments are supported and detected at start-of-run:
  - *Direct-push* — commit and push completed research directly to `main`.
  - *Branch + PR* — push each completed strategy to the assigned working branch, then automatically merge it into `main` via the GitHub merge tooling (open a PR if none exists).
  - Either way the invariant holds: **a completed, validated strategy is on `main` before the next strategy begins.** Daily research branches and extra feature branches are not created.
- **Per-strategy commits.** Staging is scoped to `research/` only. `nothing to commit` is not a failure — it simply means there was nothing new to persist.
- **Push + merge verification (mandatory).** After pushing, confirm the commit is on the remote, then confirm the merge reached `main` by checking the strategy's files are present on `main` (a squash/merge commit gives `main` a different SHA, so verify by content, not SHA equality). If verification fails: stop, report a synchronization/merge failure, do not continue.
- **Run-lock concurrency protection.** `research/.run.lock` records a UTC start time, is checked before any work, is removed on every exit path, and is gitignored — local-only, never committed or pushed. Locks older than 6 hours are treated as stale and overridden.
- **Stash recovery.** A dirty working tree at start-of-run is stashed (`git stash push -u`), never hard-reset.
- **Recovery branches.** Valuable unfinished work can be promoted to a `recovery-YYYY-MM-DD-HHMM` branch via `git stash branch`, committed, and pushed. These are the only branches created outside the normal flow.
- **Checkpointing.** Each verified push is a permanent recovery checkpoint; the next run resumes purely from GitHub state (reads `master_index.md`, sees what completed, continues). Interruption mid-strategy loses only the partial work — all completed strategies are already safe on `main`.
- **Safe reset policy.** `git reset --hard origin/main` is used only when explicitly configured, remote integrity is guaranteed, and unfinished work has already been preserved. Meaningful prior analysis is never overwritten without preserving historical context.

---

## Outputs Generated

Each run produces and updates:

| Output | Location | Contents |
|---|---|---|
| **Daily report** | `research/daily/YYYY-MM-DD.md` | Executive summary, strategies investigated (ranked by confidence), emerging patterns, red flags, implementation candidates. |
| **Per-strategy file** | `research/strategies/<slug>.md` | Overview, core logic, economic rationale, entry/exit, risk management, transaction costs & capacity, backtest/forward evidence, performance-metrics table, community sentiment, *Why This Might Be Nothing*, confidence breakdown, feasibility, similar strategies, red flags, source links, analyst notes. |
| **Master index** | `research/master_index.md` | Fingerprint, capped score, and last-updated date for every strategy. |
| **Rankings** | `research/rankings/` | Strategies sorted by capped confidence. |
| **Implementation candidates** | `research/implementation_candidates/` | Strategies with `latent_score ≥ 60` and inspectable/codeable logic (performance still unverified). |
| **Source archive** | `research/source_archive/YYYY-MM-DD.md` | URLs with retrieval timestamps and the strategy each was used for. |
| **Queue** | `research/queue.md` | Candidate state tracking across runs. |

---

## Safety & Reliability Guarantees

- **One strategy at a time, atomic.** No batching, no parallel research, no delayed persistence; each strategy is a complete transaction.
- **Steelman before scoring.** The skeptic's case is mandatory and written before any score.
- **Repository consistency validation** before every commit: valid links and cross-references, rankings matching capped scores, index entries matching strategy files, no malformed or accidentally deleted files.
- **Recovery checkpoints.** Every verified push is a permanent checkpoint; resume is always from GitHub state.
- **Concurrency protection** via the run lock, with stale-lock recovery.
- **Failure handling.** On sync, push, or merge failure the run stops immediately and reports rather than continuing on stale state. Repository integrity and recoverability outrank research speed and quantity.

---

## Disclaimer

This project is for **research and education only — it is not investment advice.** Strategies recorded here are hypotheses to test, never instructions to deploy capital. The agent **cannot verify live results**: equity curves, broker statements, and screenshots cannot be confirmed or detected as doctored, so all performance figures are **claims unless independently verified** and are labeled accordingly. Every strategy requires independent testing and validation before any real-world use.
