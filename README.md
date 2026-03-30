# MCP for Influencer Data

Fetch influencer contacts using natural language queries. This MCP (Model Context Protocol) enables AI agents to retrieve real-time data from a database of 10M+ influencers for influencer marketing, outreach, and automation.

---

## 🚀 Overview

This MCP allows you to:

- Retrieve influencer contacts using simple prompts  
- Integrate influencer discovery into AI agents  
- Access a database of 10M+ influencer profiles  
- Automate outreach and campaign sourcing  

---

## 🎯 Use Cases

- Influencer marketing campaigns  
- Creator discovery platforms  
- AI-powered outreach tools  
- Marketing automation systems  
- Lead generation for brands and agencies  

---

## 🔍 Query Capabilities

Filter influencers by:

- Location (geo)  
- Category (e.g. fitness, tech, beauty)  
- Keywords, descriptions, and tags  
- Followers, views, and 20+ performance metrics  
- Post content  

---

## ⚙️ Options

- Limit results (max 500 contacts per request)  
- Request only new contacts  
- Get a CSV file with detailed contact data (valid for 24 hours)  

---

## 💡 Example Queries

- "Give me 50 AI influencers from Ohio"  
- "Give me 10 new glamping influencers from Europe"  
- "Give me 20 new pet influencers from Asia with posts about dog shampoo"  

---

## 🛠 Setup

### 1. Register

Sign up at: https://influbalance.com/  
Get your API key (100 contacts included in trial).

---

### 2. Call the API

Use the `/responses` endpoint with MCP tool:

#### Example (plain text)

```json
{
  "input": "Give me AI influencers from Ohio",
  "model": "gpt-5.4-mini",
  "tools": [
    {
      "type": "mcp",
      "server_url": "https://go.influbalance.org/influbalance_<YOUR-API-KEY>",
      "server_label": "influbalance_get_contact",
      "require_approval": "never"
    }
  ]
}
````

---

#### Example (structured output)

```json
{
  "input": "Give me AI influencers from Ohio",
  "model": "gpt-5.4-mini",
  "tools": [
    {
      "type": "mcp",
      "server_url": "https://go.influbalance.org/influbalanceso_<YOUR-API-KEY>",
      "server_label": "influbalance_get_contact",
      "require_approval": "never"
    }
  ],
  "text": {
    "format": {
      "name": "contacts",
      "type": "json_schema",
      "schema": {
        "type": "object",
        "required": ["contacts", "link"],
        "properties": {
          "contacts": {
            "type": "array",
            "items": {
              "type": "object",
              "required": ["name", "email", "description"],
              "properties": {
                "name": { "type": "string" },
                "email": { "type": "string" },
                "description": { "type": "string" }
              }
            }
          },
          "link": {
            "type": "string"
          }
        }
      }
    }
  }
}
```

---

## 🔄 How It Works

```
User prompt
   ↓
LLM API call (/responses)
   ↓
MCP tool (Influbalance)
   ↓
Returns influencer contacts + CSV
   ↓
Structured output
   ↓
Used in your workflow
```

---

## ✅ Requirements

* LLM API with MCP / tool calling support
* Tool calling enabled

---

## 📌 Keywords

influencer marketing API, influencer database, creator discovery, influencer outreach automation, AI influencer search, MCP influencer data, influencer contacts API

