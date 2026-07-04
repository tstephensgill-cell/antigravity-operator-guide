# Prompt: RED-GATE PACKET DRAFT

**Purpose**: Used to request Anti to format a high-risk action (like a Deploy, Governance Edit, or Tool Enablement) into a structured draft for human review. It does NOT approve the action.

**Usage**: Use this when a task is ready to cross a high-risk boundary.

```text
RED-GATE DRAFT ONLY:

Draft a Red-Gate Approval Packet for <Insert Action: e.g., Deploy to Staging>.

Limits:
- Use the RedGateApprovalPacketDrafter worker.
- Format the exact verification commands and file bounds.
- Do NOT execute the action.
- Do NOT approve the action.
- Output the drafted JSON packet for human review.
- The draft packet must include real Gate Map fields: GateCategory, AllowedFiles, ForbiddenFiles, ForbiddenActions, StopConditions, VerificationCommands, RedGateRequired, HumanApprovalRequired, AutoContinueAllowed.
- The draft packet must define the Execution Surface Lock.
- The draft packet must identify whether the work touches any risk domains that must be split.
- The draft packet must identify frontend runtime/browser smoke proof requirements where relevant.
- The draft packet must not imply that architecture plans, roadmap items, or product ideas are execution approval.

Stop after outputting the draft packet.
```
