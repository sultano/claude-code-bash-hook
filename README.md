# Claude Code Bash Hook

Auto-approves safe Bash commands (build, test, read-only ops) so you're not prompted repeatedly, while deferring dangerous commands to Claude Code's permission system.

**Why?** Running `go test` 50 times shouldn't require 50 approvals, but `rm -rf` should always ask.

## Requirements

[Ollama](https://ollama.ai) with `qwen2.5-coder:7b`:

```bash
ollama pull qwen2.5-coder:7b
ollama serve
```

## Install

```bash
cp validate_tool_safety.py ~/.claude/hooks/
```

Add to `~/.claude/settings.json`:

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "python3 ~/.claude/hooks/validate_tool_safety.py"
          }
        ]
      }
    ]
  }
}
```

## How It Works

1. Code-based safety net catches known dangerous patterns (100% reliable)
2. LLM handles nuanced/novel cases
3. Safe commands are auto-whitelisted to `settings.local.json` for future runs
