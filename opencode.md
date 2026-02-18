# Prompt Archive Hook for OpenCode

Automatically archive every prompt you send to OpenCode with timestamps.

## Setup

### 1. Create the hooks directory

```bash
mkdir -p ~/.config/opencode/hooks
```

### 2. Create the archive script

Create `~/.config/opencode/hooks/archive-prompt.sh`:

```bash
#!/bin/bash
ARCHIVE_FILE="$OPENCODE_PROJECT_DIR/Prompt_Archive.md"

INPUT=$(cat)
PROMPT=$(echo "$INPUT" | python3 -c "import sys,json; print(json.load(sys.stdin).get('prompt',''))")
SESSION_ID=$(echo "$INPUT" | python3 -c "import sys,json; print(json.load(sys.stdin).get('session_id',''))")
TIMESTAMP=$(date '+%Y-%m-%d %H:%M:%S')

if [ ! -f "$ARCHIVE_FILE" ]; then
  echo "# Prompt Archive" >> "$ARCHIVE_FILE"
  echo "" >> "$ARCHIVE_FILE"
fi

echo "## $TIMESTAMP" >> "$ARCHIVE_FILE"
echo "**Session:** \`$SESSION_ID\`" >> "$ARCHIVE_FILE"
echo "" >> "$ARCHIVE_FILE"
echo "\`\`\`" >> "$ARCHIVE_FILE"
echo "$PROMPT" >> "$ARCHIVE_FILE"
echo "\`\`\`" >> "$ARCHIVE_FILE"
echo "" >> "$ARCHIVE_FILE"
echo "---" >> "$ARCHIVE_FILE"
echo "" >> "$ARCHIVE_FILE"

exit 0
```

### 3. Make the script executable

```bash
chmod +x ~/.config/opencode/hooks/archive-prompt.sh
```

### 4. Configure the hook

Add to `~/.config/opencode/config.json`:

```json
{
  "hooks": {
    "UserPromptSubmit": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "~/.config/opencode/hooks/archive-prompt.sh"
          }
        ]
      }
    ]
  }
}
```

## Note

OpenCode hook support may vary by version. Check the official documentation for the exact configuration format and environment variables available in your version.

If `OPENCODE_PROJECT_DIR` is not available, you can use `$PWD` as fallback:

```bash
ARCHIVE_FILE="${OPENCODE_PROJECT_DIR:-$PWD}/Prompt_Archive.md"
```

## Hook Input Reference

| Field        | Description               |
|--------------|---------------------------|
| `session_id` | Unique session identifier |
| `prompt`     | The user's prompt text    |
| `cwd`        | Current working directory |

## Output Location

Each project gets its own archive:

```
/your/project/path/Prompt_Archive.md
```
