# MimikFlow MCP tools (47)

Everything here goes through MimikFlow's own service layer: caps, dedup,
pending drafts and LinkedIn limits always apply. There is no bypass.

## Setup (visible even before the trial starts)

| Tool | Use |
| --- | --- |
| `get_onboarding_status` | Always first. Returns one `next_action`; follow only that. |
| `prepare_onboarding_setup` | One unsaved proposal from a website or a brief. Proposes no exclusion, on purpose. Limited runs per day per campaign. |
| `save_onboarding_setup` | Saves only approved fields. Requires `approved: true`. Starts nothing. |
| `get_linkedin_connection_link` | Secure connect/reconnect link. Re-check status afterwards; creating the link proves nothing. |

## Control tower

| Tool | Use |
| --- | --- |
| `get_account_overview` | State of the machine: which steps are on, LinkedIn account and its plan, locked sources. An off source is usually a deliberate choice, except those listed in `locked_sources` (plan-locked - never suggest them). |
| `get_pipeline_stats` | Volumes and conversion per step. |
| `check_campaign_setup` | THE tool for "it doesn't work". Returns facts (`findings`) plus the raw targeting text for you to judge. No model of ours runs, no daily cap. |
| `list_accounts` / `list_campaigns` | Agency: enumerate accounts, then their campaigns. Always pass `campaign_id` afterwards. |
| `list_meetings` | Calendar-stamped meetings only. Never conclude "no meetings" from this alone - `goal_reached` is the truth for won leads. |
| `search_help` | MimikFlow's official help content, in the user's language. Use it for anything app-side. |

## Campaign configuration

| Tool | Use |
| --- | --- |
| `get_campaign_profile` | Offer, target, voice, goal, and the `calendar` block. A connected calendar makes `goal_link` optional - do not call booking broken when a calendar is connected. |
| `update_targeting` | `my_target` and/or `my_anti_target`. See `targeting.md` before calling. |
| `update_campaign_profile` | `who_am_i`, `my_product`, `my_objections`, `sender_name`, `communication_style`, `style_examples`, `social_proof_stories`, `goal_type`, `goal_link`, `goal_label`, `custom_cold_dm_instructions`, `custom_setter_instructions`, `setter_custom_stop_condition`. Append, don't overwrite, unless asked. |
| `get_pipeline_settings` / `update_pipeline_settings` | All the knobs. See `settings.md` before calling. |
| `set_latest_update` | Fresh news / case study / offer, re-offered to silent leads. |
| `toggle_pipeline` | Turn one step on or off. Warn before turning one off: leads pile up at that step. |
| `run_pipeline_now` | One immediate run. Not available for the setter (runs on reply) or connection tracking. |
| `list_media` | The campaign's videos, documents, case studies - and where each is used today. |

## Leads and conversations

| Tool | Use |
| --- | --- |
| `list_leads`, `list_tags`, `tag_leads` | Browse and label. |
| `get_conversation` | Refreshes the live thread first. Reports `written_by` and `awaiting_reply_from`. Call it on the exact prospect immediately before writing anything. |
| `search_conversations`, `get_win_loss_data` | Raw material for analysis - you do the analysis. |
| `export_leads` | Capped at 5 calls/day, per workspace. Not pageable on purpose. |
| `snooze_lead` | Re-engage at a date, with a `note` (meeting summary, offer made) injected into that one follow-up. A won lead keeps its won status. |
| `drop_leads` / `restore_leads` | Take out of outreach / put back. Rejecting a first-message draft also drops the prospect - `restore_leads` is the way back. |
| `resume_ai_on_lead` | Hand a conversation back to the AI after a human took over. |
| `send_first_message` | Write the first message to one accepted connection now, within the daily budget. |
| `list_manual_conversations` / `adopt_conversation` | Threads started by hand, handed to the machine. One explicit confirmation per thread. |

## Inbox

| Tool | Use |
| --- | --- |
| `list_pending_messages` | Drafts waiting for approval. Only fills up in validation mode, where it is the most common invisible blocker. |
| `approve_pending_message`, `edit_and_approve_pending_message`, `reject_pending_message` | Act on a draft. Read the thread first. |
| `resolve_pending_calendar_outcome` | Record a human calendar verification. |
| `list_escalations` | Conversations the AI handed to the human. |
| `reply_to_lead` | Human reply, LinkedIn or WhatsApp. Refuses a second message in a row when the prospect has not answered - that refusal is the guard working, not an error. |

## Prospecting

| Tool | Use |
| --- | --- |
| `search_prospects` | Preview only. Saves and contacts nobody. Flags people already in the campaign. |
| `add_prospects_to_campaign` | Queue up to 100 profile URLs. `filter_mode`: `icp` (bulk), `all` (a named person), `custom`. A named person's brief goes in `cold_dm_instructions`. |

## WhatsApp and meetings

| Tool | Use |
| --- | --- |
| `list_whatsapp_numbers` | State, ceilings, and a plain-language reason when a number is quiet (warm-up, ramp-up, auto-pause). Explain it; never call the channel broken. |
| `get_whatsapp_settings` / `update_whatsapp_settings` | Wording and days/hours only, per campaign. |
| `update_meeting_moments` | What goes out around a booked meeting: confirmation, reminders, showed-up vs no-show. Can attach a media. |

## Limits you will hit

- Daily tool calls: 200 for a solo workspace, +150 per extra connected
  LinkedIn account, capped at 3000. An interactive session never gets close.
- `export_leads`: 5/day.
- `add_prospects_to_campaign`: 100 URLs per call.
- `prepare_onboarding_setup`: a few runs per day per campaign.
- `first_degree_scanner`: refused on trial and Découverte plans.
