# Self-Improving Repository

A GitHub repo that reviews its own PRs and can propose improvements to itself.

## How It Works

```
┌─────────────────┐     ┌─────────────────┐
│  Human or Bot   │────▶│   Opens PR      │
│  opens PR       │     │                 │
└─────────────────┘     └────────┬────────┘
                                 │
                                 ▼
                        ┌─────────────────┐
                        │ review-pr.yml   │
                        │ Claude reviews  │
                        │ the diff        │
                        └────────┬────────┘
                                 │
                    ┌────────────┼────────────┐
                    ▼            ▼            ▼
              ┌──────────┐ ┌──────────┐ ┌──────────┐
              │ APPROVE  │ │ COMMENT  │ │ REQUEST  │
              │          │ │          │ │ CHANGES  │
              └────┬─────┘ └──────────┘ └──────────┘
                   │
                   ▼
          ┌─────────────────┐
          │ Has auto-merge  │──No──▶ Wait for human
          │ label?          │
          └────────┬────────┘
                   │Yes
                   ▼
          ┌─────────────────┐
          │ Auto-merge!     │
          └─────────────────┘
```

## The Ouroboros Loop

```
┌─────────────────────────────────────────┐
│           self-improve.yml              │
│  (runs weekly or on-demand)             │
│                                         │
│  1. Analyze codebase                    │
│  2. Claude proposes improvement         │
│  3. Opens PR with 'auto-merge' label    │
└──────────────────┬──────────────────────┘
                   │
                   ▼
          ┌─────────────────┐
          │ review-pr.yml   │◀───────┐
          │ reviews the     │        │
          │ improvement     │        │
          └────────┬────────┘        │
                   │                 │
                   ▼                 │
          ┌─────────────────┐        │
          │ If approved:    │        │
          │ merges itself   │────────┘
          │ into the repo   │  (repo evolves)
          └─────────────────┘
```

## Setup

1. Add `ANTHROPIC_API_KEY` to repository secrets
2. Copy `.github/workflows/` to your repo
3. Optionally customize `CLAUDE_RULES.md`

## Safety Rails

- `CLAUDE_RULES.md` includes meta-protection against modifying review rules
- Auto-merge only happens with explicit label
- Humans can always override with `/lgtm` comment
- All changes are tracked in git history

## Usage

**Manual improvement:**
```bash
gh workflow run self-improve.yml -f focus="improve error handling"
```

**Watch the chaos:**
- Enable weekly schedule
- Watch your repo slowly evolve
- Hope it doesn't become Skynet

