# coding-prompts

## 🤖 AI Coding Process and Standards Framework - Focused on Failure Mitigation

The primary goal of this repository is to provide a **framework for mitigating common AI assistant failure modes** encountered during software development, particularly within the Cursor IDE environment. It centers around the mandatory workflow defined in `AI_CODING_PROCESS.md`, which is specifically designed to counteract numerous pitfalls, such as those documented in `ai_failure_modes.md`. Some of the most impactful failure modes addressed include:

*   **Code Generation Verification Failures:**
    *   AI proactively adding unverified code (e.g., imports, defaults) based on flawed assumptions ("Helpful" Additions).
    *   AI skipping rigorous verification of its *own* generated additions against the actual codebase context.
*   **Implementation Planning Failures:**
    *   Designing plans based on idealized states or generic patterns instead of deep analysis of the *current* system (Goal Fixation / Pattern Matching Over Specific Analysis).
    *   Performing shallow verification (e.g., checking component existence without understanding relationships) and failing to synthesize how components work together.
    *   Incomplete tracing of dependencies and their full implications.

While not foolproof, adhering to this framework significantly mitigates the likelihood of these common failures.

To achieve this, the framework provides standardized processes and guidelines intended to govern interactions with an AI coding assistant. It establishes a reusable template adaptable for **any project** that enforces consistency, robustness, and adherence to project-specific standards through rigorous verification and analysis steps.

The framework includes general process definitions (`AI_CODING_PROCESS.md`, `PLAN_WRITING_PROCESS.md`), core principles (`STANDARDS.md`), and an **example** of project-specific technical standards (`code_architecture_standard.md`). The `code_architecture_standard.md` file included here serves as a template and **MUST** be adapted or replaced with guidelines relevant to the specific technology stack and conventions of the target project.

**📄 Key Documents (Templates/Examples):**

*   **`AI_CODING_PROCESS.md`**: Defines the mandatory workflow the AI assistant MUST follow during general coding interactions (analysis, applying edits, verification). **The AI MUST explicitly follow and report on the steps defined in this document.** This process is specifically designed to mitigate common AI failure modes like those detailed in `ai_failure_modes.md`.
*   **`STANDARDS.md`**: Outlines the core principles, development standards, and a checklist that all code changes must adhere to. This covers aspects like code clarity, robustness, testing, and documentation.
*   **`code_architecture_standard.md`**: **(Example/Template)** Provides an *example* set of concrete technical guidelines, preferred architectural patterns, specific library usage conventions, and code examples (Python-focused in this template) that complement `STANDARDS.md`. **This file MUST be customized for the specific project.**
*   **`PLAN_WRITING_PROCESS.md`**: Outlines the mandatory process the AI assistant MUST follow specifically when asked to *generate an implementation plan*. It ensures plans are based on deep code analysis and verification before execution begins.
*   **`AI_CODING_PROCESS_CONFIRM_ADDENDUM.md`**: An *optional* addendum to `AI_CODING_PROCESS.md`. If included in the context, it instructs the AI to seek explicit user confirmation before applying major edits or running commands.
*   **`AI_CODING_PROCESS_CHANGELOG.md`**: A collaborative log tracking agreed-upon refinements and version changes made to the main `AI_CODING_PROCESS.md` document based on practical coding experiences.
*   **`PLAN_WRITING_PROCESS_CHANGELOG.md`**: A collaborative log tracking agreed-upon refinements and version changes made to the `PLAN_WRITING_PROCESS.md` document.

**🛠️ Usage (Applying the Framework to a Project):**

When using this framework for a specific project:
1.  **Customize `code_architecture_standard.md`:** Replace the example content with technical standards, patterns, library choices, and code examples relevant to your project's stack.
2.  Ensure the AI Assistant has access to the relevant documents from this framework (especially `AI_CODING_PROCESS.md`, `PLAN_WRITING_PROCESS.md`, `STANDARDS.md`, and your customized `code_architecture_standard.md`).
3.  Follow the interaction patterns below, referencing the process documents as needed.

Familiarize yourself with these documents to understand the expected interaction flow and the standards the AI is required to uphold. Here's how and when to reference them during project work:

1.  **`AI_CODING_PROCESS.md`**: **(Primary Process for Coding Tasks)**
    *   **When to Use:** Reference this when asking the AI to perform direct coding work (refactoring, adding features, fixing bugs, applying specific changes) within your project.
    *   **How to Use:** Include `@AI_CODING_PROCESS.md` in your prompt.
    *   *Example Prompt:* "`@AI_CODING_PROCESS.md Please refactor the `calculate_totals` function in `reporting.py` to improve clarity.`"

2.  **`PLAN_WRITING_PROCESS.md`**: **(Process for Generating Plans)**
    *   **When to Use:** Reference this specifically when you want the AI to *create a step-by-step implementation plan* for work within your project.
    *   **How to Use:** Include `@PLAN_WRITING_PROCESS.md` in your prompt.
    *   *Example Prompt:* "`@PLAN_WRITING_PROCESS.md Create a plan to add user authentication using OAuth2.`"

3.  **`STANDARDS.md`** & **`(Your Project's) code_architecture_standard.md`**: **(Core Standards & Project Guidelines)**
    *   **When to Use:** These define the quality and technical standards the AI *must* follow for your project.
    *   **How to Use:** No direct `@` mention is typically required in the prompt. The AI is expected to automatically acknowledge and adhere to these based on the requirements in `AI_CODING_PROCESS.md` and `PLAN_WRITING_PROCESS.md`.
    *   *Example (Implicit):* The AI should state at the beginning of a task: *"Acknowledged. I have reviewed and will adhere to the principles in `STANDARDS.md` and the detailed guidelines in your project's `code_architecture_standard.md`."*

4.  **`AI_CODING_PROCESS_CONFIRM_ADDENDUM.md`**: **(Optional Confirmation Step)**
    *   **When to Use:** Include this if you want the AI to pause and ask for your explicit confirmation before it applies significant edits or runs commands, adding an extra layer of control.
    *   **How to Use:** Include `@AI_CODING_PROCESS_CONFIRM_ADDENDUM.md` *in addition to* `@AI_CODING_PROCESS.md` in your prompt.
    *   *Example Prompt:* "`@AI_CODING_PROCESS.md @AI_CODING_PROCESS_CONFIRM_ADDENDUM.md Update the dependency version in requirements.txt and apply the change.`"

5.  **`AI_CODING_PROCESS_CHANGELOG.md`**: **(Collaborative Process Refinement Log)**
    *   **When to Use:** Primarily used to record changes to `AI_CODING_PROCESS.md` after a refinement has been discussed and agreed upon (often based on issues encountered during coding). Can also be consulted to understand process evolution.
    *   **How to Use:** After agreeing on a change to `AI_CODING_PROCESS.md`, you might ask the AI to add an entry to this changelog.
    *   *Example Prompt (Adding an Entry):* "`We agreed to update step 3.4.1.b in @AI_CODING_PROCESS.md regarding hypothesis verification. Please add an entry to @AI_CODING_PROCESS_CHANGELOG.md for version 1.27 summarizing this change and use today's date.`"
    *   *Example Prompt (Consulting):* "`@AI_CODING_PROCESS_CHANGELOG.md What was the reason for the change made in v1.25?`"

6.  **`PLAN_WRITING_PROCESS_CHANGELOG.md`**: **(Collaborative Plan Process Refinement Log)**
    *   **When to Use:** Primarily used to record changes to `PLAN_WRITING_PROCESS.md` after a refinement has been discussed and agreed upon.
    *   **How to Use:** After agreeing on a change to `PLAN_WRITING_PROCESS.md`, you might ask the AI to add an entry to this changelog.
    *   *Example Prompt (Adding an Entry):* "`We agreed to refine step 2.3 in @PLAN_WRITING_PROCESS.md for dependency checks. Please add an entry to @PLAN_WRITING_PROCESS_CHANGELOG.md for version 1.2 summarizing this change and use today's date.`"
    *   *Example Prompt (Consulting):* "`@PLAN_WRITING_PROCESS_CHANGELOG.md Why was version 1.1 created?`"

**💡 General Recommendation:** When requesting coding work or planning, explicitly mentioning the primary process document (`@AI_CODING_PROCESS.md` or `@PLAN_WRITING_PROCESS.md`) helps reinforce the requirement for the AI to follow the structured workflow.
