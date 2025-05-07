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

Coding-Clippy is here to get your AI back on track.

## The AI Coding Tool Guide

Coding-Clippy acts as a guide for AI coding tools. This toolkit works with the Cursor environment and gemini-2.5-pro model to help mitigate common AI coding pitfalls. It's easy to use and "just works".

What's included:
* **`CLIPPY.md`**: Primary workflow for coding tasks (bug fixes, features, refactoring) that enforces verification procedures, dependency checks, and impact analysis. Most effective when customized to project standards. Includes built-in planning capabilities sufficient for most common coding tasks.
* **`PLANNER.md`**: A specialized guide for creating formal, detailed implementation plans as standalone deliverables before coding begins. Best for complex architectural changes or multi-component features. (Currently in Alpha status)
* **Project-Specific Example Documents** (in `example_supporting_documents/` folder): Example reference files showing how to define code standards and architecture for your project.
  * `PROJECT_STANDARDS.md`: Example coding style guide
  * `PROJECT_ARCHITECTURE.md`: Example project blueprint

Example of `CLIPPY.md` verification step:
<p align="center">
  <img src="img/scr1.png" alt="CLIPPY.md Verification Example" />
</p>

## The Toolkit

### Process Guides
* **`CLIPPY.md`**: A process that guides AI coding tools through planning, verification, and implementation of code changes while preventing common pitfalls.
  * Usage: `@CLIPPY.md Refactor this function...`

* **`PLANNER.md`**: For when a formal, standalone implementation plan is needed before diving into code.
  * Usage: `@PLANNER.md Create a plan for adding OAuth...`
  * Example usage: "`@PLANNER.md` Create a plan for adding OAuth 2.0 authentication across our microservice architecture."
  * (Currently in Alpha status - see `KNOWN_ISSUES.md` for details)

### Example Project Configuration (in `example_supporting_documents/`)
* **`PROJECT_STANDARDS.md`**: An example coding style guide - demonstrates how to document project principles, patterns, and testing approach.

* **`PROJECT_ARCHITECTURE.md`**: An example project blueprint - shows how to document architecture, components, and structure.

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
    *   When ready, use the example files in `example_supporting_documents/` as references to create your own project-specific versions of `PROJECT_STANDARDS.md` and `PROJECT_ARCHITECTURE.md`.
    *   Providing details about project principles, coding style, and architecture allows `CLIPPY.md` to guide the AI with much greater precision.
    *   Note: Consider renaming the `example_supporting_documents/` folder to something more descriptive like `example_files/` or `reference_documents/` to better reflect their purpose.

3.  **Using the `PLANNER.md` Guide:**
    *   For most coding tasks, `CLIPPY.md`'s built-in planning capabilities (Step 3) are sufficient and you don't need to use `PLANNER.md` separately.
    *   Use `@PLANNER.md` only when you need a formal, standalone implementation plan for complex architectural changes, major refactorings, or multi-component features that require extensive upfront planning and possibly stakeholder review.
    *   Example usage: "`@PLANNER.md` Create a plan for adding OAuth 2.0 authentication across our microservice architecture."
    *   (Currently in Alpha status - see `KNOWN_ISSUES.md` for details)
