# coding-prompts 🤖✨

## Problem: AI Coding Assistants Can Be Unreliable

AI coding assistants are powerful, but sometimes they make mistakes:
*   Adding incorrect code they *think* is helpful.
*   Making plans based on guesses, not your actual code.
*   Missing the impact of changes elsewhere.

## Solution: A Framework for Better AI Collaboration

This repository provides a framework specifically designed to help AI assistants within the Cursor environment, leveraging the gemini-2.5-pro model. It is tailored for this specific setup and will most likely not function as intended with other tools or models. The core components are:

*   **Process Guidelines (e.g., `AI_CODING_PROCESS.md`, `PLAN_WRITING_PROCESS.md`):** These define mandatory workflows for the AI, including checklists for analysis, planning, and verification *before* making changes. This helps catch errors early.
*   **Project-Specific Documents (❗ `PROJECT_STANDARDS.md`, `PROJECT_ARCHITECTURE.md`):** You **MUST CUSTOMIZE** these using the provided examples. They define your project's unique coding styles, architectural patterns, and operational rules.

The process guidelines encourage using your customized project-specific documents, which provide key context on your project's coding standards and architecture. While the guidelines include general best practices as fallbacks, using your specific documents helps the AI produce code that better fits your project. This structured approach helps the AI's contributions align with your specific needs and helps mitigate common issues like incorrect assumptions or duplicated functionality.

## Key Documents & When to Use Them

*   **`AI_CODING_PROCESS.md`**: The main workflow for *all* coding tasks (fixing bugs, adding features, refactoring). **Reference this most often.** (While most effective when used with your customized `PROJECT_STANDARDS.md` and `PROJECT_ARCHITECTURE.md`, it includes fallbacks if these are not yet available.)
    *   *Use:* `@AI_CODING_PROCESS.md Refactor this function...`
*   **`PLAN_WRITING_PROCESS.md`**: A specific process for when you ask the AI to *create a plan* before coding. **(Note: Currently `Component Status: Alpha` - undergoing refinement to improve robustness of generated plans. See `KNOWN_ISSUES.md`.)**
    *   *Use:* `@PLAN_WRITING_PROCESS.md Create a plan to add OAuth...`
*   **`PROJECT_STANDARDS.md` (❗ MUST CUSTOMIZE)**: Defines *your* project's core principles, coding style, testing approach, etc. The AI **must** follow these. (Usually referenced automatically by the process documents). **See `example_supporting_documents/` for a template.**
*   **`PROJECT_ARCHITECTURE.md` (❗ MUST CUSTOMIZE)**: Describes *your* project's specific architecture, patterns, libraries, and directory structure. The AI **must** follow this. (Usually referenced automatically by the process documents). **See `example_supporting_documents/` for a template.**
*   **`learning_resources/` (Directory)**: Contains supplementary materials like `ai_failure_modes.md` and `ai_plan_writing_pitfalls.md`. These offer deeper dives into AI interaction patterns, common pitfalls, and best practices that have shaped this framework's design. Essential for understanding the *why* behind the process guidelines.
*   **`FRAMEWORK_CHANGELOG.md`**: Tracks updates made to the process documents themselves.
*   **`KNOWN_ISSUES.md`**: Lists known shortcomings or areas for improvement in the framework's process documents.

## Getting Started

1.  **❗ Customize `PROJECT_STANDARDS.md` and `PROJECT_ARCHITECTURE.md`:** Copy the templates from `example_supporting_documents/` to the root of `coding-prompts/` (or elsewhere accessible) and **fill them with your project's specific details**. This is crucial for the AI to follow *your* rules.
2.  **Reference the Process:** When asking the AI to code or plan, include the relevant process document (`@AI_CODING_PROCESS.md` or `@PLAN_WRITING_PROCESS.md`) in your prompt.
3.  **Ensure AI Access:** Make sure the AI can see these key documents (the process documents and *your customized* standards/architecture documents).

By making the AI follow these structured processes and your specific standards, you significantly improve the quality and reliability of its contributions. While not foolproof, it helps prevent many common pitfalls.
