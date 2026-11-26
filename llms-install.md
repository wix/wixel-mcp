# Installing the Wixel MCP (for AI agents)

Wixel is a hosted MCP server. There is nothing to download, build or run, and no API key.

- Server URL: `https://mcp.wix.com/wixel`
- Transport: Streamable HTTP
- Sign-in: OAuth 2.1 in the browser, with the user's Wixel account (a free account is enough to start)

## Cline

1. Open the MCP settings file. In the Cline extension: MCP Servers → Configure → Configure MCP Servers. In the Cline CLI it is `~/.cline/data/settings/cline_mcp_settings.json`.
2. Add a `wixel` entry under `mcpServers`, keeping the entries that are already there:

```json
{
  "mcpServers": {
    "wixel": {
      "type": "streamableHttp",
      "url": "https://mcp.wix.com/wixel?ref=cline",
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

`"type": "streamableHttp"` is required: without it Cline uses the older SSE transport.

3. Save the file. The server needs a sign-in before its tools work: Cline's MCP Servers panel offers to authorize it, which opens a browser page where the user signs in with their Wixel account and allows access. Signing in is the user's own step; do not ask them for a password or a token.
4. Check it: the `wixel` server shows as connected with its tools listed (`send_wixel_message`, `get_wixel_task` and others). Then ask Wixel for something small, for example "list my Wixel brands".

### If the browser sign-in does not start

Use the `mcp-remote` proxy instead, which handles the sign-in itself. It needs Node.js 18 or newer:

```json
{
  "mcpServers": {
    "wixel": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://mcp.wix.com/wixel?ref=cline"],
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

## Other MCP clients

Any client that supports remote MCP servers over Streamable HTTP with OAuth can add `https://mcp.wix.com/wixel`. Steps for Claude, ChatGPT, Cursor, VS Code, Claude Code and Gemini CLI are in [README.md](README.md).

## Using it

A request goes: `send_wixel_message` with the user's request in their own words, then `get_wixel_task` until the turn is done, then `get_wixel_asset_result` or `get_wixel_asset_files` for what Wixel made. Wixel may ask a question before it starts; show it to the user and send their answer back with the same `contextId`.
