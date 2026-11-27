# CleanTelligent API integration and MCP server: connect CleanTelligent to ChatGPT, Claude and any AI agent

**CleanTelligent API integration and MCP server by Cotlet.dev: yes, Claude, ChatGPT and other AI agents can work in your CleanTelligent account.** CleanTelligent (now Otuvy) has no official MCP server and no ChatGPT or Claude app; its own AI works inside the product, and it publishes no API documentation (checked 27 Nov 2026). Cotlet.dev connects your own signed-in CleanTelligent account to any AI agent, so you ask in plain words and the AI helps you find overdue inspections, turn deficiencies into work orders, clear old work orders and build the monthly client report.

## What you can ask

> List every site where an inspection is overdue this month and the three lowest scored sites last month. Draft the plan to fix the worst one.

> Take the deficiencies from yesterday's Oak Plaza inspection and create work orders, set the priority and assign each. Show me first.

> Which work orders are open for more than 7 days, by site and priority? Draft a status reply to each client contact.

> Build the monthly quality report for Riverside Medical from inspection scores and closed work orders and write a one page summary for their director.

> Set up the mobile survey for the Harbor office with the standard questions and tell me the response rate from the last survey.

## What quality managers and owners of commercial cleaning companies ask their AI to do in CleanTelligent

From quality managers and owners of commercial cleaning companies' posts on Reddit, forums, review sites and the CleanTelligent help center (checked 27 Nov 2026).

| Job | Ask your AI | Make it repeat |
|---|---|---|
| Inspections done and scores | "List every site where an inspection is overdue this month and the three lowest scored sites last month. Draft the plan to fix the worst one." | Weekly |
| Turn deficiencies into work orders | "Take the deficiencies from yesterday's Oak Plaza inspection and create work orders, set the priority and assign each. Show me first." | When you ask |
| Clean up the work order inbox | "Which work orders are open for more than 7 days, by site and priority? Draft a status reply to each client contact." | Daily |
| Monthly report for the client | "Build the monthly quality report for Riverside Medical from inspection scores and closed work orders and write a one page summary for their director." | Monthly |
| Client satisfaction survey | "Set up the mobile survey for the Harbor office with the standard questions and tell me the response rate from the last survey." | When you ask |
| Hours against the contract | "Show employee hours by site this week from time tracking and flag sites running over the contract hours." | Weekly |
| Daily task lists for the crew | "Create tomorrow's opening checklist for the Lakeside school in Frontline and list the task lists nobody completed yesterday." | When you ask |
| Notification rules that don't flood managers | "Show which email notifications are on for work orders and replies, and suggest the settings that cut duplicates for site managers." | When you ask |

Your AI asks you before it sends, books, replies or changes anything. Payments stay in your hands.

## CleanTelligent API

CleanTelligent (Otuvy) publishes no API documentation, and review sites disagree on whether an API exists, so access would have to be requested from Otuvy support (checked 27 Nov 2026). With Cotlet.dev you build your own CleanTelligent API: show your AI agent a job once in your signed-in CleanTelligent account, save it as an endpoint and call it from n8n, Make, Zapier or your own code. How it works: https://cotlet.dev/integrations/api/cleantelligent

Endpoints you can build in a few minutes (none of them exists until you build it):

- `aging-work-orders`: Open work orders older than 7 days by site and priority
- `create-work-orders`: Create work orders from an inspection after your OK

Call one with your Cotlet API key: `POST https://cotlet.dev/api/v1/custom/aging-work-orders`. One call runs up to 90 seconds, writes wait for your OK, payments are blocked.

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

This repo is a real MCP server. It speaks MCP over stdio, lists its CleanTelligent tools without any key, and forwards every tool call to the hosted Cotlet.dev server. The hosted URL above stays the recommended way. Use this when your client only starts local servers.

```json
{"mcpServers":{"cleantelligent":{"command":"uvx","args":["cleantelligent-mcp"],"env":{"COTLET_API_KEY":"your-key"}}}}
```

Or with Docker:

```bash
docker build -t cleantelligent-mcp .
docker run -i --rm -e COTLET_API_KEY cleantelligent-mcp
```

Tools: `cleantelligent_guide`, `cleantelligent_status`, `cleantelligent_open`, `cleantelligent_read`, `cleantelligent_click`, `cleantelligent_type`, `cleantelligent_close`. Without `COTLET_API_KEY` a tool call answers: "Set COTLET_API_KEY. Get a key at https://cotlet.dev/app".

## Command line

Python 3.10+, nothing else to install. From PyPI:

```bash
pip install cleantelligent-mcp        # or run it once: uvx cleantelligent-mcp connect
cleantelligent-mcp connect
```

Or straight from this repo:

```bash
export COTLET_API_KEY="your-key"
python3 cleantelligent_agent.py connect   # ready config for Claude Code, Cursor, ChatGPT
python3 cleantelligent_agent.py status    # is CleanTelligent ready in your Cotlet.dev account
python3 cleantelligent_agent.py guide     # CleanTelligent screens and sign-in link
python3 cleantelligent_agent.py read      # open CleanTelligent, read the page, close the browser
python3 cleantelligent_agent.py tools     # list Cotlet.dev tools
python3 cleantelligent_agent.py serve     # start the stdio MCP server (same as no argument)
```

## Questions from CleanTelligent users

**Does CleanTelligent have an API?**

CleanTelligent (Otuvy) publishes no API documentation, and review sites disagree on whether an API exists, so access would have to be requested from Otuvy support (checked 27 Nov 2026). With Cotlet.dev you build your own CleanTelligent API on your signed-in account and call it from n8n, Make or your own code: https://cotlet.dev/integrations/api/cleantelligent

**How do I connect CleanTelligent to ChatGPT?**

Add Cotlet.dev as an app in ChatGPT (Settings → Apps, connector URL `https://cotlet.dev/mcp/v1`), sign in to CleanTelligent once in your Cotlet.dev Cloud Browser, and ask ChatGPT in plain words. No code and no CleanTelligent API key.

**Is there a way to use Claude with CleanTelligent?**

Yes. In Claude Code run the one-line `claude mcp add` command above; in the Claude app add `https://cotlet.dev/mcp/v1` as a custom connector. Claude then works in your own CleanTelligent account.

**Is there a CleanTelligent MCP server?**

CleanTelligent has no official MCP server for your account. Cotlet.dev is a hosted MCP server that works in your own CleanTelligent account for you, with Claude, ChatGPT, Cursor, Codex, n8n, Make and Zapier.

**Does CleanTelligent (Otuvy) have an official MCP server?**

CleanTelligent has no official MCP server. It was renamed Otuvy, and its own AI is built into the product (insights in reports, a list creator in Frontline). Cotlet.dev works in your own Otuvy QM login in a browser (checked 27 Nov 2026).

**Is CleanTelligent the same as Otuvy?**

Yes. The product was renamed Otuvy, and many companies still sign in at their own cleantelligent.com address.

## Find it on

- [CleanTelligent on Cotlet.dev](https://cotlet.dev/integrations/mcp/cleantelligent?utm_source=github&utm_medium=readme&utm_campaign=cleantelligent&utm_content=find)
- [PyPI package cleantelligent-mcp](https://pypi.org/project/cleantelligent-mcp/)
- [Odel](https://odel.app/modules/cotlet/cleantelligent)
- [mcpbeat](https://mcpbeat.com/mcp-servers/cotlet/cleantelligent/)

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
- [Teachable MCP](https://github.com/cotlet-connect/teachable-mcp)
- [Vagaro MCP](https://github.com/cotlet-connect/vagaro-mcp)
- [Buildertrend MCP](https://github.com/cotlet-connect/buildertrend-mcp)
- [Zoopla MCP](https://github.com/cotlet-connect/zoopla-mcp)
- [Dribbble MCP](https://github.com/cotlet-connect/dribbble-mcp)
- [Hostelworld MCP](https://github.com/cotlet-connect/hostelworld-mcp)
- [Mashvisor MCP](https://github.com/cotlet-connect/mashvisor-mcp)
- [Rent Manager MCP](https://github.com/cotlet-connect/rent-manager-mcp)
- [ThriveCart MCP](https://github.com/cotlet-connect/thrivecart-mcp)
- [Fresha MCP](https://github.com/cotlet-connect/fresha-mcp)
- [Booksy MCP](https://github.com/cotlet-connect/booksy-mcp)
- [BookingKoala MCP](https://github.com/cotlet-connect/bookingkoala-mcp)
- [Milanote MCP](https://github.com/cotlet-connect/milanote-mcp)
- [Partiful MCP](https://github.com/cotlet-connect/partiful-mcp)
- [Contractor Foreman MCP](https://github.com/cotlet-connect/contractor-foreman-mcp)
- [Pixieset MCP](https://github.com/cotlet-connect/pixieset-mcp)
- [GlossGenius MCP](https://github.com/cotlet-connect/glossgenius-mcp)
- [ZenMaid MCP](https://github.com/cotlet-connect/zenmaid-mcp)
- [AroFlo MCP](https://github.com/cotlet-connect/aroflo-mcp)
- [Simpro MCP](https://github.com/cotlet-connect/simpro-mcp)
- [Workiz MCP](https://github.com/cotlet-connect/workiz-mcp)
- [Kickserv MCP](https://github.com/cotlet-connect/kickserv-mcp)
- [Service Autopilot MCP](https://github.com/cotlet-connect/service-autopilot-mcp)
- [Mindbody MCP](https://github.com/cotlet-connect/mindbody-mcp)
- [DaySmart MCP](https://github.com/cotlet-connect/daysmart-mcp)
- [Acuity Scheduling MCP](https://github.com/cotlet-connect/acuity-scheduling-mcp)
- [Jane App MCP](https://github.com/cotlet-connect/jane-app-mcp)
- [FieldEdge MCP](https://github.com/cotlet-connect/fieldedge-mcp)
- [Phorest MCP](https://github.com/cotlet-connect/phorest-mcp)
- [DINGG MCP](https://github.com/cotlet-connect/dingg-mcp)
- [Invoice Simple MCP](https://github.com/cotlet-connect/invoice-simple-mcp)
- [PhotoShelter MCP](https://github.com/cotlet-connect/photoshelter-mcp)
- [RazorSync MCP](https://github.com/cotlet-connect/razorsync-mcp)
- [ShootProof MCP](https://github.com/cotlet-connect/shootproof-mcp)
- [Invoice2go MCP](https://github.com/cotlet-connect/invoice2go-mcp)
- [Appointy MCP](https://github.com/cotlet-connect/appointy-mcp)
- [Contractor+ MCP](https://github.com/cotlet-connect/contractor-plus-mcp)
- [Petpooja MCP](https://github.com/cotlet-connect/petpooja-mcp)
- [Teachers Pay Teachers MCP](https://github.com/cotlet-connect/teachers-pay-teachers-mcp)
- [Bloom MCP](https://github.com/cotlet-connect/bloom-mcp)
- [hipages MCP](https://github.com/cotlet-connect/hipages-mcp)
- [ServiceSeeking MCP](https://github.com/cotlet-connect/serviceseeking-mcp)
- [MyJobQuote MCP](https://github.com/cotlet-connect/myjobquote-mcp)
- [MyBuilder MCP](https://github.com/cotlet-connect/mybuilder-mcp)
- [Yelp for Business MCP](https://github.com/cotlet-connect/yelp-for-business-mcp)
- [Rosie MCP](https://github.com/cotlet-connect/rosie-mcp)
- [Roofr MCP](https://github.com/cotlet-connect/roofr-mcp)
- [FieldPulse MCP](https://github.com/cotlet-connect/fieldpulse-mcp)
- [YourTradebase MCP](https://github.com/cotlet-connect/yourtradebase-mcp)
- [JobNimbus MCP](https://github.com/cotlet-connect/jobnimbus-mcp)
- [Ascora MCP](https://github.com/cotlet-connect/ascora-mcp)
- [eLocal MCP](https://github.com/cotlet-connect/elocal-mcp)
- [Alto MCP](https://github.com/cotlet-connect/alto-mcp)
- [Rightmove Plus MCP](https://github.com/cotlet-connect/rightmove-plus-mcp)
- [TurboTenant MCP](https://github.com/cotlet-connect/turbotenant-mcp)
- [Innago MCP](https://github.com/cotlet-connect/innago-mcp)
- [Property Finder MCP](https://github.com/cotlet-connect/property-finder-mcp)
- [LoopNet MCP](https://github.com/cotlet-connect/loopnet-mcp)
- [Dotloop MCP](https://github.com/cotlet-connect/dotloop-mcp)
- [SkySlope MCP](https://github.com/cotlet-connect/skyslope-mcp)
- [ShowingTime MCP](https://github.com/cotlet-connect/showingtime-mcp)
- [connectMLS MCP](https://github.com/cotlet-connect/connectmls-mcp)
- [WebPT MCP](https://github.com/cotlet-connect/webpt-mcp)
- [Tebra MCP](https://github.com/cotlet-connect/tebra-mcp)
- [ChiroTouch MCP](https://github.com/cotlet-connect/chirotouch-mcp)
- [ChiroFusion MCP](https://github.com/cotlet-connect/chirofusion-mcp)
- [Semble MCP](https://github.com/cotlet-connect/semble-mcp)
- [Nookal MCP](https://github.com/cotlet-connect/nookal-mcp)
- [Cliniko MCP](https://github.com/cotlet-connect/cliniko-mcp)
- [Gentu MCP](https://github.com/cotlet-connect/gentu-mcp)
- [splose MCP](https://github.com/cotlet-connect/splose-mcp)
- [Physitrack MCP](https://github.com/cotlet-connect/physitrack-mcp)
- [ALLDATA MCP](https://github.com/cotlet-connect/alldata-mcp)
- [Mitchell 1 ProDemand MCP](https://github.com/cotlet-connect/mitchell1-mcp)
- [Identifix MCP](https://github.com/cotlet-connect/identifix-mcp)
- [Worldpac speedDIAL MCP](https://github.com/cotlet-connect/worldpac-mcp)
- [RepairLink MCP](https://github.com/cotlet-connect/repairlink-mcp)
- [PartsTech MCP](https://github.com/cotlet-connect/partstech-mcp)
- [Fullbay MCP](https://github.com/cotlet-connect/fullbay-mcp)
- [Shopmonkey MCP](https://github.com/cotlet-connect/shopmonkey-mcp)
- [HotSchedules MCP](https://github.com/cotlet-connect/hotschedules-mcp)
- [Homebase MCP](https://github.com/cotlet-connect/homebase-mcp)
- [Sling MCP](https://github.com/cotlet-connect/sling-mcp)
- [Tanda MCP](https://github.com/cotlet-connect/tanda-mcp)
- [Rotaready MCP](https://github.com/cotlet-connect/rotaready-mcp)
- [Clover MCP](https://github.com/cotlet-connect/clover-mcp)
- [Epos Now MCP](https://github.com/cotlet-connect/eposnow-mcp)
- [SpotOn MCP](https://github.com/cotlet-connect/spoton-mcp)
- [Olo MCP](https://github.com/cotlet-connect/olo-mcp)
- [SevenRooms MCP](https://github.com/cotlet-connect/sevenrooms-mcp)
- [MaidCentral MCP](https://github.com/cotlet-connect/maidcentral-mcp)
- [Launch27 MCP](https://github.com/cotlet-connect/launch27-mcp)
- [WinTeam MCP](https://github.com/cotlet-connect/winteam-mcp)
- [Clean Smarts MCP](https://github.com/cotlet-connect/cleansmarts-mcp)
- [CleanerPlanner MCP](https://github.com/cotlet-connect/cleanerplanner-mcp)
- [Squeegee MCP](https://github.com/cotlet-connect/squeegee-mcp)
- [MoeGo MCP](https://github.com/cotlet-connect/moego-mcp)
- [Gingr MCP](https://github.com/cotlet-connect/gingr-mcp)
- [PawFinity MCP](https://github.com/cotlet-connect/pawfinity-mcp)
- [All apps connected through Cotlet.dev](https://github.com/cotlet-connect)

---

More: https://cotlet.dev/integrations/mcp/cleantelligent?utm_source=github&utm_medium=readme&utm_campaign=cleantelligent

Start free: https://cotlet.dev/signup?app=cleantelligent&utm_source=github&utm_medium=readme&utm_campaign=cleantelligent
