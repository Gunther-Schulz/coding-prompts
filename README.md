# coding-prompts 🤖✨

## Problem: AI Coding Assistants Can Be Unreliable

AI coding assistants are powerful, but sometimes they make mistakes:
*   Adding incorrect code they *think* is helpful.
*   Making plans based on guesses, not your actual code.
*   Missing the impact of changes elsewhere.

## Solution: A Framework for Better AI Collaboration

This repository provides a set of process guidelines and standards to help AI assistants (especially in Cursor) work more reliably and consistently *within your specific project*.

The core idea is the **`AI_CODING_PROCESS.md`**: a mandatory checklist the AI **must** follow for analysis, planning, and verification *before* making changes. This helps catch errors early.

**It's designed to be adapted for *any* project.**

## Key Documents & When to Use Them

*   **`AI_CODING_PROCESS.md`**: The main workflow for *all* coding tasks (fixing bugs, adding features, refactoring). **Reference this most often.**
    *   *Use:* `@AI_CODING_PROCESS.md Refactor this function...`
*   **`PLAN_WRITING_PROCESS.md`**: A specific process for when you ask the AI to *create a plan* before coding.
    *   *Use:* `@PLAN_WRITING_PROCESS.md Create a plan to add OAuth...`
*   **`PROJECT_STANDARDS.md` (❗ MUST CUSTOMIZE)**: Defines *your* project's core principles, coding style, testing approach, etc. The AI **must** follow these. (Usually referenced automatically by the process documents). **See `example_supporting_documents/` for a template.**
*   **`PROJECT_ARCHITECTURE.md` (❗ MUST CUSTOMIZE)**: Describes *your* project's specific architecture, patterns, libraries, and directory structure. The AI **must** follow this. (Usually referenced automatically by the process documents). **See `example_supporting_documents/` for a template.**
*   **`FRAMEWORK_CHANGELOG.md`**: Tracks updates made to the process documents themselves.

## Getting Started

1.  **❗ Customize `PROJECT_STANDARDS.md` and `PROJECT_ARCHITECTURE.md`:** Copy the templates from `example_supporting_documents/` to the root of `coding-prompts/` (or elsewhere accessible) and **fill them with your project's specific details**. This is crucial for the AI to follow *your* rules.
2.  **Reference the Process:** When asking the AI to code or plan, include the relevant process document (`@AI_CODING_PROCESS.md` or `@PLAN_WRITING_PROCESS.md`) in your prompt.
3.  **Ensure AI Access:** Make sure the AI can see these key documents (the process documents and *your customized* standards/architecture documents).

By making the AI follow these structured processes and your specific standards, you significantly improve the quality and reliability of its contributions. While not foolproof, it helps prevent many common pitfalls.
