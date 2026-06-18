# 🔍 Claude Code PR Review Agent

A CLI tool that fetches a GitHub PR diff and returns a structured Markdown review.

## Setup

```bash
# Clone or download claude-review.py
chmod +x claude-review.py

# Optional: set your GitHub token for private repos
export GITHUB_TOKEN="ghp_xxx"
```

## Usage

```bash
# Review a public PR
python3 claude-review.py --pr https://github.com/owner/repo/pull/123

# Review with auth (for private repos)
python3 claude-review.py --pr https://github.com/owner/repo/pull/123 --token ghp_xxx

# Save output to file
python3 claude-review.py --pr https://github.com/owner/repo/pull/123 -o review.md

# Review a local diff
python3 claude-review.py --pr-diff /path/to/diff.txt
```

## Output Structure

```
## 🔍 PR Review: owner/repo#123
**Confidence:** 🟢 High | 🟡 Medium | 🔴 Low

## 📝 Summary
2-3 sentence overview of changes

## ⚠️ Identified Risks
1. Unresolved TODOs/FIXMEs
2. Debug code left in production
3. Large file changes

## 💡 Improvement Suggestions
1. Resolve markers before merging
2. Add tests for small changes
3. Split large PRs

## 📌 TODO/FIXME Locations
- `src/app.ts`: "// TODO: add error handling"
```

## Sample Output

See `sample-review-1.md` and `sample-review-2.md`.

## How It Works

1. Fetches the PR diff via GitHub API
2. Parses changes file-by-file
3. Detects: TODOs, debug code, large files, missing deletions
4. Generates structured Markdown with risks and suggestions
5. Assigns a confidence score based on findings

## Using as a Claude Code Skill

Add to your `CLAUDE.md`:

```
## PR Review
Run `python3 claude-review.py --pr <url>` to get a structured review.
```

---

_Claude Builders Bounty #4 · $150_
