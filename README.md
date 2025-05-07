# Coding-Clippy

<p align="center">
  <img src="img/logo.png" alt="logo" width="30%" />
</p>

## Common Challenges with AI Coding Assistants

AI coding assistants are powerful tools, but they have limitations:
* Adding incorrect or non-functional code
* Making assumptions without full context
* Missing dependencies and side effects across the codebase

## A Framework for Effective Collaboration

Coding-Clippy provides a structured framework for AI assistants in the Cursor environment using the gemini-2.5-pro model. It guides AI behavior to follow best practices and project-specific requirements.

Core components:
* **Process Guidelines** (`AI_CODING_PROCESS.md`, `PLAN_WRITING_PROCESS.md`): Structured workflows that include analysis, planning, and verification steps before implementing changes.
* **Project-Specific Documents** (`PROJECT_STANDARDS.md`, `PROJECT_ARCHITECTURE.md`): Customizable templates that define your project's coding standards and architecture.

The framework works best when AI follows your customized project documents, ensuring generated code meets your specific requirements while falling back to general best practices when needed.

## Available Tools

* **`AI_CODING_PROCESS.md`**: Primary workflow for coding tasks (bug fixes, features, refactoring). Most effective when used with customized standards documents.
  * Usage: `@AI_CODING_PROCESS.md Refactor this function...`

* **`PLAN_WRITING_PROCESS.md`**: Process for creating implementation plans before coding. (Currently in Alpha status - see `KNOWN_ISSUES.md`)
  * Usage: `@PLAN_WRITING_PROCESS.md Create a plan for adding OAuth...`

* **`PROJECT_STANDARDS.md`** (requires customization): Defines project principles, coding style, and testing approach.

* **`PROJECT_ARCHITECTURE.md`** (requires customization): Documents project architecture, patterns, and structure.

* **`learning_resources/`**: Materials on AI interaction patterns and best practices.

* **`FRAMEWORK_CHANGELOG.md`**: Documents framework updates.

* **`KNOWN_ISSUES.md`**: Lists current limitations.

## Implementation Steps

1. **Customize Project Documents:** Adapt templates from `example_supporting_documents/` to your project's requirements.

2. **Reference Process Documents:** Include the relevant process document in your requests to the AI.

3. **Ensure Document Accessibility:** Make both process documents and your customized standards accessible to the AI.

Following these structured processes and project-specific standards significantly improves AI contribution quality and reliability, helping you avoid common issues.
