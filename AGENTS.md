# Repository Guidelines

## Project Context & Current Phase

This project is a multi-tenant SaaS Delivery Order Management System for delivery companies. The user is the backend developer. Work is limited to planning and requirements analysis; do not implement features, scaffold the application, or install dependencies yet.

Business requirements are not finalized. Do not invent roles, workflows, order statuses, permissions, or release scope. The next step is an Intent-phase stakeholder interview. Distinguish confirmed requirements, assumptions, and open questions.

## Technology Stack

Use Python, FastAPI, Pydantic, PostgreSQL, Alembic for migrations, and Pytest for testing. Expose a REST API. Versions, dependency management, and synchronous versus asynchronous database access remain undecided. Avoid unnecessary dependencies or architectural patterns.

## Architecture Principles

- Keep API route handlers thin; business logic must not live in routes.
- Use a service layer for application and business logic.
- Apply Fat Models, Thin Controllers where appropriate: models may encapsulate domain behavior, while services coordinate application operations.
- Separate database access from HTTP concerns; do not couple persistence logic to request or response handling.
- Use Pydantic schemas for request and response validation.
- Use FastAPI dependencies for dependency injection where appropriate.
- Maintain clear separation of concerns and prefer modular, maintainable code over premature complexity.

Present proposed changes or a diff before making major architectural decisions, including module layout, tenancy storage, authentication, or transaction strategy. These principles do not approve a specific design.

## Repository Structure & Commands

`AGENTS.md` holds contributor instructions; `README.md` records project context. Empty `app/` and `test/` directories exist, but the module layout is not finalized. No application, dependency manifest, migration setup, or runnable development/test commands exist yet. Document verified commands when implementation begins.

## Coding & Testing Conventions

Use four-space Python indentation, `snake_case` for modules/functions, and `PascalCase` for classes. Prefer descriptive names and type annotations. No formatter or linter has been selected. When implementation is authorized, use Pytest with `test_*.py` files and `test_*` functions. Cover confirmed business rules and regressions; no coverage threshold is defined.

## Contributions & Security

Git history contains only `Initial commit`; no established commit convention exists. Use concise, imperative subjects. Pull requests should explain changes, validation, and unresolved assumptions, linking relevant issues. Never commit credentials or private customer data.
