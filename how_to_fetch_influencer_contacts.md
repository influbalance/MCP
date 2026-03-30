# Fetch influencers' contacts with natural language query

Embed realtime retrieval of 10+M influencers' contacts inside your AI agents. 

You can select contacts by:
- geo
- category
- keywords, description and tags
- amount of follower, views and 20+ other parameters
- content of posts

Available options:
- limit the required amount. Maximum amount of contacts: 500
- request for new contacts only
- link on .csv file with detailed information on all selected contacts (valid 24 hours)

Examples of queries:
- "Give me 50 AI influencers from Ohio"
- "Give me new 10 glamping influencers from Europe"
- "Give me new 20 pet influencers with posts about dog shampoo, from Asia"

You can use emails of retrieved contacts immediately in E-mail campaigns and other marketing activities.

---

## Step 1: Register at [Influbalance.com](https://app.influbalance.org/)

Get your Influbalance API key and select a billing plan. 100 contacts are included in the trial period.

---

## Step 2: Add the /responses LLM API call

### Request for plain text output

```json
{
  "url": "https://api.openai.com/v1/responses",
  "body": {
    "input": "Give me AI influencers from Ohio",
    "model": "gpt-5.4-mini",
    "tools": [
      {
        "type": "mcp",
        "server_url": "https://go.influbalance.org/influbalance_<YOUR-INFLUBALANCE-API-KEY>",
        "server_label": "influbalance_get_contact",
        "require_approval": "never"
      }
    ],
    "reasoning": {
      "effort": "none"
    },
    "tool_choice": "auto",
    "instructions": "You are an AI consultant for the Influbalance.org. Use the get_contact MCP tool to retrieve contacts based on the input prompt. Print the response as described in the followup instruction of the MCP tool response.",
    "service_tier": "default",
    "max_tool_calls": 1,
    "max_output_tokens": 16384    
  },
  "method": "POST",
  "headers": {
    "Accept": "application/json",
    "Content-Type": "application/json",
    "Authorization": "Bearer <YOUR-API-KEY>"
  }
}
```

### Request for structured output

```json
{
  "url": "https://api.openai.com/v1/responses",
  "body": {
    "input": "Give me AI influencers from Ohio",
    "model": "gpt-5.4-mini",
    "tools": [
      {
        "type": "mcp",
        "server_url": "https://go.influbalance.org/influbalanceso_<YOUR-INFLUBALANCE-API-KEY>",
        "server_label": "influbalance_get_contact",
        "require_approval": "never"
      }
    ],
    "reasoning": {
      "effort": "none"
    },
    "tool_choice": "auto",
    "instructions": "You are an AI consultant for the Influbalance.org. Use the get_contact MCP tool to retrieve contacts based on the input prompt. Send the response as the structured output based on the MCP tool response.",
    "service_tier": "default",
    "max_tool_calls": 1,
    "max_output_tokens": 16384,
    "text": {
      "format": {
        "name": "contacts",
        "type": "json_schema",
        "schema": {
          "type": "object",
          "required": [
            "contacts",
            "link"
          ],
          "properties": {
            "contacts": {
              "type": "array",
              "items": {
                "type": "object",
                "required": [
                  "name",
                  "email",
                  "description"
                ],
                "properties": {
                  "name": {
                    "type": "string"
                  },
                  "email": {
                    "type": "string"
                  },
                  "description": {
                    "type": "string"
                  }
                },
                "additionalProperties": false
              },
              "description": "extract array of contacts from get_contact MCP tool response."
            },
            "link": {
              "type": "string",
              "description": "link to the .csv file with the detailed informations on selected contacts from get_contact MCP tool response."
            }
          },
          "additionalProperties": false
        }
      }
    }
  },
  "method": "POST",
  "headers": {
    "Accept": "application/json",
    "Content-Type": "application/json",
    "Authorization": "Bearer <YOUR-API-KEY>"
  }
}
```

### Notes

> Change "input", "model" and Influbalance API key in the "server_url" as you like. Do not forget to set your LLM API key.
> Make sure tool calling is **not disabled** in your API request settings. Some SDKs or wrappers restrict tool use by default.

Full description of the **/responses** call:
- [ChatGPT](https://developers.openai.com/api/reference/resources/responses/methods/create)
- [Claude] (https://platform.claude.com/docs/en/api/openai-sdk)

---

## Step 3: Process response

Use contacts in your workflow as you like

---

## How It Works

```
User/Agent sends a message what contacts to retrieve
        ↓
Your AI Agent calls LLM API, with the prompt
        ↓
LLM calls "get_contact" tools via Influbalance MCP server
        ↓
MCP returns relevant contacts, with link to .csv file
        ↓
LLM prepares structured output
        ↓
Your Agent use the selected contacts in its workflow
```

---

## Requirements

- Your LLM API must support **MCP (Model Context Protocol)** or tool calling (use /responses API)
- Tool calling must be **enabled** in your API configuration

---

## Example of plain text response

```json


```

---

## Example of structured output response

```json


```