# Ask a human expert (MCP server)

A remote MCP server that lets you, or your AI agent, bring in a human expert when stuck on a
problem that needs a creative solution. I'm Cristian Desivo: mathematics and computer science
background, 10 years taking projects from start to finish, specialized in mathematical
optimization and AI systems. I read every request and write every reply myself.

- **Endpoint:** `https://cristiandesivo.com/mcp` (Streamable HTTP, no auth)
- **Registry:** `com.cristiandesivo/ask-a-human` in the [official MCP Registry](https://registry.modelcontextprotocol.io/v0.1/servers?search=com.cristiandesivo)
- **Terms as a page:** https://cristiandesivo.com/ask/ (also as [Markdown](https://cristiandesivo.com/ask.md) and [JSON](https://cristiandesivo.com/status.json))

This repository only documents the server; the server itself runs on my website.

## Connect

**Claude Code**

```bash
claude mcp add --transport http ask-a-human https://cristiandesivo.com/mcp
```

**Cursor** (`~/.cursor/mcp.json`)

```json
{
  "mcpServers": {
    "ask-a-human": { "url": "https://cristiandesivo.com/mcp" }
  }
}
```

**VS Code**

```bash
code --add-mcp '{"name":"ask-a-human","type":"http","url":"https://cristiandesivo.com/mcp"}'
```

**Claude Desktop or claude.ai:** Settings → Connectors → Add custom connector, with the URL
`https://cristiandesivo.com/mcp`.

## Tools

| Tool | What it does |
|---|---|
| `get_status` | Whether I'm online right now, or when I'm back, and whether requests are paused |
| `get_terms` | The full terms: price, hours, reply times, how to pay, how to submit |
| `submit_request` | Sends your problem (a summary of up to 300 words) with the payment's transaction ID (PayPal, or `crypto_tx` and `network`); returns a `request_id` |
| `get_reply` | My reply to a request, by `request_id`: `waiting` until I've replied, then the text |
| `send_followup` | Continues in the same thread (answer my question, clarify, ask the next one), at no extra cost |

## How a request works

1. **Check** `get_status` and `get_terms`.
2. **Pay**, one of:
   - **20 USD by PayPal** at https://paypal.me/CristianDesivo/20USD, or sent by PayPal to
     `cdesivo92@gmail.com`. An agent with access to a PayPal account (for example through
     PayPal's Payouts API) can do this on its own, with no human.
   - **25 USDT on TRON (TRC-20)** from a crypto wallet, to `TJCSWgHwkQ4gYTotSQxz4dVGcgnkecGtZ5`.
     Only USDT on the TRON network can be matched; `get_terms` always lists the current address.
3. **Submit** with `submit_request`: the payment, an email for my reply, and a summary of the
   problem in 300 words or fewer (what it is, what has been tried, the key constraints, the
   deadline).
   - Paid by PayPal: `transaction_id`. If PayPal shows you a different transaction ID than it
     shows me, `payer_email` (the PayPal account that paid) lets the payment still be matched.
   - Paid in USDT: `crypto_tx` (the transaction hash) and `network: "tron"`. The transfer is
     checked on the chain right away, so the request reaches me within seconds.
4. **Read my reply** with `get_reply` and the `request_id`. The reply is also emailed to the
   address you gave, and a confirmation goes there right away.
5. **Follow up** with `send_followup` if you need to; a new problem is a new request.

Payments are checked before a request reaches me (PayPal against PayPal's own records, USDT on
the TRON blockchain), and one payment pays for one request.

## Terms in short

- **What you get:** my most creative solution idea for the problem, based on your summary.
- **Price:** 20 USD per request by PayPal, or 25 USDT on TRON. If the problem needs more (reviewing the full context, research,
  a prototype or an implementation), my reply says so and includes a tentative budget; taking it
  is optional.
- **Hours:** every day, 09:00 to 24:00 UTC. While I'm online I reply within 15 minutes of
  receiving your request, once your payment shows up; otherwise by 09:30 UTC.
- **Limits:** 300 words per summary or follow-up; 5 requests, 10 follow-ups and 120 reply checks
  per hour from one address.
- **Not a fit:** physical or on-site tasks; anything illegal or meant to deceive people.

## Without MCP

The same actions over plain HTTP:

| Action | Request |
|---|---|
| Status | `GET https://cristiandesivo.com/status.json` |
| Submit | `POST https://cristiandesivo.com/ask/submit/` (JSON or form fields) |
| Read the reply | `GET https://cristiandesivo.com/ask/reply/?request_id=...` |
| Follow up | `POST https://cristiandesivo.com/ask/followup/` with `{"request_id", "message"}` |

## Privacy

I read your summary and follow-ups myself; they travel by email. Don't include passwords, keys
or other secrets.

## Contact

- Website: https://cristiandesivo.com/
- Longer projects: https://cristiandesivo.com/hire_me/
