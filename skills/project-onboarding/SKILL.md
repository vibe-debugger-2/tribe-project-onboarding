---
description: Automatically helps Tribe consultants get context on projects. FIRST checks if required MCPs are set up, walks through setup if not, then gathers information from Notion, Slack, Airtable, and meeting transcripts.
triggers:
  - "I'm on the * project"
  - "I just joined *"
  - "I got assigned to *"
  - "I'm starting on *"
  - "catch me up on *"
  - "what's the status of *"
  - "tell me about the * project"
  - "I need to ramp up on *"
  - "onboard me to *"
  - "what's happening with *"
---

# Project Onboarding Skill

You help Tribe consultants get up to speed on projects.

**CRITICAL: Always check MCP setup FIRST before trying to gather project info.**

---

## Step 0: Check MCP Setup (ALWAYS DO THIS FIRST)

Before searching for any project information, verify you have access to the required tools.

### Required MCPs - Check for These Tools:

1. **Notion** - `mcp__notion__notion-search`, `mcp__notion__notion-fetch`
2. **Slack** - `mcp__slack__conversations_search_messages`, `mcp__slack__conversations_history`
3. **Airtable** - `mcp__airtable__list_bases`, `mcp__airtable__search_records`
4. **Tribe/Sybill** - `mcp__tribe__search_transcripts`, `mcp__tribe__pull_transcript`
5. **Granola** (optional) - `mcp__granola__search_meetings`

### If Any Required MCPs Are Missing:

STOP and tell the user:

"Hey! I'd love to help you get up to speed on [project], but I need to connect to a few services first.

**Missing connections:**
[List which ones are missing with ❌]

This is a one-time setup that takes about 10-15 minutes. After that, you'll be able to onboard to any project instantly!

**Want me to walk you through the setup?** I'll guide you step by step."

If they say yes, walk through each missing MCP:

---

### Notion Setup Guide
1. Go to https://www.notion.so/my-integrations
2. Click "New integration" → name it "Claude" → select Tribe workspace
3. Copy the Internal Integration Secret
4. Run this command (replace YOUR_KEY):
   ```
   claude mcp add notion -s user -e NOTION_API_KEY=YOUR_KEY -- npx -y @anthropic/notion-mcp-server
   ```
5. **Important:** Open Notion pages → ••• → Add connections → select your integration

### Slack Setup Guide
1. Check #help-board for a shared Tribe Slack token, OR
2. Create app at https://api.slack.com/apps → OAuth & Permissions
3. Add User Token Scopes: channels:history, channels:read, groups:history, groups:read, im:history, im:read, search:read, users:read
4. Install to workspace, copy User OAuth Token (xoxp-...)
5. Run:
   ```
   claude mcp add slack -s user -e SLACK_MCP_XOXP_TOKEN=YOUR_TOKEN -- npx -y @anthropic/slack-mcp-server
   ```

### Airtable Setup Guide
1. Go to https://airtable.com/create/tokens
2. Create token with scopes: data.records:read, schema.bases:read
3. Give access to all bases
4. Run:
   ```
   claude mcp add airtable -s user -e AIRTABLE_API_KEY=YOUR_TOKEN -- npx -y @anthropic/airtable-mcp-server
   ```

### Tribe/Sybill Setup Guide
1. Get Sybill API key (ask in #help-board or check Sybill settings)
2. Clone and build:
   ```
   cd ~/ && git clone https://github.com/tribe-ai/tribe-mcp.git && cd tribe-mcp && uv sync
   ```
3. Add the MCP:
   ```
   claude mcp add tribe -s user -e SYBILL_API_KEY=YOUR_KEY -e AIRTABLE_API_KEY=YOUR_AIRTABLE_KEY -- uv run --directory ~/tribe-mcp tribe-mcp
   ```

### After All Setup
Tell them: "Great! Run `/mcp` to connect everything, then ask me again about the project!"

---

## Step 1: Gather Project Context (Only If MCPs Ready)

If all MCPs are available, confirm and proceed:

"✅ You're all set up! Let me gather everything about [project]..."

Then search all sources:

**Notion:** Project pages, PRDs, specs, meeting notes, team info, timeline
**Slack:** Recent messages (2 weeks), discussions, decisions, blockers
**Airtable:** Team rosters, task tracking, project records
**Sybill/Tribe:** Meeting transcripts, summaries, action items
**Granola:** Additional meeting notes (if available)

---

## Step 2: Present Onboarding Brief

## 🚀 Project Onboarding: [Project Name]

### 📋 Overview
- **What:** [description]
- **Client:** [who]
- **Phase:** [current phase]
- **Timeline:** [dates]

### 👥 Team
| Role | Name |
|------|------|

### 🔗 Key Links
- Notion Hub: [link]
- Slack Channel: [link]
- GitHub/Drive: [links]

### 📄 Must-Read Docs
1. [doc] - [why]

### 📊 Status
- Done: [what]
- In Progress: [what]
- Upcoming: [what]

### ⚠️ Important Context
[Key things to know]

### 💬 Recent Activity
[Slack & meeting highlights]

### ✅ First Steps
1. [action]
2. [action]

---

## Guidelines

- **ALWAYS check MCPs first** - never skip this
- Be patient with setup - assume users are new to Claude Code
- Run commands for them when possible
- Include clickable links
- Be encouraging when setup completes!
