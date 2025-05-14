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

## 🚀 Quick start
**`@CLIPPY.md Add a new API endpoint for /users`**


## The Toolkit

Coding-Clippy acts as a guide for AI coding tools. This toolkit works with the [Cursor](https://www.cursor.com/) environment and [gemini-2.5-pro](hhttps://deepmind.google/technologies/gemini/) model to help mitigate common AI coding pitfalls. It's easy to use and "just works".

Coding-Clippy offers a set of guides and resources to improve AI-assisted coding:

*   **Core Workflow Guides**:
    *   🤖 **`CLIPPY.md`**: Primary workflow for coding tasks (bug fixes, features, refactoring) that enforces verification procedures, dependency checks, and impact analysis. Most effective when customized to project standards. Includes built-in planning capabilities sufficient for most common coding tasks.
    *   📜 **`PLANNER.md`**: (*Alpha status*) A specialized guide for creating detailed, formal implementation plans *before* coding begins, suitable for complex projects.
*   **Example Project Configuration** (in `templates/` folder): Example reference files showing how to define code standards and architecture for your project.
    *   `PROJECT_STANDARDS.md`: Example coding style guide - demonstrates how to document project principles, patterns, and testing approach.
    *   `PROJECT_ARCHITECTURE.md`: Example project blueprint - shows how to document architecture, components, and structure.
*   **Supporting Resources**:
    *   `learning_resources/`: Educational materials on AI behavior and best practices.
    *   `CHANGELOG.md`: Tracks updates to the toolkit.
    *   `KNOWN_ISSUES.md`: Lists current limitations.
    *   `TODO.md`: Planned enhancements and features.

## Getting Started with Coding-Clippy

Coding-Clippy is designed for immediate use and can be progressively integrated into your workflow for increasingly tailored AI guidance.

**1. Try It Out Immediately (Using `CLIPPY.md`)**

The quickest way to benefit from Coding-Clippy is by invoking `CLIPPY.md` directly in your AI prompts, as shown in **The Toolkit** section above. This enforces a structured process and verification, even without project-specific setup.

Some common ways to use `CLIPPY.md`:
*   For adding new features: *`@CLIPPY.md Add a dark mode toggle to the application settings`*
*   For fixing bugs: *`@CLIPPY.md Investigate and fix the null pointer exception when a user profile is incomplete.`*
*   For UI changes: *`@CLIPPY.md Add a 'Last Updated' timestamp to the dashboard header.`*

`CLIPPY.md` will guide the AI through:
    *   Understanding the request.
    *   Planning the changes.
    *   Verifying dependencies and potential impacts.
    *   Implementing the code.
    *   Suggesting tests.

Example of `CLIPPY.md` verification step:
<p align="center">
  <img src="img/scr1.png" alt="CLIPPY.md Verification Example" />
</p>

**2. Integrate and Customize for Your Project**

For the best results, integrate Coding-Clippy into your project and customize its reference documents:

*   **Add as a Git Submodule (Recommended):**
    This keeps the toolkit updated and separate from your project's codebase.
    ```bash
    # In your project's root directory:
    git submodule add https://github.com/Gunther-Schulz/coding-clippy
    ```

*   **Create Your Project-Specific Documents:**
    *   Develop `PROJECT_STANDARDS.md` and `PROJECT_ARCHITECTURE.md` files **within your own project**.
    *   **Note:** Do not rename these files. `CLIPPY.md` and `PLANNER.md` refer to them by these exact names.
    *   Use the examples in this toolkit's `templates/` folder (e.g., `external/coding-clippy/templates/`) as a starting point.
    *   For `CLIPPY.md` or `PLANNER.md` to access and utilize the contents of your project-specific documents (like `PROJECT_STANDARDS.md` and `PROJECT_ARCHITECTURE.md`), you **must** include them in the chat prompt alongside `@CLIPPY.md` or `@PLANNER.md` using the `@` mention (e.g., `@PROJECT_STANDARDS.md @PROJECT_ARCHITECTURE.md`). This ensures they are part of the AI's active context.
    *   Tailoring these documents allows `CLIPPY.md` to guide the AI with much greater relevance to your codebase.
    *   **Example of using `CLIPPY.md` with custom project documents:**
        *   Assuming you have `PROJECT_STANDARDS.md` and `PROJECT_ARCHITECTURE.md` in your project:
        *   *`@CLIPPY.md @PROJECT_STANDARDS.md @PROJECT_ARCHITECTURE.md Refactor the user authentication module`*

**3. Using the `PLANNER.md` Guide (For Complex Tasks)**

While `CLIPPY.md` includes planning steps sufficient for most tasks, `PLANNER.md` is available for situations requiring a formal, standalone implementation plan *before* coding. This is ideal for:

*   Complex architectural changes.
*   Major refactorings.
*   Multi-component features needing extensive upfront planning or stakeholder review.

*   **Usage Example:**
    *   *`@PLANNER.md Create a plan for adding OAuth 2.0 authentication across our microservice architecture.`*
    *   To incorporate your project standards and architecture into the planning phase:
    *   *`@PLANNER.md @PROJECT_STANDARDS.md @PROJECT_ARCHITECTURE.md Design the database schema for the new inventory management module and write the plan to a markdown file.`*
*   **(Currently in Alpha status - see `KNOWN_ISSUES.md` for details)**

**4. Controlling Output Verbosity (`CLIPPY.MD`)**

`CLIPPY.MD` allows you to control the level of detail in the AI's reporting during its execution. This helps you tailor the output to your preference, from highly detailed step-by-step accounts to concise summaries.

*   **Verbosity Levels:**
    *   `verbose`: The AI reports on every single step and sub-step. Maximum transparency.
    *   `standard` (Default): The AI provides summaries of major phases, key decisions, critical findings, and proposed changes. Balances detail with conciseness.
    *   `quiet`: The AI primarily reports only critical items like `BLOCKER:` conditions, direct questions, final outcomes, and `code_edit` proposals.
    *   `very_quiet`: The AI limits output to the absolute minimum: `BLOCKER:`s, direct questions, `code_edit` proposals, and final completion notices.

*   **How to Set Verbosity:**
    *   You can instruct the AI to use a specific verbosity level at the beginning of your task or session when invoking `@CLIPPY.MD`.
    *   **Example:**
        *   *`@CLIPPY.MD Set verbosity to verbose. Add a new API endpoint for /products`*
        *   *`@CLIPPY.MD Use quiet mode. Refactor the user_service.py file.`*
    *   If no verbosity is specified, `CLIPPY.MD` defaults to `standard` as per its internal guidelines.

*   **Impact on Process:**
    *   Importantly, changing the verbosity level **does not change the underlying rigor of the `CLIPPY.MD` process**. The AI is still required to perform all internal checks, verifications, and adhere to all procedural steps (including self-assessment via `P11`), regardless of the reporting level. The verbosity setting only affects what is shown to you.
    *   However, a general challenge with AI is ensuring deep execution versus superficial "box-checking," especially for internal steps not immediately reported. The detailed articulation required by `verbose` mode naturally encourages thorough processing. For lower verbosity levels (especially `quiet` and `very_quiet`), `CLIPPY.MD` heavily relies on the AI's mandated internal diligence and specifically on **`Procedure P11: Perform Self-Assessment` (which includes a "Substantive Thoroughness Review")** to ensure that all steps are fully executed and not just superficially acknowledged. While the framework is designed for consistent rigor, `verbose` or `standard` modes offer users greater real-time insight into the AI's detailed execution and depth of analysis.
    *   Conversely, `verbose` mode, while maximizing transparency, also requires the AI to dedicate resources to the detailed articulation of every step. In highly complex or lengthy tasks, this extensive reporting *could* subtly increase the overall cognitive load on the AI, potentially diverting some processing effort from the core problem-solving.
    *   Finally, consider the practical trade-offs in interaction speed and error discovery. `verbose` mode leads to longer AI turns and higher token usage, potentially slowing the overall task rhythm. `quiet` or `very_quiet` modes offer faster individual turns but may delay the detection of non-critical missteps due to less frequent detailed feedback, potentially requiring more significant corrections later. The `standard` verbosity level is generally recommended as it aims to provide a practical balance across these factors.
