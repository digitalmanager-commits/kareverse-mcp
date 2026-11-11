# Kareverse MCP server

[Kareverse](https://www.kareverse.com) provides elder care at home in Bangalore (Bengaluru), India, for families, including NRI children living abroad. This repository holds the public connection details and the Hermes Agent plugin for the **Kareverse MCP server**, which lets AI assistants look up Kareverse services and prices and ask the Kareverse team to call a family back.

- **Server URL:** `https://www.kareverse.com/mcp`
- **Transport:** Streamable HTTP (MCP 2024-11-05 to 2025-11-25 with `initialize`, and 2026-07-28 stateless)
- **Authentication:** none. The server is public.
- **Documentation:** https://www.kareverse.com/agents

## Tools

| Tool | What it does | Changes anything? |
| --- | --- | --- |
| `search_services` | Search services by need, optionally by category; returns price, Care Plan member price and page URL | No |
| `get_service` | Full details of one service: inclusions, exclusions, price options, notice, FAQs | No |
| `get_care_plan` | The monthly Care Plan, billing terms and condition programmes | No |
| `check_service_area` | Check a Bangalore locality or pincode, or list the service areas | No |
| `search_guides` | Search Kareverse's published guides for families | No |
| `get_contact_info` | Phone/WhatsApp, email, hours, address and service categories | No |
| `submit_enquiry` | Sends a callback request (name, phone, need) to the Kareverse team | **Yes**, creates an enquiry |

Prices are in Indian rupees and match www.kareverse.com, with a `prices_as_of` month in each answer.

## Install

### Hermes Agent

```bash
hermes plugins install kareverse
```

The package (`plugin.json` + `mcp.json` + `skills/kareverse`) adds the `kareverse` server and a skill describing how to use it. It marks the server `trust: untrusted`, so Hermes asks you before `submit_enquiry` runs.

### Any MCP client

```json
{
  "mcpServers": {
    "kareverse": {
      "type": "streamable-http",
      "url": "https://www.kareverse.com/mcp"
    }
  }
}
```

Claude Code: `claude mcp add --transport http kareverse https://www.kareverse.com/mcp`

## Data and privacy

- **Network:** every tool call goes over HTTPS to `www.kareverse.com`, operated by Kareverse (Phanes Technology Solutions Pvt Ltd). Nothing else is contacted.
- **Read-only tools** send only the search words and ids you pass in.
- **`submit_enquiry`** sends the name, phone number and any optional details (email, service, area, message) to Kareverse, where the team uses them to contact the person about this enquiry, as described in the [privacy policy](https://www.kareverse.com/privacy). The tool requires `consent: true`, which the agent should set only after the person agrees to be contacted. Enquiries are rate-limited per connection.
- **Credentials:** none. No API key, token or secret is needed or stored.
- **Telemetry:** none in this package. The server keeps standard web server logs.
- **Code in this package:** configuration and Markdown only. No Python, binaries, dependencies or self-update code. The tool set is served by the remote server at runtime.

Kareverse gives no medical advice and is not an emergency service. In a medical emergency in India, call 108 or 112.

## Support

connect@kareverse.com · WhatsApp +91 90355 54540

## Licence

MIT, see [LICENSE](LICENSE).
