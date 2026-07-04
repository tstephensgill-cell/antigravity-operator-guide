# Prompt: PLAN ONLY

**Purpose**: Used to initiate a new task, define a work package, or research a problem without granting any execution or mutation authority.

**Usage**: Paste this prompt into the human's terminal/interface for Anti.

```text
PLAN ONLY:

<Insert Goal or Task Description Here>

Limits:
- Do not execute modifying commands.
- Do not write source code files.
- You may use research/read tools to inspect the codebase.
- Provide a clear implementation plan, strategy, or Work Package proposal.
- An architecture plan is NOT execution approval. Produce a real Gate Map.
- A roadmap item is NOT a bounded Work Package.
- Gate Map is NOT a roadmap.
- Real Gate Map fields must include: GateCategory, AllowedFiles, ForbiddenFiles, ForbiddenActions, StopConditions, VerificationCommands, RedGateRequired, HumanApprovalRequired, AutoContinueAllowed.
- Ensure risk domains are NOT bundled into a single Work Package unless explicitly approved as an emergency batch.
- Determine if frontend runtime/browser smoke proof is required.
- Identify exact file touch points to establish Execution Surface Lock.
- If exact file touch points, insertion points, stop conditions, and proof requirements are missing, remain in PLAN ONLY.

Stop after outputting the plan and await further instructions.
```
