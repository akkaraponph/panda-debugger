# PANDA DEBUGGER

PANDA DEBUGGER is an Agent Skill for investigating software defects from evidence before changing code. It guides an agent through **Understand → Reproduce → Trace → Root Cause → Fix → Verify** and produces a report that separates observed results from untested explanations.

It works across programming languages and frameworks using the tools available to your agent, such as tests, a browser, logs, and traces. When evidence is missing, it says what still needs to be checked.

## Install

### Codex marketplace

Add this repository as a marketplace source on your machine, then install its plugin:

```sh
codex plugin marketplace add akkaraponph/panda-debugger
codex plugin add panda-debugger@panda-debugger
```

### Claude Code marketplace

Add the same repository as a marketplace source in Claude Code, then install its plugin:

```sh
claude plugin marketplace add akkaraponph/panda-debugger
claude plugin install panda-debugger@panda-debugger
```

Start a new Codex or Claude Code session after installing.

### Manual skill copy

Run these commands from the directory where you want to clone the repository. They install the Skill for Codex and Claude Code without the plugin commands:

```sh
git clone https://github.com/akkaraponph/panda-debugger.git
cd panda-debugger

PANDA_CODEX_SKILLS="${CODEX_HOME:-$HOME/.codex}/skills"
mkdir -p "$PANDA_CODEX_SKILLS/panda-debugger" "$HOME/.claude/skills/panda-debugger"
rsync -a skills/panda-debugger/ "$PANDA_CODEX_SKILLS/panda-debugger/"
rsync -a skills/panda-debugger/ "$HOME/.claude/skills/panda-debugger/"
```

To update a manual installation, run `git pull --ff-only` in the clone, then repeat the `PANDA_CODEX_SKILLS`, `mkdir`, and `rsync` commands above. Start a new agent session to use the updated files.

## Use

Invoke `$panda-debugger` in Codex, `/panda-debugger:panda-debugger` with the Claude Code plugin, or `/panda-debugger` with a manual Claude Code copy. Ask it to investigate an exception, failing test, API error, or unexpected behavior. Share the relevant code or repository, expected and actual behavior, logs, and steps to reproduce the problem when you have them.

Example: “Use `$panda-debugger` to investigate this API error. Trace the request to the failure, identify the root cause with evidence, and propose a fix.”

The answer starts with short, plain-English root-cause bullets and shows the evidence, confidence level, and checks that were run. For a complex path, you can ask for a Mermaid flowchart. If you ask only for an investigation, the agent will propose a fix and ask before changing code. If you ask it to fix the bug, it can implement and verify the change.
