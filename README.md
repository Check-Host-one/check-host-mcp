<p align="center">
  <a href="https://check-host.one"><img src="https://check-host.one/logo-96.png" width="72" alt="check-host.one"></a>
</p>

<h1 align="center">check-host.one MCP server</h1>

<p align="center">
  A hosted <a href="https://modelcontextprotocol.io">Model Context Protocol</a> server that lets AI agents run <b>ping</b>, <b>TCP ping</b>, <b>HTTP</b>, <b>traceroute</b> and <b>DNS</b> checks from measurement servers in <b>100+ countries</b>, powered by <a href="https://check-host.one"><b>check-host.one</b></a>.<br>
  Nothing to install. No account, no API key.
</p>

<p align="center">
  <a href="https://check-host.one"><img src="https://img.shields.io/badge/website-check--host.one-2563eb" alt="Website"></a>
  <img src="https://img.shields.io/badge/MCP-streamable%20HTTP-8b5cf6" alt="MCP streamable HTTP">
  <img src="https://img.shields.io/badge/auth-none-22c55e" alt="No auth">
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-MIT-green" alt="MIT license"></a>
</p>

Ask your assistant things like:

- *"Is example.com down for everyone or just me?"*
- *"Ping my server from Germany, Brazil and Japan and compare latency."*
- *"Is port 5432 on db.example.com reachable from AWS networks?"*
- *"Has the new MX record for example.com propagated in Asia yet?"*
- *"Traceroute to 1.1.1.1 from Tokyo and tell me where the latency jumps."*

The agent gets a per-server summary and a permanent result link (`https://check-host.one/r/...`) it can hand back to you.

**Endpoint:** `https://check-host.one/mcp` (Streamable HTTP, stateless)

## Setup

### Claude Code

```sh
claude mcp add --transport http check-host https://check-host.one/mcp
```

### Claude Desktop / claude.ai

Settings → Connectors → **Add custom connector**, and paste `https://check-host.one/mcp`.

### Cursor

`~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "check-host": { "url": "https://check-host.one/mcp" }
  }
}
```

### VS Code (GitHub Copilot)

```sh
code --add-mcp '{"name":"check-host","type":"http","url":"https://check-host.one/mcp"}'
```

Or in `.vscode/mcp.json`:

```json
{
  "servers": {
    "check-host": { "type": "http", "url": "https://check-host.one/mcp" }
  }
}
```

### Windsurf

`~/.codeium/windsurf/mcp_config.json`:

```json
{
  "mcpServers": {
    "check-host": { "serverUrl": "https://check-host.one/mcp" }
  }
}
```

### OpenAI Codex CLI

`~/.codex/config.toml`:

```toml
[mcp_servers.check-host]
url = "https://check-host.one/mcp"
```

### Gemini CLI

```sh
gemini mcp add --transport http check-host https://check-host.one/mcp
```

### Clients that only speak stdio

Bridge with [`mcp-remote`](https://www.npmjs.com/package/mcp-remote):

```json
{
  "mcpServers": {
    "check-host": { "command": "npx", "args": ["-y", "mcp-remote", "https://check-host.one/mcp"] }
  }
}
```

## Tools

| Tool | What it does |
| --- | --- |
| `ping` | ICMP ping: min/avg/max latency and packet loss per server |
| `tcp_ping` | Times TCP handshakes to a port, to check a service is reachable when ICMP is blocked |
| `http_check` | Sends an HTTP request (takes a full URL), returns status code and timings |
| `traceroute` | Hop-by-hop path from each server |
| `dns_lookup` | Resolves a name from each region, to compare answers or check propagation |
| `host_info` | ASN, network operator and geolocation of a host or IP |

Every check tool takes `target` (hostname, IP or URL), optional `locations` (free-form: country, city, continent, ASN or network name, e.g. `["Germany"]`, `["Tokyo", "AS13335"]`) and `limit` (1-25 servers).

## Limits

About 20 checks per minute per client and 30 per minute against any one target.

## Try it with curl

```sh
curl -s https://check-host.one/mcp \
  -H 'content-type: application/json' -H 'accept: application/json, text/event-stream' \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/call","params":{"name":"ping","arguments":{"target":"example.com","locations":["Germany"],"limit":2}}}'
```

## Related

- 🌐 [check-host.one](https://check-host.one): the same checks in your browser, with a live world map
- 📖 [Docs](https://check-host.one/docs) · [llms.txt](https://check-host.one/llms.txt) · [OpenAPI](https://check-host.one/openapi.json)
- SDKs: [Python](https://github.com/Check-Host-one/check-host-python) · [JavaScript / TypeScript](https://github.com/Check-Host-one/check-host-js) · [Go](https://github.com/Check-Host-one/check-host-go) · [Rust](https://github.com/Check-Host-one/check-host-rust) · [Java](https://github.com/Check-Host-one/check-host-java) · [PHP](https://github.com/Check-Host-one/check-host-php) · [Ruby](https://github.com/Check-Host-one/check-host-ruby) · [C# / .NET](https://github.com/Check-Host-one/check-host-dotnet)

`server.json` in this repo describes the server for the [official MCP registry](https://registry.modelcontextprotocol.io).

## License

MIT
