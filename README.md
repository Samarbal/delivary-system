# Delivery Order Management

## Project Context

A responsive web system for one delivery company to receive, assign, track, and complete delivery requests. Customers, drivers, and company staff use the system. The company delivers items and does not sell them.

The target is a working demonstration within five weeks, built by a solo developer with AI assistance. Multi-tenancy is outside the current scope.

## Requirements and Current Stage

The six-phase stakeholder interview is complete. See the [draft PRD](intent/delivery-management.md) for confirmed requirements, user stories, Gherkin acceptance criteria, architectural recommendations, and open questions.

Work remains in planning. Implementation is not yet authorized. Recommendations and unresolved business rules require review before adoption.

## Selected Technologies

- Next.js for Arabic and English responsive web interfaces.
- Python, FastAPI, and Pydantic for the REST API.
- PostgreSQL, Alembic migrations, and Pytest.
- SQLAlchemy was initially selected but omitted from the later AGENTS.md stack; confirm its inclusion before setup.

Versions, dependency management, and database access mode remain undecided. No application or development commands are configured.

## Demo Scope

- Phone-number and password sign-in; real email verification and password recovery.
- Customer registration and staff approval of verified driver accounts.
- Text-address delivery requests and company-defined distance-band fees selected by staff.
- First-driver-to-claim assignment and staff intervention after 15 unclaimed minutes.
- Driver progress updates, completion, and issue reporting for reassignment.
- Simulated payments; Jawwal Pay is the intended future live integration.
- Online operation with approximately 20–30 concurrent drivers and automatic dashboard updates within two seconds under normal conditions.

See the PRD for limitations and unresolved behavior. Live payments, offline synchronization, native mobile apps, multi-tenancy, and SMS verification are deferred. Ratings remain unconfirmed.

## Engineering Guidance

Follow [AGENTS.md](AGENTS.md): thin routes, service-layer business logic, appropriate model behavior, Pydantic validation, dependency injection, and database access separated from HTTP concerns. Major architectural proposals must be presented before adoption.
