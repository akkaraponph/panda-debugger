# PANDA DEBUGGER

PANDA DEBUGGER is an Agent Skill for investigating software defects from evidence before changing code. It guides an agent through **Understand → Reproduce → Trace → Root Cause → Fix → Verify** and produces a report that separates observed results from untested explanations.

The workflow is language and framework agnostic. It uses the build commands, tests, browser, debugger, logs, traces, and other tools available in the current agent environment. It does not provide those tools or guarantee a confirmed root cause when runtime access or evidence is missing.

## Install

### Claude Code plugin

```sh
claude plugin marketplace add akkaraponph/panda-debugger
claude plugin install panda-debugger@panda-debugger
```

To update, run `claude plugin marketplace update panda-debugger`, then `claude plugin update panda-debugger@panda-debugger`. Start a new session to use the updated files.

### Codex plugin

```sh
codex plugin marketplace add akkaraponph/panda-debugger
codex plugin add panda-debugger@panda-debugger
```

To update, run `codex plugin marketplace upgrade panda-debugger`, then repeat `codex plugin add panda-debugger@panda-debugger`. Start a new session to use the updated files.

### Manual copy for Codex or Claude Code

Run these commands from the directory where you want to clone the repository. They copy the same Skill to Codex and Claude Code:

```sh
git clone https://github.com/akkaraponph/panda-debugger.git
cd panda-debugger

PANDA_CODEX_SKILLS="${CODEX_HOME:-$HOME/.codex}/skills"
mkdir -p "$PANDA_CODEX_SKILLS/panda-debugger" "$HOME/.claude/skills/panda-debugger"
rsync -a skills/panda-debugger/ "$PANDA_CODEX_SKILLS/panda-debugger/"
rsync -a skills/panda-debugger/ "$HOME/.claude/skills/panda-debugger/"
```

Codex reads the Skill from `$CODEX_HOME/skills/panda-debugger` (or `~/.codex/skills/panda-debugger` when `CODEX_HOME` is unset). Claude Code reads it from `~/.claude/skills/panda-debugger`. In each directory, `SKILL.md` must be at the root alongside `references/`.

To update an existing installation, run `git pull --ff-only` in the clone, set `PANDA_CODEX_SKILLS` again, and repeat the two `rsync` commands above. Start a new agent session to use the updated files.

## Use

Invoke `$panda-debugger` in Codex, `/panda-debugger:panda-debugger` in the Claude Code plugin, or `/panda-debugger` with a manual Claude Code copy. You can also ask the agent to investigate an exception, failing test, API error, production incident, or unexpected behavior. Supply the repository or relevant code, expected and actual behavior, logs, and reproduction steps when available. The agent should inspect available code before asking for missing details.

Example: “Use `$panda-debugger` to investigate this API error. Trace the request to the failure, identify the root cause with evidence, and propose a fix.”

For an investigation-only request, the agent proposes a concrete fix and asks for approval before editing application code or tests. An explicit request to implement the fix already provides that approval. Claims that a test passed require actual execution.

## Report and demo

The [report template](skills/panda-debugger/references/report-template.md) starts with short root-cause bullets in clear B2-level English, then covers the problem, evidence, reproduction, execution flow, proposed fix, tests, verification, and remaining risks. For complex paths, the agent may add a small Mermaid flowchart or offer one after the report. The [browser guide](skills/panda-debugger/references/frontend-browser.md) and [backend evidence guide](skills/panda-debugger/references/backend-evidence.md) add focused runtime checks. The [investigation guide](skills/panda-debugger/references/investigation-guide.md) explains the shared evidence and phase gates.

Three synthetic [text scenarios](tests/scenarios) cover a nil pointer, a transaction lifecycle defect, and duplicate asynchronous processing. Two [runnable fixtures](tests/fixtures) cover a browser search race and a backend transaction bug. They use Go snippets, JavaScript, and Python as examples, not as a limit on supported languages. Use [the evaluation guide](tests/evaluation.md) to compare an agent with and without this Skill on the same case. A failing fixture proves the case is runnable; it does not by itself prove that the Skill improves debugging performance.
