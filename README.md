# Antigravity Operator Guide

**Notice:** This guide is NOT Anti's internal source of truth or governance repository. It is an external operator guide designed to help humans prompt, review, and orchestrate Anti safely.

Welcome to the **Antigravity Operator Guide**. This repository is designed for human operators and external LLM assistants (like ChatGPT, Claude, etc.) to understand how to interact with **Anti OS**—a strictly governed agentic coding assistant.

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
