# AlphaCitation reference lookup

**Is a medicine approved in a market?** This MCP server answers that, for one medicine, one market and one date, from the
regulator's record where one is held. It is open: no sign-in, no key and no fee.

- MCP endpoint (Streamable HTTP, no authentication): `https://alphacitation.com/api/v1/reference/mcp`
- Registry name: `com.alphacitation/reference`
- Documentation: https://alphacitation.com/reference#mcp

This repository holds no code. It holds this description, the Gemini CLI extension manifest and the licence for this text.

## What it answers

The `lookup_medicine_evidence` tool takes:
- a medicine, by brand or active substance;
- a market, as an ISO country code or EU;
- whether you are asking if it is registered or authorised (P1), or authorised for a listed indication (P2);
- optionally, a date (today, UTC, if left out; never a future date).

It answers with:
- a status: settled, settling_record_identified, supported_attested, reference_only or unresolved;
- the record that settles the claim where one is held, with its pointer URL;
- where that market's label differs from the US and EU labels;
- what remains unresolved.

An unresolved answer means unknown, never false.

Where records come from:
- **US:** the US prescribing information on DailyMed, as submitted to the FDA.
- **EU and EEA:** the European Commission's records, for centrally authorised medicines.
- **Elsewhere:** the answer names the regulator where one is identified, and lists US and EU labels as references only. The claim is unresolved there, or supported, never settled, by an authorisation holder's own label.

A second tool, `lookup_precedent_ruling`, returns one numbered AlphaCitation ruling on an evidence conflict (ACPREC-YYYY-NNNN), with a
fresh lookup of the current record where the ruling names a medicine, market and date. A ruling is not evidence and decides no claim
today.

## Connect it

**Claude (web or desktop):** Customize, then Connectors, then "+ Add" and "Add custom connector". Name it AlphaCitation, paste
`https://alphacitation.com/api/v1/reference/mcp` as the URL and add it. It needs no sign-in. On Team and Enterprise plans, an owner adds it under
Organization settings, then Connectors; members then connect it under Customize, then Connectors.

**Claude Code:**

```
claude mcp add --transport http alphacitation https://alphacitation.com/api/v1/reference/mcp
```

**VS Code with GitHub Copilot:** add this to `~/.copilot/mcp-config.json` (or to `.mcp.json` at the workspace root):

```json
{
  "mcpServers": {
    "alphacitation": { "type": "http", "url": "https://alphacitation.com/api/v1/reference/mcp", "tools": ["*"] }
  }
}
```

**Gemini CLI:**

```
gemini extensions install https://github.com/alphacitation/alphacitation-reference
```

**ChatGPT (on the web, where your plan and workspace allow custom MCP servers):** go to chatgpt.com/plugins, choose the plus button,
then "Add custom MCP server". Name it AlphaCitation, paste the endpoint above as the server URL, choose no authentication and create it
as a plugin.

## Example prompts

- Is Mounjaro approved in the United States today?
- Is Wegovy authorised in the EU for weight management? Cite the record.
- Is Mounjaro registered in the United Arab Emirates, and what is still unresolved?

## Limits

- Read-only.
- Up to 240 requests an hour from one address; an optional free API key raises the limit.
- One medicine, one market and one question per request. No listing, search or bulk access.
- Answers are regulatory information, not medical advice.
- An answer is AlphaCitation's assessment of the record it cites, for one claim, market and date. It is not an independent verification
  of the claim.

The same lookup is a REST API with an OpenAPI document: https://alphacitation.com/api/v1/reference/openapi.json

## Links

- Documentation: https://alphacitation.com/reference#mcp
- Privacy policy: https://alphacitation.com/privacy
- Terms: https://alphacitation.com/terms

## Licence

The text of this README is licensed under CC BY 4.0 (see LICENSE). The service and its answers are governed by the terms at
https://alphacitation.com/terms and the open-interface terms at https://alphacitation.com/reference#limits.
