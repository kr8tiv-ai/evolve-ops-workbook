# Evolve Ops Workbook + Router — the backend template

**The spine of the [Evolved](https://github.com/kr8tiv-ai/evolved) system, as a template you can copy.** A 20-tab operations workbook and a tiny secret-gated Apps Script **Router** that lets the field app and the owner dashboard read and write it — without ever holding a Google credential. Everything here is synthetic and MIT; you bring your own sheet and your own secret.

> **Part of a complete, free, open-source system for any service business** — one Google Sheet is the spine, and four surfaces share it:
> 🧠 **MCP** ([kr8tiv-ai/evolved](https://github.com/kr8tiv-ai/evolved)) · ✋ **field app** ([kr8tiv-ai/evolve-field-app](https://github.com/kr8tiv-ai/evolve-field-app)) · 📊 **workbook + router** (you are here) · 📈 **dashboard** ([kr8tiv-ai/evolve-dashboard](https://github.com/kr8tiv-ai/evolve-dashboard), live at [ops.evolveecoblasting.com](https://ops.evolveecoblasting.com)). Full stand-up guide: [evolved/docs/STAND-UP-YOUR-OWN.md](https://github.com/kr8tiv-ai/evolved/blob/main/docs/STAND-UP-YOUR-OWN.md).

## Quick start (~15 minutes)

**Prerequisites:** a Google account. That's it — no server, no database.

1. **Make your workbook.** Create a new Google Sheet. For each CSV in
   [`workbook-template/`](workbook-template/), **File → Import → Insert new
   sheet(s)** so each becomes a tab. (The data is synthetic sample content —
   clear it and keep the headers, or start from it and edit.) This 20-tab
   structure is exactly what the MCP, field app, and dashboard expect.
2. **Add the Router.** In your sheet: **Extensions → Apps Script**, paste
   [`router.gs`](router.gs).
3. **Generate your OWN secret** (`openssl rand -hex 24`) and set it in Apps
   Script → **Project Settings → Script properties** as `ROUTER_SECRET`.
4. **Run `setup()` once** (it verifies your secret is set — it will not invent
   one), then **Deploy → New deployment → Web app**, execute as *Me*, access
   *Anyone* (the secret is the gate). Copy the `/exec` URL.
5. **Point the surfaces at it** — put that URL + your secret into the field app
   and dashboard env. Done.

Prefer generating the tabs programmatically? The MCP repo ships
[`scripts/make-workbook-template.mjs`](https://github.com/kr8tiv-ai/evolved/blob/main/scripts/make-workbook-template.mjs),
which writes these same tabs for any trade.

## The Router API

`POST` a JSON body to the web-app URL (the `secret` gates every call):

| Action | Body | Does |
|---|---|---|
| `ping` | `{secret, action:"ping"}` | health check |
| `readTab` | `{secret, action:"readTab", tab, maxRows}` | read a tab's rows |
| `writeRow` | `{secret, action:"writeRow", tab, row, values:[…]}` | overwrite one row |
| `appendRow` | `{secret, action:"appendRow", tab, values:[…]}` | append a row |
| `setCell` | `{secret, action:"setCell", tab, a1, value}` | set one cell |

## Two safety choices baked in

Both are real footguns a naive router hits:

- **The secret is never written to the execution log.** Logging it would leak
  it to anyone who can view executions. Actions are logged; credentials are not.
- **`setup()` refuses to auto-generate a missing secret.** A router that mints a
  fresh random secret when the property is absent silently breaks every consumer
  at once, with no copy of the new value anywhere. You set it; the template
  verifies it.

## The 20 tabs

Start Here · Quotes · Customers · Leads · Dispatch · Expenses · Invoices ·
Inventory · Suppliers · Crew · Time Log · Job Photos · Field Notes ·
Safety (FLHA) · Reviews · Action Items · To-Do · Rate Table · Job P&L · Record Log.

## Boundary

Synthetic and template-only — no real customer data, workbook IDs, router URLs,
or secrets. Bring your own. MIT licensed; take it and run your company on it.
