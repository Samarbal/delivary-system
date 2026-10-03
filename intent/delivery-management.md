# Product Requirements Document: Delivery Order Management

Date: 2026-10-03

## 1. Purpose and Audience

Organize delivery requests from receipt through assignment, tracking, and completion for one delivery company. The company delivers items; it does not sell them. The three user groups are customers, drivers, and company staff.

The stakeholder is the sole developer, working with an AI coding assistant. The target is a working demonstration within five weeks, not a production launch. Multi-tenancy is outside the current scope.

This document records the completed six-phase interview. Confirmed requirements are separated from recommendations and unresolved details; it is not an approved production specification.

## 2. Confirmed Scope

### Accounts

- Collect phone number, email, and password; sign in with phone number and password.
- Customers register without staff approval, but must verify email before placing an order.
- Drivers register themselves, verify email, and wait for staff approval before claiming orders.
- Send real verification and password-reset emails. Password recovery uses a link sent to the verified email address.
- SMS verification is excluded from the demo.

### Order and Driver Workflow

- Customers provide item category, pickup address, and delivery address as text.
- Company staff select the distance range; the company defines a fixed delivery fee for each range.
- New available requests appear to all drivers, without proximity filtering. The first driver to claim a request receives the assignment.
- Drivers record starting, item pickup, expected arrival time, and completion. Customer confirmation is not required to complete delivery.
- An order left unclaimed for 15 minutes requires staff attention; staff can assign a driver manually.
- Drivers report delivery issues. Staff can arrange reassignment and republish the order for available drivers. Exact recovery rules remain open, especially after pickup.

### Payments and External Services

- Jawwal Pay is the intended future payment service; simulated payments are acceptable for the demo.
- Customers choose payment before delivery or when the driver arrives.
- Drivers can confirm payment. What constitutes sufficient confirmation is unresolved.
- Real email delivery is required; provider and budget are undecided.

### Platforms and Quality Targets

- Responsive web interfaces for customers, drivers, and staff, using Next.js.
- Arabic and English, with right-to-left and left-to-right layouts respectively.
- Online operation only; offline updates and synchronization are deferred.
- Support approximately 20–30 simultaneously active drivers.
- Automatically show new requests and assignment changes within two seconds under normal conditions, without manual refresh. Test conditions and the measurement method still need definition.

### Deferred or Unconfirmed

- Multi-tenancy, native mobile apps, offline synchronization, live Jawwal Pay integration, and SMS verification are outside the demo scope.
- Customer ratings were suggested but not committed to the MVP.
- Live GPS, routing, external ordering apps, cancellation/refunds, and other capabilities are not confirmed requirements.

## 3. User Stories and Acceptance Criteria

These criteria cover confirmed behavior. Proposed security and architecture details below need review before becoming implementation requirements.

### US-01: Customer Registration

As a customer, I want to register and verify my email so I can request deliveries.

```gherkin
Scenario: Customer verifies their email
  Given a customer has registered with a phone number, email, and password
  When they complete email verification using a real verification email
  Then they can place an order without staff approval

Scenario: Customer has not verified their email
  Given a registered customer has not verified their email
  When they attempt to place an order
  Then the system prevents order submission
```

### US-02: Driver Approval

As company staff, I want to approve registered drivers before they claim work.

```gherkin
Scenario: Driver becomes eligible
  Given a driver has registered and verified their email
  When company staff approve the account
  Then the driver can claim available orders

Scenario: Unapproved driver attempts to claim
  Given a driver's account has not been approved
  When they attempt to claim an order
  Then the claim is rejected
```

### US-03: Sign-In and Recovery

As a user, I want to sign in with my phone number and recover a forgotten password through email.

```gherkin
Scenario: Sign in
  Given a registered user has valid phone-number and password credentials
  When they submit those credentials
  Then they are signed in subject to their account permissions

Scenario: Recover a password
  Given a user has a verified email address
  When they request password recovery
  Then a real reset email is sent to that address
  And a valid reset link allows them to set a new password
```

### US-04: Request and Price a Delivery

As a customer, I want to request delivery between two addresses; as staff, I want to price it using company distance bands.

```gherkin
Scenario: Submit a request
  Given a customer has verified their email
  When they submit an item category and pickup and delivery addresses as text
  Then the system records their delivery request

Scenario: Set the fee
  Given the company has defined fees for distance ranges
  When staff select a range for an order
  Then that range's fee is applied to the order
```

Publication timing relative to pricing and customer price acceptance remains open.

### US-05: Claim an Order

As an approved driver, I want to see available requests and claim one.

```gherkin
Scenario: Competing claims
  Given an available order is unassigned
  When two approved drivers try to claim it concurrently
  Then exactly one driver receives the assignment
  And the other is informed that the order is no longer available

Scenario: Dashboard updates
  Given drivers have connected dashboards under normal conditions
  When an order becomes available or is claimed
  Then their dashboards reflect the change within two seconds
  And no manual refresh is required
```

### US-06: Handle Unclaimed Orders

As staff, I want to manually assign requests that remain unclaimed.

```gherkin
Scenario: Unclaimed request needs intervention
  Given an available order has remained unclaimed for 15 minutes
  When staff review the order
  Then it is identified as requiring intervention
  And staff can manually assign a driver
```

The timer origin and notification mechanism need confirmation.

### US-07: Record Delivery Progress

As a driver, I want to record progress and finish the delivery.

```gherkin
Scenario: Normal delivery
  Given a driver is assigned to an order
  When they record starting, pickup, and expected arrival time
  Then those updates are saved for the order
  When they hand over the item and mark delivery done
  Then delivery is recorded as completed without customer confirmation
```

Payment-related completion restrictions remain open.

### US-08: Report and Recover from an Issue

As a driver, I want to report a delivery issue so staff can arrange another driver.

```gherkin
Scenario: Republish an order after an issue
  Given a driver reports an issue with their assigned order
  When staff choose to republish it for reassignment
  Then it becomes available for eligible drivers to claim
  And it does not retain two active driver assignments
```

Handling an item already in the original driver's possession must be resolved before implementing this scenario after pickup.

### US-09: Simulated Payment

As a customer, I want to pay before delivery or on arrival; as a driver, I want to confirm payment.

```gherkin
Scenario Outline: Choose payment timing
  Given an order has a delivery fee
  When the customer chooses to pay <timing>
  Then the demo supports a simulated payment at that stage
  And no real money is transferred

  Examples:
    | timing          |
    | before delivery |
    | on arrival      |

Scenario: Driver confirms payment
  Given a driver is assigned to the order
  When they confirm its simulated payment
  Then the confirmation is recorded
```

Payment evidence, failure behavior, and whether unpaid deliveries may be completed require decisions.

### US-10: Bilingual Web Access

As a user, I want to use the system from a browser in Arabic or English.

```gherkin
Scenario Outline: Interface language
  When a user selects <language>
  Then the interface displays that language using <direction> layout
  And core workflows remain usable on phone and desktop screens

  Examples:
    | language | direction     |
    | Arabic   | right-to-left |
    | English  | left-to-right |
```

## 4. Technical Architecture Recommendation — For Review

Recommend one modular FastAPI application, one PostgreSQL database, and a Next.js frontend for the single company. Keep domain decisions in FastAPI services and appropriate model methods; routes validate requests, invoke services, and format responses. Keep persistence independent of HTTP. Avoid microservices and a generic tenancy framework for this demo.

Use Pydantic for API validation, Alembic for migrations, and Pytest for backend tests. SQLAlchemy was in the initial user-selected stack and README but is absent from the latest AGENTS.md; resolve that discrepancy before dependency setup. Versions and synchronous/asynchronous database access remain open.

Use Next.js components for role-specific responsive pages, forms, and dashboard interactions. Client Components support interactive state and browser APIs; keep authoritative business rules in FastAPI. This division is a proposal based on [Next.js component guidance](https://nextjs.org/docs/app/getting-started/server-and-client-components).

For live dashboard updates, propose authenticated FastAPI WebSocket connections alongside REST operations. Reload current state after reconnecting. FastAPI supports WebSockets and dependencies; an in-memory connection manager only fits a single running process, so deployment topology needs review. See [FastAPI WebSockets](https://fastapi.tiangolo.com/advanced/websockets/).

Protect claims and administrative reassignment with a database transaction and an atomic eligibility check. Database locking or a conditional update must ensure only one winner; screen-update speed alone cannot prevent double assignment. Choose the exact transaction strategy during design review. See [PostgreSQL concurrency controls](https://www.postgresql.org/docs/current/explicit-locking.html).

Keep simulated payment handling separate from email delivery and order logic. A future Jawwal Pay adapter must be designed from actual provider documentation; no live integration capability is assumed.

Proposed security baseline: HTTPS, securely hashed passwords, server-enforced role and ownership checks, expiring single-use email tokens, login/reset rate limits, and secrets outside source control. Customers should access their own orders; driver visibility of customer details before claiming needs an explicit policy. Session design and email provider selection remain architectural review items.

Suggested verification: account eligibility tests, password-reset token lifecycle tests, PostgreSQL concurrent-claim tests, delivery transition tests, simulated payment tests, real-email smoke checks, and bilingual browser workflow checks. Measure dashboard updates with 30 connected drivers under documented conditions. This is not a guarantee of production capacity.

## 5. Suggested Five-Week Plan — Not a Commitment

1. Resolve blocking rules, review architecture, then establish accounts and real email delivery.
2. Implement customer requests, staff pricing, and driver approval.
3. Implement claiming, live dashboards, and the 15-minute exception path.
4. Implement delivery progress, issue handling, and simulated payments.
5. Complete Arabic/English checks, end-to-end validation, concurrency checks, and demo preparation.

This sequence assumes sufficient developer availability and access to an email service. Budget, weekly availability, start date, and hosting are unconfirmed.

## 6. Assumptions and Open Questions

No unresolved item below is an approved requirement. The architecture and schedule above are recommendations, not implementation authorization.

- Before implementation: decide when staff price a request, when customers accept the fee, and when drivers first see it.
- Before implementation: settle availability rules, maximum active orders per driver, and eligibility for manual assignment.
- Before implementing recovery: define issue details, release of the old assignment, and custody of an item already picked up.
- Define payment failure, driver payment confirmation, cancellation/refunds, and whether completion requires payment.
- Define fee bands, currency, category values, and required address/contact details.
- Confirm whether staff and admin are one permission level, how staff accounts are created, and who can view customer data.
- Decide how ETA is supplied, when the 15-minute timer starts, and what counts as normal conditions for the two-second target.
- Select an email provider and sender setup; confirm costs, hosting, weekly availability, and demonstration date.
- Review security/session choices, data retention, backups, and any applicable privacy obligations before a real launch.
- Confirm SQLAlchemy's inclusion and decide whether customer ratings are deferred.

Status: draft
