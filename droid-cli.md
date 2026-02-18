# Prompt Archive Hook for Droid CLI

Automatically archive every prompt you send to Droid CLI with timestamps.

## Setup

### 1. Create the hooks directory

```bash
mkdir -p ~/.factory/hooks
```

### 2. Create the archive script

Create `~/.factory/hooks/archive-prompt.sh`:

```bash
#!/bin/bash
ARCHIVE_FILE="$FACTORY_PROJECT_DIR/Prompt_Archive.md"

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
chmod +x ~/.factory/hooks/archive-prompt.sh
```

### 4. Configure the hook

Add to `~/.factory/settings.json`:

```json
{
  "hooks": {
    "UserPromptSubmit": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "~/.factory/hooks/archive-prompt.sh"
          }
        ]
      }
    ]
  }
}
```

## Hook Input Reference

| Field             | Description                |
|-------------------|----------------------------|
| `session_id`      | Unique session identifier  |
| `prompt`          | The user's prompt text     |
| `cwd`             | Current working directory  |
| `transcript_path` | Path to conversation JSON  |

## Environment Variables

| Variable              | Description                        |
|-----------------------|------------------------------------|
| `FACTORY_PROJECT_DIR` | Project root where Droid started   |

## Output Location

Each project gets its own archive:

```
/your/project/path/Prompt_Archive.md
```
