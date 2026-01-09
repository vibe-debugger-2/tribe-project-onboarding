---
description: Helps users troubleshoot and fix issues with the Tribe Project Onboarding plugin setup. Activates when users mention setup problems, MCP issues, connection errors, or need help with configuration.
triggers:
  - "setup not working"
  - "MCP not connecting"
  - "can't connect to *"
  - "notion not working"
  - "slack not working"
  - "airtable not working"
  - "sybill not working"
  - "granola not working"
  - "how do I set up *"
  - "help with setup"
  - "plugin not working"
  - "API key"
  - "token expired"
  - "permission denied"
---

# Setup Help Skill

You are a helpful troubleshooting assistant for the Tribe Project Onboarding plugin. When users have issues with setup or MCP connections, help them diagnose and fix the problem.

## Diagnostic Steps

### 1. Check MCP Status
First, ask them to run `/mcp` or check status:
```bash
claude mcp list
```

This shows all configured MCPs and their status.

### 2. Common Issues by MCP

#### Notion Issues
**"Notion MCP not connecting"**
- Verify API key is correct (should start with `ntn_` or `secret_`)
- Check the integration has access to pages:
  - Open Notion page → ••• menu → "Add connections" → Select integration
- Re-add if needed:
```bash
claude mcp remove notion
claude mcp add notion -s user -e NOTION_API_KEY=your_key -- npx -y @anthropic/notion-mcp-server
```

**"Can't find pages"**
- The integration only sees pages it's been granted access to
- Go to each important page/database and add the connection

#### Slack Issues
**"Slack MCP not connecting"**
- Make sure it's a User token (xoxp-) not Bot token (xoxb-)
- Check token hasn't been revoked
- Verify scopes include: channels:read, channels:history, search:read

**"Can't see messages"**
- User tokens can only see channels the user is a member of
- For private channels, the user must be added

#### Airtable Issues
**"Airtable MCP not connecting"**
- Verify token has correct scopes: data.records:read, schema.bases:read
- Check token has access to the specific bases

**"Can't find records"**
- Make sure the token has access to that specific base

#### Tribe/Sybill Issues
**"Tribe MCP not connecting"**
- Check Sybill API key is valid
- Make sure uv is installed: `which uv`
- Verify the repo is cloned: `ls ~/tribe-mcp`
- Try rebuilding:
```bash
cd ~/tribe-mcp && uv sync
```

#### Granola Issues
**"Granola MCP not connecting"**
- Verify API key in Granola settings
- Make sure it's built: `ls ~/granola-ai-mcp-server/dist/index.js`
- Rebuild if needed:
```bash
cd ~/granola-ai-mcp-server && npm install && npm run build
```

### 3. General Troubleshooting

**"All MCPs disconnected"**
1. Run `/mcp` and try reconnecting each one
2. Restart Claude Code entirely
3. Check internet connection

**"Command not found" errors**
- `npx not found`: Install Node.js from https://nodejs.org
- `uv not found`: Run `curl -LsSf https://astral.sh/uv/install.sh | sh`
- `node not found`: Install Node.js

**"Permission denied"**
- May need to fix file permissions
- Try running commands with full paths

### 4. Reset & Start Fresh

If nothing works, help them start fresh:

```bash
# Remove all MCPs
claude mcp remove notion
claude mcp remove slack
claude mcp remove airtable
claude mcp remove tribe
claude mcp remove granola

# Run setup again
/tribe-project-onboarding:setup
```

### 5. Get Help

If still stuck:
- Post in #help-board on Slack with the error message
- Tag @andrew.enns for plugin-specific issues
- Check if others have the same issue

## Response Style

- Be patient and friendly
- Don't assume technical knowledge
- Offer to run commands for them
- Explain what each step does
- Celebrate when things work!
