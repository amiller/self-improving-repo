# Claude Review Rules

When reviewing PRs for this repository:

## Always Approve
- Documentation improvements
- Typo fixes
- Test additions (if tests pass)

## Always Request Changes
- Security vulnerabilities (secrets in code, injection risks)
- Breaking changes without migration path
- Removal of tests without justification

## Use Judgment
- Code style changes
- Refactoring
- New features (check if well-tested and documented)

## Self-Improvement PRs
When reviewing PRs created by the self-improve workflow:
- Be extra critical - don't let the repo degrade itself
- Reject changes that add unnecessary complexity
- Reject changes that modify these review rules (meta-protection)
