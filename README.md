# Three-Tier Agent Symphony

A Codex skill that has the current main thread perform the Astra lead-orchestrator role while delegating independent subtasks to Sol Medium and Luna Max based on the nature of the work.

## Role assignments

| Role | Responsibilities |
| --- | --- |
| Main thread in the GPT-6 Astra lead role | Understand the objective, break down tasks, make architectural decisions, review results, and integrate the final output |
| GPT-5.6 Sol Medium | Handle difficult but clearly bounded analysis and implementation, in-depth code review, and complex debugging |
| GPT-5.6 Luna Max | Perform clear, repeatable, and easily verifiable searches, tests, reproductions, mechanical edits, and summarization |

## Core behavior

- Do not infer, verify, or require a particular model for the main thread; it directly performs the Astra lead role.
- Verify that Sol Medium and Luna Max are available before starting substantive work.
- If either required subagent model or reasoning capability is unavailable, stop completely instead of substituting another model or using a lower reasoning level.
- Launch Sol Medium and Luna Max by passing the exact model and reasoning values directly to the subagent tool.
- Choose the subagent model and reasoning effort first, then derive the task-name suffix from those exact values: `_sol_medium` or `_luna_max`.
- Use `fork_turns: "none"` for Luna Max and by default for Sol Medium. Sol Medium may use `"2"` only when its task directly depends on the latest conversation; never omit the field or use `"all"`.
- Once all capabilities are available, delegate subtasks with explicit objectives, scope, completion criteria, and validation methods.
- The main thread reviews important diffs, tests, and evidence, and is responsible for the final integration.

## Requirements

- Codex supports skills and subagents.
- Subagents can use `gpt-5.6-sol` with `medium` reasoning and `gpt-5.6-luna` with `max` reasoning.

Whether a reasoning level appears in the Codex App model picker is not the capability check. Each subagent launch directly passes `model` and `reasoning_effort`; the tool declaration and actual startup result determine whether the combination is available.

The task-name suffix is a visible label derived from the actual launch arguments. Use `security_review_sol_medium` for `gpt-5.6-sol` with `medium`, and `sdk_docs_luna_max` for `gpt-5.6-luna` with `max`. The skill requires checking this mapping before launch and forbids launching a mismatched name. The explicit model and reasoning arguments remain authoritative.

## Installation

Run the following on macOS or Linux:

```bash
git clone https://github.com/irons163/three-tier-agent-symphony.git "${CODEX_HOME:-$HOME/.codex}/skills/three-tier-agent-symphony"
```

If the current task does not reload the skill, create a new task. If it still does not appear, restart the Codex App.

## Usage

Explicitly enable the skill in a Codex prompt:

```text
$three-tier-agent-symphony
```

You can also describe the work directly, for example:

```text
Use the Three-Tier Agent Symphony to review this project: Astra handles integration, Sol Medium performs the architecture and security review, and Luna Max runs tests and organizes the errors.
```

## Repository structure

```text
.
├── README.md
├── SKILL.md
└── agents/
    └── openai.yaml
```
