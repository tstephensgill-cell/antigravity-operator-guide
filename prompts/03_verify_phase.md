# Prompt: VERIFY / CONSOLIDATE ONLY

**Purpose**: Used to force Anti to dump exactly what it did without summarizing or mutating further. Useful for auditing raw JSON payloads, git statuses, or specific file diffs before moving to a Red-Gate approval.

**Usage**: Paste this into the terminal to audit Anti's recent work.

```text
VERIFY / CONSOLIDATE ONLY:

Do not invoke subagents.
Do not edit files.
Do not commit.
Do not push.

Produce the exact raw returned payloads / git status output / test logs for the prior execution step.
No summaries, ellipses, or paraphrases.

Run:
python C:\Users\OEM\.gemini\config\skills\kernel\safe_exec.py --stage VERIFY --command "git status --short --untracked-files=all"

Stop after TASK RESULT.
```
