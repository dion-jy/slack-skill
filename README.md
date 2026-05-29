# Slack Skill

KAIST MLML Slack workspace를 Claude Code agent가 read/write하는 CLI skill (Slack Web API).

## Setup (3분)

### 1. Slack App 만들기

1. https://api.slack.com/apps → **Create New App** → **From scratch**
2. App Name: `Claude Assistant` (또는 임의), Workspace: **KAIST MLML**
3. **OAuth & Permissions** 페이지로
4. **User Token Scopes** 추가 (bot 아님):
   - `channels:history` `channels:read`
   - `groups:history` `groups:read`
   - `im:history` `im:read`
   - `mpim:history` `mpim:read`
   - `search:read` `users:read` `chat:write`
5. **Install to Workspace** → admin 승인 시 자동, 아니면 admin 요청
6. **User OAuth Token** (xoxp-...로 시작) 복사

### 2. Link
```bash
python3 slack link xoxp-...
```

## Usage

```bash
python3 slack whoami              # 본인 정보
python3 slack channels            # 가입 채널 list
python3 slack mentions [--days N] # @mention 최근 N일
python3 slack dms                 # 모든 DM unread count
python3 slack history #general --limit 20
python3 slack send @mingyu "GRAM amortized EM 노트 정리 중"
python3 slack send #general "공지"
```

## Architecture

- **API**: Slack Web API (`slack.com/api/*`)
- **Auth**: User OAuth Token (Bearer header)
- **Storage**: `.user` (mode 0o600) — `token`, `team`, `user_id`
- **No dependencies**: stdlib only (urllib + json)

## Integration with The System

7번째 context source (sweep cron, wake cron):
- `mentions` — @mention + DM 새 메시지 → Tasks Daily 자동 시드 ("(slack)" marker)
- `dms` — unread count 1줄 보고
- `send` — agent 명령으로 직접 답장 가능

## License

MIT.
