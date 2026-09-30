# Delivery Order Management SaaS

## Confirmed Context

A multi-tenant SaaS platform for delivery companies to receive, manage, assign, track, and complete customer delivery orders. This repository is for the backend, designed and implemented by the user.

## Current Stage

Planning and requirements analysis only. Application implementation has not started and is not authorized at this stage. Business requirements are not finalized.

The next step is an Intent-phase stakeholder interview to discover actual business needs. Earlier proposed roles, order states, permission rules, and release boundaries were unconfirmed suggestions and are not requirements.

## Initial Technology Stack

- Python and FastAPI for a REST API.
- Pydantic for request and response validation.
- SQLAlchemy and PostgreSQL for persistence.
- Alembic for database migrations.
- Pytest for testing.

Versions and tooling configuration remain undecided. No installation, development, migration, or test commands are configured yet.

## Engineering Direction

Keep routes thin, place application/business logic in services, and encapsulate model behavior where appropriate. Separate database access from HTTP concerns. Use FastAPI dependency injection where appropriate and prefer clear, modular code without unnecessary dependencies or patterns. See [AGENTS.md](AGENTS.md) for contributor instructions.

## Open Questions for Discovery

- Who uses the system, and what operations does each user need?
- How are orders received, assigned, tracked, and completed in practice?
- Which data, business rules, and exceptional cases must be supported?
- What does multi-tenancy mean for company boundaries and access?
- What defines the first release and its acceptance criteria?
- Which integrations and operational constraints are required?

These questions do not prescribe answers or features. Major architectural decisions will be presented for review before adoption.

## Repository Status

`app/` and `test/` are currently empty directories. Treat them as placeholders, not a finalized module structure. No application code or dependency configuration has been added.
