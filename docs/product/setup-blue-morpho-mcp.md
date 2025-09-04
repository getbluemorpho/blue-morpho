# Model Context Protocol (MCP) 

The Model Context Protocol (MCP) is a standard for connecting Large Language Models (LLMs) to platforms like Blue Morpho. This guide covers how to connect Blue Morpho to the following AI tools using MCP:

- Cursor  
- Claude desktop  

Once connected, your AI assistants can interact with and query your Blue Morpho projects on your behalf. To see a list of available tools, see here. 

---

## Step 1: Create a personal access token

First, go to your Blue Morpho settings and create an API key (personal access token). This will be used to authenticate the MCP server with your Blue Morpho account.

---

## Step 2: Configure your AI tool

MCP-compatible tools can connect to Blue Morpho using the Blue Morpho remote MCP server.

---

### Cursor

In Cursor settings, search for "mcp". In "Tools & Integrations", click on "New MCP Server".

This creates a `mcp.json` file:

```json
{
  "mcpServers": {
    "blue-morpho": {
      "type": "url",
      "url": "https://app.getbluemorpho.com/mcp/",
      "headers": {
        "X-API-Key": "YOUR-API-KEY-HERE"
      }
    }
  }
}
```

Replace YOUR-API-KEY-HERE with your Blue Morpho API key (or preferably, use environment variables). 

Save the file. Navigate back to Settings → MCP. You should see a green active status once connected.

### Claude desktop

Navigate to Settings → Developer → Edit Config:

```json
{
  "mcpServers": {
    "blue-morpho": {
      "command": "npx",
      "args": [
        "mcp-remote",
        "https://app.getbluemorpho.com/mcp/",
        "--header",
        "X-API-Key: YOUR-API-KEY-HERE"
      ]
    }
  }
}
```

Replace YOUR-API-KEY-HERE with your Blue Morpho API key (or preferably, use environment variables). 

Restart Claude. Navigate back to Settings → Developer → Edit Config. You should see your running MCP connection.

## Step 3: test Blue Morpho MCP

Once your AI tool is connected to Blue Morpho via MCP, you can immediately begin issuing commands to interact with your projects.

Try asking your AI assistant to `list Blue Morpho projects` to make sure everything is working properly!

To learn more about the Blue Morpho tools accessible through MCP, see here. 

