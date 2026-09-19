# Writing a target that produces volume

`my_target` is free text read by MimikFlow's prospect finder and by the AI
that judges each profile. The judge sees **only what a LinkedIn profile and a
public company page show**. Everything else is invisible to it.

## The rule that decides everything

Every criterion must be something the prospect **visibly is** or **visibly
does**. A criterion that rests on an absence, on a number nobody publishes, or
on what happens inside the prospect's business, cannot be checked. It is
either ignored (wasted words) or, worse, treated as a reason to reject.

Visible: job title, seniority, function, sector, company size, location,
"hiring", "agency", "SaaS", "e-commerce", posting about a topic.
Invisible: revenue, ad spend, "has no sales funnel", "struggles with lead
gen", "spends more than X on ads", "is not satisfied with their current
provider", "has budget".

## Broad but intelligent

Volume comes from a target that a large, well-defined population satisfies.
The failure mode is **stacked criteria**: five conditions, each reasonable,
which almost nobody satisfies all at once.

Aim for three layers, no more:

1. **Who they are** - a family of roles, not one title. "Founder, CEO,
   associate director of a marketing/communication agency" beats "Head of
   Growth".
2. **Where they work** - a sector plus a size range. "Agencies and service
   companies, 5 to 50 employees".
3. **Where they are** - countries handled by the market settings, regions or
   cities only when the user genuinely sells locally.

Then, optionally, one line of signal: "bonus if they post about client
acquisition or are hiring salespeople".

## Good and bad, side by side

Too narrow (a campaign that stays empty):
> B2B SaaS founders, 10-50 employees, series A raised in the last 18 months,
> already using Hubspot, with an SDR team but no outbound process, in Paris,
> who post at least weekly on LinkedIn.

Broad and usable:
> Founders, CEOs and sales directors of B2B SaaS and digital service
> companies, roughly 10 to 200 employees, in France and Belgium. Typically
> people who sell a recurring offer and handle prospecting themselves or with
> a small sales team. Bonus if they are hiring salespeople or post about
> client acquisition.

Describing peers instead of buyers (a classic, and it kills results):
> LinkedIn prospecting experts, growth agencies, outbound consultants.
That is the user's competition, not their market. Watch for it.

## Anti-target: the default is empty

- Never propose `my_anti_target`. Never widen an existing one.
- Write it only when the user asked for that exclusion in their own words, and
  only for what they asked.
- Keep it to visible facts: a job family, a sector, a company type. Never an
  absence, never an internal state.
- Tell the user the consequence: the change also re-reads prospects already
  found and waiting (queue, pending invitations, running reminder sequences)
  and drops the ones it now excludes, drafts included. Prospects already in a
  conversation are never dropped.
- When the machine reaches the wrong people, sharpen `my_target`. That is the
  fix, every time.

## Checking a target you did not write

`check_campaign_setup` returns the user's targeting text verbatim plus what
the campaign has observed. Read the text and quote their own words back when
you find:

- criteria that stack until almost nobody qualifies;
- two rules that cancel each other;
- a criterion no profile ever shows;
- a target that describes the user's peers instead of their buyers;
- a place named in the text that is not among their selected countries.

When `observed.enough_data` is false, the campaign has not searched enough to
prove anything: judge the text only, and never estimate how many such people
exist - you have not seen the market.

Never rewrite silently. Propose the exact replacement text, get agreement,
then `update_targeting`.
