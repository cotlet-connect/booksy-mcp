# Booksy API integration and MCP server: connect Booksy to ChatGPT, Claude and any AI agent

Booksy API integration and MCP server, unofficial, by Cotlet. Your AI works in your own signed-in Booksy account: the calendar, clients, appointments, no-show fees and deposits, Boost, reviews and reports, on the same screens your login opens. No developer and no Booksy API key needed. Cotlet is independent and is not affiliated with Booksy.

**Does Booksy have an API or an MCP server?** Booksy's API is for partners only, with partner-issued keys (checked 23 Nov 2026); a business owner gets no API key. Booksy has no Zapier or Make app. Its ChatGPT app, launched in the US on 23 Oct 2026, is for clients who book, not for the business, and its AI Receptionist answers the phone in early access. Booksy's help center says a copy of your client list comes by emailing support.

## What barbers, salon owners and beauty pros ask their AI to do in Booksy

| Job | Ask your AI | Make it repeat |
|---|---|---|
| Tomorrow's confirmations | "List tomorrow's appointments with client phone numbers and draft a WhatsApp confirmation for each. Send nothing yet." | Daily |
| Empty slots tomorrow | "Which slots are empty tomorrow per barber, and which regular clients could take them?" | Daily |
| Clients who stopped booking | "Find clients who have not rebooked in 6 weeks and draft a message for each." | Weekly |
| No-shows and deposits | "List this month's no-shows and failed cancellation fees, and the clients I should ask for a deposit." | Monthly |
| Boost clients to dispute | "List this month's Boost charges and mark the clients who were already mine, so I can claim them back." | Monthly |
| Review requests | "List today's checked-out clients and draft a Google review request for each." | Daily |
| Staff commission for payroll | "Download last week's sales per staff member with commission and tips for payroll." | Weekly |
| Income for your accountant | "Export this month's income, fees and tips to a spreadsheet for my accountant." | Monthly |
| Bulk price change | "Raise all fade services by $5 and show me the new prices before you save." | When prices change |
| Appointments in a spreadsheet | "Put each day's appointments in my Google Sheet." | Your own endpoint, called from Google Sheets |
| Client list out | "Export my full client list with phone numbers and visit history to a spreadsheet." | When you need it, or before you switch |

Your AI asks you before it sends, books or changes anything. Payments stay in your hands: Cotlet refuses payments.

## Your own Booksy API, built with Cotlet

Booksy gives business owners no API key: its API is for partners only. With Cotlet you build your own: your AI does the job once in your Cloud Browser, you save it as an endpoint, and you call it from Google Sheets, WhatsApp tools, QuickBooks, Zapier, Make, n8n, your own code.

Examples you can build in a few minutes (none of them exists until you build it):

| Endpoint you build | What it returns or does |
|---|---|
| `appointments-for-day` | appointments for a date with client, phone, service and staff |
| `open-slots` | empty slots per staff member for a date |
| `lapsed-clients` | clients with no booking for the weeks you name |
| `no-shows` | no-shows and late cancellations with the fee status |
| `boost-clients` | Boost-charged clients for a period, with their first visit |
| `block-time` | blocks a time on a staff member's calendar (writes only after you allow it) |

Call it with your Cotlet API key, JSON in and JSON out:

```bash
curl -X POST https://cotlet.dev/api/v1/custom/appointments-for-day \
  -H "Authorization: Bearer $COTLET_API_KEY" -H "Content-Type: application/json" -d '{}'
```

Limits: one call runs up to 90 seconds, writes wait for your consent, payments are blocked. Start here: https://cotlet.dev/app/custom-api/new?app=booksy

## Connect in 2 minutes

Sign up at https://cotlet.dev, sign in to Booksy once in your Cloud Browser, then add Cotlet to your AI.

### Claude

Settings → Connectors → Add custom connector, paste `https://cotlet.dev/mcp/v1` and sign in with Cotlet.dev.

### ChatGPT

Open Plugins in ChatGPT, choose Create as a plugin, paste `https://cotlet.dev/mcp/v1` and sign in with Cotlet.dev. No API key needed.

### Claude Code

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

### Cursor

Copy `.cursor/mcp.json` from this repo into your project:

```json
{
  "mcpServers": {
    "cotlet": {
      "url": "https://cotlet.dev/mcp/v1",
      "headers": {
        "Authorization": "Bearer ${COTLET_API_KEY}"
      }
    }
  }
}
```

## Run it locally (stdio)

This repo is a real MCP server. It speaks MCP over stdio, lists its Booksy tools without any key, and forwards every tool call to the hosted Cotlet.dev server. The hosted URL above stays the recommended way. Use this when your client only starts local servers.

```json
{"mcpServers":{"booksy":{"command":"uvx","args":["booksy-mcp"],"env":{"COTLET_API_KEY":"your-key"}}}}
```

Or with Docker:

```bash
docker build -t booksy-mcp .
docker run -i --rm -e COTLET_API_KEY booksy-mcp
```

Tools: `booksy_guide`, `booksy_status`, `booksy_open`, `booksy_read`, `booksy_click`, `booksy_type`, `booksy_close`. Without `COTLET_API_KEY` a tool call answers: "Set COTLET_API_KEY. Get a key at https://cotlet.dev/app".

## Command line

Python 3.10+, nothing else to install. From PyPI:

```bash
pip install booksy-mcp        # or run it once: uvx booksy-mcp connect
booksy-mcp connect
```

Or straight from this repo:

```bash
export COTLET_API_KEY="your-key"
python3 booksy_agent.py connect   # ready config for Claude Code, Cursor, ChatGPT
python3 booksy_agent.py status    # is Booksy ready in your Cotlet.dev account
python3 booksy_agent.py guide     # Booksy screens and sign-in link
python3 booksy_agent.py read      # open Booksy, read the page, close the browser
python3 booksy_agent.py tools     # list Cotlet.dev tools
python3 booksy_agent.py serve     # start the stdio MCP server (same as no argument)
```

## Questions from Booksy pros
**Does Booksy have an API or a Zapier app?**

Booksy's API is for partners only, and Booksy has no Zapier or Make app (checked 23 Nov 2026). With Cotlet you build your own endpoints in your signed-in Booksy Biz account and call them from Google Sheets, Zapier, Make or n8n.

**Is the Booksy app in ChatGPT for business owners?**

No. Booksy's ChatGPT app is for clients who want to book. Cotlet lets your own ChatGPT or Claude work in your Booksy Biz account for you.

**Can AI send my Booksy clients a confirmation every morning?**

Your AI lists tomorrow's appointments with phone numbers and drafts each confirmation. You send them, or your own endpoint hands them to your WhatsApp tool.

**Can AI tell me which Booksy Boost clients were really mine?**

Yes. It lists the month's Boost charges and marks the clients who visited you before, so you know which charges to dispute.

**Can AI find Booksy clients who stopped booking?**

Yes. It lists clients with no booking for the weeks you choose and drafts a message for each. Nothing goes out until you say yes.

**How do I export my Booksy client list without emailing support?**

Your AI reads your clients with phone numbers and visit history from the screens your login opens and saves them to a spreadsheet.

## Find it on

- [Booksy MCP server on Cotlet.dev](https://cotlet.dev/integrations/booksy-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=booksy&utm_content=find)
- [Booksy API with Cotlet](https://cotlet.dev/integrations/api/booksy?utm_source=github&utm_medium=readme&utm_campaign=booksy&utm_content=find)
- [Does Booksy have an API or MCP server?](https://cotlet.dev/api-mcp-status/booksy?utm_source=github&utm_medium=readme&utm_campaign=booksy&utm_content=find)
- [PyPI package booksy-mcp](https://pypi.org/project/booksy-mcp/)

## More barber and beauty apps on Cotlet

- [Squire MCP](https://github.com/cotlet-connect/squire-mcp)
- [GlossGenius MCP](https://github.com/cotlet-connect/glossgenius-mcp)
- [StyleSeat MCP](https://github.com/cotlet-connect/styleseat-mcp)
- [Fresha MCP](https://github.com/cotlet-connect/fresha-mcp)
- [Salonized MCP](https://github.com/cotlet-connect/salonized-mcp)
