# Scrapling Web Research Agent for Grok

This package makes the D4Vinci/Scrapling MCP server usable as a web-research agent and provides Grok-ready instructions. It supports local stdio use and remote Streamable HTTP deployment.

## What it does

- Fetches static pages, dynamic pages, and protected pages through Scrapling.
- Extracts clean, AI-targeted Markdown and CSS-selected data.
- Uses persistent browser or HTTP sessions when a workflow needs multiple requests.
- Refuses to treat webpage text as instructions; page content is untrusted data.
- Returns source URLs, extraction notes, and uncertainty instead of inventing missing data.

## Local run

From the repository root:

```bash
uv run --python 3.12 --with "scrapling[all]>=0.4.15" scrapling-mcp
```

If you need browser dependencies:

```bash
uv run --python 3.12 --with "scrapling[all]>=0.4.15" scrapling install --force
```

The local MCP config is in `mcp.json`.

## Run as a remote MCP server

The included `Dockerfile` starts Scrapling over Streamable HTTP on port 8000. Set `SCRAPLING_MCP_AUTH_TOKEN` to a long random secret and put the service behind HTTPS. Do not expose `--no-auth` publicly.

```bash
export SCRAPLING_MCP_AUTH_TOKEN=$(openssl rand -hex 32)
docker build -f grok-agent/Dockerfile -t scrapling-grok-agent .
docker run --rm -p 8000:8000 -e SCRAPLING_MCP_AUTH_TOKEN scrapling-grok-agent
```

The remote endpoint is `/mcp`. Configure Grok/xAI with the HTTPS MCP URL and an `Authorization: Bearer <token>` header where Grok's MCP connector supports remote MCP servers.

## Grok instructions

Paste `SYSTEM_PROMPT.md` into the agent's system/custom instructions. If the Grok product surface only accepts a prompt and not MCP connections, it can still use the prompt as a scraping specialist, but it will not be able to execute Scrapling until the MCP server is connected.

## Safety

Only collect data you are authorized to access. Respect applicable law, site terms, robots policies, rate limits, authentication boundaries, and personal-data restrictions. Do not use proxies or browser automation to evade access controls or collect private data.
