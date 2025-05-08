# AI Assistant - Standard Coding Process

**ATTENTION AI ASSISTANT: This document outlines the mandatory process you **MUST** follow during general coding interactions to ensure consistent adherence to the project's standards. This process requires adherence to `PROJECT_STANDARDS.md` (Core Principles & Checklist) and the detailed guidelines in the project's specific `PROJECT_ARCHITECTURE.md` (Patterns, Structures, Examples for the relevant language/framework). This process complements, but does not replace, the detailed standards themselves. You **MUST** explicitly report the performance and outcome of mandatory checks as specified below.**

**Goal:** To improve consistency and proactively catch deviations from standards by incorporating explicit checks **and reporting** into the workflow, ensuring a strong emphasis on fundamentally robust solutions over quick fixes or workarounds.
**Interaction Model:** This process assumes **autonomous execution** by the AI, with user intervention primarily reserved for points explicitly marked with the literal text `**BLOCKER:**`. These points are identified within the procedures. Therefore, meticulous self-verification and clear, proactive reporting as outlined below are paramount for demonstrating adherence.

**Toolkit Component Version: Belongs to AI Collaboration Toolkit v0.3.0. See CHANGELOG.md for detailed history.** <!-- MODIFIED -->

---

## Overall Process Structure (Modular Design)

This core document (`CLIPPY.MD`) defines the introduction, core principles, critical checks, and overall structure of the AI coding process. The detailed steps and procedures are organized into separate module files within the `coding-prompts/clippy_modules/` directory for clarity and maintainability.

The AI Assistant **MUST** consult the relevant module file when executing a specific phase or procedure of the workflow. The Table of Contents below provides links to these modules.

---

## Table of Contents (Modular Structure)

1.  [Introduction & Goal](#ai-assistant---standard-coding-process) (This file)
2.  [Core Principles & Critical Checks Summary](#core-principles--critical-checks-summary) (This file)
3.  [Overall Process Structure](#overall-process-structure-modular-design) (This file)
4.  [General Coding Workflow](./clippy_modules/01_GENERAL_WORKFLOW.md) (Steps 0-6)
5.  [Reusable Verification Procedures](./clippy_modules/02_REUSABLE_PROCEDURES.md)
6.  [Exception Handling Procedures](./clippy_modules/03_EXCEPTION_HANDLING.md)
7.  [Runtime Error Diagnosis and Resolution Protocol](./clippy_modules/04_RUNTIME_ERROR_PROTOCOL.md)
8.  [Glossary of Key Terms](./clippy_modules/05_GLOSSARY.md)
9.  [References](./clippy_modules/06_REFERENCES.md)

---

## Core Principles & Critical Checks Summary

*(Content of this section remains the same as before - See original content starting line 58)*

**Key Principle:** While this process provides structure, the primary takeaway is that **strict adherence to the existing verification and checking steps is the most crucial factor** in preventing errors and ensuring robust, maintainable code.
**Self-Driven Compliance Principle:** The AI is responsible for proactively initiating and completing **all** required steps and verifications outlined herein **without external prompting** (except when the literal text `**BLOCKER:**` is used, as this explicitly requires user input). The structured reporting and summaries mandated by this process serve as the primary **proof** of this self-driven compliance; **the act of providing such reports or summaries does not, by itself, constitute a reason to pause or await user input unless explicitly stated by a `**BLOCKER:**` or a specific procedural step requiring confirmation.** Furthermore, to provide clear proof of step execution, the AI **MUST** prefix relevant sections of its responses with the corresponding step number and name (e.g., '**Step 3.2: Identify Standards & Verify Alignment**').

**Note on "Implementation Plan" and "The Plan" References:** Throughout this document, references to an "implementation plan" or "the plan" can mean either:
    a. A formal plan document (e.g., `*.md` file) provided as explicit input for the coding task.
    b. The AI's own internally formulated plan for addressing the user's request, as developed during Step 3 ("Pre-computation Standards Check").
Specific sub-steps that are exclusively relevant when a formal plan document (a) is provided are explicitly conditioned (e.g., "If an implementation plan is provided..."). In the absence of such a formal plan document, these conditional steps may be considered N/A by the AI, and "the plan" refers to the AI's self-derived approach (b).

**Proactive Context Gathering for Robustness:** To further enhance the robustness of understanding and planning, the AI should proactively favor obtaining more comprehensive file context (e.g., via `read_file` with `should_read_entire_file=True` when file size and token limits permit) over relying on minimal or narrowly-scoped partial views. This is particularly encouraged for files directly involved in the core logic of a requested change, even if an initial partial view appears sufficient for the most immediate edit. Such broader context helps in identifying potential indirect impacts and ensures changes are made with fuller situational awareness, aligning with the principle of creating fundamentally robust solutions.

**Holistic Codebase Understanding for Integration and Reusability:** Beyond understanding the immediate file of modification, the AI **MUST** strive to develop a sufficient understanding of the surrounding codebase context when planning and implementing changes. This includes, but is not limited to:
    *   Actively and thoroughly searching for existing relevant functionalities to promote reusability and prevent duplication (as mandated in Step 3.1).
    *   Meticulously analyzing potential impacts on, and interactions with, other modules, components, and layers of the application (as mandated in `Procedure: Analyze Impact`).
A superficial approach to codebase analysis that results in poor integration, duplicated effort, or a clear misunderstanding of the architectural context is a significant deviation from project standards. The goal is to ensure changes are not just locally correct but also fit cohesively and efficiently within the broader system.

**Principle of Successful Edit Application:** An edit to a file is considered successfully applied ONLY when:
    a. The intended planned change is correctly implemented.
    b. The file is left in a state free of any new errors, regressions, or significant unintended modifications introduced by the editing process.
    c. All mandatory verification steps for the edit have been completed and reported.

**Absolute Critical Checks (Non-Negotiable):**

1.  **Verify ALL Diff Lines:** Meticulously verify **every single line** in *both* proposed (`code_edit`) and applied (`edit_file`/`reapply`) diffs against the plan and verified facts. **Treat tool output with extreme skepticism.** See clarification in Step 4 warning.
2.  **Fact-Check Dependencies & Assumptions:** **MUST** factually verify the existence and correctness of **all** dependency references (imports, requires) and critical assumptions (API structures, interfaces) using tools **immediately** when planned (Step 3) and **again** after edits (Step 4). **Do not assume validity.** (See `Procedure: Verify Dependency Reference`, `Procedure: Verify Hypothesis`).
3.  **Address Root Cause:** Prioritize investigating and fixing the underlying cause of issues over applying superficial patches or workarounds. (See Step 3.4, Section 5: `Exception Handling Procedures`).
4.  **Preserve Logic:** When refactoring, meticulously document and ensure the preservation of essential original logic, explicitly justifying any intentional changes. (See `Procedure: Ensure Logic Preservation`).
5.  **Handle Blockers/Deviations Systematically:** Follow the defined procedures when encountering unclear root causes, architectural issues, necessary workarounds, or unexpected deviations in proposed/applied edits. (See Section 5: `Exception Handling Procedures`, Step 4.2.1.d).
6.  **Verify Tool Output Congruence:** For tools where specific outputs are expected based on inputs (e.g., `read_file` returning a specific number of lines or full content, `grep_search` finding all instances), the AI **MUST** actively verify that the tool's actual output aligns with the explicit request parameters and expected outcome. Discrepancies **MUST** be acknowledged and handled before proceeding as if the request was fully met.

**Work with Facts:**
    - Implement based *only* on specified requirements or approved proposals. *Suggest* potential improvements, necessary deviations, or alternative approaches (based on broader knowledge or identified shortcomings) *for discussion and confirmation* before implementation.
    - Ask for clarification if information is missing; do not guess or make assumptions.
    - Do not introduce default values or fallback behaviors unless specifically required.
    - **Handling Information Discrepancies:**
        *   When different sources of information appear to conflict (e.g., runtime errors/tracebacks vs. `read_file` tool output vs. previously applied edits vs. logs), the AI **MUST**:
            1.  Explicitly acknowledge the discrepancy observed.
            2.  Prioritize the most direct evidence of the *runtime state* (typically tracebacks and relevant logs) when forming immediate hypotheses for diagnosing runtime errors.
            3.  Clearly state the prioritized evidence and the resulting hypothesis.
            4.  If tool output (like `read_file`) seems inconsistent with runtime evidence, state this possibility (e.g., "potential for stale tool output") and formulate a plan to reconcile or confirm the actual code state if necessary for the next step (e.g., by attempting a re-read or focusing verification on the specific lines indicated by the runtime evidence).
        *   The goal is transparently address conflicting data and proceed methodically based on the best available evidence for the immediate task (diagnosis or implementation).

---
