# Coding-Clippy

<p align="center">
  <img src="img/logo.png" alt="logo" width="30%" />
</p>

> An easy-to-use toolkit for AI coding with Cursor and the Gemini-2.5-pro model.

## It Looks Like You're Writing Some Code

AI coding tools are happy to:
* Write incorrect code with complete confidence
* Make assumptions instead of checking your actual codebase
* Overlook important dependencies and side effects

## The AI Coding Tool Guide

Coding-Clippy acts as a guide for AI coding tools. This toolkit works with the Cursor environment and gemini-2.5-pro model to mitigate common AI coding pitfalls. It's easy to use and "just works".

What's included:
* **`CLIPPY.md`**: Primary workflow for coding tasks (bug fixes, features, refactoring) that enforces verification procedures, dependency checks, and impact analysis. Most effective when customized to project standards.
* **`PLANNER.md`**: When planning is needed before coding - a specialized guide for creating thoughtful implementation plans. (Currently in Alpha status)
* **Project-Specific Documents** (`PROJECT_STANDARDS.md`, `PROJECT_ARCHITECTURE.md`): Customizable templates that define exactly how code should look and work.

## The Toolkit

### Process Guides
* **`CLIPPY.md`**: A process that guides AI coding tools through planning, verification, and implementation of code changes while preventing common pitfalls.
  * Usage: `@CLIPPY.md Refactor this function...`

* **`PLANNER.md`**: For when a well-thought-out plan is needed before diving into code.
  * Usage: `@PLANNER.md Create a plan for adding OAuth...`
  * (Currently in Alpha status - see `KNOWN_ISSUES.md` for details)

### Project Configuration (Optional but recommended)
* **`PROJECT_STANDARDS.md`** (needs customization): A coding style guide - helps the AI understand project principles, patterns, and testing approach.

* **`PROJECT_ARCHITECTURE.md`** (needs customization): A project blueprint - informs the AI about architecture, components, and structure.

### Supporting Resources
* **`learning_resources/`**: Materials on AI behavior patterns and best practices.

* **`CHANGELOG.md`**: Documentation of toolkit updates.

* **`KNOWN_ISSUES.md`**: Current limitations being addressed.

## Getting Started with Coding-Clippy

Coding-Clippy is designed for immediate use, helping AI coding tools follow a structured process for more reliable results:

1.  **Try It Out Immediately:**
    *   The easiest way to start is to invoke `CLIPPY.md` directly in your requests to the AI.
    *   For example: "`@CLIPPY.md` Refactor this function to improve performance."
    *   `CLIPPY.md` will enforce its core structured process and verification checks, even without project-specific configurations.

2.  **Customize for Best Results:**
    *   While `CLIPPY.md` works out-of-the-box, its true power is unlocked when tailored to a project.
    *   When ready, customize `PROJECT_STANDARDS.md` and `PROJECT_ARCHITECTURE.md` (templates in `example_supporting_documents/`).
    *   Providing details about project principles, coding style, and architecture allows `CLIPPY.md` to guide the AI with much greater precision.

3.  **Using the `PLANNER.md` Guide:**
    *   For tasks requiring detailed upfront planning, use `@PLANNER.md` (e.g., "`@PLANNER.md` Create a plan to add OAuth 2.0 authentication."). (Currently in Alpha status)

4.  **Ensure Document Accessibility:**
    *   Make sure the AI coding tool has access to `CLIPPY.md`, `PLANNER.md`, and (when customized) `PROJECT_STANDARDS.md` and `PROJECT_ARCHITECTURE.md`.
