---
name: three-tier-agent-symphony
description: "Have the main thread perform the GPT-5.6 Sol lead-orchestrator role, delegate difficult but clearly bounded work to GPT-5.6 Terra Max, and assign clear, repeatable work to GPT-5.6 Luna Max. Use when the user requests Sol orchestration, Terra Max or Luna Max subagents, tiered multi-agent coding, parallel code review, module analysis, independent feature implementation, testing, debugging, or result integration. Do not infer or require switching the main model; before delegation, check only the required subagent model and reasoning combinations."
---

# Three-Tier Agent Symphony

Have the current main thread perform Sol's orchestration role: understand the objective, break down tasks, make architectural decisions, verify completion, and integrate results. Delegate difficult but clearly bounded work to Terra Max, and clear, repeatable, easily verifiable work to Luna Max.

## 1. Check subagent capabilities before delegation

The current main thread directly assumes Sol's orchestration role. The skill cannot reliably verify or switch the main thread's model, so do not stop based on self-identification, a model name, a system prompt, or missing observability. Do not ask the user to switch the main model or remove model restrictions.

Before actual delegation, check the subagent capabilities exposed by the current environment:

1. Confirm that the subagent tool can directly specify these combinations:
   - Terra Max: `model = "gpt-5.6-terra"`, `reasoning_effort = "max"`.
   - Luna Max: `model = "gpt-5.6-luna"`, `reasoning_effort = "max"`.
2. If the tool declares support but an actual launch is rejected because of model or reasoning incompatibility, treat that capability as unavailable.
3. Proceed to Section 2 only after confirming that Terra Max and Luna Max are available.

Every time Terra Max or Luna Max is launched, pass the corresponding `model` and `reasoning_effort` values directly to the subagent tool. Do not depend on a custom agent configuration file. Do not use the App's Max visibility setting in the model picker as a capability check; use the subagent tool declarations and actual launch results.

### Any required capability is unavailable: complete hard stop

If `Terra Max`, `Luna Max`, or either required model/reasoning combination is unavailable, immediately stop the entire workflow, not just delegation. Apart from checking subagent capabilities and explaining how to enable them, do not run tools or advance the substantive task. This includes:

- Do not read or inventory the project, code, documents, or external data.
- Do not perform architecture design, compliance boundary analysis, requirements breakdown, risk analysis, or test planning.
- Do not create substitute subagents or let the main thread proceed alone with work that could otherwise be done safely.
- Do not downgrade to `high`, `xhigh`, or another model/reasoning configuration on your own.

Do not introduce exceptions that let the main thread continue alone. While this skill is active, a missing required capability is a complete stopping condition. Use a different workflow only if the user explicitly cancels this skill or explicitly changes the required model combination.

The only permitted setup exception: if the user explicitly asks the AI to help enable the required subagent capabilities, the AI may guide them through the App settings. If desktop control is available, it may also help inspect the available model and reasoning options. This exception does not permit reading the original project, analyzing the original task, or advancing any main-thread work.

### A required model or reasoning combination is unavailable

State precisely which model or reasoning combination is missing, then ask the user to confirm the model and reasoning controls in the Codex App:

- Terra Max requires `gpt-5.6-terra` with `max`.
- Luna Max requires `gpt-5.6-luna` with `max`.

If an option is absent, ask the user to check their account plan, workspace administrator model policies, and current provider. The skill cannot unlock an unavailable model. Maintain the hard stop until every required combination is available.

## 2. Map the work

The main thread, acting in the Sol lead role, first defines the overall objective, constraints, acceptance criteria, and dependencies, then routes tasks according to the nature of the work:

| Role | Assigned work |
| --- | --- |
| Main thread in the Sol lead role | Ambiguous requirements, architectural and cross-module decisions, task breakdown, conflict resolution, final acceptance, and integrated output |
| Terra Max | Difficult but self-contained module analysis, nontrivial independent implementation, in-depth code review, security or concurrency reasoning, and complex root-cause investigation |
| Luna Max | Code search and fact gathering, test execution and bug reproduction, log classification, mechanical edits, small features with precise specifications, and structured summaries |

Do not delegate trivial tasks merely to use subagents. If core requirements remain unclear, the main thread should resolve them first instead of relying solely on increased subagent reasoning effort.

## 3. Delegate with an explicit contract

Every subagent prompt must include:

- **Objective**: describe one independently achievable outcome.
- **Context and inputs**: provide the necessary files, symbols, errors, or specifications.
- **Scope**: list the modules or files the agent may read and write.
- **Restrictions**: identify interfaces, behaviors, and adjacent work that must not be changed.
- **Completion criteria**: define verifiable acceptance criteria.
- **Validation**: specify tests, type checks, lint checks, reproduction steps, or evidence.
- **Report format**: request a summary, changed files, validation results, risks, and unresolved questions.

Use a hub-and-spoke structure by default: the main thread delegates directly to Terra and Luna, and both report directly to the main thread. Do not let Terra manage Luna unless Terra has been authorized to take full ownership of an independent subsystem.

## 4. Control concurrent writes

Parallelize only independent work. When multiple agents share a working directory, assign exclusive file or module ownership to each writing task. If scopes overlap, run the tasks sequentially or use isolated worktrees.

Suitable cross-checking patterns:

- Terra implements a complex feature, Luna creates or runs focused tests, and the main thread performs the final integration.
- Luna completes a clearly specified change, Terra performs an in-depth review, and the main thread decides whether to accept it.
- Luna gathers reproduction and log evidence, Terra analyzes the root cause, and the main thread decides on the cross-module fix.

## 5. Verify and integrate

After all required results are available, the main thread:

1. Checks whether subagents stayed within scope and met their completion criteria.
2. Directly inspects important diffs, file references, tests, and reproduction evidence rather than accepting summaries alone.
3. Resolves conflicting conclusions using code and validation results.
4. Performs integration validation appropriate to the level of risk.
5. Reports the completed work, validation results, remaining risks, and the actual division of work among the main thread, Terra Max, and Luna Max in the final response.

Do not treat an agent's own claim of completion as final approval. Final responsibility remains with the main thread.
