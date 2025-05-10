# Project Standards

**Version:** 1.0
**Last Updated:** YYYY-MM-DD

## 1. Introduction

This document outlines the core coding principles, style guidelines, and development practices that **MUST** be followed by all contributors to this project. Adherence ensures code quality, consistency, maintainability, and robustness. This document complements `PROJECT_ARCHITECTURE.md`, which details project-specific technical patterns and structures. **The AI Assistant's operational process for adhering to these general standards and any project-specific architecture is detailed in `CLIPPY.md`.**

## 2. Core Principles

*   **Readability:** Code should be clear, concise, and easy for others (and your future self) to understand. Prioritize clarity over overly clever optimizations.
*   **Simplicity (KISS):** Keep solutions as simple as possible while fulfilling requirements. Avoid unnecessary complexity.
*   **Correctness:** Code must function correctly according to specifications and handle edge cases appropriately.
*   **Robustness:** Code should be resilient to errors, invalid inputs, and unexpected conditions. Implement proper error handling and validation.
*   **Maintainability:** Write code that is easy to modify, debug, and extend. Follow established patterns and keep components loosely coupled.
*   **Testability:** Design code with testing in mind. Ensure components can be easily unit-tested and integration-tested.
*   **Consistency:** Adhere to the standards outlined in this document and the patterns in `PROJECT_ARCHITECTURE.md` to maintain a consistent codebase.

**Note:** The AI Assistant is expected to follow the procedures outlined in `CLIPPY.md` to ensure these principles are met, including specific verification, fact-checking, and planning steps to promote robust and correct implementations.

## 3. Code Style & Formatting

## 4. Code Quality & Correctness

*   **Function/Method Conciseness:** Functions and methods should be kept concise, focused on a single responsibility, and avoid excessive nesting. Aim for shorter, understandable units; consider refactoring or decomposing if they become overly long or complex. The AI Assistant includes planning steps to promote this (refer to `CLIPPY.md`, e.g., Steps 3.2, 3.4.0.b, and potential additions like the conciseness check in Step 4.2.1).
*   **DRY Principle (Don't Repeat Yourself):** Avoid code duplication. Actively seek to reuse existing logic. The AI Assistant includes steps to search for existing implementations (refer to `CLIPPY.md`, e.g., Step 3.1).

## 5. Naming Conventions

## 6. Error Handling & Logging

*   **Philosophy:** Fail fast and explicitly. Catch exceptions where they can be meaningfully handled or appropriately logged and re-raised. Avoid catching generic `Exception` unless absolutely necessary and documented.
*   **Custom Exceptions:** Define specific custom exceptions inheriting from base exception classes (e.g., `ProviderError`, `ValidationError`) in the relevant domain or application layer. See `PROJECT_ARCHITECTURE.md` for hierarchy examples.
*   **Logging:**
    *   Use the `loguru` library (recommended), with setup typically in `[project_module]/core/logging_setup.py` (or as defined in project configuration). **The AI Assistant will adhere to the project's chosen logging framework and configuration.**
    *   Log messages should be informative and provide context.

## 7. Testing

## 8. Documentation

## 9. Dependency Management

## 10. Security

## 11. Version Control (Git)

## 12. Pre-commit Hooks

## 13. Implementation Workflow Guidance

*Note: The following is high-level guidance. Project-specific workflows may vary. The AI Assistant follows its own detailed, mandatory operational workflow outlined in `CLIPPY.md` for all coding tasks.*

1.  Understand requirements and refer to `PROJECT_ARCHITECTURE.MD` for design patterns.
2.  Define necessary interfaces or use existing ones.
3.  Implement components, ensuring they adhere to standards for DI, configuration, logging, etc.
4.  Write comprehensive tests.
5.  Ensure documentation (docstrings, READMEs) is updated.
