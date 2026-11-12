# kwery MCP server

Live prices, offers and trust scores from Amazon, Google Shopping, Idealo, eBay, Kaufland and
more, as tools for your AI assistant. Ask in plain language, for example *"Where is GTIN
4006381333962 cheapest in Germany?"*, and the assistant looks it up on the marketplaces live.

kwery is a hosted remote [MCP](https://modelcontextprotocol.io) server. There is nothing to
install: point your client at the URL below and send your API key.

| | |
|---|---|
| Server URL | `https://api.kwery.co/mcp` |
| Transport | Streamable HTTP |
| Authentication | Your `kwy_live_` API key in the `x-api-key` header (or `Authorization: Bearer`) |
| Registry | `co.kwery/mcp` in the [official MCP Registry](https://registry.modelcontextprotocol.io) |
| Full guide | [docs.kwery.co/guides/mcp](https://docs.kwery.co/guides/mcp) |

**Get an API key:** [sign up at kwery.co](https://kwery.co/signup); the free plan's credits work
with MCP too.

## Connect

Replace `kwy_live_...` with your key.

### Claude Code

```bash
claude mcp add --transport http kwery https://api.kwery.co/mcp \
  --header "x-api-key: kwy_live_..."
```

Add `--scope user` to use it in every project.

### Claude (claude.ai and Claude Desktop)

**Customize → Connectors → + Add → Add custom connector**

1. Name `kwery`, URL `https://api.kwery.co/mcp`, then **Continue**.
2. Authentication: **No sign-in**. Claude may tag "Sign in now" as *Detected*; the API key header
   below is what kwery uses instead.
3. **Add header**: name `x-api-key`, value your key, **Required** ticked. Then **Add**.

### Cursor

`.cursor/mcp.json` (project) or `~/.cursor/mcp.json` (all projects):

```json
{
  "mcpServers": {
    "kwery": {
      "url": "https://api.kwery.co/mcp",
      "headers": { "x-api-key": "${env:KWERY_API_KEY}" }
    }
  }
}
```

### VS Code

`.vscode/mcp.json`:

```json
{
  "inputs": [
    { "type": "promptString", "id": "kwery-api-key", "description": "kwery API key", "password": true }
  ],
  "servers": {
    "kwery": {
      "type": "http",
      "url": "https://api.kwery.co/mcp",
      "headers": { "x-api-key": "${input:kwery-api-key}" }
    }
  }
}
```

### Other clients

Any client that supports remote MCP servers over Streamable HTTP with a custom header: use
`https://api.kwery.co/mcp` and send `x-api-key`.

## Tools

| Tool | Use it for | Cost |
|---|---|---|
| `get_product_offers` | One product on one marketplace: price, offers, trust score | The source's credit rate |
| `compare_prices` | One product on up to 5 marketplaces, ranked by price | The sum of the sources that returned a result |
| `submit_batch` | Up to 1,000 identifiers on one marketplace as a background job | Estimate first, charged per result |
| `get_batch_status` | Progress of a batch job | Free |
| `get_batch_results` | Results of a finished job, paged | Free |
| `list_sources` | Marketplaces, countries, identifier types and credit rates | Free |
| `get_account` | Plan, credit balance and credits used | Free |

Identifiers: GTIN/EAN, ASIN (Amazon), the marketplace's own product ID, and PZN where supported.
Prompts `price_check` and `catalog_sweep` and the resource `kwery://sources` are included.

`lowest_price` is the cheapest new, in-stock offer including shipping. Every result carries a
[trust score](https://docs.kwery.co/guides/trust); below 0.7 means a person should look before
acting on it.

## Example prompts

- "Where is GTIN 4006381333962 cheapest in Germany right now?"
- "What does ASIN B08R57HZ4Z cost on Amazon Germany, including shipping?"
- "Price these 200 GTINs on Kaufland DE, show me the credit estimate first."
- "How many kwery credits do I have left?"

## Pricing and limits

MCP calls use the same credits as the [kwery API](https://kwery.co/pricing). Cached results and
failed lookups are free. Lookups wait up to 50 seconds; if offers are still loading, the
assistant gets what has arrived and can fetch the rest later. More in the
[guide](https://docs.kwery.co/guides/mcp).

## Support

[kwery.co](https://kwery.co) · [docs.kwery.co](https://docs.kwery.co)

This repository holds the server's documentation and registry metadata. The server itself is
hosted by kwery.
