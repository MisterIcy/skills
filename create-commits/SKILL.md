---
name: create-commits
description: Creates git commits following the Conventional Commits specification. Performs git status to identify changes, reviews diffs to understand scope, collaborates with humans on staging files, and generates valid commit messages optimized for both agents and humans. Handles edge cases including reverts, amends, merge commits, and rebase commits. Use when committing changes, creating git commits, staging files, or when the user asks to commit code changes.
license: MIT
compatibility: Requires git
---

# Create Commits

## When to Use

Use this skill when:
- The user asks to commit changes, create a commit, or stage files
- You need to commit code changes after making modifications
- The user wants to create a commit message following Conventional Commits
- You need to handle special commit scenarios (revert, amend, merge, rebase)

## Core Workflow

Follow this workflow for every commit:

1. **Check repository status**
   - Run `git status` to see all changes (staged, unstaged, untracked)
   - Identify which files were modified, added, deleted, or renamed

2. **Review changes to understand scope**
   - Read diffs for modified files using `git diff` (unstaged) or `git diff --cached` (staged)
   - Read new files that were added
   - Understand the purpose and impact of changes
   - Identify the scope (e.g., `auth`, `api`, `ui`, `docs`)

3. **Collaborate on staging**
   - Present the list of changed files to the user
   - Ask which files should be included in the commit
   - Stage files using `git add <file>` or `git add -p` for partial staging
   - Discard unwanted changes only with explicit user approval using `git restore <file>` or `git checkout -- <file>`

4. **Determine commit type and scope**
   - Analyze changes to identify the appropriate type (feat, fix, docs, etc.)
   - Determine scope from the files changed (e.g., `feat(auth)`, `fix(api)`)
   - Check for breaking changes (API changes, config changes, etc.)

5. **Create commit message**
   - Write a clear, concise description following Conventional Commits format
   - Include body if additional context is needed
   - Add breaking change footer if applicable

6. **Execute commit**
   - Run `git commit -m "<message>"` or `git commit` for multi-line messages
   - Verify the commit was created successfully

## Commit Message Format

Follow the [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) specification:

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

### Types

- **feat**: New feature (correlates with MINOR in SemVer)
- **fix**: Bug fix (correlates with PATCH in SemVer)
- **docs**: Documentation changes
- **style**: Code style changes (formatting, missing semicolons, etc.)
- **refactor**: Code refactoring without changing functionality
- **perf**: Performance improvements
- **test**: Adding or updating tests
- **build**: Build system or dependency changes
- **ci**: CI/CD configuration changes
- **chore**: Other changes that don't modify src or test files
- **revert**: Reverting a previous commit

### Scope

Scope is optional and should be a noun describing the codebase section:
- `feat(auth)`: Authentication feature
- `fix(api)`: API bug fix
- `docs(readme)`: README documentation
- `refactor(utils)`: Utility functions refactoring

### Description

- Use imperative mood: "add feature" not "added feature" or "adds feature"
- Lowercase (except for proper nouns)
- No period at the end
- Be specific and concise
- Focus on what changed, not why (why goes in body if needed)

### Body (Optional)

- Provide additional context about the change
- Explain the "why" behind the change
- Reference related issues: `Closes #123` or `Fixes #456`
- Separate from description by blank line

### Breaking Changes

Indicate breaking changes in two ways:

1. **In type/scope**: Append `!` before colon
   ```
   feat(api)!: change authentication endpoint
   ```

2. **In footer**: Add `BREAKING CHANGE:` footer
   ```
   feat(api): change authentication endpoint

   BREAKING CHANGE: The /auth endpoint now requires API key in header instead of query parameter
   ```

## Examples

### Simple feature commit
```
feat(auth): add JWT token refresh endpoint

Adds POST /auth/refresh endpoint that accepts refresh token and returns new access token.
Implements token rotation for enhanced security.
```

### Bug fix with scope
```
fix(api): prevent null pointer in user validation

When user object is null, the validation middleware now returns 400 instead of crashing.
```

### Documentation update
```
docs(readme): update installation instructions

Adds Node.js 18+ requirement and clarifies environment variable setup.
```

### Breaking change
```
feat(api)!: migrate to v2 authentication

BREAKING CHANGE: Authentication now uses Bearer tokens instead of API keys.
Update your clients to use Authorization header.
```

### Revert commit
```
revert: feat(api): add rate limiting

This reverts commit abc123def456.

Rate limiting caused issues with batch operations.
Refs: #789
```

## Edge Cases

### Reverting Commits

When reverting:
1. Use type `revert`
2. Reference the commit SHA(s) being reverted in the body
3. Explain why the revert is necessary
4. Use `Refs:` footer to link related issues

```
revert: feat(api): add caching layer

This reverts commit abc123def456.

Caching caused stale data issues in production.
Refs: #456
```

### Amending Commits

When amending (`git commit --amend`):
- Keep the same type and scope if changes are minor (typos, formatting)
- Change type/scope if the amendment significantly alters the commit's purpose
- Add `(amend)` note in body if helpful for context

```
fix(api): correct typo in error message

(amend) Fixed typo: "eror" -> "error"
```

### Merge Commits

For merge commits created by `git merge`:
- Git automatically creates merge commit messages
- Don't modify unless user explicitly requests it
- If creating manual merge commit, use format:

```
merge: integrate feature-branch into main

Merges feature-branch that adds user authentication.
Resolves conflicts in auth middleware.
```

### Rebase Commits

During interactive rebase (`git rebase -i`):
- Each commit can be reworded individually
- Follow Conventional Commits format for each commit message
- Maintain logical grouping of changes

### Empty Commits

If no changes to commit:
- Inform the user that there are no changes
- Don't create empty commits unless explicitly requested
- If requested, use `git commit --allow-empty` with appropriate message

### Multiple Related Changes

If changes span multiple concerns:
- **Preferred**: Create separate commits for each concern
- **Alternative**: Use a single commit with multiple scopes if changes are tightly coupled
- Ask the user if unsure about splitting commits

Example of splitting:
```
feat(auth): add login endpoint
feat(auth): add logout endpoint
docs(auth): update API documentation
```

## Best Practices

1. **Always check status first** - Never assume what files changed
2. **Read diffs before committing** - Understand the full scope of changes
3. **Collaborate with humans** - Ask before staging, discarding, or committing
4. **One logical change per commit** - Keep commits focused and atomic
5. **Write clear descriptions** - Future you (and other agents) will thank you
6. **Use scopes consistently** - Establish and follow project conventions
7. **Reference issues** - Link commits to issues/tickets when applicable
8. **Be specific** - "fix bug" is less helpful than "fix null pointer in user validation"

## Collaboration Guidelines

- **Never commit without user approval** - Always confirm before executing `git commit`
- **Present options clearly** - Show what will be committed and ask for confirmation
- **Explain your reasoning** - Share why you chose a particular type/scope
- **Handle conflicts gracefully** - If staging fails, explain why and suggest solutions
- **Respect user preferences** - If user has commit message preferences, follow them
