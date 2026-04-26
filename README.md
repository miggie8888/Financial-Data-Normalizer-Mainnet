# Financial Data Normalizer — Node.js (Base Mainnet)

**Live endpoint:** `https://fdn-node-production.up.railway.app`  
**Network:** Base Mainnet (eip155:8453) — real USDC via Coinbase CDP  
**Price:** $0.002 USDC per call

The most complete financial compliance agent in the x402 ecosystem. Combines verified TradFi data (GLEIF LEI, OpenFIGI FIGI), on-chain DeFi normalization, OFAC sanctions screening, and AML pattern detection — all in one pay-per-call agent on Base mainnet.

> Testnet/Python version: [niche-agent](https://github.com/miggie8888/niche-agent)

---

## Tools (8)

| Tool | Data Source | Category | Description |
|---|---|---|---|
| `normalize_financial_record` | Claude Haiku | TradFi | Normalize raw trade/transaction records to ISO 8601/4217 |
| `normalize_onchain_transaction` | Claude Haiku | DeFi | DeFi tx → TradFi-compatible record with protocol identification |
| `validate_regulatory_fields` | Claude Haiku | Compliance | MiFID II, Dodd-Frank, EMIR, SEC Rule 605 field validation |
| `enrich_counterparty` | GLEIF Global LEI Index | TradFi | Verified LEI code, jurisdiction, entity status |
| `lookup_instrument` | OpenFIGI | TradFi | Verified FIGI from ISIN, CUSIP, SEDOL, or ticker |
| `resolve_wallet_identity` | Ethplorer + Claude | DeFi | Wallet address → entity name, type, risk context |
| `screen_sanctions` | OFAC API v4 | Compliance | Real-time SDN + NONSDN + UN sanctions screening with fuzzy name match and crypto wallet address support |
| `detect_aml_patterns` | Claude Haiku | Compliance | Structuring, smurfing, layering, velocity spike, round-trip, and threshold avoidance detection with SAR recommendation |

---

## What Makes This Unique

The only x402 agent that can run a complete compliance pipeline in a single session:

```
normalize_onchain_transaction  → structured DeFi record
resolve_wallet_identity        → entity identification
screen_sanctions               → OFAC SDN check
validate_regulatory_fields     → MiFID II / Dodd-Frank gaps
detect_aml_patterns            → risk score + SAR recommendation
```

Total cost: $0.010 USDC. Replaces 4–5 enterprise vendor contracts.

---

## Quick Start

### Confirm tools available (free)
```bash
curl https://fdn-node-production.up.railway.app/tools
```

### Check live status
```bash
curl https://fdn-node-production.up.railway.app/health
```

### Screen for sanctions (x402 payment or Stripe key)
```bash
curl -X POST https://fdn-node-production.up.railway.app/invoke \
  -H "x-api-key: cus_YOUR_ID.YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "tool": "screen_sanctions",
    "input": {
      "names": ["Lazarus Group"],
      "wallet_addresses": ["0x098B716B8Aaf21512996dC57EB0615e2383E2f96"],
      "min_score": 80
    }
  }'
```

### Detect AML patterns
```bash
curl -X POST https://fdn-node-production.up.railway.app/invoke \
  -H "x-api-key: cus_YOUR_ID.YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "tool": "detect_aml_patterns",
    "input": {
      "entity_name": "Shell Corp LLC",
      "lookback_days": 7,
      "transactions": [
        {"id":"t1","amount":9800,"currency":"USD","date":"2026-04-20","from":"acct_A","to":"acct_B","type":"wire"},
        {"id":"t2","amount":9500,"currency":"USD","date":"2026-04-21","from":"acct_A","to":"acct_C","type":"wire"},
        {"id":"t3","amount":9700,"currency":"USD","date":"2026-04-22","from":"acct_A","to":"acct_D","type":"wire"}
      ]
    }
  }'
```

### Verify LEI (Goldman Sachs)
```bash
curl -X POST https://fdn-node-production.up.railway.app/invoke \
  -H "x-api-key: cus_YOUR_ID.YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "tool": "enrich_counterparty",
    "input": { "counterparty_name": "Goldman Sachs International" }
  }'
```

---

## Payment

| Rail | Network | Token | Price |
|---|---|---|---|
| x402 | Base Mainnet (eip155:8453) | USDC | $0.002/call |
| Stripe | — | USD | $0.002/call |

Real USDC via Coinbase CDP facilitator. Any autonomous agent with a funded Base wallet can call this endpoint with zero signup.

---

## Discovery

| Protocol | Endpoint |
|---|---|
| x402 manifest | `/.well-known/x402` |
| A2A / Google agent.json | `/.well-known/agent.json` |
| MCP manifest | `/.well-known/mcp.json` |
| ERC-8004 identity | `/.well-known/erc-8004` |
| OpenAPI spec | `/openapi.json` |
| LLMs.txt | `/llms.txt` |
| Tool listing | `/tools` |

---

## Tech Stack

- Node.js + Express
- `@x402/express` — x402 payment middleware
- `@coinbase/x402` — CDP facilitator JWT auth (Base mainnet)
- `@anthropic-ai/sdk` — Claude Haiku inference
- GLEIF API — verified LEI data (free, no key)
- OpenFIGI API — verified FIGI data (free, optional key)
- OFAC API v4 — real-time sanctions screening (SDN + NONSDN + UN)
- Ethplorer API — on-chain wallet labels (free)
- Railway — deployment (~$5/month)
- Stripe — metered billing for API key customers

---

## Compliance Coverage

**Sanctions lists screened:**
- OFAC SDN (19,381 records — updated every 2 minutes)
- OFAC NONSDN / Consolidated (443 records)
- UN Security Council Consolidated (1,013 records)

**Regulations supported:**
- MiFID II (EU)
- Dodd-Frank (US)
- EMIR (EU derivatives)
- SEC Rule 605 (US order execution)

**AML patterns detected:**
- Structuring (CTR threshold avoidance)
- Smurfing (split transactions)
- Layering (rapid multi-hop movement)
- Velocity spikes
- Round-trip transactions
- Dormancy activation
- Threshold avoidance

---


## Relationship to Python Agent

| | Python Agent | Node Agent (this) |
|---|---|---|
| Network | Base Sepolia (testnet) | Base Mainnet |
| Payment | Testnet USDC | Real USDC |
| Facilitator | x402.org (public) | Coinbase CDP |
| Tools | 8 | 8 |
| Best for | Discovery, testing | Production payments |

---

## Tags
`x402` `a2a` `mcp` `erc8004` `fintech` `finance` `lei` `gleif` `figi` `openfigi` `compliance` `ofac` `sanctions` `aml` `kyc` `sdn` `anti-money-laundering` `mifid` `defi` `onchain` `base` `usdc` `tradfi` `virtuals`
