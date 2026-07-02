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

Stop after outputting the draft packet.
```
