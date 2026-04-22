# Financial-Data-Normalizer-Mainnet
A production-ready financial data normalization agent built for AI pipelines. Callable by any autonomous agent via MCP, A2A, or x402 — no signup, no API keys required for crypto payments. The Node.js version of the Financial Data Normalizer agent, running real USDC payments on Base mainnet via the Coinbase CDP facilitator.

**Live endpoint:** `https://fdn-node-production.up.railway.app`
**Network:** Base Mainnet (eip155:8453) — real USDC via Coinbase CDP
**Price:** $0.002 USDC per call


> For the testnet/Python version see: [niche-agent](https://github.com/your-username/niche-agent)

---

## Quick Start

### Discover tools (free)
```bash
curl https://fdn-node-production.up.railway.app/tools
```

### Check x402 status
```bash
curl https://fdn-node-production.up.railway.app/debug/x402
```

### Call with Stripe API key
```bash
curl -X POST https://fdn-node-production.up.railway.app/invoke \
  -H "x-api-key: cus_YOUR_ID.YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "tool": "enrich_counterparty",
    "input": { "counterparty_name": "Goldman Sachs International" }
  }'
```

### Call via x402 (any agent with Base wallet)
No API key needed — just a funded wallet on Base mainnet. The endpoint returns HTTP 402 with payment instructions, your agent pays $0.002 USDC, and the data comes back.

```
POST https://fdn-node-production.up.railway.app/invoke
```

---

## Tools

| Tool | Data Source | Category |
|---|---|---|
| `normalize_financial_record` | Claude Haiku | TradFi |
| `normalize_onchain_transaction` | Claude Haiku | DeFi |
| `validate_regulatory_fields` | Claude Haiku | Compliance |
| `enrich_counterparty` | GLEIF Global LEI Index | TradFi |
| `lookup_instrument` | OpenFIGI | TradFi |
| `resolve_wallet_identity` | Ethplorer + Claude | DeFi |

---

## Discovery

| Protocol | Endpoint |
|---|---|
| x402 manifest | `/.well-known/x402` |
| A2A / Google agent.json | `/.well-known/agent.json` |
| MCP manifest | `/.well-known/mcp.json` |
| ERC-8004 identity | `/.well-known/erc-8004` |
| LLMs.txt | `/llms.txt` |
| Tool listing | `/tools` |

---

## Payment

| Rail | Network | Token | Price |
|---|---|---|---|
| x402 | Base Mainnet (eip155:8453) | USDC | $0.002/call |
| Stripe | — | USD | $0.002/call |

Real USDC payments via Coinbase CDP facilitator. Any autonomous agent with a funded Base wallet can call this endpoint with zero signup.

---

## Relationship to Python Agent

This Node.js agent is the mainnet payment version of the [Python FDN agent](https://github.com/your-username/niche-agent).

- **Python agent** → Base Sepolia testnet, discovery/testing
- **Node agent** → Base mainnet, real USDC, production payments

Both expose identical tools and response formats.

---

## Tech Stack

- Node.js + Express
- `@x402/express` — x402 payment middleware
- `@coinbase/x402` — CDP facilitator auth (mainnet JWT)
- `@anthropic-ai/sdk` — Claude Haiku inference
- GLEIF API — verified LEI data
- OpenFIGI API — verified FIGI data
- Ethplorer API — on-chain wallet labels
- Railway — deployment

---

## Tags
`x402` `a2a` `mcp` `erc8004` `fintech` `finance` `lei` `gleif` `figi` `openfigi` `compliance` `mifid` `defi` `onchain` `base` `usdc` `tradfi`
