# MimikFlow setup skill

Set up a [MimikFlow](https://mimikflow.com) LinkedIn prospecting account end to end through the official MimikFlow MCP connector.

The skill covers the whole path: checking whether the person already has an account, signing up or connecting the connector, connecting LinkedIn, writing an offer and an ideal customer profile tuned for volume, setting the sending parameters, opening the extra lead sources, queuing the first prospects, and going live deliberately.

It also carries MimikFlow's own guardrails: no exclusion list proposed on the agent's own initiative, no validation mode, and no LinkedIn safety limit reshaped from a chat.

## Install

Install with the open Agent Skills CLI:

```bash
npx skills add mohamedalani/mimikflow-skills --skill mimikflow-setup
```

Or copy this folder into your agent's skills directory:

- Claude Code / Claude Desktop: `~/.claude/skills/mimikflow-setup/`
- Project-local Claude skill: `.claude/skills/mimikflow-setup/`
- Any Agent Skills-compatible runtime: install the folder containing `SKILL.md`

## Requires

The [MimikFlow](https://mimikflow.com) MCP connector on Pro-level access (the free trial counts). Add `https://mimikflow.com/api/mcp` as a custom connector and authorize it, or paste the secret connection link from **Settings > MCP**.

## Files

- `SKILL.md` — setup flow, step by step.
- `references/tools.md` — the connector tools, grouped.
- `references/targeting.md` — writing a target that produces volume.
- `references/settings.md` — every writable setting and its values.
- `references/troubleshooting.md` — the “it doesn't work” decision tree.
- `references/vocabulary.md` — internal IDs to product words, in French and English.

## Official links

- Product: https://mimikflow.com
- MCP endpoint: https://mimikflow.com/api/mcp
- Support: hello@mimikflow.com
