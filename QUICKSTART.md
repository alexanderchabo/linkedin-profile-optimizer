# LinkedIn Profile Optimizer - Quick Start Guide

Get your LinkedIn profile optimized in 30 minutes with AI automation!

## 🎯 Choose Your Path

### Path 1: Full Automation (Recommended - 30 min)

Perfect if you want AI to handle profile retrieval and analysis.

**Prerequisites**:

- Claude Desktop installed
- 10 minutes for setup

**Steps**:

1. [Install MCP Server](#step-1-install-mcp-server) (5 min)
2. [Configure Authentication](#step-2-configure-authentication) (5 min)
3. [Run First Analysis](#step-3-run-first-analysis) (10 min)
4. [Review & Implement](#step-4-review--implement) (10 min)

### Path 2: Manual Optimization (1-2 hours)

Perfect if you prefer hands-on control or can't use MCP.

**Steps**:

1. Copy your LinkedIn profile to [current-profile.md](profile/current-profile.md)
2. Work through the [optimization checklist](README.md#-optimization-checklist)
3. Create optimized version in [optimized-profile.md](profile/optimized-profile.md)
4. Update LinkedIn with changes
5. Track metrics in [metrics.md](analytics/metrics.md)

---

## 🤖 Path 1: Full Automation Setup

### Step 1: Install MCP Server

#### Option A: uvx (Recommended)

```bash
# Install uv if you haven't
curl -LsSf https://astral.sh/uv/install.sh | sh

# Install Playwright
uvx playwright install chromium

# Test installation
uvx linkedin-scraper-mcp --help
```

#### Option B: Docker

```bash
# Just need Docker installed
docker pull stickerdaniel/linkedin-mcp-server:latest
```

### Step 2: Configure Authentication

#### Create LinkedIn Session

```bash
uvx linkedin-scraper-mcp --get-session
```

This will:

1. Open a browser window
2. Let you log into LinkedIn manually
3. Save authentication to `~/.linkedin-mcp/session.json`
4. Handle 2FA/captcha (you have 5 minutes)

⚠️ **Important**: Never commit this session file! (Already in `.gitignore`)

#### Configure Claude Desktop

**macOS Location**: `~/Library/Application Support/Claude/claude_desktop_config.json`

**Add this**:

```json
{
  "mcpServers": {
    "linkedin": {
      "command": "uvx",
      "args": ["linkedin-scraper-mcp"]
    }
  }
}
```

**Or for Docker**:

```json
{
  "mcpServers": {
    "linkedin": {
      "command": "docker",
      "args": [
        "run",
        "--rm",
        "-i",
        "-v",
        "~/.linkedin-mcp:/home/pwuser/.linkedin-mcp",
        "stickerdaniel/linkedin-mcp-server:latest"
      ]
    }
  }
}
```

#### Restart Claude Desktop

Close completely and reopen. You should see LinkedIn tools available!

#### Configure VS Code (Alternative)

If you're using VS Code with GitHub Copilot, create `.vscode/mcp.json` in your workspace:

```json
{
  "servers": {
    "linkedin": {
      "type": "stdio",
      "command": "uvx",
      "args": ["linkedin-scraper-mcp"]
    }
  },
  "inputs": []
}
```

**Or for Docker**:

```json
{
  "servers": {
    "linkedin": {
      "type": "stdio",
      "command": "docker",
      "args": [
        "run",
        "--rm",
        "-i",
        "-v",
        "~/.linkedin-mcp:/home/pwuser/.linkedin-mcp",
        "stickerdaniel/linkedin-mcp-server:latest"
      ]
    }
  },
  "inputs": []
}
```

Then reload VS Code (Cmd+Shift+P → "Developer: Reload Window").

### Step 3: Run First Analysis

Open Claude Desktop or GitHub Copilot in VS Code and paste this prompt:

```markdown
I'm using the LinkedIn MCP server to optimize my profile.

Please do the following:

1. Get my LinkedIn profile: https://www.linkedin.com/in/YOUR-PROFILE/

2. Analyze it against best practices:
   - Headline (keywords, value proposition)
   - About section (hook, clarity, CTA)
   - Experience descriptions (results vs tasks)
   - Skills optimization
   - SEO effectiveness

3. Generate a prioritized list of improvements with specific rewrites
   for the top 5 changes.

4. Save the analysis to this repo's analytics folder.
```

**Replace `YOUR-PROFILE` with your LinkedIn username!**

### Step 4: Review & Implement

Claude will provide:

- ✅ Current state analysis
- ✅ Specific improvement recommendations
- ✅ Rewritten content suggestions
- ✅ Priority ranking

**Your action items**:

1. Review suggestions (5 min)
2. Apply top 3 changes to LinkedIn (10 min)
3. Document changes in [analytics/metrics.md](analytics/metrics.md)
4. Schedule next review (see [automation scripts](mcp-setup/automation-scripts.md))

---

## 📅 Ongoing Optimization

### Weekly (5 minutes)

Run the [Weekly Profile Health Check](mcp-setup/automation-scripts.md#script-1-weekly-profile-health-check)

Prompt:

```
Weekly LinkedIn health check for profile: [YOUR URL]
- Completeness score
- Recent activity check
- Quick wins for this week
- One content idea
```

### Monthly (20 minutes)

Run the [Monthly Deep Optimization](mcp-setup/automation-scripts.md#script-2-monthly-deep-optimization)

### Quarterly (1 hour)

Full [performance review](mcp-setup/automation-scripts.md#quarterly-profile-performance-review) with competitive analysis

---

## 🎓 Next Level Automation

Once you're comfortable with basic automation:

### Job Search Automation

- [Automated job matching](mcp-setup/example-prompts.md#-job-search--analysis)
- Gap analysis vs. job requirements
- Application material generation

### Content Strategy

- [Weekly content ideas](mcp-setup/automation-scripts.md#weekly-content-idea-generator)
- Industry trend monitoring
- Competitor content analysis

### Industry Intelligence

- Skills gap monitoring
- Salary trend tracking
- Company research automation

**Explore**: [Example Prompts](mcp-setup/example-prompts.md) | [Automation Scripts](mcp-setup/automation-scripts.md)

---

## 🐞 Troubleshooting

### "LinkedIn MCP tools not found"

**For Claude Desktop**:
- ✅ Check config file is valid JSON
- ✅ Config file in correct location: `~/Library/Application Support/Claude/claude_desktop_config.json`
- ✅ Restart Claude Desktop **completely**
- ✅ Session file exists: `~/.linkedin-mcp/session.json`

**For VS Code**:
- ✅ Check `.vscode/mcp.json` is valid JSON
- ✅ Reload VS Code window (Cmd+Shift+P → "Developer: Reload Window")
- ✅ Ensure you're in the correct workspace
- ✅ Session file exists: `~/.linkedin-mcp/session.json`

### "Authentication failed"

```bash
# Refresh your session
uvx linkedin-scraper-mcp --get-session
```

Then restart Claude Desktop or reload VS Code window.

### "Browser automation issues"

```bash
# Reinstall Playwright
uvx playwright install chromium --force
```

### Still stuck?

- Check [full troubleshooting guide](mcp-setup/README.md#-troubleshooting)
- [Open an issue](https://github.com/alexanderchabo/linkedin-profile-optimizer/issues)
- [LinkedIn MCP Server issues](https://github.com/stickerdaniel/linkedin-mcp-server/issues)

---

## 📊 Track Your Progress

Use these metrics to measure success:

| Metric              | Baseline | Month 1 | Month 2 | Month 3 |
| ------------------- | -------- | ------- | ------- | ------- |
| Profile Views       |          |         |         |         |
| Search Appearances  |          |         |         |         |
| Connection Requests |          |         |         |         |
| Content Engagement  |          |         |         |         |
| Job Opportunities   |          |         |         |         |

Track in [metrics.md](analytics/metrics.md)

---

## 💡 Pro Tips

1. **Set Reminders**:
   - Weekly: Monday 9 AM
   - Monthly: First Monday
   - Quarterly: Last week of quarter

2. **Save Everything**: Keep all AI analysis outputs for historical reference

3. **Implement Gradually**: Don't change everything at once. Test and measure.

4. **Stay Consistent**: Regular small updates > rare big overhauls

5. **Engage Authentically**: Automation is for analysis, not for posting/engaging

---

## ⚠️ Important Reminders

- ✅ MCP is for **read-only analysis** - don't automate posting/engaging
- ✅ Respect LinkedIn's **Terms of Service**
- ✅ Keep session files **secure and private**
- ✅ Use for **personal optimization** only
- ✅ Be mindful of **rate limiting**

---

## 🎉 You're All Set!

**Ready to start?**

1. ⬜ Install MCP Server
2. ⬜ Create session & configure Claude
3. ⬜ Run first profile analysis
4. ⬜ Implement top 3 changes
5. ⬜ Set up weekly automation reminder
6. ⬜ Track metrics baseline

**Questions?** Check the [full documentation](mcp-setup/README.md) or [example prompts](mcp-setup/example-prompts.md).

---

Last updated: February 7, 2026
