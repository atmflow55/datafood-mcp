# DataFood — Universal Data Hub for AI Agents (MCP Server)

[![Live](https://img.shields.io/badge/MCP-live-brightgreen)](https://toughlovesec.win/mcp)
[![Protocol](https://img.shields.io/badge/protocol-2025--06--18-blue)](https://modelcontextprotocol.io)
[![License: MIT](https://img.shields.io/badge/license-MIT-green)](./LICENSE)

**DataFood** is a remote MCP server that gives AI agents one-call access to **16 cross-niche data sources** — crypto, DeFi, security, weather, news, finance, web3 — through a single Streamable-HTTP endpoint.

> **Bundle micropayments save 92%** vs. paying per-API to 16 different vendors.

- **Live endpoint:** `https://toughlovesec.win/mcp` (Streamable HTTP, JSON-RPC 2.0)
- **Discovery:** [`/.well-known/mcp.json`](https://toughlovesec.win/.well-known/mcp.json)
- **Showcase / dashboard:** [`/agent-mesh`](https://toughlovesec.win/agent-mesh)
- **Publisher:** [TOUGH LOVE SECURITY](https://toughlovesec.win)
- **Protocol version:** `2025-06-18`

---

## What's in the box (16 data types)

| Category | Types |
|---|---|
| **Security / Web3** | `token-risk`, `breach`, `contract-risk`, `gas-prices-eth` |
| **Crypto / DeFi** | `crypto-price`, `crypto-trending`, `defi-yield` |
| **Finance** | `stock-quote`, `forex` |
| **News / Trends** | `news-hn-top`, `news-reddit`, `github-trending` |
| **Real-world** | `weather`, `flights-near`, `real-estate-rss`, `sports-scores` |

All 16 types share one schema, one auth, one billing surface.

---

## Tools (4)

| Tool | What it does | Pricing |
|---|---|---|
| `datafood_query` | Fetch one data type. Free 1-row preview. | preview: $0 / full: $0.001 |
| `datafood_bundle` | 1–20 queries across any types in a single call. | 3–5: $0.0035 / 6–10: $0.005 / 11–20: $0.008 |
| `datafood_portfolio_ask` | NL Q&A on a Plaid-linked portfolio with citations. | free during beta |
| `datafood_watch_session` | Open a public `/watch/{id}` URL streaming agent events. | free |

Day pass: **$0.99** · Week pass: **$4.99** (Stripe Checkout or x402 micropayment header).

---

## Quickstart

### Claude Desktop / Cursor / any MCP client

```jsonc
{
  "mcpServers": {
    "datafood": {
      "transport": "http",
      "url": "https://toughlovesec.win/mcp"
    }
  }
}
```

### Raw JSON-RPC

**`tools/list`:**
```bash
curl -s -X POST https://toughlovesec.win/mcp \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/list"}' | jq
```

**`tools/call` (free preview):**
```bash
curl -s -X POST https://toughlovesec.win/mcp \
  -H "Content-Type: application/json" \
  -d '{
    "jsonrpc":"2.0","id":2,"method":"tools/call",
    "params":{
      "name":"datafood_bundle",
      "arguments":{
        "free": true,
        "queries":[
          {"type":"crypto-price","q":"BTC"},
          {"type":"weather","q":"NYC"},
          {"type":"news-hn-top"}
        ]
      }
    }
  }' | jq
```

---

## Payment paths

DataFood supports two agent-native payment methods:

1. **x402** — pay-per-call via the `X-Payment` HTTP header (HTTP 402 challenge/response).
2. **Stripe Checkout** — pass `session_id` from a `/api/checkout/*` flow into `datafood_bundle`. Day/week passes auto-unlock.

Free 1-row preview (up to 5 bundle items) requires **no auth**.

---

## Cost comparison (real numbers)

A typical agent fetching 10 data types per task:

| Method | Cost / call | Vendors to manage |
|---|---|---|
| 10 individual APIs (Alchemy, Etherscan, OpenWeather, NewsAPI, …) | ~$0.06 | 10 |
| **DataFood `datafood_bundle` (10 queries)** | **$0.005** | **1** |

**~92% cheaper, one auth surface, one invoice.**

---

## Links

- **Website:** https://toughlovesec.win
- **Agent Mesh dashboard:** https://toughlovesec.win/agent-mesh
- **OpenAPI spec:** https://toughlovesec.win/openapi.json
- **MCP descriptor:** https://toughlovesec.win/.well-known/mcp.json
- **Status / health:** https://toughlovesec.win/healthz

## License

MIT — see [`LICENSE`](./LICENSE).

---

*DataFood is built and operated by [TOUGH LOVE SECURITY](https://toughlovesec.win), a security-and-data infrastructure shop. Contact: `lemorris@toughlovesec.win`.*
