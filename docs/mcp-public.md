# Claimora MCP

Claimora is a structured claim graph. Science claims are extracted from paper abstracts, with evidence, contradictions, and innovation flags. A law graph for jurisdiction-scoped records sits on the same service and is growing.

## Endpoint

Streamable HTTP:

`https://claimora.si/mcp`

`POST` requires `Content-Type: application/json`. Any `Host` is accepted.

## Tools

| Tool | What it returns |
|---|---|
| `query_claims` | Ranked claims with a short excerpt, status, and evidence strength. |
| `get_evidence` | Provenance for one claim. Excerpts stay short unless `full_text` is true. |
| `get_contradictions` | Opposing claims and the strength of each contradiction. |
| `get_updates` | Claims updated at or after an ISO-8601 timestamp. |
| `get_innovation_flags` | Innovation flags, strongest first. |
| `search_smart` | Claims matched from a natural-language question. This build is lexical. |

List hits stay short: claim id, status, evidence strength, and a shortened excerpt. Pass a claim id from a search into `get_evidence` or `get_contradictions`.

## Free and paid

10 free calls per UTC day. Paid calls after that are 0.01 USDC.

Send the token in `X-Claimora-Paid-Token`. A valid token skips the daily quota. `get_innovation_flags`, and `get_evidence` with `full_text` true, cost 0.01 USDC per call and return HTTP 402 without a valid token or a balance that covers the call.

Fund the account at [https://claimora.si/credits](https://claimora.si/credits).

## Claude and Cursor

One HTTP config for both. Cursor reads `~/.cursor/mcp.json` or `.cursor/mcp.json`. Claude Code reads the same shape in `.mcp.json`.

```json
{
  "mcpServers": {
    "claimora": {
      "type": "http",
      "url": "https://claimora.si/mcp",
      "headers": {
        "X-Claimora-Paid-Token": "YOUR_TOKEN"
      }
    }
  }
}
```

Leave `headers` out until `/credits` has issued a token. Replace `YOUR_TOKEN` with that value. The header is `X-Claimora-Paid-Token`.

## Law records

Law records are available under the same endpoint, `https://claimora.si/mcp`, and will grow. They use the same six tools, the same free quota, and the same paid token.
