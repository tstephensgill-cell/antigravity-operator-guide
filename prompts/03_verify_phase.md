# Prompt: VERIFY / CONSOLIDATE ONLY

**Purpose**: Used to force Anti to dump exactly what it did without summarizing or mutating further. Useful for auditing raw JSON payloads, git statuses, or specific file diffs before moving to a Red-Gate approval.

**Usage**: Paste this into the terminal to audit Anti's recent work.

```text
VERIFY / CONSOLIDATE ONLY:

Do not invoke subagents.
Do not edit files.
Do not commit.
Do not push.

Limits:
- Proof Contract: claims are not proof. Present raw verifier evidence.
- Verify against the Execution Surface Lock.
- Confirm no unapproved tracked files changed.
- Confirm no unapproved untracked files were created inside the repo.
- Confirm any known Antigravity artifact side-effects, such as implementation_plan.md, are only artifacts if they do not appear in repo proof.
- For frontend app-shell/runtime work, node -c is not enough; Anti-run localhost smoke proof is required before marking safe to push where Anti has local runtime access (human manual smoke only if explicitly chosen). Data-mutating smoke requires explicit approval unless dev/intercept-safe. Push without smoke requires explicit accepted no-smoke risk.

Produce the exact raw returned payloads / git status output / test logs for the prior execution step.
No summaries, ellipses, or paraphrases.

Run:
python C:\Users\OEM\.gemini\config\skills\kernel\safe_exec.py --stage VERIFY --command "git status --short --untracked-files=all"

Stop after TASK RESULT.
```
