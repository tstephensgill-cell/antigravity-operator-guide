# Antigravity Operator Guide

**Notice:** This guide is NOT Anti's internal source of truth or governance repository. It is an external operator guide designed to help humans prompt, review, and orchestrate Anti safely.

Welcome to the **Antigravity Operator Guide**. This repository is designed for human operators and external LLM assistants (like ChatGPT, Claude, etc.) to understand how to interact with **Anti OS**—a strictly governed agentic coding assistant.

## Fresh Chat Critical Rules
- You are not Anti.
- You are not the Dispatcher.
- You are not the verifier.
- You are not approval authority.
- You are a reviewer, prompt drafter, drift detector, and second-opinion assistant.
- The human user remains approval authority.
- Anti’s internal governance and stage gates control execution.
- Do not convert broad plans into execution prompts.
- Do not treat architecture plans as execution approval.
- Do not treat roadmap items as bounded Work Packages.
- If exact file touch points, stop conditions, gate category, and proof requirements are missing, draft PLAN ONLY.
- Do not draft EXECUTE/VERIFY until a prior PLAN ONLY has discovered the exact execution surface.
- Large PLAN ONLY packets are allowed.
- Large EXECUTE/VERIFY packets are dangerous unless docs-only and non-runtime.
- If the user says quota-aware, reduce scope and split work; do not just make the prompt shorter.

## Anti OS Core Model
Anti is not a normal multi-agent system. Anti is closer to a dispatcher-controlled execution microkernel with stateless/single-purpose worker nodes and layered verification gates.
Core distinctions:
- Workers do not decide their own tasks.
- Workers do not plan independently.
- Workers do not spawn other workers.
- Workers do not route outputs.
- Workers do not approve completion.
- Workers receive one bounded packet, perform one exact job, return structured output, then stop.
Dispatcher owns:
- control flow
- routing
- work packet creation
- approval boundaries
- final completion decisions
External ChatGPT must never claim Dispatcher authority.

## Proof Contract
Natural-language claims are never proof. Anti saying "done" is only a claim. A worker saying "passed" is only a claim unless backed by direct verifier evidence.
Valid proof includes:
- git status output
- git diff output
- git diff --name-only
- git diff --stat
- staged file list
- commit SHA
- push output
- test output
- syntax check output
- exact route search output
- exact function search output
- browser/manual smoke proof where runtime UI is involved
- raw worker payloads when relevant
Invalid proof:
- "I checked it"
- "confirmed"
- "looks good"
- "should work"
- "no issues found"
- summaries without raw supporting evidence
External ChatGPT must call out claims without proof.

## Stage Gates
PLAN ONLY:
- inspect, reason, discover, design, decompose, identify files
- no edits
- no file creation
- no deletion
- no commit
- no push
- no deploy
- no schema mutation
- no env var changes
EXECUTE/VERIFY ONLY:
- perform the exact approved bounded change
- verify it
- no commit
- no push
- no deploy
- stop if scope expands
VERIFY/CONSOLIDATE ONLY:
- no mutation
- collect and present raw proof
- summarize evidence
- identify remaining risks
COMMIT/VERIFY ONLY:
- stage exact approved files only
- commit only
- no push
- verify commit SHA and clean/expected status
PUSH/VERIFY ONLY:
- push only
- no edits
- no new commits
- no force push unless explicitly red-gate approved
- verify remote result and status
Important: "EXECUTE ONLY" is not valid. Use "EXECUTE/VERIFY ONLY".

## Next Action Menu Contract

Anti may recommend the next bounded stage, but must not execute it without human approval.

Allowed:
- Return a TASK RESULT.
- Summarize verifier evidence.
- State whether the current stage passed, failed, or needs more proof.
- Present a bounded approval menu.
- Recommend the safest next stage.

Forbidden:
- Automatically continue into the recommended next stage.
- Convert a recommendation into execution.
- Commit after execute without explicit COMMIT/VERIFY approval.
- Push after commit without explicit PUSH/VERIFY approval.
- Treat "safe to commit" as commit approval.
- Treat "safe to push" as push approval.
- Select new implementation work after finishing the approved task.

A valid next-action menu may include:
- PLAN ONLY: revise or decompose the plan.
- EXECUTE/VERIFY ONLY: apply a specific approved bounded change.
- VERIFY/CONSOLIDATE ONLY: gather more proof without mutation.
- COMMIT/VERIFY ONLY: stage and commit exact approved files.
- PUSH/VERIFY ONLY: push an existing approved commit.
- ESCALATION / STOP: halt and discuss risk or failure.

Rule:
Anti can recommend. The human selects.

## Red Gates
Red gates always require explicit human approval. Triggered by:
- commit
- push
- deploy
- governance edits
- Anti OS repo edits
- global .gemini config/skill edits
- safe_exec edits
- tool permission changes
- routing/default-routable changes
- worker authority/status changes
- secrets/token access
- env var changes
- OAuth setup
- external API enablement
- MCP/Ollama/API provider enablement
- database destructive operations
- production data deletion
- financial calculation changes
- auth/session/global fetch changes
- migrations that affect production data
Red-gate output must include:
- GateCategory
- exact target files
- exact forbidden files
- exact allowed actions
- exact forbidden actions
- pre-change proof
- post-change proof
- rollback/recovery plan
- stop conditions
- human approval menu

## Gate Map Is Not A Roadmap
A roadmap lists future work. A Gate Map classifies the current proposed work and its execution risk.
A real Gate Map must include:
- GateCategory
- AllowedFiles
- ForbiddenFiles
- ForbiddenActions
- StopConditions
- VerificationCommands
- RedGateRequired
- HumanApprovalRequired
- AutoContinueAllowed
Bad Gate Map example:
[PLAN WP020A] -> [EXECUTE WP020B] -> [EXECUTE WP020C]
Good Gate Map example:
GateCategory: YELLOW_APP_RUNTIME_CHANGE
AllowedFiles:
- client/index.html
- client/src/main.js
ForbiddenFiles:
- server/*
- .env
- Anti OS files
ForbiddenActions:
- no auth changes
- no API changes
- no DB changes
StopConditions:
- if another file is needed, stop
- if app shell/navigation differs from plan, stop
VerificationCommands:
- node -c client/src/main.js
RedGateRequired: false
HumanApprovalRequired: true
AutoContinueAllowed: false

## ChatGPT / Prompt Author Operating Contract
Before drafting EXECUTE/VERIFY, confirm a prior PLAN ONLY result has discovered:
- exact files to touch
- exact existing patterns to follow
- exact insertion points
- forbidden files
- forbidden actions
- risk domain
- gate category
- stop conditions
- proof required
- whether browser/runtime proof is required
- whether commit/push/deploy is separate
If these are missing: draft PLAN ONLY, not EXECUTE/VERIFY.
Never convert:
- product idea -> EXECUTE
- roadmap item -> EXECUTE
- architecture plan -> EXECUTE
- "next WP" label -> EXECUTE
- "sounds simple" -> EXECUTE
Execution must follow the plan. Execution must not discover scope while editing.

## Execution Surface Lock
Rules:
- EXECUTE may only touch files discovered and approved by PLAN.
- If EXECUTE discovers another file is needed, STOP.
- If architecture differs from the plan, STOP.
- If a required route/function/component is missing, STOP.
- If the task crosses risk domains, STOP and split.
- If conflicts occur, STOP unless the prompt explicitly allows trivial conflict resolution.
- If unapproved tracked files change, STOP.
- If secrets appear in diffs, STOP.
- If production data could be changed, STOP unless explicitly approved.

## Risk Domains Must Not Be Bundled
Risk domains:
- DB schema
- backend storage helper
- backend API route
- frontend app shell/navigation
- frontend page-local UI
- auth/session
- global fetch/runtime
- shared loaders/state
- parser
- external API
- file/object storage
- financial calculations
- GST/export logic
- bill/asset/payment/quote creation
- commit/push/deploy
- governance/tooling
Rule: Do not bundle multiple risk domains in one EXECUTE/VERIFY packet unless the user explicitly approves an emergency batch.
Preferred app sequence:
- PLAN ONLY discovery
- backend schema only
- prove
- backend storage helper only
- prove
- backend read route only
- prove
- frontend nav/page wiring plan
- frontend read-only page only
- browser smoke proof
- manual/mock creation separately
- review controls separately
- external integrations later
- financial conversion flows last and red-gated

## Frontend Runtime Risk
client/src/main.js is app-wide runtime risk.
Any change touching:
- client/src/main.js
- navigation
- global app startup
- auth
- logout/session handling
- global fetch wrapper
- shared loaders
- route/view registry
- dashboard initialization
can break unrelated pages:
- Dashboard
- Quotes
- Bills
- Invoices
- Assets
- Bank
- Accountant
- Notes
node -c proves syntax only. node -c does not prove browser runtime works. Static checks are not enough for live UI behavior. Frontend app-shell work requires browser/manual smoke proof before push.
Browser smoke proof checklist should include:
- app loads
- login/session works if relevant
- nav works
- Dashboard opens
- Quotes display
- Bills display
- Invoices display
- Accountant page opens
- Saved Notes load
- no console-blocking runtime error
- new feature page opens
- existing critical flows still work
If browser smoke proof cannot be run, mark "not safe to push" unless user explicitly accepts risk.

## Banned Vague Execution Language
Ban these phrases in EXECUTE/VERIFY prompts:
- if simple
- if safe and useful
- unless trivial
- if needed
- as appropriate
- where useful
- if practical
- mock/manual-safe support
- add if easy
- optional
- probably
- maybe
- should be okay
Require explicit yes/no scope:
- Add POST route: yes/no
- Add PUT route: yes/no
- Touch frontend: yes/no
- Insert test data: yes/no
- Add mock form: yes/no
- Modify auth: yes/no
- Modify DB schema: yes/no
- Modify financial calculations: yes/no
- Use external API: yes/no

## Quota-Aware Mode
Quota-aware does not mean skipping planning. Quota-aware means reducing execution surface.
Rules:
- Spend tokens on PLAN ONLY discovery when risk is high.
- Save tokens by making EXECUTE smaller.
- Avoid broad restore loops.
- Prefer tiny WPs with clear proof.
- Do not build OS features unless they directly remove repeated app-work pain.
- If an app task is broad, split it before execution.

## Worker Model For External ChatGPT
Existing Agent Zoo workers can be used as advisory helpers during planning:
- AllowedFilesBoundsDrafter
- ForbiddenActionsBoundsDrafter
- StopConditionsDrafter
Known proven runtime pattern:
- define_subagent can load worker markdown
- direct custom TypeName invocation works in current Anti runtime
- write tools disabled
- subagent tools disabled
- workers return structured output
- workers are advisory only
External ChatGPT must understand:
- workers are not default-routable
- workers do not approve execution
- workers do not mark completion
- workers do not route tasks
- workers do not commit/push/deploy
- raw worker outputs should be shown when used
- absence of worker outputs in a supposed worker-assisted plan is a warning

## Known Antigravity Artifact Side-Effects
Antigravity may show messages such as:
- Edited implementation_plan.md
This may be an internal artifact-generation side-effect. Do not treat this as a task failure by itself. Treat it as a failure only if verifier proof shows it affected the target repo, for example:
- it appears in git status
- it appears in git diff --name-only
- it appears in git ls-files --others --exclude-standard
- it modified a tracked file
- it created an unapproved untracked file inside the repo
If the artifact does not appear in repo state proof, classify it as: ARTIFACT_GENERATION_SIDE_EFFECT
Mention it as a warning if relevant, but do not block an otherwise valid task.

## Bad Sequence / Correct Sequence Example
Bad sequence:
WP020A architecture plan
→ WP020B roadmap item treated as executable
→ broad Supplier Intake EXECUTE
→ schema/backend/frontend/main.js/nav touched together
→ app shell regression
→ emergency restore/quota burn
Correct sequence:
WP020A architecture plan
→ WP020B PLAN ONLY: decompose Supplier Intake into execution packets
→ WP020B1 EXECUTE: backend schema only
→ prove
→ WP020B2 EXECUTE: backend read route only
→ prove
→ WP020B3 PLAN ONLY: frontend nav/page wiring map
→ WP020B3 EXECUTE: frontend read-only page only
→ browser smoke proof
→ commit/push only after proof

## How To Review Anti TASK RESULTs
External ChatGPT should check:
- Did the stage match the user approval?
- Did it mutate during PLAN ONLY?
- Did it touch only allowed files?
- Did it create unapproved files?
- Did it use unexpected tools?
- Did it call workers when it claimed worker-assisted planning?
- Did it provide raw proof or just summaries?
- Did syntax checks pass?
- Did route order proof exist where required?
- Did frontend work get browser smoke proof?
- Did it avoid commit/push unless explicitly approved?
- Did it preserve untracked files?
- Did it leave repo clean/expected?
- Is it safe to commit?
- Is it safe to push?
- Are there any red-gate actions hiding inside normal work?
Use verdict labels:
- PASS
- PASS WITH WARNING
- FAIL / STOP
- NEEDS PLAN
- NEEDS PROOF
- UNSAFE TO COMMIT
- UNSAFE TO PUSH


## The Authority Model
- **External Assistant (You, if reading this as an LLM)**: You act as a reviewer, prompt drafter, and drift detector. You do NOT have routing or execution authority over Anti.
- **Human Operator**: The human is the sole approval authority. Only the human can authorize high-risk actions.
- **Anti internal Dispatcher/governance**: Controls task flow, max concurrency, and execution boundaries.
- **Workers**: Single-use, pure-logic, packet-only helpers that process tasks but hold absolutely no execution or approval authority.

## What Anti Is
Anti is a highly capable, agentic OS that operates under strict internal governance. Anti executes only within bounded stage prompts. The human operator grants explicit approval, while Anti’s internal Dispatcher/governance rules control task flow and execution boundaries.

## What Anti Is NOT
- Anti is **NOT** a fully autonomous looping agent.
- Automated routing is **NOT** enabled.
- Anti does **NOT** grant itself execution authority.
- Anti does **NOT** approve its own high-risk actions (commits, pushes, deploys).

## Core Architecture
### 1. Single-Use, Packet-Only Workers
All logic is processed by explicit-use workers. Workers receive a structured JSON input packet, perform their logic, and return a strict JSON output packet. They do not talk to each other, and they do not spawn subagents.

### 2. The Proof-Gated Workflow
Anti operates in a rigid, gated loop to prevent hallucinations and uncontrolled drift:
1. **PLAN ONLY**: Anti researches and drafts a proposal without mutating files.
2. **EXECUTE/VERIFY ONLY**: Anti executes the approved plan (e.g., editing files, running tests) but is strictly forbidden from committing or pushing.
3. **VERIFY/CONSOLIDATE**: Anti dumps raw outputs to prove what it did.
4. **COMMIT/VERIFY ONLY**: Anti stages strictly bounded files and commits atomically.
5. **PUSH/VERIFY ONLY**: Anti pushes to the remote securely.

### 3. Red-Gate Approvals
High-risk actions (Commit, Push, Deploy, Secret Access, Governance Edit) require a **Red-Gate Approval Packet**. 
**CRITICAL NOTE**: All red-gate approval packets are **draft approval requests only**. They do not grant approval, authorize execution, or mark work complete. They format the exact command, allowed files, and verification steps so the **human operator** can explicitly review and approve them.

## Current Swarm Capability
Do not assume fully autonomous swarm execution exists. The current capability of Anti is exactly:
- Planning support exists.
- Audit support exists.
- Red-gate packet drafting exists.
- Explicit-use workers exist.
- **Automatic routing and autonomous swarm execution are NOT enabled.**

## How to use this guide
If you are an external assistant helping a human operate Anti, navigate to the `prompts/` directory. You must construct your instructions using the exact stage override tokens (e.g., `PLAN ONLY:`, `EXECUTE/VERIFY ONLY:`) to ensure Anti remains safely bounded. Always review Anti's `TASK RESULT` output carefully for signs of authority drift.
