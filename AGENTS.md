# Repository Guidelines

## Project Structure & Module Organization

This workspace currently contains no application code, tests, assets, or build configuration. Establish a clear layout when adding the first implementation. Prefer `src/` for application code, `tests/` for automated tests, `docs/` for supporting documentation, and `assets/` for static resources where appropriate. Organize related functionality together and document the chosen architecture in `README.md`.

## Build, Test, and Development Commands

No build, test, or local development commands are configured yet. When introducing a toolchain, document exact installation, development, build, and test commands in `README.md`. Keep these commands synchronized with the actual scripts or configuration. Do not assume commands such as `npm test` exist before the corresponding tooling is added.

## Coding Style & Naming Conventions

Follow the standard conventions of the language selected for the project. Use consistent indentation within each file and descriptive names for modules, functions, and variables. Prefer small modules with clear responsibilities. Add an appropriate formatter and linter alongside the initial implementation, and document their invocation. Avoid mixing formatting-only changes with functional changes.

## Testing Guidelines

No testing framework or coverage threshold is currently defined. Introduce tests with new functionality and regression tests for bug fixes. Use descriptive test names that identify the behavior and expected result. Keep tests deterministic, isolate external dependencies, and document how to run the suite.

## Commit & Pull Request Guidelines

No Git metadata is available, so existing commit conventions cannot be verified. Use concise, imperative commit subjects, such as `Add delivery status validation`. Keep each commit focused. Pull requests should explain the change, link relevant issues, and report validation performed or why it was not run. Include screenshots for visible interface changes.

## Security & Configuration

Never commit credentials, tokens, or private customer data. Document required configuration using placeholder values, and exclude local secrets and generated artifacts through `.gitignore` when introducing them.
