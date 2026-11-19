# WorkQuote MCP server for ChatGPT and Claude

WorkQuote connects to ChatGPT, Claude, Codex and Claude Code through Cotlet.dev, so your AI agent can work with estimates, jobs, invoices and reports in your own WorkQuote account. This repository provides a Python MCP stdio adapter for the hosted Cotlet.dev browser tools.

Unofficial, not affiliated with, endorsed by or sponsored by WorkQuote.

## What can an AI agent do in WorkQuote?

The [live WorkQuote integration page](https://cotlet.dev/integrations/workquote-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=workquote) describes these workflows:

- Read new quote requests and client details.
- Draft estimates with labor, parts, disposal, discounts and tax.
- Read the schedule with addresses and drive times.
- Schedule work from approved estimates or add tasks.
- Complete jobs, review invoices and send them after approval.
- Record payments and read income, payments and estimate reports.

Writes ask for confirmation by default. Review recipients and amounts before approving a send or payment record.

## How do I connect WorkQuote to ChatGPT or Claude?

1. [Get access through Cotlet.dev](https://cotlet.dev/signup?app=workquote&utm_source=github&utm_medium=readme&utm_campaign=workquote).
2. Open `web.workquote.app` in your Cotlet.dev Cloud Browser and sign in yourself.
3. In ChatGPT, add an MCP connector. In Claude, add a custom connector. Use `https://cotlet.dev/mcp/v1/connect` and sign in through Cotlet.dev.
4. Ask your agent to read a quote request or draft an estimate.

## How do I connect Codex or Claude Code?

Create an API key in the Cotlet.dev console and set `COTLET_API_KEY` in your environment.

Codex configuration:

```toml
[mcp_servers.cotlet]
url = "https://cotlet.dev/mcp/v1"
bearer_token_env_var = "COTLET_API_KEY"
```

Claude Code:

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

## How do I run the local MCP adapter?

Use Python 3.10 or newer. The adapter uses the standard library.

```bash
git clone https://github.com/cotlet-connect/workquote-mcp.git
cd workquote-mcp
python3 workquote_agent.py serve
```

For a client that starts local MCP servers, use an absolute path to the downloaded adapter:

```json
{"mcpServers":{"workquote":{"command":"python3","args":["/absolute/path/workquote-mcp/workquote_agent.py","serve"],"env":{"COTLET_API_KEY":"your-key"}}}}
```

The local adapter exposes seven browser helpers: `workquote_guide`, `workquote_status`, `workquote_open`, `workquote_read`, `workquote_click`, `workquote_type` and `workquote_close`. These are browser helpers, distinct from the hosted workflow catalog on the integration page. Call the guide first. Without an API key, listing tools works and tool calls return setup instructions.

```bash
python3 workquote_agent.py connect
python3 workquote_agent.py guide
python3 workquote_agent.py status
```

The adapter forwards calls to `https://cotlet.dev/mcp/v1`; it requires a Cotlet.dev account and a signed-in WorkQuote Cloud Browser. At a sign-in page, sign in yourself. Close the browser session when finished.

## Does WorkQuote have an official API or MCP server?

The Cotlet.dev page's research dated 17 November 2026 found no public WorkQuote API, public developer documentation or official ChatGPT or Claude connector. This repository is the independent Cotlet.dev route. See the live page for the dated findings and sources.

## What should I ask first?

Start with a read, such as "Read my new quote requests" or "Show tomorrow's jobs with addresses and drive times". For an estimate, ask for a draft with line items and review it before approving a send. WorkQuote's plan and send allowances continue to apply.

Source: [WorkQuote on Cotlet.dev](https://cotlet.dev/integrations/workquote-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=workquote). Source page retrieved 19 November 2026.
