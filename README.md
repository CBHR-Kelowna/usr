# CBHR USR

Coldwell Banker Horizon Realty client follow-up system.

## Build order

This project is intentionally being built in three phases:

1. **Design**
2. **Database**
3. **UI**

We do not design the database around old Make/Webflow constraints, and we do not build the management UI until the customer-facing experience is locked.

## Current phase: Design

The design phase covers the complete client-facing journey:

- Ultimate Service Request email
  - buyer
  - seller
  - single agent, co-agent, or team display name
- Monthly Google review campaign
  - agent/team GBP + brokerage review
  - brokerage-only fallback
  - $100 Visa gift card or $100 donation
  - confirmation entry experience
- Shared CBHR visual and copy system

## Lifecycle

```text
Transaction closes
      |
      + 7 days
      |
Ultimate Service Request
      |
Client enters monthly review pool
      |
Month end
      |
Google review campaign
      |
Agent/team review when GBP exists
Brokerage review
Confirm giveaway entry
```

## Design source

The visual system follows the CBHR Premium Editorial direction and the established CBLUX implementation:

- CB Blue as the authority colour
- bright editorial surfaces
- strong sans-serif hierarchy
- restrained serif accent only where it earns its place
- warm accent used sparingly
- fine borders and spacing before shadows
- direct, human copy
- no em dashes
- no generic luxury styling
- no SaaS/card-grid look

See [docs/DESIGN.md](docs/DESIGN.md).

## Existing production

The old Webflow/Make implementation remains production until the replacement is ready. Do not reactivate or mutate the stopped monthly campaign simply to test the new system.
