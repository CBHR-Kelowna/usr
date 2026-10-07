# Design Phase

Status: **active**

This file is the design contract for the USR rebuild. Database and UI work should follow this contract rather than rediscovering the customer journey later.

## 1. Product sequence

The client experience is one continuous follow-up system:

1. Transaction is marked closed.
2. Ultimate Service Request email is scheduled for 7 days later.
3. The client completes or ignores the service survey.
4. The client remains eligible for the month-end review campaign.
5. One monthly email asks for the relevant Google reviews and giveaway confirmation.
6. Giveaway confirmation happens on a first-party CBHR page.

There is no third review email.

## 2. Ultimate Service Request

### Purpose

USR is private service feedback after a completed transaction. It is not the Google review ask.

### Tone

- direct
- appreciative
- calm
- human
- short
- brokerage-led

Do not use:

- "award-winning office" filler
- referral asks in the same email
- fake luxury language
- long explanations
- agent email addresses
- agent biographies
- em dashes

### Subject

Preferred:

`How did we do, {{client.firstName}}?`

Fallback:

`How did we do? | Coldwell Banker Horizon Realty`

### Required states

Buyer:
- "Now that your purchase has closed..."
- Buyer survey destination

Seller:
- "Now that your sale has closed..."
- Seller survey destination

Agent identity:
- show only the agent name, co-agent display name, or team name
- never show agent email in the customer-facing email
- the visual template stays the same

### Canonical hierarchy

1. Coldwell Banker Horizon Realty masthead
2. Ultimate Service® label
3. "How did we do?"
4. client and transaction context
5. one survey action
6. short thank-you
7. brokerage footer

## 3. Monthly Review Campaign

### Purpose

Generate Google reviews after the USR touchpoint.

This is one month-end email. It does not split agent and brokerage review requests into separate sends.

### GBP state

When the agent or team has a Google Business Profile:

1. Review agent/team
2. Review Coldwell Banker Horizon Realty
3. Confirm giveaway entry

### No-GBP state

1. Review Coldwell Banker Horizon Realty
2. Confirm giveaway entry

### Giveaway

The campaign promise remains review-to-win.

Prize:

- $100 Visa gift card, or
- $100 donation to one of the featured local organizations

The winner chooses after selection. We do not ask every entrant to choose a reward before they win.

### Customer identity

Show:
- client first name
- agent name or team name

Do not show:
- agent email
- role/title unless there is a future proven need
- agent portrait in this campaign

## 4. Entry Confirmation

Google Forms is not part of the new design.

The email links to a signed, first-party CBHR URL. The page already knows the recipient and campaign.

The page asks for one action:

`Confirm my entry`

No duplicate name or email fields.

Success state:

`You're in.`

The database implementation will later decide the exact token format and event model.

## 5. Visual system

Source direction: CBHR Premium Editorial and CBLUX Standard Brand.

### Core colours

- CB Blue: `#012169`
- Deep Navy: `#001844`
- Ink: `#10151F`
- Ink Soft: `#263142`
- Slate: `#687386`
- Muted: `#8A94A6`
- Line: `#D8DFEA`
- Line Soft: `#E8EDF4`
- Cool Surface: `#F6F8FB`
- Pearl Surface: `#F8F6F1`
- Warm Accent: `#B8A46A`

### Typography

Primary hierarchy:
- Montserrat or similar geometric sans

Body:
- Roboto, Inter, Arial, sans-serif

Serif:
- selective accent only
- currently used for the $100 prize value
- not the dominant email language

### Composition

Use:
- bright editorial layout
- disciplined spacing
- one strong blue masthead
- one thin warm accent rule
- pearl surface for one special callout
- fine separators
- restrained 8px button radius
- strong left alignment
- clear action hierarchy

Avoid:
- card soup
- oversized serif hero copy
- gold luxury theme
- multiple decorative containers
- heavy shadows
- large agent photos
- centered marketing paragraphs
- SaaS-style interface visuals inside emails

## 6. Locked customer-facing assets

Canonical design files:

- `design/emails/usr.html`
- `design/emails/monthly-review.html`
- `design/pages/review-entry.html`

These are design references, not production renderers yet.

## 7. Next phase gate

Do not start the management UI until:

- USR email is approved
- monthly review email is approved
- confirmation page is approved

After that, Phase 2 defines the MongoDB model and migration contract. Phase 3 builds the management UI against that model.
