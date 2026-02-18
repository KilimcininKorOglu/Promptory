# Promptory

A lightweight hook system that automatically archives every prompt you send to AI coding assistants. Each project maintains its own `Prompt_Archive.md` file, creating a searchable history of your AI interactions.

## Supported Tools

| Tool        | Guide                            |
|-------------|----------------------------------|
| Droid CLI   | [droid-cli.md](droid-cli.md)     |
| Claude Code | [claude-code.md](claude-code.md) |
| OpenCode    | [opencode.md](opencode.md)       |

## How It Works

The `UserPromptSubmit` hook triggers every time you submit a prompt. It receives the prompt content via stdin as JSON and appends it to a `Prompt_Archive.md` file in your project directory.

## Output Example

```markdown
# Prompt Archive

## 2024-02-18 14:30:45
**Session:** `abc123-def456`

```
Fix the authentication bug in login.ts
```

---

## 2024-02-18 14:35:12
**Session:** `abc123-def456`

```
Add unit tests for the auth module
```

---
```

## Git Integration

To track your prompt history in git:

1. Keep `Prompt_Archive.md` tracked (don't add to `.gitignore`)
2. Commit it along with your code changes
3. Your prompt history becomes part of your project's git history

To exclude from git, add to `.gitignore`:

```
Prompt_Archive.md
```

## Requirements

- Python 3 (for JSON parsing)
- AI coding assistant with hooks support

## License

MIT
