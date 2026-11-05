# PANDA DEBUGGER

PANDA DEBUGGER is an Agent Skill for investigating software defects from evidence before changing code. It guides an agent through **Understand → Reproduce → Trace → Root Cause → Fix → Verify** and produces a report that separates observed results from untested explanations.

## Install

Copy this directory to a skills location supported by your agent, such as `~/.codex/skills/panda-debugger`. The directory name should be `panda-debugger`, with `SKILL.md` at its root. The other files provide investigation guidance and demo material.

## Use

Invoke `$panda-debugger` or ask the agent to investigate an exception, failing test, API error, production incident, or unexpected behavior. Supply the repository or relevant code, expected and actual behavior, logs, and reproduction steps when available. The agent should inspect available code before asking for missing details.

Example: “Use `$panda-debugger` to investigate this API error. Trace the request to the failure, identify the root cause with evidence, and propose a fix.”

The agent may implement a fix when the task requests it. If the request is only to investigate, the output is a concrete proposal and test plan. Claims that a test passed require actual execution.

## Report and demo

The [report template](references/report-template.md) starts with short root-cause bullets in clear B2-level English, then covers the problem, evidence, reproduction, execution flow, fix, tests, verification, and remaining risks. The [investigation guide](references/investigation-guide.md) explains the evidence and phase gates.

Three synthetic [test scenarios](tests/scenarios) cover a nil pointer, a transaction lifecycle defect, and duplicate asynchronous processing. Use [the evaluation guide](tests/evaluation.md) to compare an agent with and without this Skill on the same scenario. The scenarios are text fixtures; they do not constitute a runnable application or proof that any bug was reproduced.
