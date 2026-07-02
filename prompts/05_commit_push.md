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
```
