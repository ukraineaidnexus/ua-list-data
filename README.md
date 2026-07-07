# ua-list-data

Data files behind **[ukraineaidnexus.org](https://ukraineaidnexus.org)**: organisations,
volunteers, fundraisers, creators, and accounts worth following.

**License: CC0 (public domain).** Use this data however you like, no permission needed.
See [LICENSE](LICENSE).

Site code and templates all rights reserved, not covered by this license.

## Structure

```
data/orgs/<id>.yaml         Ukrainian/foreign NGOs and volunteers
data/fundraisers/<file>.yaml   Live and past fundraisers, linked to an org
data/creators/<id>.yaml     Ukrainian creators with a storefront or vouch
data/follow/<id>.yaml       Accounts worth following (OSINT, journalists, official)
```

The scammer register is **not** in this repo, since it carries legal
exposure. Report a scammer through the
[submission form](https://ukraineaidnexus.org/submit/?type=scammer) instead.

## Submitting a change

1. Fork this repo
2. Add or edit a YAML file under the right folder (schema below)
3. Open a pull request against `main`
4. A maintainer reviews and merges. Nothing publishes automatically.

## Universal rules

- `id`: lowercase-hyphens. `dates`: YYYY-MM-DD.
- `draft: true` hides an entry from the live site without deleting it.
- `location`: coarse only (city or country, never finer), and only what the person or
  org has published themselves. Precise locations for in-country volunteers are a
  doxxing risk.
- Strip tracking parameters before adding any link: X `?s=20`, Facebook `&rdid=...`,
  `utm_*` everywhere.
- Never suggest PayPal `webscr` donate links. They're not used on this site.
- `payment_note`, `warning`, `notes`, and `verified_by` accept markdown.

## Schema: data/orgs/&lt;id&gt;.yaml

```yaml
id: example-org                # required, lowercase-hyphens
name: "Name"                   # required
name_ua: "Українська назва"    # optional, Ukrainian name shown after the English one
type: ua-ngo                   # required: ua-ngo | ua-volunteer | foreign-ngo | foreign-volunteer
status: active                 # required: active | inactive
description: "..."             # required, English
email: address@example.com     # optional, top-level only (not under handles)
location: "Kyiv, Ukraine"      # optional, coarse only, self-published info only
warning: "..."                 # optional, red flag box, markdown allowed
notes: "..."                   # optional, markdown allowed
verified_by: "..."             # optional, markdown allowed, who vouches and why
payment_note: "..."            # optional, markdown allowed, only shows if a payment field exists
draft: true                    # optional, hides the entry

handles:                       # all optional; bare handle unless noted
  x: handle_without_at
  bluesky: name.bsky.social
  facebook: name               # or full URL
  telegram: name
  whatsapp: "380XXXXXXXXX"     # international format, no +
  instagram: handle
  threads: handle
  tiktok: handle
  reddit: username
  discord: "https://discord.com/invite/..."   # full URL only
  linkedin: "https://linkedin.com/company/x"  # full URL only
  signal: "https://signal.me/#p/..."          # full URL only
  youtube: ChannelHandle
  mastodon: "https://server/@user"            # full URL only
  substack: newslettername
  medium: name

links:
  website: https://...
  linktree: https://linktr.ee/example
  donate: https://...          # actual donate pages only (WayForPay, Donorbox, hosted-button PayPal)
  buymeacoffee: https://buymeacoffee.com/...
  transparency: https://...    # reports, spreadsheets, achievement pages
  registration: https://opendatabot.ua/c/XXXX   # official registry link
  monobank: https://send.monobank.ua/jar/...
  privat: https://privat24.ua/send/...
  other:
    - { label: "Label", url: "https://..." }    # NON-payment links only
  donate_other:                # payment links with no dedicated key above
    - { label: "Wise", url: "https://...", icon: card }
    - { label: "Monobank jar — drones", url: "https://...", icon: jar, ua: true }
    - { label: "Cancer treatment fund", url: "https://...", icon: heart, personal: true }

paypalme: handle               # renders paypal.me/<handle>
paypal: ["email@example.com"]  # click-to-copy chip, no payment link
last_verified: 2026-07-07
```

## Schema: data/fundraisers/YYYY-MM-orgid-shortname.yaml

```yaml
draft: true                    # delete this line to publish
org: org-id                    # required, must match a data/orgs id
title: "..."                   # required
beneficiary: "Who it's for"
description: >
  ...
goal: { amount: 13900, currency: USD }   # USD | EUR | GBP | UAH; omit for open-ended
raised: 0                      # optional, integers only
announced: 2026-06-11          # required
expires: 2026-08-11            # omit for open-ended
status: active                 # required: active | completed | expired
links:
  x: https://x.com/.../status/...
  donate: https://...
  monobank: https://send.monobank.ua/jar/...
  privat: https://privat24.ua/send/...
  donate_other:
    - { label: "Wise", url: "https://...", icon: card }
paypalme: handle
paypal: ["email@..."]
report: null                   # completion report link, set alongside status: completed
```

## Schema: data/creators/&lt;id&gt;.yaml

```yaml
id: name                       # required
name: "Name"                   # required
makes: "What they make."       # required
handles: { ... }               # same options as orgs
shop:
  - { label: "Etsy shop", url: "https://..." }
other:
  - { label: "...", url: "..." }
notes: "..."                   # flag DM-only orders: no buyer protection
vouched_by: "..."
last_verified: 2026-07-04
```

Requires a real storefront, or a vouch from an actual buyer.

## Schema: data/follow/&lt;id&gt;.yaml

```yaml
id: name                       # required
name: "Name"                   # required
why: "Why worth following."    # required
category: "OSINT / analysis"   # Journalist | OSINT / analysis | Fact-checking | Official
links: { website: https://... }
handles: { ... }
last_verified: 2026-07-04
