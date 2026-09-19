---
name: mimikflow-setup
description: MimikFlow LinkedIn prospecting - set up a new account end to end through the MimikFlow MCP connector, retune an existing campaign for volume, or diagnose one that finds nobody, sends nothing or gets no replies. Use when someone connects the MimikFlow connector, wants to start prospecting, or says their campaign is not working. FR and EN.
---

# MimikFlow setup

[MimikFlow](https://mimikflow.com) runs LinkedIn B2B prospecting 24/7: it finds prospects matching an
ideal customer profile, sends invitations, writes a personalized first
message, sends spaced reminders, and lets an AI answer prospects and book
calls. This skill configures that machine through the MCP connector, for
**volume**: a wide funnel on people the user actually sells to.

## Guardrails

These beat your defaults. They hold for every branch of this skill.

1. **Setup saves, it never sends.** Outreach starts at step 7, on an explicit
   go, and never before.
2. **Never propose an exclusion.** `my_anti_target` only when the user asked
   for that exact exclusion in their own words - see `references/targeting.md`.
3. **Never propose validation mode** (`cold_dm_mode` / `setter_mode` =
   `semi_manual`). It holds every AI message for approval and turns an
   autonomous machine into a daily manual inbox. Only if the user names it.
4. **No volume cap is writable**, by design. Sending limits belong to the
   LinkedIn account's plan and are a human decision taken in the app. Read
   them, explain them, never present them as something you can lift.
5. **Confirm before any write** that changes targeting, queues prospects or
   sends a message. Show the exact text first.
6. **Product words only.** Pipeline ids, setting keys, status codes and tool
   names are for tool calls - `references/vocabulary.md` has what to say
   instead, FR and EN.
7. **Interactive use only.** No recurring polling, no scheduled sweeps, no
   extraction into a dashboard.

## Step 0 - Connector

Skip if the MimikFlow tools are already available in this session.

Otherwise, give the user their path and wait:

- **OAuth (recommended):** add `https://mimikflow.com/api/mcp` as a custom
  connector, then authorize. No secret to paste.
- **Secret link:** MimikFlow > Settings > MCP tab > generate the link, paste
  that full URL as the endpoint. It carries a secret: never echo it back.
- **No MimikFlow account:** sign up at `https://mimikflow.com`, then come
  back. The connector needs a Pro-level access; the free trial counts, the
  Découverte plan does not.

A brand-new account exposes only four tools until LinkedIn is connected and
the trial starts. That is expected, not a bug: the rest appears by itself.

**Done when** a MimikFlow tool call returns a result.

## Step 1 - New or existing

```
get_onboarding_status
```

Follow **only** its `next_action`:

| `next_action` | Go to |
| --- | --- |
| `prepare_setup` | Step 2 |
| `connect_linkedin` / `reconnect_linkedin` | Step 3 |
| `access_unavailable` | Stop. Point to the app or hello@mimikflow.com |
| `preview_prospects` | Existing user: step 4, or `references/troubleshooting.md` if they came with a complaint |

Several LinkedIn accounts (agency): run `list_accounts` then `list_campaigns`,
and pass an explicit `campaign_id` in every call from here on.

**Done when** you have one `next_action` and, where several campaigns exist,
the `campaign_id` you will work on.

## Step 2 - Offer, target, voice

1. Ask for a public website URL (or a three-line brief) and what a won deal
   looks like - a booked call, usually.
2. `prepare_onboarding_setup` with `website` and any facts already given in
   `draft`. Nothing is saved.
3. Show the proposal in plain language: who they are, what they sell, who they
   want to reach, the market, the voice. Ask them to correct it.
4. **Rewrite `my_target` yourself** before saving. The auto-proposal is a
   draft, and targeting decides the volume: apply `references/targeting.md`.
5. `save_onboarding_setup`, `approved: true`, only the validated fields.

The proposal carries no exclusion list, on purpose. Do not add one.

**Done when** the user has said, about the target text you are saving, that
these are the people they want to talk to.

## Step 3 - LinkedIn

`get_linkedin_connection_link` returns a secure link. Give it, explain it
opens LinkedIn's own login, then wait. A dead session is the most common cause
of "nothing is happening" weeks later.

**Done when** `get_onboarding_status` itself reports LinkedIn connected -
never because the link was created.

## Step 4 - Preview

```
search_prospects  query: "<the target in plain language>"  count: 10-20
```

Contacts and saves nobody. Read the profiles with the user. Off-target results
mean the target text is wrong: fix `my_target`, never add exclusions. Thin
results mean the query is too narrow or too jargon-heavy - try the buyer's job
title plus their sector plus a city.

**Done when** the user recognizes their market in the list.

## Step 5 - Tune for volume

Values, enums and defaults: `references/settings.md`.

1. **Markets** - `target_countries`, `target_country_mode` (`equal` when every
   market counts the same). `target_regions` only for a genuinely local
   business: it narrows hard.
2. **Quality gates** - the machine drops profiles with no photo, under 50
   connections, or an open-to-work banner, before any AI reads them. On a
   young or non-Western audience those gates eat real volume: consider
   `lead_filter_min_connections` at 0-20, and `lead_filter_require_photo`
   `false` if the user accepts more noise.
3. **Extra sources** - `toggle_pipeline` `on` for `engagement_scraper`,
   `profile_views`, `invite_manager`. `first_degree_scanner` is the biggest
   source of all but it is Pro-paid: locked on trial and Découverte, and the
   tool refuses. Do not promise it before it is available.
4. **First message** - formality, length, emoji, CTA mode. `opener_only`
   converts better at the top of the funnel than asking for a call in message
   one. The base style is one choice over two keys: a preset, or the user's
   example message. Setting both ships the example and silently drops the
   preset they picked.
5. **Follow-ups** - `timing_max_follow_ups` 4 to 6 is the volume-friendly
   range. Fewer follow-ups shortens the sequence, it does not space it out.
6. **Sending windows** - wide days and hours per step, in `user_timezone`.
   Invitations already run every day 8h-23h and are not adjustable.
7. **Leave custom instruction fields empty** unless a specific observed
   pattern calls for one. Empty means MimikFlow's built-in behavior applies:
   varied reminders, respects a no, stops on silence, escalates. An empty
   optional field is never a gap and never a cause.

**Done when** each of the seven is either set or consciously left at its
default, and the user knows which trade-offs you took.

## Step 6 - Prime the pipeline

- `search_prospects` on two or three angles, then
  `add_prospects_to_campaign` with the validated URLs (100 max per call). A
  long list simply lands over several days; queued prospects cost nothing
  until the machine can afford to read them.
- A named individual gets `filter_mode: "all"` - the ICP filter is built for
  bulk sourcing and would reject a hand-picked contact. Their brief goes in
  `cold_dm_instructions`, scoped to that one person.
- `set_latest_update` with fresh news, a case study or a new offer: it gives
  silent leads something new to receive.
- `list_media`: an offer that needs a demo or a case study should have one
  wired. Media and the messages around a booked meeting are the two things
  almost every user leaves empty, and both convert.

**Done when** the campaign holds a batch of queued prospects the user
validated.

## Step 7 - Go live

1. Recap in plain words: who gets contacted, from where, what the first
   message says, how many reminders, on which days and hours.
2. Ask for an explicit go.
3. `toggle_pipeline` `on`: `lead_scraper`, `connection_sender`,
   `cold_dm_sender`, `follow_up`, `setter`.
4. `run_pipeline_now` `lead_scraper` for an immediate first batch. Daily
   safety limits still apply: "now" never means "more".
5. `check_campaign_setup` and read the findings aloud.

**Done when** `check_campaign_setup` returns no `blocked` finding.

## Step 8 - What happens next

Day 1-2 prospects found and invitations going out. Day 2-5 first acceptances,
then the first messages. Week 1-2 replies, AI conversations, first bookings.
Nothing is instant: LinkedIn caps invitations per week and messages per day,
and that ceiling comes from their LinkedIn plan.

Offer a day-2 check-in: `get_account_overview`, `get_pipeline_stats`,
`list_escalations`, `list_pending_messages`.

## References

- `references/targeting.md` - read before writing any `my_target` or
  `my_anti_target`.
- `references/settings.md` - read before any `update_pipeline_settings` or
  `update_campaign_profile` call.
- `references/troubleshooting.md` - read when the user says results are bad,
  nothing is happening, or they are disappointed.
- `references/tools.md` - the 47 connector tools, when a step does not name
  the one you need.
- `references/vocabulary.md` - internal id to product name, FR and EN.

Anything about the app's own screens (calendar, broadcast, billing, plan
availability): call `search_help` rather than guessing.
