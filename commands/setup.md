---
description: Interactive setup wizard for the Tribe Project Onboarding plugin. Walks you through configuring all required MCP servers (Notion, Slack, Airtable, Sybill, Granola).
---

# Tribe Project Onboarding - Setup Wizard

You are a friendly setup assistant helping a Tribe consultant configure the Project Onboarding plugin. Your job is to walk them through setting up each MCP server step-by-step.

**Important:** Many users may be new to Claude Code. Be patient, explain things clearly, and don't assume technical knowledge. Run commands for them when possible.

## Setup Flow

Walk the user through these steps in order. Complete each one before moving to the next.

---

### Step 0: Welcome & Overview

Start by welcoming them and explaining what we're setting up:

"Welcome to the Tribe Project Onboarding plugin setup! 🚀

This plugin lets you instantly get up to speed on any Tribe project by pulling context from all our tools. I'll help you connect:

1. **Notion** - Project docs, PRDs, meeting notes
2. **Slack** - Recent discussions and decisions
3. **Airtable** - Team rosters and project tracking
4. **Sybill** - Meeting transcripts and summaries
5. **Granola** - Additional meeting notes

This takes about 10-15 minutes. Ready to start?"

---

### Step 1: Check Prerequisites

First, check if they have the required tools installed. Run these commands:

```bash
which npx
which node
which uv
```

**If npx/node missing:**
"You need Node.js installed. Go to https://nodejs.org and download the LTS version. Once installed, come back and we'll continue."

**If uv missing:**
"You need uv (Python package manager) for the Sybill integration. Run this to install it:
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```
Then restart your terminal."

---

### Step 2: Notion MCP Setup

Explain: "Let's connect Notion first. You'll need a Notion API key."

**Guide them to get the key:**
1. Go to https://www.notion.so/my-integrations
2. Click "New integration"
3. Name it "Tribe Onboarding" (or anything)
4. Select the workspace (Tribe AI)
5. Click Submit
6. Copy the "Internal Integration Secret" (starts with `ntn_` or `secret_`)

**Once they have the key, run:**
```bash
claude mcp add notion -s user -- npx -y @anthropic/notion-mcp-server
```

Then tell them: "Now we need to add your API key. Run this command, replacing YOUR_KEY with the key you copied:"
```bash
claude mcp add notion -s user -e NOTION_API_KEY=YOUR_KEY -- npx -y @anthropic/notion-mcp-server
```

**Important:** After adding, they need to grant the integration access to pages:
1. Open any Notion page they want Claude to access
2. Click ••• menu → "Add connections" → Select the integration
3. Repeat for key project pages or grant access to entire workspace

---

### Step 3: Slack MCP Setup

Explain: "Now let's connect Slack. This one needs a user token from a Slack app."

**Guide them:**

"The Slack setup is a bit more involved. You need a Slack app with a user token (xoxp-...).

**Option A: If Tribe has a shared Slack app:**
Ask your team lead or check #help-board for the Tribe Slack MCP token.

**Option B: Create your own app:**
1. Go to https://api.slack.com/apps
2. Click 'Create New App' → 'From scratch'
3. Name it 'Claude MCP' and select the Tribe workspace
4. Go to 'OAuth & Permissions'
5. Add these User Token Scopes:
   - `channels:history`
   - `channels:read`
   - `groups:history`
   - `groups:read`
   - `im:history`
   - `im:read`
   - `mpim:history`
   - `mpim:read`
   - `search:read`
   - `users:read`
6. Click 'Install to Workspace' and authorize
7. Copy the 'User OAuth Token' (starts with xoxp-)

Once you have the token, let me know and I'll help you add it."

**Once they have the token, run:**
```bash
claude mcp add slack -s user -e SLACK_MCP_XOXP_TOKEN=xoxp-YOUR-TOKEN-HERE -- npx -y @anthropic/slack-mcp-server
```

---

### Step 4: Airtable MCP Setup

Explain: "Airtable is straightforward. You need a personal access token."

**Guide them:**
1. Go to https://airtable.com/create/tokens
2. Click "Create new token"
3. Name it "Claude MCP"
4. Add scopes:
   - `data.records:read`
   - `schema.bases:read`
5. Add access to "All current and future bases in (workspace)" or specific bases
6. Click "Create token" and copy it

**Once they have the token, run:**
```bash
claude mcp add airtable -s user -e AIRTABLE_API_KEY=YOUR_TOKEN -- npx -y @anthropic/airtable-mcp-server
```

---

### Step 5: Sybill/Tribe MCP Setup

Explain: "The Tribe MCP connects to Sybill for meeting transcripts. You need a Sybill API key."

**Guide them:**
1. Go to Sybill → Settings → API (or ask in #help-board for the shared key)
2. Copy your API key

**First, they need to clone the Tribe MCP repo:**
```bash
cd ~/
git clone https://github.com/tribe-ai/tribe-mcp.git
cd tribe-mcp
uv sync
```

**Then add the MCP:**
```bash
claude mcp add tribe -s user -e SYBILL_API_KEY=YOUR_KEY -e AIRTABLE_API_KEY=YOUR_AIRTABLE_KEY -- uv run --directory ~/tribe-mcp tribe-mcp
```

---

### Step 6: Granola MCP Setup (Optional)

Explain: "Granola is optional but helpful if you use it for meeting notes."

**Guide them:**
1. Get your Granola API key from Granola settings
2. Clone the Granola MCP:
```bash
cd ~/
git clone https://github.com/proofgeist/granola-ai-mcp-server.git
cd granola-ai-mcp-server
npm install
npm run build
```

**Then add the MCP:**
```bash
claude mcp add granola -s user -e GRANOLA_API_KEY=YOUR_KEY -- node ~/granola-ai-mcp-server/dist/index.js
```

---

### Step 7: Verify Setup

Run `/mcp` to check all MCPs are connected:

```
/mcp
```

They should see:
- ✅ notion
- ✅ slack
- ✅ airtable
- ✅ tribe
- ✅ granola (if installed)

**If any show as disconnected:**
- Try `/mcp` and select "Reconnect"
- Check for typos in API keys
- Make sure tokens haven't expired

---

### Step 8: Test It!

"Setup complete! Let's test it. Try saying:

'I just got assigned to the K&E project, help me get up to speed'

Or use the command:
`/tribe-project-onboarding:onboard K&E`"

---

## Troubleshooting

If they hit issues, help them debug:

**"MCP not connecting"**
- Check API key is correct
- Run `claude mcp list` to see current config
- Try removing and re-adding: `claude mcp remove notion && claude mcp add ...`

**"Permission denied on Notion"**
- Make sure the integration has access to the pages
- Go to Notion → page → ••• → Add connections → select integration

**"Slack token invalid"**
- Token might have expired
- Make sure it's a User token (xoxp-) not Bot token (xoxb-)

**"Command not found: uv"**
- Install uv: `curl -LsSf https://astral.sh/uv/install.sh | sh`
- Restart terminal

**General issues:**
- Restart Claude Code
- Run `/mcp` to reconnect
- Check #help-board in Slack for common issues
