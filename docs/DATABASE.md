# Database v1

The canonical operational database lives in MongoDB. Existing Webflow and Make
data stores remain integration/projection surfaces during migration.

## Collections

### `profiles`

Canonical business identities for people, teams, and the brokerage.

```text
type: person | team | brokerage
status: pending | active | inactive
```

Important surfaces:

- identity and contact
- professional roles, services, BCFSA licensing and PREC
- REALTOR.ca Agent Key
- media
- application capabilities
- public profile content
- team relationships
- Google review relationship
- Webflow reference metadata

A User/login is not a Profile. Future onboarding will connect authenticated
Users to Profiles through a separate access model.

### `google_business_profiles`

Canonical external Google review destinations.

```text
status: candidate | stale | verified
ownerProfileId -> profiles._id
```

Profiles reference a Google destination with one of:

- `direct`
- `inherit_team`
- `none`

Only `verified` Google Business Profiles may be used in a production review
campaign. This avoids duplicating team Place IDs across members and keeps stale
or unverified profiles out of live sends.

### `client_experiences`

One durable record for one client on one side of one closed transaction.

Supports:

- buyer or seller
- one primary Profile
- optional co-agent Profile
- Ultimate Service lifecycle
- monthly-review eligibility
- source/idempotency metadata

Provider rules are enforced by the service layer:

- a Team is one primary provider with no co-agent
- a non-team collaboration is one primary Person plus optional co Person

### `campaigns`

One campaign-level record for each monthly review run.

Production campaigns are unique by service month.

Lifecycle:

```text
draft -> validating -> ready -> approved -> sending -> completed
```

Campaigns contain audience boundaries, policy, prize configuration, template
version, approval/send timestamps, and draw state.

### `campaign_recipients`

Durable send-time record for each campaign recipient.

A production client experience can belong to only one production monthly
review campaign.

The recipient freezes the exact values used at send time:

- client identity
- buyer/seller side
- primary/co provider identity
- brokerage identity
- resolved review targets
- Google Business Profile and Place ID used
- template/variant
- delivery state
- entry state

Profile changes after send therefore do not rewrite history.

### `engagement_events`

Append-only event stream referencing the experience/campaign/recipient IDs.
Event type is intentionally extensible.

Examples:

- `usr_sent`
- `usr_clicked`
- `usr_completed`
- `monthly_sent`
- `provider_review_clicked`
- `brokerage_review_clicked`
- `entry_confirmed`
- `winner_selected`
- `prize_fulfilled`

Events do not duplicate client PII.

## Idempotency and indexes

The database enforces:

- unique client-experience idempotency keys
- unique external-source IDs when present
- one production monthly campaign per period
- one recipient per campaign + experience
- one production monthly follow-up per experience
- unique entry token hashes
- unique event idempotency keys when supplied
- unique REALTOR.ca Agent Keys on Profiles
- unique Webflow item references
- unique Google Place IDs

## Legacy Make queue

The old Monthly Campaign Customers store is not clean canonical history.

A 100-row audit found malformed buyer emails caused by the known co-agent path
bug, trailing whitespace, duplicate recipient rows, missing Place IDs, and
historical providers no longer in the active Profile roster. The legacy store
also lacks reliable close-date/campaign-period metadata.

The existing 414 records must therefore be migrated through a reviewable import
report, not copied directly into live eligibility.

## Migration boundary

Do not modify or delete the old `flyagents`, `Agents`, `Teams`, Make data
stores, or production USR scenarios until parity is proven.

Webflow is linked by its item ID and remains a website projection during the
transition. Long-term direction is Mongo -> Webflow, not Webflow -> Mongo.
