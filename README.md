# coding-prompts 🤖✨

## Problem: AI Coding Assistants Can Be Unreliable

AI coding assistants are powerful, but sometimes they make mistakes:
*   Adding incorrect code they *think* is helpful.
*   Making plans based on guesses, not your actual code.
*   Missing the impact of changes elsewhere.

(These are detailed further in `ai_failure_modes.md`)

## Solution: A Framework for Better AI Collaboration

This repository provides a set of process guidelines and standards to help AI assistants (especially in Cursor) work more reliably and consistently *within your specific project*.

The core idea is the **`AI_CODING_PROCESS.md`**: a mandatory checklist the AI **must** follow for analysis, planning, and verification *before* making changes. This helps catch errors early.

**It's designed to be adapted for *any* project.**

## Key Documents & When to Use Them

*   **`AI_CODING_PROCESS.md`**: The main workflow for *all* coding tasks (fixing bugs, adding features, refactoring). **Reference this most often.**
    *   *Use:* `@AI_CODING_PROCESS.md Refactor this function...`
*   **`PLAN_WRITING_PROCESS.md`**: A specific process for when you ask the AI to *create a plan* before coding.
    *   *Use:* `@PLAN_WRITING_PROCESS.md Create a plan to add OAuth...`
*   **`PROJECT_STANDARDS.md`**: Core principles (clarity, robustness, etc.) the AI must always follow. (Usually referenced automatically by the processes).
*   **`PROJECT_ARCHITECTURE.md` (❗ EXAMPLE - MUST CUSTOMIZE)**: *Your* project's specific rules, patterns, libraries. **Replace this with your own.** (Also usually referenced automatically).
*   **(Optional) `AI_CODING_PROCESS_CONFIRM_ADDENDUM.md`**: Adds an extra "Are you sure?" step before the AI applies edits or runs commands.
    *   *Use:* `@AI_CODING_PROCESS.md @AI_CODING_PROCESS_CONFIRM_ADDENDUM.md Update dependencies...`
*   **Changelogs (`*_CHANGELOG.md`)**: Track updates made to the process documents themselves.

## Getting Started

1.  **❗ Customize `PROJECT_ARCHITECTURE.md`:** Replace the example content with your project's specific technical standards, patterns, library choices, etc. This is crucial for the AI to follow *your* rules.
2.  **Reference the Process:** When asking the AI to code or plan, include the relevant process document (`@AI_CODING_PROCESS.md` or `@PLAN_WRITING_PROCESS.md`) in your prompt.
3.  **Ensure AI Access:** Make sure the AI can see these key documents.

By making the AI follow these structured processes and your specific standards, you significantly improve the quality and reliability of its contributions. While not foolproof, it helps prevent many common pitfalls.
