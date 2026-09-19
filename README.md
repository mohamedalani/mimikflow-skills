# Launch and optimize MimikFlow prospecting with your AI agent

Turn a simple description of your ideal customer into a reviewed, ready-to-launch [MimikFlow](https://mimikflow.com) LinkedIn prospecting campaign.

This official skill helps your AI agent configure a new account, improve the volume of an existing campaign, or diagnose why prospects, invitations or replies are not coming through. It works through the official MimikFlow MCP connector and asks for approval before making consequential changes.

## What you get

- **Launch faster:** move from an offer and target market to a configured campaign.
- **Find more relevant prospects:** tune targeting, markets, lead sources and quality filters without narrowing the funnel unnecessarily.
- **Fix campaigns that stall:** identify where volume is being lost and what to change next.
- **Keep control:** preview prospects, review proposed settings and explicitly approve outreach before anything goes live.

## Example

> **You:** Set up MimikFlow to reach French B2B SaaS founders with 10–50 employees. Keep the first message short and conversational.
>
> **Your agent:** prepares the target, previews matching profiles, recommends the campaign settings, queues only the prospects you approve, and asks for confirmation before going live.

The same skill can also handle requests such as:

- “Why is my campaign finding almost nobody?”
- “Help me increase prospect volume without targeting random people.”
- “Review my MimikFlow setup before I launch.”

## Install

```bash
npx skills add mohamedalani/mimikflow-skills --skill mimikflow-setup
```

Then connect the [official MimikFlow MCP connector](https://mimikflow.com/api/mcp) and tell your agent who you want to reach.

You can also copy the repository into your agent's skills directory:

- Claude Code or Claude Desktop: `~/.claude/skills/mimikflow-setup/`
- Project-local Claude skill: `.claude/skills/mimikflow-setup/`
- Any Agent Skills-compatible runtime: install the folder containing `SKILL.md`

## What the skill handles

1. Checks whether the MimikFlow account and LinkedIn connection are ready.
2. Turns the offer and ideal customer into a practical targeting brief.
3. Previews matching prospects before saving anyone.
4. Tunes markets, lead sources, quality filters, messaging and follow-ups.
5. Queues an approved first batch of prospects.
6. Runs a final setup check and asks for an explicit go before launch.
7. Diagnoses campaigns that find nobody, send nothing or receive few replies.

Detailed guidance is included for targeting, campaign settings, troubleshooting, product vocabulary and all supported MCP tools.

## Built-in safeguards

- Setup never starts outreach by itself.
- Targeting, queued prospects and message changes require confirmation.
- The skill never claims it can override LinkedIn safety limits.
- It does not invent exclusion lists or enable manual validation mode unless requested.
- Secret MCP connection links are never repeated back to the user.

## Requirements

A [MimikFlow](https://mimikflow.com) account with Pro-level access. The free trial counts. Add `https://mimikflow.com/api/mcp` as a custom connector and authorize it, or use the secret connection link available under **Settings > MCP**.

## Included files

- `SKILL.md` - the complete setup, optimization and diagnostic workflow.
- `references/targeting.md` - targeting that preserves useful volume.
- `references/settings.md` - writable campaign settings and trade-offs.
- `references/troubleshooting.md` - the low-volume diagnostic tree.
- `references/tools.md` - the available connector tools.
- `references/vocabulary.md` - product language in French and English.

## Official links

- [MimikFlow](https://mimikflow.com)
- [MimikFlow MCP connector](https://mimikflow.com/api/mcp)
- [Install from skills.sh](https://www.skills.sh/mohamedalani/mimikflow-skills/mimikflow-setup)
- Support: hello@mimikflow.com
