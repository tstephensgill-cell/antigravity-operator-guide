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
- Anti-run localhost smoke proof where runtime UI is involved (human manual smoke only if explicitly chosen)
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
Anti supports base stages and explicit bundled stages. EXECUTE ONLY may be valid when it appears as an exact, bounded, approved menu option. EXECUTE/VERIFY is a valid bundled stage when execution and verification are approved together. ChatGPT must preserve the exact stage/bundle used by Anti’s approval menu and evaluate whether the option is exact, bounded, and properly itemized.

## ChatGPT Guidance: Bootstrap And Source Of Truth

This guide is for external ChatGPT assistants helping the user operate Anti. It is not Anti’s runtime governance and does not replace Anti’s README or boot process.

Anti must complete global bootstrap before task work. If Anti reports `BOOTSTRAP_BLOCKED`, ChatGPT should not suggest falling back to a local project README or continuing with task work. The correct guidance is to resolve bootstrap/read permission first.

Source-of-truth order:
1. Global Anti README / bootstrap-loaded governance.
2. Boot/agent directive, only where it does not conflict with README.
3. Project-local rules, loaded only after global governance.
4. External operator guide, advisory only for ChatGPT prompt drafting/review.

If a boot/agent directive conflicts with README.md, README.md wins.

ChatGPT must not claim bootstrap is loaded unless Anti has provided bootstrap proof in the current session.

## ChatGPT Guidance: Preserve Anti Stage/Menu Governance

This guide is for external ChatGPT assistants helping the user operate Anti. It must not invent a different governance model.

Anti supports base stages:
- CAPABILITY
- PLAN
- VERIFY
- EXECUTE
- COMMIT
- PUSH
- ESCALATION

Anti may also use explicitly approved bundled stages:
- EXECUTE/VERIFY
- COMMIT/VERIFY
- PUSH/VERIFY
- VERIFY BATCH

ChatGPT must not tell the user that `EXECUTE ONLY` is invalid by default. `EXECUTE ONLY` can be valid when it appears as an exact, bounded approval-menu option with a clear stage, target, action, allowed scope, forbidden scope, and stop condition.

`EXECUTE/VERIFY` is appropriate when execution and verification are approved as one itemized bundle, but it is not the only valid execution form.

Numbered approval menus are part of Anti’s operator workflow.

A number reply such as `1`, `2`, or `3` is valid only when:
- a numbered Approval Menu appeared in the immediately preceding assistant message
- the number maps to exactly one option
- the selected option contains the full stage/target/action text
- the user adds no extra conditions, questions, or ambiguity

The selected number authorizes only the exact option text. It does not authorize helper steps, discovery, tests, commits, pushes, cleanup, roadmap work, or follow-up work unless those actions are explicitly listed inside that exact option.

If the user replies with conditions or feedback, such as `Approve 1 but...`, `Change 1 so that...`, or `I like option 1 but...`, ChatGPT should treat it as PLAN feedback, not approval.

Menu options expire after one user turn.

ChatGPT should preserve Anti’s numbered approval-menu workflow, while still checking that each option is bounded, stage-correct, risk-aware, and not converting architecture or roadmap work into execution.

## ChatGPT Guidance: Hidden Helper Step And Checkpoint Caution

Some boot, skill, or local directives may mention helper actions such as checkpoints, Omega hooks, Gatekeeper audits, file reads, test runs, backups, git commits, or git stashes.

ChatGPT must not treat these helper actions as automatically approved.

Under Anti’s exact option scope governance, helper steps are only allowed when they are explicitly included in the selected approval option.

Examples of helper steps that must be itemized before use:
- file reads
- directory listings
- grep/search commands
- tests
- server starts
- browser smoke checks
- Omega hooks
- Gatekeeper audits
- checkpoint commits
- git stash
- backup file creation
- cleanup
- rollback
- staging
- commit
- push

If a helper step is required for safety but was not listed in the selected option, ChatGPT should recommend returning to PLAN and drafting a corrected numbered approval menu that explicitly includes that helper step.

If a boot or local directive appears to require an unapproved commit, stash, backup, or checkpoint, ChatGPT should treat it as a governance conflict and follow the README/source-of-truth rule.

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
node -c proves syntax only. node -c does not prove browser runtime works. Static checks are not enough for live UI behavior. Frontend app-shell work requires Anti-run localhost smoke proof before push where Anti has local runtime access.

External ChatGPT must not assume the human user personally runs smoke tests.
- Default wording should be "Anti-run localhost smoke proof".
- Anti may run the app locally using existing project scripts.
- Anti may use browser/devtools/runtime automation if available.
- Anti should gather smoke proof directly when possible.
- If localhost points at production data, read-only smoke is allowed but data-mutating smoke requires explicit user approval.
- POST/PUT/PATCH/DELETE actions are data-mutating unless proven intercepted/dev-safe.
- If safe interception/dev DB is unavailable, Anti should report NEEDS USER APPROVAL FOR DATA-MUTATING SMOKE.
- Push without smoke is allowed only with explicit user-accepted no-smoke risk.

Anti-run localhost smoke proof checklist should include:
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

If Anti-run localhost smoke proof cannot be run, mark "not safe to push" unless user explicitly accepts no-smoke risk.

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

## Specialist / Subagent Operations Guide For ChatGPT

This section teaches external ChatGPT assistants how to reason about Anti’s native worker/subagent system when reviewing Anti output or drafting prompts.

This is advisory guidance for ChatGPT. It does not grant worker authority, does not change Anti runtime behaviour, and does not replace Anti’s README/bootstrap governance.

### Native Subagent Mechanics

Anti may use native subagent tools to create hidden background worker conversations.

Important concepts:
- `define_subagent` defines an in-session worker from a prompt/specification.
- `invoke_subagent` invokes that worker for a bounded packet of work.
- `send_message` may continue or communicate with a specific subagent conversation when explicitly approved.
- Subagents are hidden/background worker chats with unique conversation IDs.
- Subagent definitions are session-bound. Local markdown worker files remain the source of truth.
- If the session ends or the worker is not currently defined, Anti must reload/redefine from the approved local worker source before invocation.
- A worker definition is not a permanent promotion, not a registry change, and not default routing authority.
- A worker invocation is not proof by itself. Raw worker payloads and verifier evidence are still required.

External ChatGPT should check whether worker-assisted plans show:
- exact worker names
- exact worker packets
- exact raw worker outputs
- exact tool flags
- allowed files/context
- forbidden actions
- stop conditions
- whether any worker attempted scope expansion

### Subagent Tool Flags

When drafting or reviewing subagent prompts, ChatGPT should check the explicit tool flags.

Important flags:
- `enable_write_tools`
- `enable_subagent_tools`
- `enable_mcp_tools`

Safe pure-logic worker pattern:
- `enable_write_tools=false`
- `enable_subagent_tools=false`
- `enable_mcp_tools=false`

Meaning:
- no file writes
- no worker spawning
- no MCP/tool expansion
- advisory output only

Warning signs:
- write tools enabled without an EXECUTE-approved target
- subagent tools enabled for a worker that should be atomic
- MCP tools enabled without explicit red-gate approval
- a worker attempts file reads/writes not listed in its packet
- a worker attempts to route, approve, commit, push, deploy, or mark completion

Tool flags must be treated as operational bounds, not cosmetic settings.

### Workspace Mode: inherit vs branch

Some subagent systems support workspace modes such as:
- `inherit`
- `branch`

ChatGPT must not assume either mode is safe by default.

General guidance:
- `inherit` means the worker operates in the parent/current workspace context. This is usually appropriate for pure-logic advisory work when write tools are disabled and the worker only receives bounded context.
- `branch` may isolate work in a separate branch/workspace. This may be useful for experimental implementation, but it increases complexity and must be explicitly approved if it changes git/workspace state.

Rules:
- Pure-logic advisory workers should normally use inherited context with write/subagent/MCP tools disabled.
- Write-enabled subagents require explicit EXECUTE approval, exact allowed files, exact branch/workspace expectations, and verification.
- Branch/workspace creation, switching, or mutation is not implied by worker use.
- If workspace mode is missing from a risky worker plan, ChatGPT should ask Anti to clarify before execution.

### Bootstrap Delegation For Pure-Logic Workers

Anti’s Rule 0A bootstrap mandate applies to the main Anti session. Pure-logic workers can run into a conflict if they are tool-disabled but also try to independently read BOOTSTRAP.md.

Known operating pattern:
- The Dispatcher / primary Anti session completes bootstrap.
- The Dispatcher passes bounded governance/context into the worker packet.
- The worker packet may include a marker such as `BOOTSTRAP_DELEGATED_BY_DISPATCHER`.
- The worker then performs only its bounded pure-logic task.
- The worker does not independently read BOOTSTRAP.md unless that read is explicitly part of the approved task.

Expected phrase:
`BOOTSTRAP_DELEGATED_BY_DISPATCHER`

ChatGPT should recommend this pattern for pure-logic workers with write/tools disabled.

Warning signs:
- a pure-logic worker blocks because it tries to read BOOTSTRAP.md itself
- a worker leaks `BOOTSTRAP_LOADED` text into a strict JSON response
- a worker treats delegated bootstrap as execution authority
- a worker uses delegated bootstrap to expand scope

Important boundary:
Write-enabled workers must NOT inherit this exemption casually.
If a worker has write tools enabled, file-read/tool authority must be explicitly approved by the selected Anti stage/menu option.

### JSON-Only Worker Output And BOOTSTRAP Leakage

Some strict JSON workers may leak wrapper text such as:
- `BOOTSTRAP_LOADED`
- markdown fences
- explanatory prose before/after JSON
- partial bootstrap/status messages outside the schema

For strict schema workers, ChatGPT should recommend prompts that require:
- output starts with `{`
- output ends with `}`
- no markdown fences
- no bootstrap wrapper text
- no prose outside the JSON object
- missing-context refusal follows the worker’s exact schema

A worker returning useful content with wrapper leakage may be a partial success, but it is not a strict schema pass.

Classify as:
- `PASS` only if the output obeys the schema and the content is valid
- `PASS WITH WARNING` if content is useful but formatting/schema has minor leakage
- `NEEDS_REFINEMENT` if the worker still violates its contract
- `FAIL / STOP` if the worker invents context, expands authority, or returns unsafe instructions

### Transcript Audit Path

Native subagents can produce transcript evidence that is stronger than a summary.

ChatGPT should ask Anti to use transcript proof when validating worker/tool behaviour, especially after subagent tests or swarm work.

Transcript verification should look for:
- the worker conversation ID
- the worker definition/spec used
- the invocation packet
- tool flags used at definition/invocation time
- `ExecutedTools` or equivalent tool-use records
- whether the worker used forbidden tools
- whether the worker attempted file reads/writes
- whether the worker spawned other subagents
- whether the worker called MCP/external tools
- whether the worker output matched the requested schema
- whether wrapper text leaked into strict JSON output

Important:
Do not invent transcript paths.

The exact transcript location may depend on Antigravity version, workspace, or project settings. If the transcript path is not already verified in the current session, ChatGPT should ask Anti to verify it using approved discovery before relying on it.

A safe transcript-audit prompt should ask Anti to:
- identify the current transcript/log location from verified local evidence
- extract only the relevant worker conversation(s)
- report conversation ID(s)
- report `ExecutedTools` or equivalent tool-use evidence
- avoid printing secrets or unrelated conversation content
- stop if transcript location is unknown or inaccessible

A transcript summary without tool-use evidence is not enough to prove tool isolation.

### Swarm Throttle Policy

Large worker batches can hit model/runtime rate limits.

Known operating lesson:
- high concurrency can cause 429/rate-limit failures
- a concurrency level around 4 has been safer than large swarms
- very large batches, such as 20+ simultaneous/near-simultaneous worker calls, should be treated as high risk unless explicitly approved
- retries can burn quota and hide failure patterns

ChatGPT should not encourage large autonomous swarms unless the user explicitly approves:
- worker count
- concurrency
- retry policy
- stop conditions
- evidence retention
- failure handling
- quota/rate-limit behaviour

Safer default:
- small bounded batches
- preserve raw worker outputs
- stop on 429/rate limit
- reduce concurrency before retrying
- do not retry blindly
- do not count a skipped/failed worker as passed

### Invocation Caps And No-Retry Rule

For quota-aware Anti work, worker invocation counts should be planned.

Prompt-review checklist:
- How many workers will be defined?
- How many workers will be invoked?
- Is each worker invocation necessary?
- Is concurrency specified?
- Is there a no-retry or limited-retry rule?
- Does the prompt stop on rate-limit/429?
- Are failed worker outputs preserved?
- Are partial outputs labelled correctly?

Default guidance:
If rate limits, 429s, missing outputs, or malformed worker payloads occur, stop and return a TASK RESULT. Do not blindly retry or expand the swarm.

### Known Partial-Pass Worker Pattern

A worker can improve but still not fully pass.

Example pattern:
- a worker detects a boundary issue and returns a `StopReason`
- but still fills a field with invalid content
- or still stuffs multiple files into a single-file field
- or still returns data that violates its own worker contract

This is not a full pass.

Classify as:
- `PASS WITH WARNING` when output is usable but contract weakness remains
- `NEEDS_REFINEMENT` when the worker behaviour must be patched before relying on it
- `FAIL / STOP` when the output could cause unsafe routing, execution, or false completion

Worker validation must check both:
- explicit refusal/StopReason behaviour
- all returned fields obey the worker contract

## External Platform Mode Caution

Some behaviour may come from the Antigravity platform UI rather than Anti OS governance files.

External ChatGPT assistants must distinguish:

- Anti OS governance: rules verified from Anti README, BOOTSTRAP, HANDOFF, directives, or approved repo evidence.
- Platform UI context: behaviour reported by the user or shown in screenshots, but not necessarily present in Anti OS files.

If the user reports a platform mode such as Turbo Mode, ChatGPT should treat it as USER-PROVIDED platform context unless verified from current platform documentation or UI evidence.

Do not claim platform UI modes are Anti OS governance unless they exist in Anti OS source-of-truth.

Do not ask Anti to prove platform UI settings by grepping the Anti OS repo.

If a platform mode appears to reduce manual review or safety prompts, ChatGPT should preserve Anti OS governance more carefully:
- bootstrap first
- README/source-of-truth hierarchy
- explicit stage authority
- numbered approval menus
- exact option scope
- no auto-advance
- no hidden helper steps
- proof before completion
- human approval for red gates

External platform modes do not reduce Anti governance requirements and do not override ChatGPT/OpenAI/system safety rules.

### How ChatGPT Should Review Worker-Assisted Anti Results

When Anti claims worker-assisted planning or verification, ChatGPT should check:

- Were workers actually invoked?
- Were raw worker outputs shown?
- Were tool flags shown or inferable from verifier proof?
- Did workers stay advisory?
- Did workers avoid routing/approval/completion authority?
- Did any worker attempt to read files, write files, spawn subagents, or call MCP?
- Did strict JSON workers return valid naked JSON?
- Did any worker leak bootstrap wrapper text?
- Did transcript evidence confirm tool isolation?
- Did rate limits or 429s occur?
- Were partial passes labelled correctly?
- Did the final Anti plan still require human approval?

Absence of raw worker outputs in a supposed worker-assisted plan is a warning.

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
