# Bevia MCP

**Query your Bevia map from any MCP-capable AI client.**

[Bevia](https://bevia.co) observes continuity across the surfaces where
your cognition lives — Claude, ChatGPT, Cursor, Windsurf, Obsidian,
GitHub, Slack, and more — and compiles it into a navigable map of your
thinking: territories, landmarks, contributors, and how they evolve
over time.

This MCP server is the **Reasoning surface** of that map. Connect it to
your AI client and the client can answer questions like:

- *What have I been working on lately?* (`get_briefing`, recent moments)
- *Which territories grew this week? What's new on the map?*
- *Where did my notes contradict each other? What connected unexpectedly?*
- *What do I know about X?* (recall across everything Bevia has seen)
- *What's the working context I should inherit for this session?*
  (`get_continuity_preamble`)

Works symmetrically with Claude (Desktop, Code, claude.ai), ChatGPT,
Cursor, Windsurf, Codex, and any other MCP-capable client.

## About this repository

The Bevia MCP server is **remote** — it runs as part of the hosted
Bevia service. This repository is its public home for MCP registries:
the [`server.json`](./server.json) manifest plus these connection
instructions. There is no server code to build or run here.

For clients that prefer a local stdio server, the thin open-source
proxy [`@neuroverse/bevia-mcp-server`](https://www.npmjs.com/package/@neuroverse/bevia-mcp-server)
(MIT) forwards JSON-RPC verbatim to the remote endpoint with your token
attached.

## Connect

Two facts, any client:

- **MCP URL:** `https://bevia.co/api/mcp` (Streamable HTTP)
- **Auth:** a Bevia token — generate one at
  [bevia.co/connect/ai](https://bevia.co/connect/ai). The token is
  shown once at creation; the database stores only its hash.

### Remote (recommended)

Clients with native remote-MCP support point straight at the URL:

- **Claude Desktop / claude.ai** — Settings → Connectors → Add custom
  connector → paste `https://bevia.co/api/mcp`. Browser clients
  authenticate via OAuth automatically; desktop clients can use the
  Bearer token.
- **ChatGPT** — Settings → Connectors → Create → paste the MCP URL and
  authenticate with your token.
- **Cursor and other JSON-config clients:**

```json
{
  "mcpServers": {
    "bevia": {
      "url": "https://bevia.co/api/mcp",
      "headers": { "Authorization": "Bearer YOUR_BEVIA_TOKEN" }
    }
  }
}
```

### Local stdio proxy

Anywhere only stdio is supported:

```json
{
  "mcpServers": {
    "bevia": {
      "command": "npx",
      "args": ["@neuroverse/bevia-mcp-server"],
      "env": { "BEVIA_TOKEN": "YOUR_BEVIA_TOKEN" }
    }
  }
}
```

The proxy keeps no state, caches nothing, and logs nothing beyond fatal
errors. New Bevia tools appear automatically — the remote endpoint owns
the tool list.

## What the tools expose

Reads over your own map: cartographic queries (territories grown, new
territories, recent moments and concepts, recall, contradictions,
notable connections, territory detail, contributor activity, recurring
landmarks, emerged routes), synthesis reads (briefing, continuity
preamble, entity dossiers, intelligence dynamics, cross-AI relays), and
structural lists (projects, team entities).

Access is scope-gated per token. Read scopes (`structural`,
`operational`, `interpretive`) control how much behavioral
characterization a token can pull. The one write-shaped tool,
`ingest_session` (an agent contributing a transcript *into* your map),
requires the `ingest` scope — **off by default** for every token, and
every ingest call is logged to a transparency log you can audit.

Your map stays yours: everything the tools read is also exportable, for
free, at any time, from your Bevia account.

## Cost

Connecting is free. Tool calls are metered against your Bevia plan
budget — most reads are well under a cent; LLM-backed tools (briefing,
drift detection) cost more. Insufficient-budget errors surface as MCP
tool errors with `isError` set.

## Issues

Problems connecting? Open an issue here, or reach us via
[bevia.co](https://bevia.co).

## License

The contents of this repository are MIT licensed. See [LICENSE](./LICENSE).
