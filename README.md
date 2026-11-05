# PANDA DEBUGGER

PANDA DEBUGGER is an Agent Skill for investigating software defects from evidence before changing code. It guides an agent through **Understand → Reproduce → Trace → Root Cause → Fix → Verify** and produces a report that separates observed results from untested explanations.

## Install

Run these commands from the directory where you want to clone the repository. They install the same Skill for Codex and Claude Code without copying the repository's `.git` directory:

```sh
git clone https://github.com/akkaraponph/panda-debugger.git
cd panda-debugger

PANDA_CODEX_SKILLS="${CODEX_HOME:-$HOME/.codex}/skills"
mkdir -p "$PANDA_CODEX_SKILLS/panda-debugger" "$HOME/.claude/skills/panda-debugger"
rsync -a --exclude='.git' ./ "$PANDA_CODEX_SKILLS/panda-debugger/"
rsync -a --exclude='.git' ./ "$HOME/.claude/skills/panda-debugger/"
```

Codex reads the Skill from `$CODEX_HOME/skills/panda-debugger` (or `~/.codex/skills/panda-debugger` when `CODEX_HOME` is unset). Claude Code reads it from `~/.claude/skills/panda-debugger`. In each directory, `SKILL.md` must be at the root alongside `references/` and `tests/`.

To update an existing installation, run `git pull --ff-only` in the clone, set `PANDA_CODEX_SKILLS` again, and repeat the two `rsync` commands above. Start a new agent session to use the updated files.

## Use

Invoke `$panda-debugger` in Codex or `/panda-debugger` in Claude Code. You can also ask the agent to investigate an exception, failing test, API error, production incident, or unexpected behavior. Supply the repository or relevant code, expected and actual behavior, logs, and reproduction steps when available. The agent should inspect available code before asking for missing details.

Example: “Use `$panda-debugger` to investigate this API error. Trace the request to the failure, identify the root cause with evidence, and propose a fix.”

The agent may implement a fix when the task requests it. If the request is only to investigate, the output is a concrete proposal and test plan. Claims that a test passed require actual execution.

## Report and demo

The [report template](references/report-template.md) starts with short root-cause bullets in clear B2-level English, then covers the problem, evidence, reproduction, execution flow, fix, tests, verification, and remaining risks. For complex paths, the agent may add a small Mermaid flowchart or offer one after the report. The [investigation guide](references/investigation-guide.md) explains the evidence and phase gates.

Three synthetic [test scenarios](tests/scenarios) cover a nil pointer, a transaction lifecycle defect, and duplicate asynchronous processing. Use [the evaluation guide](tests/evaluation.md) to compare an agent with and without this Skill on the same scenario. The scenarios are text fixtures; they do not constitute a runnable application or proof that any bug was reproduced.
