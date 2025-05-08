# Coding-Clippy

<p align="center">
  <img src="img/logo.png" alt="logo" width="30%" />
</p>

<p align="center">
  <strong>⚡ Vibe Coder Certified ⚡</strong>
</p>

> An easy-to-use toolkit for AI coding with Cursor and the Gemini-2.5-pro model.

## It Looks Like You're Writing Some Code

AI coding tools are happy to:
* Write incorrect code with complete confidence
* Make assumptions instead of checking your actual codebase
* Overlook important dependencies and side effects

Coding-Clippy is here to have your back and get your AI back on track.

## The AI Coding Tool Guide

Coding-Clippy acts as a guide for AI coding tools. This toolkit works with the Cursor environment and gemini-2.5-pro model to help mitigate common AI coding pitfalls. It's easy to use and "just works".

What's included:
* 🤖 **`CLIPPY.md`**: Primary workflow for coding tasks (bug fixes, features, refactoring) that enforces verification procedures, dependency checks, and impact analysis. Most effective when customized to project standards. Includes built-in planning capabilities sufficient for most common coding tasks.
* 📜 **`PLANNER.md`**: (Currently in **Alpha** status.) A specialized guide for creating formal, detailed implementation plans as standalone deliverables before coding begins. Best for complex architectural changes or multi-component features.
* **Project-Specific Example Documents** (in `templates/` folder): Example reference files showing how to define code standards and architecture for your project.
  * `PROJECT_STANDARDS.md`: Example coding style guide
  * `PROJECT_ARCHITECTURE.md`: Example project blueprint

Example of `CLIPPY.md` verification step:
<p align="center">
  <img src="img/scr1.png" alt="CLIPPY.md Verification Example" />
</p>

## The Toolkit

### Process Guides
* 🤖 **`CLIPPY.md`**: A process that guides AI coding tools through planning, verification, and implementation of code changes while preventing common pitfalls.
  * Usage: *`@CLIPPY.md Add a new API endpoint for /users`*

* 📜 **`PLANNER.md`**: For when a formal, standalone implementation plan is needed before diving into code.
  * Usage: *`@PLANNER.md Create a plan for adding OAuth...`*
  * (Currently in Alpha status - see `KNOWN_ISSUES.md` for details)

### Example Project Configuration (in `templates/`)
* **`PROJECT_STANDARDS.md`**: An example coding style guide - demonstrates how to document project principles, patterns, and testing approach.

* **`PROJECT_ARCHITECTURE.md`**: An example project blueprint - shows how to document architecture, components, and structure.

### Supporting Resources
* **`learning_resources/`**: Materials on AI behavior patterns and best practices.

* **`CHANGELOG.md`**: Documentation of toolkit updates.

* **`KNOWN_ISSUES.md`**: Current limitations being addressed.

* **`CLIPPY_Optional_Additions.md`**: Contains experimental principles and guidelines. These are candidates for possible future incorporation into the main `CLIPPY.md` script to address specific recurring issues or to further refine AI behavior based on ongoing testing and project needs.

* **`TODO.md`**: A list of planned enhancements, features to investigate, and potential improvements for the `Coding-Clippy` toolkit, particularly for `CLIPPY.md`.

## Getting Started with Coding-Clippy

Coding-Clippy is designed for immediate use, helping AI coding tools follow a structured process for more reliable results:

1.  **Try It Out Immediately:**
    *   The easiest way to start is to invoke `CLIPPY.md` directly in your requests to the AI.
    *   For example: *`@CLIPPY.md Add a new API endpoint for /users`*
    *   `CLIPPY.md` will enforce its core structured process and verification checks, even without project-specific configurations.

2.  **Integrate and Customize for Best Results:**
    *   To effectively use `Coding-Clippy` with your project, consider adding this repository as a Git submodule. This allows you to easily keep the toolkit updated while maintaining a clean separation from your project's codebase.
      ```bash
      # In your project's root directory:
      git submodule add <URL_to_Coding-Clippy_repository> external/coding-clippy
      # (Replace <URL_to_Coding-Clippy_repository> with the actual Git URL of this toolkit)
      git submodule update --init --recursive
      ```
    *   Create and maintain your project-specific `PROJECT_STANDARDS.md` and `PROJECT_ARCHITECTURE.md` files **directly within your own project** (e.g., in your project's root, a dedicated `docs/` folder, or even a `.github/` folder).
    *   You can use the files in this toolkit's `templates/` directory (located within the submodule, e.g., `external/coding-clippy/templates/`) as templates or starting points for your own project documents.
    *   When using `@CLIPPY.md` or `@PLANNER.md` from the submodule, you will then reference your project's versions of these documents by providing their path relative to your project root (e.g., `@PROJECT_STANDARDS.md`, `@docs/PROJECT_ARCHITECTURE.md`).
    *   Tailoring these documents with your project's specific principles, coding style, and architecture enables `CLIPPY.md` (and other toolkit components) to guide the AI with significantly greater accuracy and relevance to your codebase.

3.  **Using the `PLANNER.md` Guide:**
    *   For most coding tasks, `CLIPPY.md`'s built-in planning capabilities (Step 3) are sufficient and you don't need to use `PLANNER.md` separately.
    *   Use `@PLANNER.md` only when you need a formal, standalone implementation plan for complex architectural changes, major refactorings, or multi-component features that require extensive upfront planning and possibly stakeholder review.
    *   Example usage: "`@PLANNER.md` Create a plan for adding OAuth 2.0 authentication across our microservice architecture."
    *   (Currently in Alpha status - see `KNOWN_ISSUES.md` for details)
