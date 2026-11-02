---
name: commit
description: Use this skill ONLY when the user explicitly asks to create a git commit, commit staged changes, or uses phrases like "commit", "haz commit", "commitea", "commit the changes", "make a commit". Generates a concise Conventional Commit message and commits ONLY files currently in the git staging area. Do NOT use this skill for unstaged changes, do NOT auto-commit after edits, and do NOT commit unless the user explicitly requests it.
disable-model-invocation: true
---

# Commit Skill

This skill guides Claude through creating a git commit from staged changes with a concise, Conventional Commit-style message.

## When to use

- ONLY when the user explicitly asks to commit (e.g., "commit", "haz commit", "commitea", "make a commit", "commit the staged changes").
- ONLY when the user says it is okay to commit.
- NEVER auto-commit after a code change unless the user explicitly requested it.

## When NOT to use

- Do NOT use this skill just because files were modified.
- Do NOT commit unstaged files.
- Do NOT stage files automatically (`git add`).
- Do NOT include a "Co-authored-by:" trailer.

## Steps

1. Check which files are staged:
   ```bash
   git diff --cached --name-only
   ```
2. If there are no staged files, tell the user and stop. Do not commit.
3. Inspect the staged diff to understand the changes:
   ```bash
   git diff --cached
   ```
4. Draft a commit message following Conventional Commits:
   - Format: `<type>(<scope>): <short description>`
   - Common types: `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`.
   - Keep the first line under 50 characters.
   - Use a concise, imperative description (e.g., "fix(auth): validate token expiry").
   - Add a body only if the change needs extra context; keep it short.
5. Do NOT include "Co-authored-by:" in the message.
6. Commit only the staged files:
   ```bash
   git commit -m "<type>(<scope>): <description>"
   ```
7. Confirm the commit hash to the user.

## Example

User: "haz commit de los cambios"
Staged files: `Sources/Auth/TokenValidator.swift`
Commit message: `fix(auth): validate token expiry`

## Important reminders

- Only staged files are committed.
- The commit message must be concise and follow Conventional Commits.
- Never add "Co-authored-by:".
- Never commit without the user's explicit request.
