# Token Saver

Reduce Claude Code token usage without altering functionality. This skill enforces efficient patterns, suggests model switching, and monitors context growth.

## When to activate
- ALWAYS active — these rules apply to every conversation
- Invokable via `/token-saver` for a mid-conversation efficiency check

## Core Rules (ALWAYS FOLLOW)

### 1. Targeted Reads — NEVER read full files blindly
- ALWAYS use `offset` + `limit` when you know which section you need
- Use `Grep` FIRST to find the relevant line numbers, THEN read only those lines
- For files >200 lines: read the first 30 lines to understand structure, then target specific sections
- BAD: `Read file.py` (reads 2000 lines, burns tokens)
- GOOD: `Grep "def process_data" file.py` → `Read file.py offset=145 limit=30`

### 2. Model-Appropriate Work
Suggest model switches to the user when appropriate:
- **Haiku** (`/model haiku`): grep, file lookups, formatting, simple Q&A, running commands, generating boilerplate, SQL queries, status checks
- **Sonnet** (`/model sonnet`): coding tasks, bug fixes, code review, moderate analysis, writing tests
- **Opus** (`/model opus`): complex architecture, multi-file refactors, deep debugging, planning, nuanced analysis

When the task is simple (lookup, grep, format), say: *"This is a quick lookup — consider `/model haiku` to save tokens."*

### 3. Concise Responses
- Lead with the answer, not the reasoning
- Max 3-5 sentences for simple questions
- No preamble ("Let me...", "I'll now...", "Sure, I can...")
- No trailing summaries ("In summary...", "To recap...")
- Code blocks: only show the changed portion, not the entire file
- Tables: only when genuinely clearer than prose

### 4. Smart Tool Usage
- Use `Glob` not `find` (dedicated tool = fewer tokens in output)
- Use `Grep` not `cat file | grep` (dedicated tool)
- Use `Edit` not `Write` for changes (sends diff, not full file)
- Batch independent tool calls in a single message (parallel execution)
- NEVER spawn an Agent for something that takes 1-2 tool calls
- Agents are expensive — each gets its own context. Only use for genuinely parallel, complex, multi-step work

### 5. Context Management
- After 15+ back-and-forth messages, suggest `/compact` to the user
- After completing a major task, suggest `/compact` before starting the next one
- When conversation has accumulated many large tool outputs, suggest `/compact`
- Phrase: *"Context is getting large — `/compact` would save tokens for the rest of this session."*

### 6. Avoid Waste
- Don't re-read files you already read in this conversation (use your memory of the content)
- Don't re-run commands that already succeeded unless state changed
- Don't explore code you don't need for the current task
- Don't add unsolicited improvements, refactors, or documentation
- If a task is done, stop. Don't offer "would you also like me to..."

## When invoked via `/token-saver`

Run a mid-conversation efficiency check and report:

```
## Token Efficiency Check

### Current Session
- Messages exchanged: ~{count}
- Large file reads: {list any full-file reads that could have been targeted}
- Agent spawns: {count} — were they all necessary?
- Model: {current} — appropriate for current work?

### Recommendations
- [ ] Run /compact (context is large)
- [ ] Switch to /model haiku (current task is simple)
- [ ] Next reads: use offset+limit, not full file
```

## CLAUDE.md Integration

When this skill is first loaded, check if `~/.claude/CLAUDE.md` or the project CLAUDE.md contains token efficiency rules. If not, suggest adding:

```markdown
# Efficiency
- Prefer targeted file reads (offset+limit) over full reads
- Use Grep before Read to find line numbers
- Batch independent tool calls in parallel
- Suggest /compact after major task completion
- Keep responses concise — answer first, explain only if asked
```
