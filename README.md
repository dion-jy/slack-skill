# Slack Skill

A tiny CLI to read/write a Slack workspace via the Slack Web API — designed to be used as a Claude Code agent skill (context source). Stdlib only, no dependencies.

## Setup

### 1. Create a Slack App

1. https://api.slack.com/apps → **Create New App** → **From scratch**
2. App Name: `Claude Assistant` (anything), Workspace: your workspace
3. Go to **OAuth & Permissions**
4. Add **User Token Scopes** (NOT bot scopes):
   - `channels:history` `channels:read`
   - `groups:history` `groups:read`
   - `im:history` `im:read`
   - `mpim:history` `mpim:read`
   - `users:read` (author names)
   - `search:read` (mention search), `chat:write` (sending) — optional
5. **Install to Workspace** (org admins may require approval)
6. Copy the **User OAuth Token** (starts with `xoxp-`)

### 2. Link

```bash
python3 slack link xoxp-...
```

The token is stored in `.user` (mode 0600), which is gitignored.

## Usage

```bash
python3 slack whoami                       # account info
python3 slack channels                     # joined channels
python3 slack history "#some-channel" --limit 20
python3 slack mentions --days 3            # needs search:read
python3 slack dms                          # DM unread counts (needs users:read)
python3 slack send "#some-channel" "msg"   # needs chat:write
```

The skill degrades gracefully: if a scope is missing (e.g. `users:read`),
reads still work and author names fall back to user IDs.

## Architecture

- **API**: Slack Web API (`slack.com/api/*`)
- **Auth**: User OAuth Token (Bearer header)
- **Storage**: `.user` (mode 0600) — token + cached team/user_id
- **No dependencies**: stdlib only (urllib + json)

## License

MIT.
