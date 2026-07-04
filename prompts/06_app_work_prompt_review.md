# Prompt: App-Work Prompt Review
**Purpose**: Use this template to review a proposed Anti app-work prompt before it is executed by Anti.
**Usage**: Ask ChatGPT to review the drafted prompt against the Anti OS safety model.

```text
Please review the following proposed Anti OS app-work prompt against these safety criteria:
1. Should this prompt be PLAN ONLY instead of EXECUTE/VERIFY?
2. Does a prior PLAN ONLY discovery exist for this exact work?
3. Are exact AllowedFiles known and listed?
4. Are exact insertion points defined?
5. Are exact allowed actions specified?
6. Are exact ForbiddenFiles listed?
7. Are exact forbidden actions specified?
8. Are stop conditions concrete?
9. Are real Gate Map fields present? Required fields:
- GateCategory
- AllowedFiles
- ForbiddenFiles
- ForbiddenActions
- StopConditions
- VerificationCommands
- RedGateRequired
- HumanApprovalRequired
- AutoContinueAllowed
10. Does this work bundle multiple risk domains, such as DB + backend + frontend?
11. Is frontend runtime/app-shell risk involved, such as client/src/main.js, navigation, global startup, auth, global fetch, shared loaders, or route/view registry?
12. Is browser smoke proof explicitly required?
13. Is there any secret/env/data/destructive-operation risk involved?
14. Does a red-gate scan reveal hidden red-gate actions?
15. Are there any banned vague execution phrases, such as "if simple", "if needed", "where useful", "optional", or "as appropriate"?
16. Does the prompt use invalid approval menu tokens like "EXECUTE ONLY" instead of "EXECUTE/VERIFY ONLY"?
17. Does the prompt allow execution to discover scope while editing?
18. Should this work be split into smaller Work Packages?
19. Is this safe to commit?
20. Is this safe to push?

Return your review in this exact format:
- Verdict: [PASS | PASS WITH WARNING | FAIL / STOP | NEEDS PLAN | NEEDS PROOF | UNSAFE TO COMMIT | UNSAFE TO PUSH]
- Main Risk: <description>
- Missing Plan/Proof: <list>
- Recommended Smaller WP Split: <how to break it down>
- Corrected Prompt: <provide the corrected prompt if safe>
- STOP Recommendation: <explain why to stop, if not safe>
```
