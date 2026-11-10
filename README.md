# How to connect Booksy to Claude Code, ChatGPT and any AI agent

**Yes, Claude, ChatGPT and other AI agents can work in your Booksy account.** Booksy's official ChatGPT app helps clients find professionals and see live open slots, with the final booking confirmed on Booksy. Cotlet.dev connects your own signed-in Booksy business account to any AI agent, so you can ask in plain words and the AI books, reschedules and follows up with clients for you.

## What you can ask

> Book Maria Lopez for a skin fade with Tony on Thursday at 4 pm.

> What does Saturday look like per staff member? List open slots longer than 30 minutes.

> Ryan cancelled his Tuesday 11 am. Find clients who often book Tuesday mornings and offer them the slot.

> Which clients haven't booked in 60 days? Make a list with their last service and visit date.

> Move all of Kevin's Friday appointments to Dana and message the clients.

## Connect in 2 minutes

Get your API key at https://cotlet.dev/app and export it as `COTLET_API_KEY`.

### Claude Code

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

### ChatGPT

Settings → Apps → add connector URL `https://cotlet.dev/mcp/v1`, then sign in with Cotlet.dev. No API key needed.

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

## Questions from Booksy users

**Does Booksy have a public API for developers?**

You don't need one. Cotlet.dev works in your own signed-in Booksy account, so your AI agent can read and change your bookings and client list without any API key from Booksy.

**How do I connect Booksy to ChatGPT?**

Booksy's own ChatGPT app is for clients finding professionals and open slots. To manage your business account, add Cotlet.dev as an app in ChatGPT (Settings → Apps, connector URL `https://cotlet.dev/mcp/v1`), sign in to Booksy once in your Cotlet.dev Cloud Browser, and ask ChatGPT in plain words. No code and no Booksy API key.

**Is there a way to use Claude with Booksy?**

Yes. Booksy has no official connector in Claude's directory, but in Claude Code you can run the one-line `claude mcp add` command above; in the Claude app add `https://cotlet.dev/mcp/v1` as a custom connector. Claude then works in your own Booksy account.

**Is there a Booksy MCP server?**

Yes. Cotlet.dev is a hosted MCP server that works in your Booksy account for you, 24/7, with Claude, ChatGPT, Cursor, Codex, n8n, Make and Zapier.

## Find it on

- [Booksy on Cotlet.dev](https://cotlet.dev/integrations/booksy-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=booksy&utm_content=find)
- [PyPI package booksy-mcp](https://pypi.org/project/booksy-mcp/)
- [Odel](https://odel.app/modules/cotlet/booksy)
- [Step-by-step guide (PDF) on Internet Archive](https://archive.org/details/cotlet-connect-booksy-to-ai-agents-guide)

## Also works with

- [Mangomint MCP](https://github.com/cotlet-connect/mangomint-mcp)
- [Wanderlog MCP](https://github.com/cotlet-connect/wanderlog-mcp)
- [SmugMug MCP](https://github.com/cotlet-connect/smugmug-mcp)
- [Garmin Connect MCP](https://github.com/cotlet-connect/garmin-mcp)
- [Housecall Pro MCP](https://github.com/cotlet-connect/housecall-pro-mcp)
- [Medium MCP](https://github.com/cotlet-connect/medium-mcp)
- [Crexi MCP](https://github.com/cotlet-connect/crexi-mcp)
- [DistroKid MCP](https://github.com/cotlet-connect/distrokid-mcp)
- [LinkedIn MCP](https://github.com/cotlet-connect/linkedin-mcp)
- [EstimateOne MCP](https://github.com/cotlet-connect/estimateone-mcp)
- [Google Search Console MCP](https://github.com/cotlet-connect/google-search-console-mcp)
- [Lichess MCP](https://github.com/cotlet-connect/lichess-mcp)
- [Strava MCP](https://github.com/cotlet-connect/strava-mcp)
- [StreamElements MCP](https://github.com/cotlet-connect/streamelements-mcp)
- [Meetup MCP](https://github.com/cotlet-connect/meetup-mcp)
- [AccuRanker MCP](https://github.com/cotlet-connect/accuranker-mcp)
- [GetYourGuide MCP](https://github.com/cotlet-connect/getyourguide-mcp)
- [Idealista MCP](https://github.com/cotlet-connect/idealista-mcp)
- [Riverside MCP](https://github.com/cotlet-connect/riverside-mcp)
- [Udemy MCP](https://github.com/cotlet-connect/udemy-mcp)
- [Faire MCP](https://github.com/cotlet-connect/faire-mcp)
- [Buildertrend MCP](https://github.com/cotlet-connect/buildertrend-mcp)
- [Fresha MCP](https://github.com/cotlet-connect/fresha-mcp)
- [BookingKoala MCP](https://github.com/cotlet-connect/bookingkoala-mcp)
- [All apps connected through Cotlet.dev](https://github.com/cotlet-connect)

---

More: https://cotlet.dev/integrations/booksy-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=booksy

Start free: https://cotlet.dev/signup?app=booksy&utm_source=github&utm_medium=readme&utm_campaign=booksy
