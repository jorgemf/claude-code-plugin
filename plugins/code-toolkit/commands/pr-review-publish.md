---
description: "Publish PR review findings to GitHub with inline code suggestions"
argument-hint: "[--dry-run]"
allowed-tools: ["Bash", "Glob", "Grep", "Read", "Task"]
---

# Publish PR Review to GitHub

Analyze the current PR and publish review comments with actionable code suggestions that can be accepted with one click.

**Arguments:** "$ARGUMENTS"

## Prerequisites

- A PR must exist for the current branch
- The `gh` CLI must be authenticated

## Workflow

### 1. Verify PR exists

```bash
gh pr view --json number,state,headRefName
```

If no PR exists or it's closed, inform the user and stop.

### 2. Get PR diff and metadata

```bash
gh pr diff
gh pr view --json number,headRefOid
```

### 3. Analyze changes

For each modified file, identify issues that warrant inline comments:

**Focus on:**
- Bugs that have clear fixes
- CLAUDE.md violations with straightforward corrections
- Error handling improvements
- Type safety issues

**Skip:**
- Style preferences without clear fixes
- Issues that require architectural changes
- Pre-existing problems not introduced by this PR

### 4. Generate suggestion blocks

For each issue, create a GitHub suggestion block:

````markdown
```suggestion
corrected code here
```
````

**Requirements for suggestions:**
- Must be syntactically valid
- Must replace complete lines (GitHub requirement)
- Must preserve surrounding context
- Should be minimal - only change what's necessary

### 5. Publish comments

Use the GitHub API to post inline review comments:

```bash
gh api repos/{owner}/{repo}/pulls/{pr_number}/comments \
  -f body="Issue description

\`\`\`suggestion
fixed code
\`\`\`" \
  -f commit_id="{head_sha}" \
  -f path="{file_path}" \
  -f line={line_number}
```

Or for multi-line suggestions:
```bash
gh api repos/{owner}/{repo}/pulls/{pr_number}/comments \
  -f body="..." \
  -f commit_id="{head_sha}" \
  -f path="{file_path}" \
  -f start_line={start} \
  -f line={end} \
  -f start_side="RIGHT" \
  -f side="RIGHT"
```

### 6. Post summary comment

After inline comments, post a summary:

```bash
gh pr comment --body "## PR Review

Posted X inline comments with suggestions.

**Issues found:**
- [Brief list]

Click 'Apply suggestion' on each comment to accept the fix."
```

## Dry Run Mode

If `--dry-run` is passed in arguments:
- Show what comments would be posted
- Do NOT actually post to GitHub
- Useful for reviewing before publishing

## Output Format

For each issue found:

```
### {file}:{line}

**Issue:** {description}

**Suggestion:**
```{language}
{corrected code}
```

**Status:** {Posted | Would post (dry-run)}
```

## Tips

- Run `/code-toolkit:pr-review` first to get a comprehensive local review
- Use `--dry-run` to preview comments before posting
- Suggestions work best for localized fixes, not architectural changes
- The PR author can accept suggestions with one click in GitHub UI

## Notes

- Only posts comments for issues with clear, actionable fixes
- Skips issues that require manual judgment or broader changes
- Each suggestion must replace complete lines (GitHub limitation)
- Comments are posted as review comments, not regular comments
