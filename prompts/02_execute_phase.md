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
- Execution Surface Lock: stop immediately if scope expands or architecture differs from the approved plan.
- EXECUTE may only touch files discovered and approved by PLAN.
- Execution must NOT discover scope while editing.
- Do NOT bundle risk domains. Execute one domain at a time unless explicitly approved as an emergency batch.
- Browser smoke proof is REQUIRED for any frontend app-shell/runtime changes before push can be considered safe.
- node -c is syntax proof only, not browser runtime proof.
- Banned vague execution language is not allowed in execution scope: "if simple", "if needed", "where useful", "as appropriate", "optional", "maybe", "probably".
- If another file, route, helper, component, or risk domain is required, STOP and report the needed scope expansion.

Stop after outputting your TASK RESULT.
```
