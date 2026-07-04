# Prompt: COMMIT / PUSH Separation

**Purpose**: Used to finalize tested work. Commit and Push must ALWAYS be handled in two separate, atomic stages.

## 1. Commit Phase
```text
COMMIT/VERIFY ONLY:

Stage exactly these files:
<list explicit files>

Verify with `git diff --cached --name-only`.

Commit with: `git commit -m "<message>"`

Limits:
- NO PUSH.
- NO broad git add.
- Stop after TASK RESULT.
```

## 2. Push Phase (Requires Human Approval)
```text
PUSH/VERIFY ONLY:

Run: `git push origin <branch>`

Limits:
- NO force push.
- NO further edits.
- NO extra commits.
- Stop after TASK RESULT.
- Ensure browser smoke proof has been completed successfully before push for any frontend runtime/app-shell work.
- Verify the Execution Surface Lock remained intact before finalizing commit or push readiness.
- Do not push if verification is only syntax/static proof and the change touched client/src/main.js, navigation, global app startup, auth, global fetch, shared loaders, or route/view registry.
- Do not push if unapproved files changed.
- Do not push if the TASK RESULT relies on claims without raw proof.
```
