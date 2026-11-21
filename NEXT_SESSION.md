# NEXT_SESSION.md

_Last checkpoint: 2026-11-20 20:03 on MacBook (Antigravity)_

> ⚠️ **This file is OVERWRITTEN at every `/checkpoint`.** It is a **pointer, not a store.** Durable architecture and testing instructions live in `CLAUDE.md`.

## TL;DR

Preserved and verified the statusline duration/cost rendering fix (`statusline.sh` + `test_statusline.sh` Tests 20–22), dropped the temporary Gemini Mirror pilot scaffold (`.agents/` and the temporary `AGENTS.md` pilot section), verified the full suite offline inside a Seatbelt sandbox (**95 passed, 0 failed**; proved red **66 passed, 28 failed** against prior `ee713ae`), and merged cleanly into `main`.

EGM: design n/a — bounded bug fix for session duration/cost display with no design choice · built n/a — same bounded bug fix, verified with prove-red + 95/95 offline sandboxed tests and `scope-gate.sh` (`lane_count: 0`)

## First actions on the other machine

### macOS (MacBook / Mac mini)
```zsh
cd "$HOME/Code Projects/infrastructure/claude-code-statusline"
git pull --ff-only origin main
```

### Windows (FalconXtreme)
```powershell
cd "E:\Code Projects\infrastructure\claude-code-statusline"
git pull --ff-only origin main
```

## Recreate local-only files

- None in this repository. Runtime credentials (`~/.claude/.credentials.json` or macOS Keychain) and cache (`~/.claude/usage-exact.json`) live outside the repo and are managed by Claude Code. Run `bash install.sh` only when intentionally updating `~/.claude/hooks/statusline.sh`.

## What's done (this session)

```sh
git log --oneline ee713ae..HEAD
```

- Show elapsed session duration in `statusline.sh` even when `total_cost_usd` is missing, `null`, or numerically zero (`0`, `0.0`, `-0`, `0e0`, etc.), render `<1m` for sub-second positive durations, and preserve cost-only display when duration is missing or zero.
- Added integration tests 20–22 in `test_statusline.sh` covering duration-without-cost, sub-second positive durations, and cost-without-duration.
- Removed the temporary Gemini Mirror pilot files (`.agents/` and the pilot paragraph in `AGENTS.md`) so the repository stays clean.
- **Gates measured this session:**
  - Offline Seatbelt sandbox proof (`curl` exit `6`, `/usr/bin/security` exit `126`, `ls ~/.claude` exit `1`, clean `HOME`): passed.
  - Prove-red against `ee713ae:statusline.sh` in offline Seatbelt sandbox: **66 passed, 28 failed**.
  - Full suite (`GATE_STAMP_FORCE=1 /bin/bash test_statusline.sh` in offline Seatbelt sandbox with clean `HOME`): exit `0`, **95 passed, 0 failed**.
  - `bash ~/.claude/skills/review-all/scope-gate.sh --base origin/main` → `lane_count: 0`.
  - Secret diff audit → 0 matches.
  - Note: did not run `bash install.sh` (live `~/.claude/hooks/statusline.sh` installation left untouched).

## What's pending (in priority order)

1. Optional: run `bash install.sh` on each host if you want the updated `statusline.sh` deployed into `~/.claude/hooks/statusline.sh`.

## Prompt to paste into next session

```
I'm resuming work on infrastructure/claude-code-statusline. Read NEXT_SESSION.md and CLAUDE.md at the repo root for full context. The duration-without-cost fix is merged on main with 95/95 offline tests passing.
```

## Hard rules for this project

- Run `test_statusline.sh` offline with a clean `HOME` (or Seatbelt sandbox blocking network, `/usr/bin/security`, and real `~/.claude`) so tests never touch live OAuth credentials or hit `api.anthropic.com`.
- Never commit credentials, cache files, or temporary pilot scaffolds.
