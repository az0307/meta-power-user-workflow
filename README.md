# Meta Power User Tricks & Workflow README

## Table of Contents

- [AI Tab Organization Strategies](#ai-tab-organization-strategies)
- [Calendar-Driven Productivity Systems](#calendar-driven-productivity-systems)
- [agent.md Deep Dive for Agent Guidance and MCP Integration](#agentmd-deep-dive-for-agent-guidance-and-mcp-integration)
- [Best Workflow Examples from n8n, MCP, GitHub Real Use Cases](#best-workflow-examples-from-n8n-mcp-github-real-use-cases)
- [Composio Workflow Examples for AI Agent Automation](#composio-workflow-examples-for-ai-agent-automation)
- [Meta Tips for Power Automation Including Comet Browser Workflows](#meta-tips-for-power-automation-including-comet-browser-workflows)
- [Actionable Next-Action Prompts for Fast Re-Use](#actionable-next-action-prompts-for-fast-re-use)
- [Links and Resources](#links-and-resources)

## AI Tab Organization Strategies

Managing numerous browser tabs efficiently can significantly boost productivity. Modern AI-powered tab organization leverages content analysis and automated grouping:

- **AI Categorization**: Use AI to analyze tab content and URLs for topic-based grouping (e.g., work, entertainment, reference) automatically.
- **Browser Extensions**: Custom AI-powered extensions can declutter tabs with one-click grouping and categorize tabs dynamically for quick access.
- **Practical Strategy**: Combine manual pinning with AI groups to prioritize tabs that require immediate attention.

**Example Concept (Pseudocode):**

```javascript
// Pseudocode - AI tab grouping function

function categorizeTabs(tabs) {
  const categories = {};
  tabs.forEach(tab => {
    const category = aiAnalyzeTabContent(tab.title, tab.url);
    if (!categories[category]) categories[category] = [];
    categories[category].push(tab);
  });
  return categories;
}
```

This approach reduces cognitive overload and streamlines workflows.

## Calendar-Driven Productivity Systems

Using calendars as the backbone of productivity helps in time-blocking, prioritizing tasks, and creating focus zones.

- **Time Blocking**: Allocate specific blocks for focused work, meetings, and breaks.
- **Goal Alignment**: Integrate daily and weekly goals into calendar events.
- **T.A.C.O Framework**: Terminate, Automate, Consolidate, Outsource tasks to increase efficiency.
- **Tools**: Google Calendar or Outlook Calendar with integration for reminders and notifications.
- **AI Assistance**: Use AI to generate daily agenda emails and prioritized task lists based on calendar events.

## agent.md Deep Dive for Agent Guidance and MCP Integration

AGENTS.md is a specialized README for AI coding agents, designed to give explicit, machine-readable instructions, avoiding ambiguity in automation tasks.

**Core Concepts of AGENTS.md:**

- Machine-friendly instructions replacing vague human language.
- Document environment setup, testing steps, pull request guidelines.
- Collaboration-friendly for multiple agents to prevent conflicts.

**MCP (Model Context Protocol) Integration:**

- MCP provides a protocol to connect AI agents with external tools and services.
- MCP tools can be integrated into agents using MCP clients.
- Example to connect to GitHub MCP server:

```csharp
await using var mcpClient = await McpClientFactory.CreateAsync(new StdioClientTransport(new()
{
    Name = "MCPServer",
    Command = "npx",
    Arguments = ["-y", "--verbose", "@modelcontextprotocol/server-github"],
}));
```

This setup enables agents to perform automated repository tasks like issue management, branch creation, and more.

## Best Workflow Examples from n8n, MCP, GitHub Real Use Cases

**n8n Workflow Highlights:**

- Automate GitHub issues creation on specific triggers.
- Sync GitHub data with project management tools.
- Scheduled tasks for repository status monitoring.

**Example automation to create a GitHub issue via n8n:**

```json
{
  "nodes": [
    {
      "parameters": {
        "repository": "example-repo",
        "title": "Automated Issue",
        "body": "This issue was created by an automated workflow."
      },
      "name": "Create GitHub Issue",
      "type": "n8n-nodes-base.github",
      "typeVersion": 1,
      "position": [500, 300]
    }
  ]
}
```

**MCP with GitHub Example:**

- Use MCP servers for natural language querying of repos.
- Automate pull request creation and code review summaries.

**GitHub MCP Server Repo:**

- https://github.com/github/github-mcp-server

## Composio Workflow Examples for AI Agent Automation

**What is Composio?**

Composio is a platform that provides AI agents with access to 100+ tools and APIs through a unified interface. It enables seamless integration between AI assistants (Claude, GPT-4, etc.) and external services like GitHub, Slack, Gmail, Google Calendar, Notion, and more.

**Key Benefits:**

- Pre-built integrations for popular tools (no custom API coding needed)
- Unified authentication and permission management
- Actions and triggers for complete workflow automation
- Works with all major AI frameworks (LangChain, CrewAI, AutoGPT, etc.)

**Installation:**

```bash
pip install composio-core
composio login
composio add github  # Add and authenticate with GitHub
composio add gmail   # Add and authenticate with Gmail
composio add slack   # Add and authenticate with Slack
```

### Workflow Example 1: Automated GitHub Issue Triage

This workflow automatically triages incoming GitHub issues, labels them, and notifies the team via Slack.

```python
from composio import ComposioToolSet, App, Action

# Initialize Composio
toolset = ComposioToolSet()

# Get GitHub and Slack tools
github_tools = toolset.get_tools(apps=[App.GITHUB])
slack_tools = toolset.get_tools(apps=[App.SLACK])

# Workflow: Monitor new GitHub issues
def triage_github_issue(issue_data):
    # Use AI to analyze issue content
    analysis = ai_agent.analyze(issue_data['body'])

    # Add labels based on AI analysis
    toolset.execute_action(
        action=Action.GITHUB_ADD_LABEL,
        params={
            "owner": "your-org",
            "repo": "your-repo",
            "issue_number": issue_data['number'],
            "labels": analysis['suggested_labels']  # e.g., ["bug", "high-priority"]
        }
    )

    # Notify team in Slack
    toolset.execute_action(
        action=Action.SLACK_SEND_MESSAGE,
        params={
            "channel": "#engineering",
            "text": f"🚨 New issue triaged: {issue_data['title']}\nPriority: {analysis['priority']}\nLink: {issue_data['html_url']}"
        }
    )

# Trigger on new issue webhook
# composio trigger github issue_opened --callback triage_github_issue
```

### Workflow Example 2: AI-Powered Email to Task Automation

Automatically convert important emails into calendar events and GitHub issues.

```python
from composio import ComposioToolSet, App, Action
from datetime import datetime, timedelta

toolset = ComposioToolSet()

def process_important_email(email):
    # AI extracts action items from email
    action_items = ai_agent.extract_tasks(email['body'])

    for item in action_items:
        if item['type'] == 'meeting':
            # Create calendar event
            toolset.execute_action(
                action=Action.GOOGLECALENDAR_CREATE_EVENT,
                params={
                    "summary": item['title'],
                    "description": f"From email: {email['subject']}",
                    "start_time": item['suggested_time'],
                    "duration": 30,  # 30 minutes
                    "attendees": item['participants']
                }
            )

        elif item['type'] == 'task':
            # Create GitHub issue
            toolset.execute_action(
                action=Action.GITHUB_CREATE_ISSUE,
                params={
                    "owner": "your-org",
                    "repo": "tasks",
                    "title": item['title'],
                    "body": f"**Source:** {email['subject']}\n\n{item['description']}",
                    "labels": ["email-generated"]
                }
            )

    # Archive the processed email
    toolset.execute_action(
        action=Action.GMAIL_MODIFY_MESSAGE,
        params={
            "message_id": email['id'],
            "add_labels": ["processed"],
            "remove_labels": ["INBOX"]
        }
    )

# Monitor Gmail with filter
# composio trigger gmail new_email --filter "is:important" --callback process_important_email
```

### Workflow Example 3: Daily Standup Automation

Automatically generate and post daily standup reports by aggregating data from multiple sources.

```python
from composio import ComposioToolSet, App, Action
from datetime import datetime, timedelta

toolset = ComposioToolSet()

def generate_daily_standup():
    yesterday = (datetime.now() - timedelta(days=1)).strftime("%Y-%m-%d")

    # Get GitHub activity
    commits = toolset.execute_action(
        action=Action.GITHUB_LIST_COMMITS,
        params={
            "owner": "your-org",
            "repo": "your-repo",
            "since": yesterday
        }
    )

    # Get completed calendar events
    events = toolset.execute_action(
        action=Action.GOOGLECALENDAR_LIST_EVENTS,
        params={
            "time_min": yesterday,
            "time_max": datetime.now().strftime("%Y-%m-%d")
        }
    )

    # Get closed GitHub issues
    closed_issues = toolset.execute_action(
        action=Action.GITHUB_LIST_ISSUES,
        params={
            "owner": "your-org",
            "repo": "your-repo",
            "state": "closed",
            "since": yesterday
        }
    )

    # AI generates summary
    standup_report = ai_agent.generate_standup({
        "commits": commits,
        "meetings": events,
        "completed_issues": closed_issues
    })

    # Post to Slack
    toolset.execute_action(
        action=Action.SLACK_SEND_MESSAGE,
        params={
            "channel": "#daily-standup",
            "text": f"📊 *Daily Standup Report - {datetime.now().strftime('%Y-%m-%d')}*\n\n{standup_report}"
        }
    )

# Schedule daily at 9 AM
# composio schedule daily --time "09:00" --callback generate_daily_standup
```

### Workflow Example 4: Smart Meeting Notes with Follow-ups

Automatically create meeting notes and generate follow-up tasks.

```python
from composio import ComposioToolSet, App, Action

toolset = ComposioToolSet()

def process_meeting_end(event_data):
    meeting_title = event_data['summary']

    # AI generates meeting notes (assuming you have transcript/recording)
    notes = ai_agent.generate_meeting_notes(event_data)

    # Create Notion page for meeting notes
    notion_page = toolset.execute_action(
        action=Action.NOTION_CREATE_PAGE,
        params={
            "parent_database_id": "your-meetings-db-id",
            "properties": {
                "Title": meeting_title,
                "Date": event_data['start_time'],
                "Attendees": event_data['attendees']
            },
            "content": notes['formatted_notes']
        }
    )

    # Create GitHub issues for action items
    for action_item in notes['action_items']:
        toolset.execute_action(
            action=Action.GITHUB_CREATE_ISSUE,
            params={
                "owner": "your-org",
                "repo": "tasks",
                "title": action_item['title'],
                "body": f"**From Meeting:** {meeting_title}\n**Assigned:** {action_item['assignee']}\n**Due:** {action_item['due_date']}\n\n{action_item['description']}",
                "assignees": [action_item['assignee']]
            }
        )

    # Send summary email
    toolset.execute_action(
        action=Action.GMAIL_SEND_EMAIL,
        params={
            "to": event_data['attendees'],
            "subject": f"Meeting Notes: {meeting_title}",
            "body": f"Meeting notes: {notion_page['url']}\n\nAction items created in GitHub."
        }
    )

# Trigger when calendar event ends
# composio trigger googlecalendar event_ended --callback process_meeting_end
```

### Integration with AI Agents

**Using Composio with Claude via LangChain:**

```python
from langchain.agents import initialize_agent, AgentType
from langchain_anthropic import ChatAnthropic
from composio_langchain import ComposioToolSet, App

# Initialize Claude
llm = ChatAnthropic(model="claude-sonnet-4-5-20250929")

# Initialize Composio toolset with specific apps
toolset = ComposioToolSet()
tools = toolset.get_tools(apps=[App.GITHUB, App.SLACK, App.GMAIL])

# Create agent with tools
agent = initialize_agent(
    tools=tools,
    llm=llm,
    agent=AgentType.OPENAI_FUNCTIONS,
    verbose=True
)

# Natural language task execution
result = agent.run(
    "Check my GitHub notifications, create issues for any bug reports, "
    "and send me a summary in Slack"
)
```

### Best Practices for Composio Workflows

1. **Authentication**: Use OAuth for secure, user-specific integrations
2. **Error Handling**: Always wrap API calls in try-catch for robust workflows
3. **Rate Limiting**: Be mindful of API limits for each connected service
4. **Triggers vs Polling**: Use webhooks (triggers) instead of polling when available
5. **Logging**: Log all workflow executions for debugging and audit trails
6. **Permissions**: Use least-privilege principle - only request necessary scopes

### Composio CLI Power Commands

```bash
# List available apps
composio apps

# List actions for an app
composio actions github

# Test an action
composio action-test github star_repo --params '{"owner":"composio", "repo":"composio"}'

# View connected accounts
composio accounts

# Setup webhook triggers
composio trigger github issue_opened --callback "python triage.py"

# Export workflow configuration
composio export-workflow > workflow.json
```

### Composio vs Other Automation Tools

| Feature | Composio | n8n | MCP | Zapier |
|---------|----------|-----|-----|--------|
| **AI Agent Native** | ✅ Yes | ⚠️ Limited | ✅ Yes | ❌ No |
| **Open Source** | ✅ Yes | ✅ Yes | ✅ Yes | ❌ No |
| **Code-First** | ✅ Python/JS | ⚠️ GUI + Code | ✅ Protocol | ❌ GUI Only |
| **# of Integrations** | 100+ | 400+ | Growing | 5000+ |
| **Self-Hostable** | ✅ Yes | ✅ Yes | ✅ Yes | ❌ No |
| **AI Framework Support** | LangChain, CrewAI, etc. | Custom | Claude, etc. | None |
| **Best For** | AI agent automation | Visual workflows | Claude integration | Non-technical users |

**When to use Composio:**
- Building AI agents that need to interact with external tools
- Python/TypeScript-first development workflows
- Need granular control over authentication and permissions
- Want to use LangChain, CrewAI, or similar frameworks

**When to combine tools:**
- Use **Composio** for AI agent tool access + **n8n** for complex business logic workflows
- Use **MCP** for Claude-specific integrations + **Composio** for broader tool ecosystem
- Use **Composio** for backend automation + **Comet Browser** for frontend web automation

## Meta Tips for Power Automation Including Comet Browser Workflows

**Comet Browser Workflows:**

- AI-powered browser that automates web tasks from simple text or voice commands.
- Automates routine browsing actions like logging in, sorting data, and filling forms.
- Integrate Comet with other automation tools to offload repetitive manual work.

**Example usage:**

- "Log into [website] and sort items by price" triggers a fully automated session handling clicks and inputs.

**Power Automation Tips:**

- Combine browser AI automation (Comet) with backend workflows (n8n, MCP).
- Use voice commands to quickly initiate complex automation.
- Leverage connector integrations to bridge data between apps.

## Actionable Next-Action Prompts for Fast Re-Use

- "Create a GitHub issue with title 'Bug Report' and include logs attached."
- "Schedule a 30-minute focus block in Google Calendar tomorrow at 9 AM."
- "Organize all tabs into categories: Work, Reference, Social."
- "Run tests on project X and report failures via Slack."
- "Trigger Comet Browser to automate login and data extraction on site Y."

Reuse these prompts in AI agents or automation scripts to speed up workflow execution.

## Links and Resources

- [AGENTS.md Specification and Examples](https://github.com/openai/agents.md)
- [GitHub MCP Server Repository](https://github.com/github/github-mcp-server)
- [Composio Platform - AI Agent Tool Integration](https://composio.dev/)
- [Composio Documentation](https://docs.composio.dev/)
- [Composio GitHub Repository](https://github.com/ComposioHQ/composio)
- [n8n Workflows Collection (2000+ workflows)](https://github.com/Zie619/n8n-workflows)
- [MCP Tools Guide - Google ADK](https://google.github.io/adk-docs/tools/mcp-tools/)
- [Comet Browser by Perplexity](https://www.perplexity.ai/comet/)
- [Productivity Systems Framework – T.A.C.O Method](https://lifehackmethod.com/blog/productivity-systems/)

---

This README provides actionable guidelines, examples, and resources to empower power users in AI-driven tab management, productivity through calendar systems, agent documentation and integration, automation workflows via n8n and MCP, and advanced browser automation using Comet. All sections are optimized for direct GitHub usage or quick downloads with Markdown support.
