# Prompt: EXECUTE/VERIFY ONLY (Green-Gate)

**Purpose**: Used to approve a specific plan and authorize safe, local execution (editing code, running tests). Strictly forbids committing, pushing, or high-risk actions.

**Usage**: Use this prompt after a `PLAN ONLY` response has been reviewed and accepted by the human operator.

```text
EXECUTE/VERIFY ONLY:

Approve the plan. Proceed with execution.

Limits:
- Do NOT commit.
- Do NOT push.
- Do NOT alter governance files unless explicitly requested.
- Confine edits strictly to the approved files.
- Run local tests to verify changes.

Stop after outputting your TASK RESULT.
```
