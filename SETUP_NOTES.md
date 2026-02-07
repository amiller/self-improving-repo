# Self-Improving Repo Setup Notes

## What's Done
- [x] Repo created: https://github.com/amiller/self-improving-repo
- [x] Initial commit pushed with workflows and rules
- [x] Files in place:
  - `.github/workflows/review-pr.yml` - Claude reviews every PR
  - `.github/workflows/self-improve.yml` - Claude proposes improvements weekly
  - `CLAUDE_RULES.md` - Review guidelines with meta-protection
  - `README.md` - Docs with ASCII diagrams

## What's Left

### 1. Add ANTHROPIC_API_KEY secret
```bash
gh secret set ANTHROPIC_API_KEY -R amiller/self-improving-repo
# Paste your Anthropic API key when prompted
```

### 2. Test the review workflow
Open a test PR to trigger `review-pr.yml`:
```bash
git checkout -b test-pr
echo "# Test" > test.md
git add test.md && git commit -m "Test PR for Claude review"
git push origin test-pr
gh pr create --title "Test: Claude should review this" --body "Testing the self-review workflow"
```

### 3. Test the self-improvement workflow
Trigger manually:
```bash
gh workflow run self-improve.yml -f focus="add a simple hello world script"
```

### 4. Optional: Enable weekly schedule
The `self-improve.yml` has a cron trigger (Sunday midnight) but it's disabled by default on new repos. Push any commit to enable scheduled workflows, or manually enable in Actions settings.

## How It Works

```
PR opened → review-pr.yml triggers → Claude reviews diff → posts GitHub review
                                                            ↓
                                              if APPROVE + has 'auto-merge' label
                                                            ↓
                                                      auto-merges

self-improve.yml (weekly/manual) → Claude proposes change → opens PR with 'auto-merge' label
                                                                        ↓
                                                              review-pr.yml reviews it
                                                                        ↓
                                                              if good → merges → repo evolves
```

## Safety Rails
- `CLAUDE_RULES.md` has meta-protection: rejects changes to review rules
- Auto-merge only with explicit label
- Humans can override with comments
- All history in git

## Debugging
- Check Actions tab: https://github.com/amiller/self-improving-repo/actions
- Workflow logs show Claude's raw responses
- If review fails, check if API key is set correctly

## Future Ideas
- `/lgtm` comment parsing to override Claude's decision
- Sigstore attestation of Claude's reviews (prove review happened)
- Multiple Claude "personalities" (strict reviewer vs lenient)
- Rate limiting on self-improvement to prevent runaway evolution
