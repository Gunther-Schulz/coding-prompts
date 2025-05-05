# Project Standards

**Version:** 1.0
**Last Updated:** YYYY-MM-DD

## 1. Introduction

This document outlines the core coding principles, style guidelines, and development practices that **MUST** be followed by all contributors to this project. Adherence ensures code quality, consistency, maintainability, and robustness. This document complements `PROJECT_ARCHITECTURE.md`, which details specific technical patterns and structures.

## 2. Core Principles

*   **Readability:** Code should be clear, concise, and easy for others (and your future self) to understand. Prioritize clarity over overly clever optimizations.
*   **Simplicity (KISS):** Keep solutions as simple as possible while fulfilling requirements. Avoid unnecessary complexity.
*   **Correctness:** Code must function correctly according to specifications and handle edge cases appropriately.
*   **Robustness:** Code should be resilient to errors, invalid inputs, and unexpected conditions. Implement proper error handling and validation.
*   **Maintainability:** Write code that is easy to modify, debug, and extend. Follow established patterns and keep components loosely coupled.
*   **Testability:** Design code with testing in mind. Ensure components can be easily unit-tested and integration-tested.
*   **Consistency:** Adhere to the standards outlined in this document and the patterns in `PROJECT_ARCHITECTURE.md` to maintain a consistent codebase.

## 3. Code Style & Formatting

*   **Language:** Python 3.11+
*   **Style Guide:** Adhere strictly to [PEP 8](https://www.python.org/dev/peps/pep-0008/).
*   **Formatting:** Use `black` for automated code formatting. Configuration is in `pyproject.toml`.
*   **Linting:** Use `ruff` for linting. Configuration is in `pyproject.toml`. All code MUST pass linting checks before merging.
*   **Imports:**
    *   Follow PEP 8 import ordering (standard library, third-party, local application).
    *   Use `isort` (via `ruff`) for automated import sorting.
    *   Avoid wildcard imports (`from module import *`).
    *   Use absolute imports for local modules where possible (e.g., `from beat_the_books.domain.models import Game`) unless relative imports significantly improve clarity within closely related submodules.
*   **Line Length:** Maximum 100 characters.

## 4. Naming Conventions

*   **Modules:** `lowercase_with_underscores.py`
*   **Packages:** `lowercase_with_underscores`
*   **Classes:** `CapWords` (e.g., `GameProvider`, `AnalysisResult`)
*   **Functions & Methods:** `lowercase_with_underscores()` (e.g., `fetch_game_data()`, `calculate_odds()`)
*   **Variables:** `lowercase_with_underscores` (e.g., `market_price`, `team_name`)
*   **Constants:** `ALL_CAPS_WITH_UNDERSCORES` (e.g., `DEFAULT_TIMEOUT`, `MAX_RETRIES`)
*   **Private Members:** Use a single leading underscore (`_private_method`, `_internal_state`) for internal implementation details. Avoid `__double_leading_underscore` unless necessary for name mangling in inheritance scenarios.

## 5. Error Handling & Logging

*   **Philosophy:** Fail fast and explicitly. Catch exceptions where they can be meaningfully handled or appropriately logged and re-raised. Avoid catching generic `Exception` unless absolutely necessary and documented.
*   **Custom Exceptions:** Define specific custom exceptions inheriting from base exception classes (e.g., `ProviderError`, `ValidationError`) in the relevant domain or application layer. See `PROJECT_ARCHITECTURE.md` for hierarchy examples.
*   **Logging:**
    *   Use the `loguru` library configured in `beat_the_books/config/logging.py`.
    *   Log messages should be informative and provide context.
    *   Use appropriate log levels: `DEBUG` for detailed diagnostics, `INFO` for operational messages, `WARNING` for potential issues, `ERROR` for runtime errors, `CRITICAL` for severe errors.
    *   Avoid logging sensitive information (passwords, API keys).

## 6. Testing

*   **Framework:** `pytest`
*   **Requirement:** All new features and bug fixes **MUST** include corresponding tests (unit and/or integration).
*   **Unit Tests:** Focus on testing individual functions or classes in isolation. Use mocking (`unittest.mock`) to isolate dependencies. Strive for high unit test coverage for core logic.
*   **Integration Tests:** Test interactions between components or with external systems (e.g., database, APIs). Use fixtures and potentially test containers where appropriate.
*   **Test Location:** Tests reside in the `tests/` directory, mirroring the `src/` structure (e.g., `tests/domain/test_models.py` tests `src/beat_the_books/domain/models.py`).
*   **Assertions:** Use clear and specific `pytest` assertions.

## 7. Documentation

*   **Docstrings:** All public modules, classes, functions, and methods **MUST** have docstrings following the [Google Python Style Guide](https://google.github.io/styleguide/pyguide.html#38-comments-and-docstrings).
*   **READMEs:** Maintain clear `README.md` files at the project root and potentially within complex sub-packages.
*   **Architecture:** Key architectural decisions and patterns are documented in `PROJECT_ARCHITECTURE.md`.
*   **Inline Comments:** Use sparingly for explaining complex or non-obvious logic. Avoid comments that merely restate the code.

## 8. Dependency Management

*   **Tool:** Use `conda` for environment management and `pip` (within conda environment) for package installation.
*   **Definition Files:**
    *   `environment.yml`: Defines the conda environment and core dependencies.
    *   `pyproject.toml`: Defines project metadata, build dependencies, and tool configurations (black, ruff, pytest). Used by `pip` for installation.
    *   `conda-lock.yml`: Generated lock file for reproducible conda environments.
*   **Adding Dependencies:** Add new dependencies to `environment.yml` (for conda-managed) or `pyproject.toml` (under `[project.dependencies]`) and regenerate lock files (`conda-lock`).

## 9. Security

*   **Input Validation:** Always validate and sanitize external input (API requests, user input, file contents). Use Pydantic models for data validation where applicable.
*   **Secrets Management:** **NEVER** commit secrets (API keys, passwords, credentials) directly into the codebase. Use environment variables or a dedicated secrets management solution (e.g., HashiCorp Vault, AWS Secrets Manager), accessed via configuration.
*   **Dependencies:** Keep dependencies up-to-date to patch known vulnerabilities. Regularly review dependency licenses.

## 10. Version Control (Git)

*   **Branching Model:** Use a feature-branch workflow (e.g., Gitflow or GitHub Flow). Create branches from `development` or `main` for new features or fixes.
*   **Commit Messages:** Follow the [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) specification (e.g., `feat: add new provider API client`, `fix: correct odds calculation error`).
*   **Pull Requests (PRs):**
    *   All changes **MUST** be submitted via Pull Requests.
    *   PRs should be focused and address a single feature or fix.
    *   PR descriptions should clearly explain the change and link to relevant issues.
    *   PRs require at least one approval from another team member before merging.
    *   All CI checks (linting, testing) **MUST** pass before merging.
*   **Merging:** Use squash merges or rebase merges to maintain a clean commit history on the main branches.

## 11. Pre-commit Hooks

*   Use `pre-commit` hooks defined in `.pre-commit-config.yaml` to automatically run checks (e.g., formatting, linting) before commits are created. Ensure hooks are installed (`pre-commit install`).
