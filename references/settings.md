# Writable settings

All of these go through `update_pipeline_settings` (`updates` object) unless
stated otherwise. Read with `get_pipeline_settings` first. An empty value
means MimikFlow's default applies - that is a normal state, never a gap.
Present each setting to the user by what it does, never by its key.

## Market and language

| Key | Values | Note |
| --- | --- | --- |
| `target_country` | 2-letter code | The main market. |
| `target_countries` | comma-separated codes | Every market the campaign searches. The first volume lever. |
| `target_country_mode` | `equal`, `primary` | `equal` = all markets served the same; `primary` = the main one served twice as much. |
| `target_regions` | free text, 400 chars | Cities/regions. Narrows hard - use only for a genuinely local business. |
| `outreach_language` | e.g. `fr`, `en` | Language the machine writes in. |
| `user_timezone` | IANA name | Drives the sending windows. |

## Quality gates before any AI reads a profile

| Key | Default | Volume effect |
| --- | --- | --- |
| `lead_filter_require_photo` | `true` | `false` widens the funnel, adds noise. |
| `lead_filter_min_connections` | `50` | Lower to 0-20 for young or non-Western audiences. |
| `lead_filter_block_open_to_work` | `true` | Keep on unless the user sells to job seekers. |
| `lead_filter_excluded_keywords` | empty | Comma-separated, matched on headline / position / summary. Filters before anything runs, but it is still an exclusion: guardrail 2 applies. |

## First message

| Key | Values |
| --- | --- |
| `cold_dm_formality` | `tu`, `vous` (senior and formal markets expect `vous`) |
| `cold_dm_length` | `court`, `moyen`, `long` |
| `cold_dm_emoji` | `aucun`, `minimal`, `modéré` |
| `cold_dm_cta_mode` | `opener_only`, `soft_meeting` |
| `cold_dm_template` | a preset: `freelance_decouverte`, `freelance_dirigeant_preuve`, `directeur_debutant`, `directeur_confirme`, `corporate_c_level`, `solo_direct`, `solo_discussion`, `startup_founder`, `consultant_expert`, `agence_diagnostic`, `saas_decideur`, `coach_freelance`, `coach_corporate`, `recruteur_manager`, `pme_dirigeant` |
| `cold_dm_custom_template` | the user's own example message |
| `timing_cold_dm_wait_days` | 0-30, delay after acceptance |

The base style is **one** choice over two keys. Setting both ships a message
the user did not pick, because the example message wins over the preset.
Writing one clears the other; the tool reports it in `cleared`.

Custom hook instructions live in `custom_cold_dm_instructions`
(`update_campaign_profile`).

## Follow-ups

| Key | Values |
| --- | --- |
| `follow_up_mode` | `auto`, `templates` |
| `follow_up_pace` | `relaxed`, `standard`, `dynamic` |
| `follow_up_instructions` | free text, 2000 chars - THE relance instructions field |
| `timing_max_follow_ups` | 1-8 (4-6 is the volume-friendly range) |
| `timing_max_follow_ups_after_reply` | 0-8 |
| `follow_up_offer_pitch_position` | which follow-up carries the offer |
| `timing_connection_timeout_days` | 5-90 |
| `follow_up_phase1/2/3_style`, `..._custom` | per-phase tone |
| `follow_up_video_number` | which follow-up carries the video |

Lowering the number of follow-ups **shortens** the sequence, it does not
space it out: the rhythm stays the same.
`timing_follow_up_wait_days` is retired and not writable - do not try.

## The AI that answers prospects

`setter_style` (`naturel`, `professionnel`, `décontracté`),
`setter_max_phrases` (1-6), `setter_abbreviations` (`oui`/`non`).
Extra hand-off situations go in `setter_custom_stop_condition`
(`update_campaign_profile`).

## Sending windows

`days_cold_dm_sender`, `days_follow_up`, `days_setter` (empty = every day),
`hours_start_*` / `hours_end_*` (0-23, in `user_timezone`),
`setter_schedule_custom` (`on`/`off`).
Wider windows mean more sends. Invitations always run every day 8h-23h and
their days and hours are deliberately not writable.

## Around a booked meeting (the most underused, and it converts)

`meeting_touches` (JSON list, up to 8 scheduled messages, offsets in hours
from -336 to +336, each optionally carrying a `media_id` from `list_media`),
or the simpler toggles: `nurture_pre_meeting` (+`_days_before`,
`_instructions`), `nurture_post_meeting` (+`_instructions`),
`meeting_reminder`, `meeting_noshow_recovery`, `meeting_cancel_recovery`
(these three are live defaults - `off` disables them),
`meeting_recovery_instructions`, `setter_post_booking`,
`meeting_schedule_custom`, `days_meeting`, `hours_start_meeting`,
`hours_end_meeting`.

For WhatsApp, the same plan is set with `update_meeting_moments`.

## Sending modes

`cold_dm_mode` and `setter_mode` accept `auto` and `semi_manual`. Guardrail 3:
never propose `semi_manual`.

## Read-only on purpose

Volume caps - `weekly_connections_target`, `max_cold_dms_daily`,
`max_profile_checks_daily`, `max_messages_daily` - are readable via
`get_pipeline_settings` and not writable. Guardrail 4.

## Campaign profile fields (`update_campaign_profile`)

`who_am_i`, `my_product`, `my_objections`, `sender_name`,
`communication_style`, `style_examples`, `social_proof_stories`, `goal_type`
(`call` or `link`), `goal_link`, `goal_label`, `custom_cold_dm_instructions`,
`custom_setter_instructions`, `setter_custom_stop_condition`.

`style_examples` is read as a sample of the user's voice, not as a message to
reuse: keep it short and generic enough that no sentence of it could land in
a prospect's inbox verbatim.
