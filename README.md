# Token Saver — Claude Code Skill

Reduce Claude Code token usage without altering functionality. Works passively via CLAUDE.md rules and actively via `/token-saver` efficiency checks.

## What it does

### Always Active (via CLAUDE.md)
- **Targeted file reads** — uses `offset` + `limit` instead of reading entire files (~60-80% fewer tokens)
- **Grep before Read** — finds line numbers first, then reads only relevant sections
- **Parallel tool calls** — batches independent operations in a single message
- **No wasted agents** — avoids spawning subagents for 1-2 tool call tasks
- **Concise responses** — answer first, explain only if asked
- **Context management** — suggests `/compact` when conversations get long

### Model Switching Suggestions
| Task Type | Suggested Model | Savings |
|-----------|----------------|---------|
| Lookups, grep, formatting, SQL | `/model haiku` | ~5x cheaper |
| Coding, bug fixes, tests | `/model sonnet` | ~2x cheaper |
| Architecture, deep analysis | `/model opus` | Full power |

### Invokable Check (`/token-saver`)
Run mid-conversation to get an efficiency audit:
- Flags full-file reads that could have been targeted
- Counts agent spawns and whether they were necessary
- Recommends model switches
- Suggests `/compact` if context is large

## Installation

Copy the skill into your Claude Code skills directory:

```bash
cp -r token-saver ~/.claude/skills/
```

Add efficiency rules to your global CLAUDE.md:

```bash
cat >> ~/.claude/CLAUDE.md << 'EOF'

## Token Efficiency (ALWAYS ACTIVE)
- Use targeted file reads with offset + limit — never read full files blindly
- Use Grep first to find line numbers, then Read only those lines
- Batch independent tool calls in parallel
- Never spawn Agents for tasks that take 1-2 tool calls
- Keep responses concise — answer first, explain only if asked
- Suggest /compact after 15+ messages or major task completion
- Suggest /model haiku for simple lookups, grep, formatting
EOF
```

## License

MIT
