# LinkedIn MCP Server Setup

The LinkedIn MCP (Model Context Protocol) server enables AI assistants like Claude to access your LinkedIn data for automated profile analysis, job searches, and content management.

## 🎯 What This Enables

- **Automated Profile Analysis**: Get your current LinkedIn profile data without manual copying
- **Job Search**: Search and analyze job postings directly
- **Company Research**: Research companies and their posts
- **Content Ideas**: Analyze what top performers in your industry are posting
- **Profile Optimization**: AI can compare your profile against job requirements

## 🚀 Quick Setup (Recommended)

### Prerequisites

- Install [uv](https://docs.astral.sh/uv/): `curl -LsSf https://astral.sh/uv/install.sh | sh`
- Install Playwright: `uvx playwright install chromium`

### Step 1: Create LinkedIn Session

```bash
uvx linkedin-scraper-mcp --get-session
```

This opens a browser window where you:

1. Log into LinkedIn manually
2. Complete any 2FA/captcha challenges (5 minute timeout)
3. Session is automatically saved to `~/.linkedin-mcp/session.json`

⚠️ **Security Note**: The session file contains sensitive authentication data. Keep it secure and never commit it to git.

### Step 2: Configure Claude Desktop

Add to your Claude Desktop config (`~/Library/Application Support/Claude/claude_desktop_config.json` on macOS):

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

### Step 3: Restart Claude Desktop

After restarting, Claude will have access to LinkedIn tools!

### Step 2 (Alternative): Configure VS Code

If you're using VS Code with Copilot, add to your workspace `.vscode/mcp.json`:

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

**For Docker in VS Code**:

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

Then reload VS Code window (Cmd+Shift+P → "Developer: Reload Window").

## 🐳 Alternative: Docker Setup

If you prefer Docker:

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

## 🔧 Available Tools

Once configured, your AI assistant can use these tools:

| Tool                  | Description                                                      | Example Use                             |
| --------------------- | ---------------------------------------------------------------- | --------------------------------------- |
| `get_person_profile`  | Get detailed profile including work history, education, contacts | Analyze your own profile or competitors |
| `get_company_profile` | Extract company info including employees                         | Research potential employers            |
| `get_company_posts`   | Get recent posts from a company                                  | Content inspiration and trends          |
| `search_jobs`         | Search with keywords and location filters                        | Find relevant opportunities             |
| `get_job_details`     | Detailed info about specific job posting                         | Match your profile to requirements      |
| `close_session`       | Clean up browser resources                                       | End automation session                  |

## 📋 Example Workflows

### 1. Profile Analysis

```
Analyze my LinkedIn profile: https://www.linkedin.com/in/YOUR-PROFILE/
Suggest 3 specific improvements based on industry best practices.
```

### 2. Job Matching

```
Get my profile from LinkedIn, then search for "Senior Software Engineer"
jobs in San Francisco. Compare my profile to the top 3 results and
suggest what I should add.
```

### 3. Content Research

```
What has Anthropic been posting about recently?
https://www.linkedin.com/company/anthropic/
Give me 5 content ideas inspired by their approach.
```

### 4. Company Research

```
Research this company for interview prep:
https://www.linkedin.com/company/COMPANY-NAME/
Summarize their recent posts, employee count, and culture indicators.
```

### 5. Competitive Analysis

```
Analyze these 3 profiles of people in similar roles:
[URLs]
What common elements make their profiles stand out?
```

## 🔄 Session Management

Sessions expire periodically. If you encounter authentication issues:

```bash
# Refresh your session
uvx linkedin-scraper-mcp --get-session
```

Then restart Claude Desktop.

## ⚠️ Important Notes

- **Terms of Service**: Use in accordance with LinkedIn's ToS. This is for personal use only.
- **Rate Limiting**: Be mindful of request frequency to avoid triggering LinkedIn's protections.
- **Privacy**: Never share your session file or commit it to version control.
- **Session Location**: `~/.linkedin-mcp/session.json` (already in `.gitignore`)

## 🐞 Troubleshooting

### "Authentication failed"

- Re-run `uvx linkedin-scraper-mcp --get-session`
- Ensure you completed the login process fully
- Check if LinkedIn flagged unusual activity

### "Tool not found" in Claude

- Verify config file syntax (valid JSON)
- Restart Claude Desktop after config changes
- Check `~/.linkedin-mcp/session.json` exists

### "Tool not found" in VS Code

- Verify `.vscode/mcp.json` syntax (valid JSON)
- Reload VS Code window (Cmd+Shift+P → "Developer: Reload Window")
- Check `~/.linkedin-mcp/session.json` exists
- Ensure you're in the correct workspace folder

### Browser automation issues

- Ensure Playwright is installed: `uvx playwright install chromium`
- Try running manually first to verify: `uvx linkedin-scraper-mcp --get-session`

## 📚 Resources

- [LinkedIn MCP Server GitHub](https://github.com/stickerdaniel/linkedin-mcp-server)
- [Model Context Protocol Docs](https://modelcontextprotocol.io/)
- [Claude Desktop](https://claude.ai/download)

---

**Next Steps**: Once configured, try the example workflows in this directory to automate your profile optimization!
