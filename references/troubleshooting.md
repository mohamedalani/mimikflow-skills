# "It doesn't work"

Almost every complaint about results is a setting, not the machine. Answering
with a settings fix beats any reassurance.

## Always start here

```
check_campaign_setup   (campaign_id when several exist)
```

It returns two different things, and they must not be treated the same way:

- **`findings`** are facts the product already decided, each with a plain
  meaning. A `blocked` severity means nothing can leave right now: say that
  first, plainly, with what to do about it.
- **`targeting`** is raw text nothing has judged. That part is yours to read -
  see `targeting.md`.

It runs no model of ours and has no daily budget, so take the time to do the
analysis properly.

## Decision tree

**No prospects found**
- Dead LinkedIn session -> `get_linkedin_connection_link`, reconnect.
- Prospect search turned off -> `toggle_pipeline` `lead_scraper` `on`.
- Targeting too narrow, stacked or invisible criteria -> rewrite `my_target`.
- An anti-target nobody remembers writing -> read it back to the user; it
  vetoes every profile the machine reads.
- Region text naming a place outside the selected countries.
- Quality gates eating the funnel -> `lead_filter_min_connections`,
  `lead_filter_require_photo`.

**Prospects found, no invitations**
- Connection requests turned off, or the LinkedIn account's weekly ceiling is
  reached (readable, not writable - explain it).
- LinkedIn is throttling the account: the pace backs off on purpose.

**Invitations accepted, no first message**
- First message step turned off.
- Validation mode on (`cold_dm_mode` = `semi_manual`): drafts pile up in
  `list_pending_messages` waiting for a human. The most common invisible
  blocker. Rejecting a first-message draft also drops the prospect -
  `restore_leads` is the way back.
- A two-hour sending window.
- `send_first_message` sends one now, inside the daily budget.

**Messages sent, few replies**
- Default diagnosis: the first message does not fit the people actually being
  contacted. Read real threads (`get_conversation` on leads stuck at first
  contact), look at who those people are (`list_leads`), compare with
  `my_target`. Then propose exact values: formality, length, emoji, CTA mode,
  the hook angle in `custom_cold_dm_instructions`, or a sharper `my_target`.
  One change at a time, confirmed, re-checked after a week or two of sends.

**Replies, no meetings**
- The AI that answers prospects is off, or the plan does not include it.
- No calendar connected and no booking link -> `get_campaign_profile` shows
  the `calendar` block. A connected calendar makes `goal_link` optional: do
  not call booking broken when a calendar is connected.
- Escalations waiting on a human -> `list_escalations`.

**Meetings booked, nobody shows up**
- Nothing is set around the meeting -> `update_meeting_moments`, or the
  meeting keys of `update_pipeline_settings`. A confirmation right after
  booking, a reminder the day before, and a message adapted to whether the
  prospect showed up are what turn booked calls into calls that happen.

**WhatsApp is quiet**
- `list_whatsapp_numbers` gives a plain-language reason: warm-up silence after
  connecting, ramp-up, or an automatic pause. Explain it. It is WhatsApp's own
  rule against numbers that behave like machines, never something to work
  around, and never a broken channel.

## Two framing rules that matter

**Never assert a defect.** The conversation history MimikFlow stores is a
synced copy of the LinkedIn thread. A message stored twice shows as
`stored_copies > 1`; a timestamp can be missing. If something looks broken or
absurd, present it neutrally as a probable sync artifact and suggest checking
the real thread on LinkedIn. Never report it as a bug or as messages the
machine actually sent.

**Never call an empty optional field the cause** (SKILL.md, step 5.7). Suggest
filling one only against a specific recurring pattern in the user's own
conversations.
