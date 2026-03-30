# MCP for Influencer Data: Fetch Influencers' Contacts with Natural Language Query

Embed real-time retrieval of 10M+ influencer contacts inside your AI agents for influencer marketing, outreach, and automation.

You can select contacts by:
- geo
- category
- keywords, descriptions, and tags
- number of followers, views, and 20+ other parameters
- content of posts

Available options:
- limit the requested amount (maximum 500 contacts)
- request only new contacts
- receive a link to a .csv file with detailed information on all selected contacts (valid for 24 hours)

Examples of queries:
- "Give me 50 AI influencers from Ohio"
- "Give me 10 new glamping influencers from Europe"
- "Give me 20 new pet influencers from Asia with posts about dog shampoo"

You can use emails of retrieved contacts immediately in email campaigns and other marketing activities.

---

## Step 1: Register at [Influbalance.com](https://influbalance.com)

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
    "instructions": "You are an AI consultant for Influbalance.org. Use the get_contact MCP tool to retrieve contacts based on the input prompt. Print the response as described in the MCP tool response.",
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
````

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
    "instructions": "You are an AI consultant for Influbalance.org. Use the get_contact MCP tool to retrieve contacts based on the input prompt. Return structured output based on the MCP tool response.",
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
              "description": "Array of contacts returned by the get_contact MCP tool."
            },
            "link": {
              "type": "string",
              "description": "Link to the .csv file with detailed information on selected contacts."
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

> Change "input", "model", and the Influbalance API key in "server_url" as needed. Make sure to set your LLM API key.
> Ensure tool calling is **enabled** in your API request settings. Some SDKs restrict tool usage by default.

Full description of the **/responses** call:

* [https://developers.openai.com/api/reference/resources/responses/methods/create](https://developers.openai.com/api/reference/resources/responses/methods/create)
* [https://platform.claude.com/docs/en/api/openai-sdk](https://platform.claude.com/docs/en/api/openai-sdk)

---

## Step 3: Process Response

Use the retrieved contacts in your workflow as needed.

---

## How It Works

```
User/Agent sends a request describing the desired influencers
        ↓
Your AI agent calls the LLM API with the prompt
        ↓
LLM calls the "get_contact" tool via Influbalance MCP server
        ↓
MCP returns relevant contacts + CSV link
        ↓
LLM formats the response
        ↓
Your agent uses the contacts in its workflow
```

---

## Requirements

* Your LLM API must support **MCP (Model Context Protocol)** or tool calling (/responses API)
* Tool calling must be **enabled** in your API configuration

---

## Keywords

influencer marketing API, influencer database, creator discovery, influencer outreach, AI influencer search, MCP influencer data, influencer contacts API, social media marketing tools
