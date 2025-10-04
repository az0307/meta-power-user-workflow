# Meta Power User Tricks & Workflow README

## Table of Contents

- [AI Tab Organization Strategies](#ai-tab-organization-strategies)
- [Calendar-Driven Productivity Systems](#calendar-driven-productivity-systems)
- [agent.md Deep Dive for Agent Guidance and MCP Integration](#agentmd-deep-dive-for-agent-guidance-and-mcp-integration)
- [Best Workflow Examples from n8n, MCP, GitHub Real Use Cases](#best-workflow-examples-from-n8n-mcp-github-real-use-cases)
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
- [n8n Workflows Collection (2000+ workflows)](https://github.com/Zie619/n8n-workflows)
- [MCP Tools Guide - Google ADK](https://google.github.io/adk-docs/tools/mcp-tools/)
- [Comet Browser by Perplexity](https://www.perplexity.ai/comet/)
- [Productivity Systems Framework – T.A.C.O Method](https://lifehackmethod.com/blog/productivity-systems/)

---

This README provides actionable guidelines, examples, and resources to empower power users in AI-driven tab management, productivity through calendar systems, agent documentation and integration, automation workflows via n8n and MCP, and advanced browser automation using Comet. All sections are optimized for direct GitHub usage or quick downloads with Markdown support.
