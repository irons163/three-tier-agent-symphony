# Three-Tier Agent Symphony

A Codex skill that has the current main thread perform the Sol lead-orchestrator role while delegating independent subtasks to Terra Max and Luna Max based on the nature of the work.

## Role assignments

| Role | Responsibilities |
| --- | --- |
| Main thread in the Sol lead role | Understand the objective, break down tasks, make architectural decisions, review results, and integrate the final output |
| GPT-5.6 Terra Max | Handle difficult but clearly bounded analysis and implementation, in-depth code review, and complex debugging |
| GPT-5.6 Luna Max | Perform clear, repeatable, and easily verifiable searches, tests, reproductions, mechanical edits, and summarization |

## Core behavior

- Do not infer, verify, or require a particular model for the main thread; it directly performs the Sol lead role.
- Verify that Terra Max and Luna Max are available before starting substantive work.
- If either required subagent model or Max reasoning capability is unavailable, stop completely instead of substituting another model or using a lower reasoning level.
- Launch Terra Max and Luna Max by passing the exact model and reasoning values directly to the subagent tool.
- Once all capabilities are available, delegate subtasks with explicit objectives, scope, completion criteria, and validation methods.
- The main thread reviews important diffs, tests, and evidence, and is responsible for the final integration.

## Requirements

- Codex supports skills and subagents.
- Subagents can use `gpt-5.6-terra` and `gpt-5.6-luna`, and both support `max` reasoning effort.

Whether Max appears in the Codex App model picker is not the capability check. Each subagent launch directly passes `model` and `reasoning_effort`; the tool declaration and actual startup result determine whether the combination is available.

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
Use the Three-Tier Agent Symphony to review this project: the main thread handles integration, Terra Max performs the architecture and security review, and Luna Max runs tests and organizes the errors.
```

## Repository structure

```text
.
├── README.md
├── SKILL.md
└── agents/
    └── openai.yaml
```
