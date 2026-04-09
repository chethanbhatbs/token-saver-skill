# Token Saver — Claude Code Skill

Reduce Claude Code token usage without losing functionality. Works passively via CLAUDE.md rules and actively via `/token-saver` efficiency checks.

## Prerequisites

- [Claude Code](https://claude.ai/claude-code) CLI installed
- GitHub CLI (`gh`) for one-line install

## Installation

**One-line install:**

```bash
gh repo clone chethanbhatbs/token-saver-skill ~/.claude/skills/token-saver
```

**Manual install:**

```bash
git clone https://github.com/chethanbhatbs/token-saver-skill.git
cp -r token-saver-skill/ ~/.claude/skills/token-saver/
```

**Verify it's installed:**

```bash
ls ~/.claude/skills/token-saver/
```

You should see `SKILL.md` (and any other skill files).

## Usage

```
/token-saver              # Run mid-conversation efficiency audit
```

### What it does

- **Targeted reads** — uses `offset` + `limit` instead of reading full files (~60-80% fewer tokens)
- **Grep before Read** — finds line numbers first, reads only relevant sections
- **Model switching** — suggests `/model haiku` for simple tasks (~5x cheaper)
- **Context management** — suggests `/compact` when conversations get long
- **No wasted agents** — avoids spawning subagents for 1-2 tool call tasks
- **Concise responses** — answer first, explain only if asked

### Model Recommendations

| Task Type | Model | Savings |
|-----------|-------|---------|
| Lookups, grep, formatting, SQL | `/model haiku` | ~5x cheaper |
| Coding, bug fixes, tests | `/model sonnet` | ~2x cheaper |
| Architecture, deep analysis | `/model opus` | Full power |

### Optional: Add global rules

For always-on efficiency, add to `~/.claude/CLAUDE.md`:

```bash
cat >> ~/.claude/CLAUDE.md << 'EOF'

## Token Efficiency
- Use targeted file reads with offset + limit
- Use Grep first to find line numbers, then Read only those lines
- Batch independent tool calls in parallel
- Suggest /compact after major task completion
EOF
```

## How Claude Code Skills Work

Skills are markdown files in `~/.claude/skills/` that give Claude Code specialized instructions for specific tasks. When you invoke a skill (e.g., `/Token Saver`), Claude reads the `SKILL.md` and follows its instructions.

## Uninstall

```bash
rm -rf ~/.claude/skills/token-saver
```

## License

MIT
