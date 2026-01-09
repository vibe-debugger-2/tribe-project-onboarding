# Tribe Project Onboarding Plugin

**Turn multi-day project ramp-ups into minutes.**

A Claude Code plugin that helps Tribe consultants instantly get up to speed on any project by gathering context from all Tribe data sources.

## The Problem

Tribe consultants frequently rotate between projects (internal and client-facing). Each time, they lose valuable time:
- Hunting for the right Notion pages
- Searching Slack for recent discussions
- Finding out who's on the team and their roles
- Catching up on meeting decisions
- Locating repos, drives, and other resources

## The Solution

Just say: **"I just got assigned to the K&E project"**

The plugin automatically:
- Searches **Notion** for project docs, PRDs, specs, and trackers
- Searches **Slack** for recent discussions and decisions
- Queries **Airtable** for team rosters and task status
- Pulls **meeting transcripts** from Sybill and Granola
- Compiles everything into a scannable onboarding brief

---

## Quick Start (5 minutes if you have API keys ready)

### 1. Install Claude Code
If you don't have Claude Code yet:
```bash
npm install -g @anthropic/claude-code
```

### 2. Install the Plugin
```bash
claude plugin install tribe-project-onboarding@tribe-marketplace
```

### 3. Run the Setup Wizard
```bash
claude
```
Then type:
```
/tribe-project-onboarding:setup
```

The wizard will walk you through connecting each service. Just follow the prompts!

---

## Detailed Setup (First-time Claude Code users)

New to Claude Code? No problem. The setup wizard handles everything.

### What You'll Need
Before starting, gather these (the wizard will tell you where to get them):
- **Notion API key** - From notion.so/my-integrations
- **Slack user token** - From a Slack app (xoxp-...)
- **Airtable token** - From airtable.com/create/tokens
- **Sybill API key** - From Sybill settings (or ask in #help-board)
- **Granola API key** - Optional, from Granola settings

### Run Setup
Start Claude Code and run the setup command:
```
/tribe-project-onboarding:setup
```

The wizard will:
1. ✅ Check you have required tools (Node.js, etc.)
2. ✅ Walk you through getting each API key
3. ✅ Configure each MCP connection for you
4. ✅ Test that everything works

**Takes about 10-15 minutes the first time. After that, it just works!**

---

## Usage

### Slash Command
```
/tribe-project-onboarding:onboard K&E
/tribe-project-onboarding:onboard FIS
/tribe-project-onboarding:onboard "Recruiter Assistant"
```

### Natural Language (Just Talk!)
The plugin automatically activates when you mention projects:
- "I just got assigned to the Datavant project"
- "Catch me up on K&E"
- "What's the status of the FIS engagement?"
- "I need to ramp up on Microsoft E&O"
- "Tell me about the Daily Brief Agent project"

---

## What You Get

A comprehensive onboarding brief including:

| Section | What's Included |
|---------|-----------------|
| **Overview** | What the project is, who it's for |
| **Team** | Names, roles, contact info |
| **Key Links** | Notion hub, Slack channel, GitHub, Google Drive |
| **Must-Read Docs** | Prioritized reading list |
| **Current Status** | Phase, milestones, blockers |
| **Recent Activity** | Last 2 weeks of discussions and meetings |
| **First Steps** | What to do first |

---

## Troubleshooting

### Something not working?
Just ask Claude:
- "Notion MCP not connecting"
- "Help with setup"
- "My Slack token isn't working"

The plugin has a built-in troubleshooting skill that will help diagnose issues.

### Common Fixes

| Problem | Solution |
|---------|----------|
| MCP not connecting | Run `/mcp` and reconnect |
| Can't see Notion pages | Add the integration to pages (••• → Add connections) |
| Slack token invalid | Make sure it's a user token (xoxp-) not bot token (xoxb-) |
| "uv not found" | Install: `curl -LsSf https://astral.sh/uv/install.sh \| sh` |

### Still stuck?
- Post in **#help-board** on Slack
- Tag **@andrew.enns** for plugin issues

---

## Plugin Structure

```
tribe-project-onboarding/
├── .claude-plugin/
│   └── plugin.json              # Plugin manifest
├── commands/
│   ├── onboard.md               # Main onboarding command
│   └── setup.md                 # Interactive setup wizard
├── skills/
│   ├── project-onboarding/
│   │   └── SKILL.md             # Auto-activates on project mentions
│   └── setup-help/
│       └── SKILL.md             # Troubleshooting assistance
└── README.md
```

---

## Data Sources

| Source | What It Provides |
|--------|------------------|
| **Notion** | Project pages, PRDs, specs, meeting notes, trackers |
| **Slack** | Recent discussions, decisions, announcements |
| **Airtable** | Team rosters, task tracking, project data |
| **Sybill** | Meeting transcripts and AI summaries |
| **Granola** | Additional meeting notes and context |

---

## For Developers

### Test Locally
```bash
claude --plugin-dir ./tribe-project-onboarding
```

### Plugin Commands
- `/tribe-project-onboarding:onboard <project>` - Get onboarded
- `/tribe-project-onboarding:setup` - Run setup wizard

### Contributing
1. Clone the repo
2. Make changes
3. Test locally
4. Submit a PR

---

## License

MIT - Built for Tribe AI

---

**Built at the Tribe AI Offsite Hackathon 2026** 🚀

*Turn "I just joined" into "I'm already up to speed"*
