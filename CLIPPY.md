# AI Assistant - Standard Coding Process

**ATTENTION AI ASSISTANT: This document outlines the mandatory process you **MUST** follow during general coding interactions to ensure consistent adherence to the project's standards. This process requires adherence to `PROJECT_STANDARDS.md` (Core Principles & Checklist) and the detailed guidelines in the project's specific `PROJECT_ARCHITECTURE.md` (Patterns, Structures, Examples for the relevant language/framework). This process complements, but does not replace, the detailed standards themselves. You **MUST** explicitly report the performance and outcome of mandatory checks as specified below.**

**Goal:** To improve consistency and proactively catch deviations from standards by incorporating explicit checks **and reporting** into the workflow, ensuring a strong emphasis on fundamentally robust solutions over quick fixes or workarounds.
**Interaction Model:** This process assumes **autonomous execution** by the AI, with user intervention primarily reserved for points explicitly marked with the literal text `**BLOCKER:**`. These points are identified within the procedures. Therefore, meticulous self-verification and clear, proactive reporting as outlined below are paramount for demonstrating adherence.

**Toolkit Component Version: Belongs to AI Collaboration Toolkit v0.2.17. See CHANGELOG.md for detailed history.**

we
---

## Table of Contents

1.  [Introduction & Goal](#ai-assistant---standard-coding-process)
2.  [Core Principles & Critical Checks Summary](#core-principles--critical-checks-summary)
3.  [General Coding Workflow](#general-coding-workflow)
    *   [Step 0: Model Compatibility & Awareness Check](#0-model-compatibility--awareness-check)
    *   [Step 1: Confirm Standards Awareness](#1-confirm-standards-awareness-initial-step)
    *   [Step 2: Confirm Goal](#2-confirm-goal-recommended)
    *   [Step 3: Pre-computation Standards Check (Planning)](#3-pre-computation-standards-check-planning-phase)
    *   [Step 4: Edit Generation & Verification Cycle](#4-edit-generation--verification-cycle)
    *   [Step 5: Adherence Checkpoint](#5-adherence-checkpoint-final-step-in-cycle-critical)
    *   [Step 6: Summarize Deferred Observations (Optional)](#6-summarize-deferred-observations-optional)
4.  [Reusable Verification Procedures](#reusable-verification-procedures)
    *   [`Procedure: Ensure Sufficient File Context`](#procedure-ensure-sufficient-file-context)
    *   [`Procedure: Prepare Robust Edit Tool Input`](#procedure-prepare-robust-edit-tool-input)
    *   [`Procedure: Verify Dependency Reference`](#procedure-verify-dependency-reference)
    *   [`Procedure: Analyze Impact`](#procedure-analyze-impact)
    *   [`Procedure: Verify Hypothesis`](#procedure-verify-hypothesis)
    *   [`Procedure: Verify Diff`](#procedure-verify-diff)
    *   [`Procedure: Ensure Logic Preservation`](#procedure-ensure-logic-preservation)
    *   [`Procedure: Verify Framework Compatibility`](#procedure-verify-framework-compatibility)
    *   [`Procedure: Verify Configuration Usage Impact`](#procedure-verify-configuration-usage-impact)
    *   [`Procedure: Verify Reapply Diff`](#procedure-verify-reapply-diff)
    *   [`Procedure: Verify Edit File Diff`](#procedure-verify-edit-file-diff)
5.  [Exception Handling Procedures](#exception-handling-procedures)
    *   [`Procedure: Handle Unclear Root Cause / Missing Info`](#procedure-handle-unclear-root-cause--missing-info)
    *   [`Procedure: Handle Architectural Decisions`](#procedure-handle-architectural-decisions)
    *   [`Procedure: Handle Necessary Workaround`](#procedure-handle-necessary-workaround)
    *   [`Procedure: Consult on Ambiguous Missing Dependency`](#procedure-consult-on-ambiguous-missing-dependency)
    *   [`Procedure: Handle Failed Verification for Existing Dependency`](#procedure-handle-failed-verification-for-existing-dependency)
    *   [`Procedure: Handle Deviation`](#procedure-handle-deviation)
    *   [`Procedure: Request Manual Edit`](#procedure-request-manual-edit)
6.  [Glossary of Key Terms](#glossary-of-key-terms)
7.  [Protocol for Runtime Error Diagnosis and Resolution](#7-protocol-for-runtime-error-diagnosis-and-resolution)
8.  [References](#8-references)

---

## Core Principles & Critical Checks Summary

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

## General Coding Workflow

When responding to user requests involving code analysis, planning, or modification, you **MUST** integrate the following steps **and explicitly report their execution and outcome in your responses**:

**Meta-Instruction on Reporting Detail:** The level of detail in reporting for each executed step (1 through 6) **MUST** remain consistently explicit and thorough, adhering to the specific requirements outlined for each step and procedure in this document. This applies equally to proactive development/refactoring tasks and to reactive debugging/error resolution tasks (see Section 7: Protocol for Runtime Error Diagnosis and Resolution). If a specific check or sub-step is genuinely not applicable in a given context, it **MUST** be explicitly marked `[-] N/A` with a concise justification, rather than being omitted silently.

### 0. Model Compatibility & Awareness Check

*   **Trigger:** At the very beginning of handling any coding-related request.
*   **Action:**
    1.  **Identify Current Model:** The AI Assistant **MUST** identify the model it is currently operating as.
    2.  **Verify Compatibility:**
        *   If the current model is `gemini-2.5-pro`, proceed to Step 1.
        *   If the current model is **NOT** `gemini-2.5-pro`:
            *   The AI Assistant **MUST** state its current model name.
            *   The AI Assistant **MUST** inform the user: "This coding process (`AI_CODING_PROCESS.md`) is optimized for `gemini-2.5-pro`. You are currently interacting with `[Actual Model Name]`. Some capabilities, verification rigor, or complex instruction interpretations assumed by this process might not be fully supported or optimally performed by `[Actual Model Name]`, potentially leading to suboptimal code, errors, or process deviations. Are you sure you want to continue with `[Actual Model Name]` using this process?"
            *   **BLOCKER:** The AI Assistant **MUST NOT** proceed with the coding workflow (Step 1 onwards) until the user explicitly confirms they wish to continue with the current non-optimized model. If the user does not confirm, await further instructions.
*   **Next Step:** Upon successful model verification (or user confirmation for non-optimized models), proceed to Step 1.

### 1. Confirm Standards Awareness (Initial Step)

*   **Trigger:** At the very beginning of handling any coding-related request.
*   **Action:** Explicitly state that you have read and understood the requirements outlined in `PROJECT_STANDARDS.md` and the project's specific `PROJECT_ARCHITECTURE.md`. *Example: "Acknowledged. I have reviewed and will adhere to the principles in `PROJECT_STANDARDS.md` and the detailed guidelines in `PROJECT_ARCHITECTURE.md`."*

### 2. Confirm Goal (Recommended)

*   **Trigger:** Before detailed planning, especially for complex or potentially ambiguous requests.
*   **Action:** Briefly restate your understanding of the user's ultimate goal for this change and ask for confirmation.
*   *Example: "To confirm my understanding, the goal is to refactor X to achieve Y, primarily by implementing pattern Z. Is this correct?"*

### 3. Pre-computation Standards Check (Planning Phase)

**3.0. Assess Target File Complexity & Ensure Sufficient Context (Initial Check):**
*   **Action:** Execute `Procedure: Ensure Sufficient File Context` (Section 4).
*   Subsequent impact analysis (Step 3.4.1) and edit generation (Step 4.1) must apply maximum scrutiny based on the (ideally complete) information obtained.

**NOTE:** Foundational checks (Steps 3.4, 3.5, 3.6) take precedence. If analysis reveals underlying issues (robustness, unknown root cause, architectural conflicts), these **MUST** be addressed by **STOPPING the standard plan and executing the appropriate procedure from Section 5: `Exception Handling Procedures`** *before* proceeding with the original task. Embrace necessary detours.

*   **Trigger:** Before generating a multi-step plan or proposing a specific code edit (`edit_file` call).
*   **Action:**

    `3.1.` **Search for Existing Logic:** Before planning implementation details (especially for utilities, validation, calculations), perform a preliminary search (`grep_search`, `codebase_search`) for existing implementations of the required functionality (referencing Audit Sections 1.B, 1.C). Briefly document findings, **explicitly stating the tool(s) used, query term(s), and specific key findings (files/functions) or confirming none found.** (e.g., "`grep_search` for `format_date`: Found existing utility `utils.format_date`, will reuse." or "`codebase_search` for `validate_user_input`: No specific existing validator found, planning new implementation.").

    `3.2.` **Identify Standards & Verify Alignment:** Explicitly identify the Core Principles or specific sections from `PROJECT_STANDARDS.md` **and relevant technical patterns/structures from the project's `PROJECT_ARCHITECTURE.md`** that are most pertinent to the request. Briefly state how the proposed plan/edit aligns with these standards, **citing the primary relevant section/pattern name(s)**. **Confirm the plan prioritizes the simplest, clearest solution that fully meets requirements and adheres to all standards. Furthermore, when the plan involves generating new functions, methods, or significant blocks of new logic, the AI MUST ensure this planned new code itself will be structured for simplicity (e.g., appropriate length, limited nesting, clear control flow) and adhere to the Single Responsibility Principle. If a conceptual unit of new code seems inherently complex, the plan SHOULD proactively include its decomposition into smaller, more focused units.**
        *   *Example 1 (DI Focus): "Planning to add component X. This requires adhering to the Dependency Injection principles (`PROJECT_STANDARDS.md`) and specific DI configuration guidelines (`PROJECT_ARCHITECTURE.md`). Plan involves updating the DI config module per standard practice."*
        *   *Example 2 (Error Handling Focus): "Plan includes error handling for Y, following Error Handling guidelines (`PROJECT_STANDARDS.md` - Section 2.A) and hierarchy examples (`PROJECT_ARCHITECTURE.md` - `StandardErrorHandling` pattern), using standard logging."*
        *   *(Note for AI: If `PROJECT_STANDARDS.md` or `PROJECT_ARCHITECTURE.md` are not explicitly provided in the current context, or if they are provided but do not cover a specific framework/library detail relevant to the task, the AI **MUST** state this. Alignment should then be based on: 1) general software design principles, 2) any architectural patterns observed directly in the existing codebase, 3) the specific requirements of the user's request, and critically, 4) canonical documentation or widely accepted best practices for the specific framework/library in use (e.g., Typer, SQLAlchemy, React). Hypotheses derived from these framework/library best practices (e.g., 'Typer expects command groups to be registered at module level for discoverability') MUST be explicitly stated and verified as per Step 3.4.1.b.)*

    `3.3.` **Check "Work with Facts":** Confirm the plan/edit relies *only* on provided facts, requirements, or verified information. **This includes ensuring that any file content used for planning or generating edits is sufficiently complete to avoid guesswork; if partial file views are ambiguous or insufficient for the task, seek clarification or more complete data before proceeding.** If a full file read was attempted (e.g. using `should_read_entire_file=True`) but not fully returned by the tool, this **MUST** be treated as insufficient/incomplete data, triggering clarification or data request as per `Procedure: Ensure Sufficient File Context`. State if clarification is needed. *Example: "Proceeding based on requirement Z. Clarify if other factors apply."*

    `3.3.1.` **Reconcile Plan with Current Code State (SUGGESTION 2):** Before detailed planning for a specific file (or before generating `code_edit` for it), if the AI has access to an implementation plan and the current file content, it **SHOULD** perform a preliminary comparison.
        *   If the current file content indicates that some parts of the implementation plan for that file *already appear to be implemented or partially implemented*, the AI **MUST**:
            a.  Explicitly state this observation (e.g., "Observation: `src/module/file.py` appears to have existing code that aligns with Plan Steps X and Y regarding feature Z.").
            b.  Briefly confirm its understanding of the *remaining scope of work* from the plan for that file.
            c.  If the divergence is significant or creates ambiguity for the remaining steps, the AI **MAY** ask the user for clarification or confirmation on how to proceed with the remaining parts of the plan in light of the existing code.
            d.  This check helps ensure the AI doesn't attempt to re-implement already existing logic or misinterpret the plan's applicability to a partially evolved codebase.

    `3.4.` **Check "Robust Solutions":** Confirm the plan avoids hacks/workarounds and addresses the root cause.
        **Note on Framework Warnings:** When a framework issues a `RuntimeWarning` related to core invocation patterns (e.g., async function not awaited, incorrect registration), prioritize understanding and resolving the *source of the warning* itself. Treating only the subsequent errors (e.g., `NoneType` errors due to incomplete setup) without addressing the warning may lead to superficial fixes. The warning often points to the true root cause of instability or incorrect behavior.
        `3.4.0.a` **Assess Interacting Existing Patterns (SUGGESTION 1):** When planned changes significantly interact with or depend upon existing architectural patterns or critical execution paths within the target file(s) (e.g., application startup, core callbacks, central configuration), the AI **MUST** briefly assess these *existing* patterns for obvious robustness concerns that could be exacerbated by the planned changes (e.g., late initializations, potential for shared state issues, overly complex existing logic that the new code will plug into).
            *   **If significant concerns directly relevant to the stability of the planned integration are identified, they should be highlighted.**
            *   This is not a full audit of existing code, but a focused check on immediate interaction points critical for the success of the planned changes.
            *   *Example: 'The plan involves adding new CLI groups. Existing CLI group registration in `app.py` occurs late in a complex callback. This existing pattern might pose a risk to the new groups\'\'\'availability if the callback fails early. Consider if registrations should be moved to module level for robustness.'*

        `3.4.0.b` **Assess Target Code Complexity & Propose Refactoring (If Needed):** Before generating an edit for a specific code block (e.g., a function, method), the AI **MUST** assess its complexity (considering factors like length, nesting depth, number of responsibilities, overall clarity).
            *   **Trigger:** If the target code block is deemed excessively complex, making it difficult to modify reliably with automated tools or challenging to maintain.
            *   **Action:**
                i.  **STOP** proceeding with the originally planned edit on the complex block.
                ii. **Propose Preliminary Refactoring:** Explicitly propose a refactoring plan to break down the complex block into smaller, more manageable units (e.g., helper functions, smaller methods).
                iii.**Explain Rationale:** Clearly state why refactoring is recommended (e.g., "Refactoring function `X` into smaller helpers is proposed to improve clarity, testability, and significantly reduce the risk of automated edit tool errors during the planned modification.").
                iv. **BLOCKER:** State that this proposal requires user confirmation. **Do NOT proceed with the refactoring NOR the original planned edit on the complex code until the user explicitly approves the refactoring plan OR instructs to proceed with the original edit despite the complexity risks.**
            *   *Example Proposal:* "**Step 3.4.0.b: Assess Target Code Complexity:** The target function `_perform_mapping` in `review.py` is highly complex (significant length, multiple nested conditions handling different entity types and actions). **Proposal:** Before applying the planned logging addition, I propose refactoring `_perform_mapping` into smaller helper functions (e.g., `_find_target_canonical`, `_upsert_map_entry`, `_cleanup_unresolved_logs`). **Rationale:** This will improve readability and maintainability, and critically, it will significantly increase the likelihood of the subsequent logging addition being applied correctly by the edit tool without unintended side-effects. **BLOCKER:** Do you approve this preliminary refactoring plan, or should I attempt the original edit on the existing complex function (with increased risk)?*"

        **Data Integrity Priority:** When encountering **unexpectedly missing or invalid data** from external sources or between internal components:
            *   **Default Action:** Treat as a **critical error**. Log clearly and raise an appropriate, specific error (e.g., `ConversionError`, `ValidationError`) to halt processing for the affected item.
            *   **Justification:** Prioritizes surfacing data quality/integration issues over potentially masking them.
            *   **Deviation (Requires Explicit Approval):** Implementing fallbacks/defaults **MUST STOP** standard plan and follow `Procedure: Handle Necessary Workaround` (Section 5), clearly stating the data integrity compromise. **This requires explicit user approval (see procedure).**

        `3.4.1.` **Analyze Impact & Verify Assumptions:**
            `a.` **Perform `Procedure: Analyze Impact` (Section 4)**: Identify and list affected call sites, perform enhanced scope checks for core changes, check circular dependencies, and consider data representation impacts. State outcome. **When reporting on enhanced scope checks (Procedure Step 2), MUST include specific inheritors/consumers identified.**
            `b.` **Perform `Procedure: Verify Hypothesis` (Section 4) for ALL Assumptions**: **Immediately** after stating each assumption (external APIs, internal interfaces, config, data formats, library usage), execute and report the verification using the specified structured format. **CRITICAL:** Includes verifying *interface consistency* between interacting components and assumptions about *functional equivalence* when replacing logic. **DO NOT** proceed until verification outcome is 'Confirmed'. **The structured report generated by `Procedure: Verify Hypothesis` MUST immediately follow the statement of the hypothesis in the response text before proceeding to state or verify any subsequent hypotheses or continue with other planning steps.** **If verification for an *existing* dependency fails, STOP standard plan and execute `Procedure: Handle Failed Verification for Existing Dependency` (Section 5).** If verification for any other assumption fails, **STOP** and revise the plan.** *(Reminder: Address Failure Mode 2 - Verify against actual codebase state, avoid assumptions based solely on general patterns)* **(SUGGESTION 3 STARTS HERE) If an implementation plan is provided, but its 'Key Assumptions' section (or equivalent) appears sparse or incomplete for a task involving complex interactions with existing code, the AI **SHOULD proactively identify and state any additional critical assumptions it is making about the current state or structure of the existing codebase that are necessary for the plan's successful execution.** These self-identified assumptions **MUST** then also be verified using this procedure. (SUGGESTION 3 ENDS HERE)** Furthermore, when a formal implementation plan document IS provided, ALL its explicit assumptions, prescribed actions involving specific code interactions (e.g., calling a function with an expected signature, modifying a data structure in an expected way), and referenced existing code structures (e.g., function names, class structures, module interfaces purportedly used or modified by the plan) MUST be treated as hypotheses by the AI. These plan-derived hypotheses MUST be immediately verified against the current codebase using tools (`read_file`, `grep_search`, `codebase_search`) before any code generation based on those specific parts of the plan proceeds. The AI MUST NOT assume the provided plan's representation of existing code is perfectly current or correct, and MUST report the outcome of these verifications.** *(Reminder: Address Failure Mode 2 - Verify against actual codebase state, avoid assumptions based solely on general patterns)*
            `c.` **Enumerate Edge Cases/Errors (Mandatory for Logic Changes):** List key potential edge cases/errors for new/modified logic and state **specifically how the plan addresses each enumerated case**. *Example: "Edge Cases: 1. Empty input list: Handled by initial check `if not input_list: return`. 2. Division by zero: Handled by pre-check `if divisor != 0`."*
            `d.` **Perform `Procedure: Verify Framework Compatibility` (Section 4)**: For framework entry points or library interactions, verify signature compatibility and adherence to established project patterns. State outcome.
            `e.` **Perform `Procedure: Ensure Logic Preservation` (Section 4) (Mandatory for Logic Replacement):** When replacing/restructuring logic blocks, document original behavior first ( **citing specific conditions or execution paths from the original code.** ), then detail how the new logic preserves it, explicitly justifying intentional changes and presenting trade-offs if necessary.
            `f.` **Perform `Procedure: Verify Configuration Usage Impact` (Section 4)**: When changing config values, identify usage locations and confirm compatibility/updates. State outcome.
            `g.` **Consider Testability:** Briefly assess if the proposed code structure facilitates unit/integration testing. Note if dependencies are injectable or if the structure hinders testability.
            `h.` **Consider Observability:** Briefly assess if the change warrants new or updated structured logging or metrics for monitoring and diagnostics, referencing project standards if available.
            `i.` **Consider Configuration Needs & Anti-Hardcoding:** Assess if the proposed change requires new configurable parameters. If so, the plan should outline how these will be integrated with existing configuration mechanisms and explicitly affirm the avoidance of hardcoding values that should be configurable.
            `j.` **Consider Resource Management:** If the planned code interacts with system resources (e.g., files, network connections, database sessions, locks), the plan **MUST** detail how these resources will be properly acquired and, critically, consistently released (e.g., using context managers or `try...finally` blocks), especially ensuring cleanup in error scenarios.

        `3.4.2.` **Verification for Moves/Renames:** Use *multiple* search strategies (as detailed in `Procedure: Analyze Impact`, Section 4) to confirm impact. **DO NOT** rely on a single 'no results' search. *Example: 'Modifying `parser_x` impacts `processor_y`. Plan includes verifying compatibility.'*

        `3.4.3.` **Prefer Atomic Edits:** Break down complex changes into smaller, atomic edits where feasible.

    `3.5.` **Handle Blockers (Unclear Root Cause / Missing Info / Standard Conflicts):**
        *   **Trigger:** If Steps 3.3 or 3.4 reveal unclear root causes, missing info, standard conflicts, failing standard debugging methods, or certain failed dependency verifications (see `Procedure: Handle Failed Verification for Existing Dependency`, Section 5).
        *   **Action:** **STOP** proposing a direct fix. **Execute `Procedure: Handle Unclear Root Cause / Missing Info` (Section 5).**

    `3.6.` **Handle Architectural Decisions / Suboptimal Patterns:**
        *   **Trigger:** When analysis reveals architectural conflicts, significant trade-offs between valid paths, or foundational inconsistencies.
        *   **Action:** **IMMEDIATELY STOP** detailed planning. **Execute `Procedure: Handle Architectural Decisions` (Section 5). This is a **BLOCKER:** requiring user guidance.**

    `3.7.` **Trace Data Flow (If Applicable):**
        *   **Trigger:** For validation errors OR modifications to data path functions.
        *   **Action:** Trace data from origin to consumption/validation, reading relevant intermediate functions.

    `3.8.` **Improve Edit Tool Reliability (Before `edit_file` Call):**
        *   **Trigger:** Before calling `edit_file`.
        *   **Action:** Execute `Procedure: Prepare Robust Edit Tool Input` (Section 4) when formulating the `code_edit` string and `instructions` field. This includes being explicit in `instructions` (add vs. modify/replace) and ensuring sufficient context/anchoring.

    `3.9.` **Exception for Diagnostics:**
        *   **Trigger:** For temporary deviations (e.g., print statements/temporary logging).
        *   **Action:**
            `a. MAY` implement directly.
            `b. MUST` state: **"Applying TEMPORARY change for diagnostics."**
            `c. MUST` justify necessity.
            `d. MUST` include marker: **"REMINDER: Temporary code MUST be reverted/fixed."**
            `e. Plan MUST` include reverting/fixing it later **and explicitly verifying its complete removal.**

    `3.10.` **Perform Pre-computation Verification Summary (Before Step 4):**
        *   **Trigger:** Before proceeding to Step 4.
        *   **Action:** **MUST** generate a **Pre-computation Verification Summary**. Structure using the following format. Mark steps as `[x]` if completed, or `[-] N/A: [brief justification]` if not applicable, providing a concise reason.
        ```markdown
        **Pre-computation Verification Summary:**
        - `[x/-] 1. Standards Alignment:` *[Brief confirmation 3.2 performed.]*
        - `[x/-] 2. Impact Analysis:` *[Brief confirmation `Procedure: Analyze Impact` (3.4.1.a) performed. Outcome: e.g., 'Standard impact checked', 'Core impact N/A'.]*
        - `[x/-] 3. Hypothesis Verification:` *[Brief confirmation `Procedure: Verify Hypothesis` (3.4.1.b) performed for ALL assumptions. Outcome: e.g., 'All verified', 'Assumption X confirmed'.]*
        - `[x/-] 4. Logic Preservation Plan:` *[Brief confirmation `Procedure: Ensure Logic Preservation` (3.4.1.e) performed if applicable. Mark `[x]` if done, `[-]` if N/A.]*
        - `[x/-] 5. Blocker Checks:` *[Brief confirmation blocker checks (3.5/3.6) performed and guidance sought/received via Section 5 procedures if triggered. Mark `[x]` if checked & resolved/approved, `[-]` if N/A.]*
        - `[x] 6. Confirmation:` Pre-computation verification summary complete. *(Always `[x]`)*
        ```
        *(Note: This summary serves as mandatory proof that pre-computation checks were completed autonomously before proceeding. Explicit justification is required for any step marked N/A.)*

        **IMMEDIATE NEXT ACTION (Unless Blocked in Step 3):** Upon outputting this summary, and if no `**BLOCKER:**` from Step 3 has halted the process, you **MUST** immediately and autonomously continue to Step 4.1 (Generate Proposed `code_edit` Diff Text) for the first relevant file. This transition to Step 4 **MUST** occur within the same AI response turn. **Do not pause for user input here.**

    `3.11.` **Verify Action Preconditions (Before Generating Edit):** Before proceeding to Step 4.1 for a file, explicitly re-verify the *immediate preconditions* for the planned action based on the most recent file content obtained (ideally in Step 3.0). Examples:
        *   If planning to *delete* specific lines/code blocks, **MUST** briefly re-confirm using `grep_search` or targeted `read_file` that those elements *currently exist*.
        *   If planning to *add* a specific function/class/import, **MUST** briefly re-confirm it does *not already exist* in the intended form/location.
        *   If planning to *modify* a block, **MUST** briefly re-confirm the block exists and appears structurally compatible with the planned change.
        If a precondition is not met (e.g., trying to delete something that's already gone), **STOP**, report the discrepancy, and revise the plan (potentially skipping the edit for this file). This check must be explicitly confirmed in the response before Step 4.1.

        **IMMEDIATE NEXT ACTION (Unless Blocked in Step 3 or Precondition Check Failed):** Upon successful completion and confirmation of the Precondition Check (3.11) (or if it was N/A), and if no `**BLOCKER:**` from Step 3 has halted the process, you **MUST** immediately and autonomously continue to Step 4.1 (Generate Proposed `code_edit` Diff Text) for the first relevant file. This transition to Step 4 **MUST** occur within the same AI response turn. **Do not pause for user input here.**

### 4. Edit Generation & Verification Cycle

*   **Purpose:** This step covers the critical cycle of generating a proposed code edit, verifying it rigorously *before* application, applying it, and verifying the result *after* application.
*   **Core Cycle:** [Generate -> Pre-Verify -> Apply -> Post-Verify -> Summarize]
*   **Sequential Execution and Autonomous Operation:**
    *   The sub-steps within this section (4.1 through 4.5, corresponding to the Core Cycle) **MUST** be executed sequentially in their presented numerical order for each file being modified. No sub-step may be skipped or reordered during the processing of a file.
    *   **Autonomous Execution and Turn Management for Steps 4.1 to 4.3:**
        *   Step 4.1 (Generate Proposed `code_edit` Diff Text) and Step 4.2 (Pre-Apply Verification – including the execution and full reporting of `Procedure: Verify Diff` as per Step 4.2.1.b, and the "Pre-Edit Confirmation Statement" as per Step 4.2.2) **MUST** be completed sequentially and fully reported before proceeding to Step 4.3.
        *   After the "Pre-Edit Confirmation Statement" (Step 4.2.2) is output, thereby confirming the successful completion of all Pre-Apply Verification checks:
            *   The AI **MUST** proceed to Step 4.3 (Apply Edit) by initiating the relevant tool call (e.g., `edit_file`, `reapply`) within the **same response turn** in which the "Pre-Edit Confirmation Statement" (Step 4.2.2) is output. The AI's response turn will conclude with the tool call being issued. No other statements, planning activities, or user prompts are permitted between the full completion and reporting of Step 4.2 and the initiation of the Step 4.3 tool call.
        *   The only natural pause point within this per-file cycle (before commencing Step 4.4) is while awaiting the result of the tool call made in Step 4.3.
        *   **Do not pause for general user input during this per-file cycle (Steps 4.1 through 4.5)** unless a sub-step explicitly states it is a `**BLOCKER:**` or specifically requires asking for user confirmation (e.g., Step 2, or procedures in Section 5 that explicitly call for halting and user input).
    *   **Application to Multi-File Changes:** When a single user request or task necessitates changes to multiple files, this entire [Generate -> Pre-Verify -> Apply -> Post-Verify -> Summarize] cycle (Steps 4.1 through 4.5) **MUST** be fully completed for one individual file *before* commencing Step 4.1 (Generate Proposed `code_edit` Diff Text) for any subsequent file involved in the same task. This per-file atomicity ensures that each modification is applied and verified against a consistent and up-to-date state of the codebase.
*   **Rationale for Sequential and Mandatory Execution:** Each step in this cycle (4.1 through 4.5) logically builds upon the successful and verified completion of the preceding one. This strict order is paramount for systematically preventing errors, ensuring that all changes meticulously align with the verified plan, and proactively maintaining the integrity and quality of the codebase. All five sub-steps are mandatory to form a comprehensive verification loop around every code modification.

    **CRITICAL WARNING:** VERIFY ALL DIFF LINES. Failure to meticulously verify the *entire* diff (both proposed and applied) is a primary cause of regressions. **Verification means ensuring every changed/added line aligns with the plan, is syntactically correct, all referenced imports/symbols/variables are valid, and no unexpected logic changes or side-effects are introduced.** Treat tool output (`edit_file`, `reapply`) with extreme skepticism. For instance:
*   *If your intent was to add a specific function, but the diff also deletes an unrelated class, this is a critical tool failure.*
*   *If your plan was to rename a variable within one method, but the diff shows modifications to logic in other methods, this is a critical tool failure.*
*   *If you aimed to insert a simple log statement, but the diff also introduces new helper functions or complex conditional blocks, this is a critical tool failure.*
In all such cases where the tool's changes significantly exceed or deviate from the precise, narrow intent, it must be treated as a tool misapplication, even if some unintended changes seem minor. **This includes being skeptical of what the diff *doesn't* show. If an intended change, such as a deletion, is not explicitly confirmed by the diff, assume it may not have happened and verify directly (see `Procedure: Verify Diff`, Step 7).**

    #### 4.1 Generate Proposed `code_edit` Diff Text
    *   **Action:** Based on the verified plan from Step 3, formulate the `code_edit` content (the proposed diff text).
        *   Apply `Procedure: Prepare Robust Edit Tool Input` (Section 4) for guidelines on constructing the `code_edit` string (including context, anchoring, handling of sensitive files, block movements, deprecated code handling) and the `instructions` field.
        *   **Output of this Step:** The formulated `code_edit` string (the proposed diff text). **You MUST present this string clearly as the output of Step 4.1 before proceeding to Step 4.2.**
        *   This step ONLY involves formulating the `code_edit` text content. You **MUST NOT** call `edit_file` or `reapply` during this step. Application occurs *only* in Step 4.3 after successful verification in Step 4.2.

    #### 4.2 Pre-Apply Verification (Mandatory Before 4.3)
    *   The following verification steps **MUST** be successfully completed and reported *before* proceeding to Step 4.3 (Apply Edit).
    *   **Purpose:** To meticulously scrutinize the *proposed `code_edit` diff* against the verified plan *before* calling the `edit_file` tool.
    *   **Mandate:** **DO NOT SKIP OR RUSH.** Treat proposed diffs skeptically.
    *   **Action:** Perform the following checks on the generated `code_edit` from 4.1:

        `4.2.1` **Perform Granular Final Review & Deviation Handling Loop (Pre-Edit):**
            *   **WARNING:** AI model may add unrequested lines.
            *   `a.` **Summarize Pre-Edit Context:** Briefly summarize relevant existing code structure from recent `read_file` calls.
            *   `b.` **Perform Diff Verification:** **MUST** execute `Procedure: Verify Diff` (Section 4) on the *proposed `code_edit` diff*. The 'intent' for this verification is the plan established in Step 3. **The execution of `Procedure: Verify Diff` MUST be reported using the multi-line checklist format detailed in that procedure's "Execution Reporting" section and example.**

        `4.2.2` **Generate Pre-Edit Confirmation Statement:** Before proceeding to 4.3, **MUST** provide brief statement confirming 4.2.1 checks (Context Summary and Diff Verification), **mentioning explicit verification of key assumptions/dependencies** and logic preservation if applicable.
            *   *Example (Refactoring): "**Step 4.2: Pre-Apply Verification:** Complete. Context summarized. `Procedure: Verify Diff` executed on proposed edit against plan (Outcome: Verified, no deviations). Key assumption 'API X' verified (Outcome: Confirmed). Logic Preservation: Confirmed plan preserves behavior. Proceeding to Apply Edit (4.3)."*
            *   *Example (Deviation Handling): "**Step 4.2: Pre-Apply Verification:** Complete. Context summarized. `Procedure: Verify Diff` executed on proposed edit against plan (Outcome: Verified, deviations handled - reported in `Procedure: Handle Deviation`). Key assumption 'Model Y' verified (Outcome: Confirmed). Logic Preservation: N/A. Proceeding to Apply Edit (4.3)."*

    #### 4.3 Apply Edit (After Successful 4.2)
    *   After successful completion and reporting of Step 4.2 (Pre-Apply Verification), **MUST** call the appropriate edit tool (e.g., `edit_file` or `reapply`).

    #### 4.4 Post-Apply Verification (Mandatory After 4.3 Tool Call Result)
    *   **Purpose:** To meticulously verify the *actual diff applied* to the file.
    *   **Action:** After a successful `edit_file` or `reapply` call result, explicitly check **and report the outcome of each** of the following:

        `4.4.1` **Verify Edit Application:**
            *   `a.` Post-Reapply Verification:** ** **CRITICAL:** If modification resulted from `reapply`:
                *   **Perform `Procedure: Verify Reapply Diff` (Section 4)**. This involves treating the diff as new, re-performing full pre-edit verification (4.2.1 checks) on the applied diff, explicitly handling deviations, and logging confirmation.
            *   `b.` Post-edit_file Verification:** ** **CRITICAL:** For diffs from standard `edit_file` (not `reapply`):
                *   **WARNING:** Treat Diff Output with Extreme Skepticism.
                *   **Perform `Procedure: Verify Edit File Diff` (Section 4)**. This includes: diff match, semantic spot-check, **mandatory** dependency re-verification (`Procedure: Verify Dependency Reference`, Section 4), context line check, final logic preservation validation, and discrepancy handling.
            *   `c.` No Introduced Redundancy:** ** Check for duplicate logic, unnecessary checks, redundant mappings Remove if found.

        `4.4.2` **Check for Leftover Code & Dependencies:**
            *   `a.`  Confirm Absence of Leftover Code/Comments: ** **MUST** verify that no old code remains commented out and that temporary/AI process comments have been removed.
            *   `b.`  Verify Downstream Consumers & Cleanup: **
                i.  **Dependency Re-verification:** **RE-VERIFICATION:** For changes involving modified/added interfaces, paths, symbols, or core models, **explicitly state re-verification is being performed**, **re-execute the codebase search (`grep_search`)** from planning (Step 3.4.1), and report findings to confirm all dependents were updated or unaffected. **Do not rely solely on initial planning search.** If missed updates found, **trigger self-correction (4.4.3)**.
                ii. **Verification of Deletion:** **CRITICAL (AFTER `delete_file`):** Confirm removal by searching for dangling references (absolute path fragment, relative references). **Report search strategies and outcome.** If lingering references found, **MUST trigger self-correction (4.4.3)**.
                iii.**Functional Check:** Confirm consumers still function as expected (via analysis, suggesting tests if available).

        `4.4.3` **Self-Correct if Necessary:**
            *   `a.` Trigger:** If reviews (4.2.1, 4.4.1, 4.4.2) reveal violations, incorrect application, redundancy, or leftover cleanup issues, **or the introduction of new errors/regressions in any part of the edited file, even if unrelated to the primary planned change.** Also triggered by specific missed mandatory steps (see below).
            *   `b.` Action:**
                a.  **STOP** proceeding with flawed logic/state. State the issue discovered.
                b. **MUST** explicitly state flaw** based on standard/process (e.g., "Workaround violated standards." or "Applied diff mismatch.").
                c.**Triggers for Self-Correction (STOP and Address):**
                    a.  **Missed mandatory immediate verification for a stated hypothesis (Step 3.4.1.b / `Procedure: Verify Hypothesis`) during the preceding planning phase.**
                    b.  Missed mandatory Pre-Edit Verification (4.2.1), Post-Reapply Verification (4.4.1.a), Post-Edit File Verification (4.4.1.b), or Dependency Re-verification (4.4.2.b.i).
                    c.  Missed mandatory Post-Action Verification Summary (4.4.3) for the preceding edit.
                    d.  Missed mandatory halt for guidance under blocker procedures (e.g., 3.6 / `Procedure: Handle Architectural Decisions`).
                    e.  Any other missed mandatory step, check, reporting, or verification from this document.
                    f.  Missed required Enhanced Scope Impact Analysis (part of `Procedure: Analyze Impact`) for core component modification.
                    g.  The `edit_file` or `reapply` tool reported that "no changes were made" when the AI's plan explicitly intended to make a modification (e.g., deleting specific lines, adding specific lines).
                d. **MUST** revise plan/next step for investigation, correction, or cleanup. State the corrective action. **This corrective action MUST aim to restore the overall integrity and correctness of the file, addressing both deviations from the planned change AND any new issues introduced by the edit tool.** If the file's state is a result of the edit tool making unintended modifications to unrelated code, the primary goal of the self-correction is to achieve the user's intended change *without* these unintended side-effects, or to clean up those side-effects if the intended change is already present.
                e.  **If Tool Failure Persists OR Edit Application Requires Complex Cleanup:**
                    Execute `Procedure: Request Manual Edit` (Section 5) under the following conditions:
                    i.  **Persistent Correction Failure:** Repeated attempts (e.g., >3 attempts for the same planned logical change to a specific file section, or if **2 consecutive `edit_file`/`reapply` attempts for a single planned change fail to produce the desired, verified outcome without significant unintended side-effects**) to apply a *necessary and planned correction* fail **(SUGGESTION 4 STARTS HERE) (especially if the failed edits involve large structural block movements or changes in files known to be particularly complex or sensitive, and the `edit_file` tool consistently fails to place these accurately). (SUGGESTION 4 ENDS HERE)**
                    ii. **Grossly Disproportionate or Corrupting Unintended Modifications:** If a single `edit_file` or `reapply` application, particularly for an edit with a very narrow and specific intent (e.g., adding a few specific lines, fixing a typo, simple refactoring of a small block), results in modifications that are grossly disproportionate to the intent or fundamentally corrupt the file's structure or syntax in unintended ways. This includes, but is not limited to:
                        *   Deletion or alteration of large code blocks unrelated to the direct target of the edit.
                        *   Widespread reordering of code.
                        *   Introduction of syntax errors or significant logical errors into previously correct and unrelated sections of the file.
                        If an immediate attempt to create a highly targeted `code_edit` to *only revert the unintended corruptions* (while preserving the intended change if it was partially applied correctly) also fails or leads to further significant errors, OR if the *same pattern* of disruptive modification occurs on a second consecutive attempt on the same file for the same overall task, then proceed to `Procedure: Request Manual Edit`. The goal is to avoid lengthy, unpredictable self-correction loops when the editing tool demonstrates a fundamental misunderstanding of the file structure or edit boundaries for a specific, narrow task.
                    This involves a **BLOCKER:** step.

    #### 4.5 Generate Post-Action Verification Summary (After Successful 4.4)
    *   `a.` Trigger:** Immediately following the *final* successful verification (Step 4.4) for the task's edits.
    *   `b.` Action:** **MUST** generate a **Post-Action Verification Summary**. Confirms post-apply checks were **performed and documented**. Structure using the following format. Mark steps as `[x]` if completed, or `[-] N/A: [brief justification]` if not applicable, providing a concise reason.
        ```markdown
        **Post-Action Verification Summary:**
        - `[x/-] 1. Edit Application Analysis:` *[Brief confirm checks 4.4.1.a (if `reapply`), **4.4.1.b (via `Procedure: Verify Edit File Diff`, Section 4)**, 4.4.1.c (redundancy) done. State: 'Applied diff matches final intent.' OR 'Applied diff discrepancies: [List/Justification].'.]*
        - `[x/-] 2. Leftover Code & Dependency Analysis:` *[Brief confirm check **4.4.2.a (artifacts)** and **4.4.2.b (explicit dependency re-verification/deletion checks)** done. Outcome: e.g., "Cleanup OK", "Re-verification confirmed no missed updates".]*
        - `[x/-] 3. Correction Assessment:` *[Brief confirm check 4.4.3 performed. State if corrections were needed/made. Mark `[x]` if checked & OK, `[-]` if N/A.]*
        - `[x] 4. Confirmation:` Post-Action verification summary complete for `[filename(s)/task]`. *(Always `[x]`)*
        ```
        *(Note: This summary serves as mandatory proof that post-action verifications were completed autonomously. Explicit justification is required for any step marked N/A.)*

### 5. Adherence Checkpoint (Final Step in Cycle): **CRITICAL:**

*   **Trigger:** Before concluding interaction for the current task/request cycle.
*   **Action:** Perform a final **self-assessment** check: Have all mandatory steps (1-4, including required sub-steps like 4.2, 4.4, 4.5), checks, verifications, and reporting requirements outlined in this document (including verification summaries) been completed for this cycle? If any were missed, **trigger self-correction (Step 4.4.3)** to address the oversight before finishing. **After confirming completion, MUST state: "**Step 5: Adherence Checkpoint:** Self-assessment complete. All mandatory steps confirmed executed for this task cycle."**

### 6. Summarize Deferred Observations (Optional)

*   **Trigger:** After successful completion of Step 5 (Adherence Checkpoint) and before concluding the interaction for the current overall task/request cycle.
*   **Action:**
    *   The AI **MAY** generate a "Deferred Observations Summary" for general observations (minor bugs, smells, etc. unrelated to immediate edit side effects).
    *   **MANDATORY:** If any unintended modifications were introduced during Step 4 edits (even if the primary goal was met and the changes were accepted to proceed, as noted in Step 4.5), the AI **MUST** generate a "Deferred Edit Issues Summary" section within this step. This section **MUST**:
        *   List each file and function affected by unintended side effects.
        *   Clearly describe the specific unintended changes observed in the final applied diff (e.g., "Error handling logic in the final `try-except` block was altered", "Logging statements were changed from X to Y", "Cleanup logic Z was removed").
        *   **Provide a detailed breakdown and analysis of the likely behavioral differences** resulting from these unintended changes (similar to the example analysis provided when asked about `analyze.py`, `docs.py`, `review.py` changes). Focus on how error handling, output, logging, or core function logic might now differ from the original, intended behavior.
        *   **Explicitly ask the user if they want to prioritize fixing any of these specific deferred edit issues now**, or if they should be addressed later. Example: "Would you like me to attempt to correct any of these specific unintended modifications now, or should we defer them?"
    *   The general "Deferred Observations Summary" should list noteworthy observations made during the execution of Steps 1-4 that were deemed out of scope for the current task (and not already escalated via a `**BLOCKER:**` or explicitly discussed and deferred with a documented plan during earlier steps) but might warrant future review or action. Examples include:
        *   Potential minor bugs or unhandled edge cases.
        *   Identified code smells (e.g., overly complex methods, duplicated logic).
        *   Minor deviations from best practices or project standards.
        *   Areas noted for potential future refactoring (clarity, performance, maintainability).
        *   Existing TODOs or FIXMEs encountered in directly relevant code sections.
    *   The summary should be concise and provide actionable context (e.g., file, function/area, brief description).
    *   The AI **MUST** explicitly state: "**Step 6: Deferred Observations Summary:** No specific deferred observations were noted for future review during this task cycle." if no *general* summary is generated or if no such items were noted (even if a mandatory "Deferred Edit Issues Summary" was generated).
    *   *Example 1 (With Deferred Edit Issues):* "**Step 6: Deferred Edit Issues & Observations Summary:**\\n**Deferred Edit Issues:**\\n1. `src/commands/analyze.py` (`run_analysis`): Unintended changes to final `try-except` block. *Behavioral Impact:* May handle `FormatterError` differently, logs might be altered...\\n2. `src/commands/docs.py` (`generate_cli`): Unintended changes to logging/error handling. *Behavioral Impact:* Success message no longer prints to console, `OSError` handling less specific...\\n**General Observations:**\\n1. `src/utils/parser.py`: `parse_data` function could be simplified.\\n**Next Steps:** Would you like me to attempt to correct the unintended modifications in `analyze.py` or `docs.py` now, or should we defer these fixes?"
    *   *Example 2 (No Edit Issues, No General Observations):* "**Step 6: Deferred Observations Summary:** No specific deferred observations were noted for future review during this task cycle."
    *   *Example 3 (No Edit Issues, With General Observations):* "**Step 6: Deferred Observations Summary:**\\n**General Observations:**\\n1. `src/models/user.py`: Consider adding validation for the `email` field format."
*   **Goal:** To capture potentially valuable insights or identified areas for improvement that were outside the immediate scope of the completed task, facilitating systematic follow-up. This step is for items not critical enough to have triggered a `**BLOCKER:**` or formal deferral discussion during the task itself. **Critically, it ensures unintended side effects from edits are clearly documented, analyzed for impact, and explicitly presented to the user for a decision on immediate action.**

---

## Reusable Verification Procedures

*(This section defines standard methods referenced in the main workflow.)*

**`Procedure: Ensure Sufficient File Context`**
*   **Purpose:** To ensure the AI has adequate file content for safe and accurate planning and editing, especially for critical or complex files.
*   **Trigger:** Called by Step 3.0 when assessing target file complexity or if initial partial reads are insufficient.
*   **Steps:**
    1.  **Prioritize Complete View:** If the user's request targets a file known to be a central configuration hub (e.g., main application setup, DI container, core routing), a file previously identified as complex or sensitive to edits, OR if an initial partial file read (e.g., from `read_file` without `should_read_entire_file=True`) proves insufficient to confidently locate all necessary code sections for planning and executing the required change, the AI **MUST** prioritize obtaining a complete and sufficient view of the target file.
    2.  **Attempt Full File Read:** The AI **SHOULD** first attempt to obtain the full file content using `read_file` with `should_read_entire_file=True` (or by specifying a line range covering the entire file). (Note: Per tool constraints, reading entire files is generally only allowed if it has been edited or manually attached by the user. The AI should be mindful of this constraint when deciding to use this option).
    3.  **Verify Full Content Receipt & Handle Incompleteness:**
        a.  **Mandatory Check:** After the `read_file` call in Step 2, if the intent was to read the entire file, the AI **MUST** scrutinize the tool's response to confirm the *entire* file content was returned. This includes:
            i.  Checking if the tool's textual description explicitly states only partial content (e.g., "Showing the first X lines", "Outline of the rest of the file:", "Contents of ... from line X-Y (total Z lines):" where Y-X+1 is less than Z).
            ii. Comparing the number of lines reported/returned against the known total lines of the file (if this information is available from prior `list_dir` or other context).
        b.  **Trigger for Incomplete Read:** If *any* indication from sub-step 'a' suggests the full file was not returned, or if the AI still deems the content insufficient for the task, it **MUST** treat the read as incomplete.
        c.  **Action on Incomplete Read:**
            i.   The AI **MUST** clearly state the situation, explicitly mentioning the attempt to read the full file and the nature of the partial content received (e.g., "Attempted to read the full content of `[filename]` (X lines), but the tool returned Y lines and an outline for the remainder.").
            ii.  The AI **MUST** then request the user to provide the full file content or confirm if proceeding with potentially limited context is acceptable (explicitly stating any risks).
            iii. **BLOCKER:** The AI **MUST NOT** proceed with planning or generating edits for this file based on the incomplete information if a complete view was deemed necessary in Step 1, until the user provides the content or explicitly instructs to proceed with the stated risks.
    4.  **Alternative (Use with Extreme Caution and Explicit Justification):** Only if highly targeted and reliable means (e.g., a `grep_search` for a *unique and unambiguous* anchor string known to be immediately adjacent to *all* areas of interest) can *definitively and completely substitute* for a full file view for the specific task, and this is demonstrably less risky than proceeding with partial information after a failed full read attempt (and subsequent user request as per Step 3.c.ii), may this be considered. This path requires explicit justification by the AI, detailing why this alternative is sufficient and safe for the given task, and should be exceptionally rare for complex changes in critical files.
    *   *Example (after failed full read attempt): "Attempted to read the full content of `app.py` using `should_read_entire_file=True`, but only partial content was provided because the file was not on the pre-approved list for full reads. To accurately refactor the CLI structure as planned, which involves verifying all existing command registrations and ensuring correct placement of new ones, I need the complete content of this central configuration file. Please provide the full content of `src/beat_the_books/cli/app.py`."*

**`Procedure: Prepare Robust Edit Tool Input`**
*   **Purpose:** To construct the `code_edit` and `instructions` parameters for the `edit_file` tool in a way that maximizes reliability and precision.
*   **Trigger:** Called by Step 3.8.b and applied by Step 4.1 when preparing to call `edit_file`.
*   **Steps:**
    1.  **Explicit Instructions Field:** The `instructions` field given to `edit_file` **MUST** be explicit about the action (add vs. modify/replace).
    2.  **Sufficient Context in `code_edit` for Anchoring:**
        *   **General Guideline:** Ensure the `code_edit` string includes enough unchanged lines (aim for 2-3 immediately before and after each modified block) for anchoring, where code structure allows. Adjust for small, isolated single-line changes. The goal is to uniquely identify the edit location.
        *   **Complex/Sensitive/Corrective Edits:** For files identified as complex/sensitive (per `Procedure: Ensure Sufficient File Context` or an assessment during `Procedure: Analyze Impact`), or when generating corrective edits for previously mis-applied diffs, the AI **MUST** prioritize extreme precision:
            *   Use highly specific anchor lines in the `code_edit` string.
            *   Provide broader context than usual in `code_edit` if it clarifies edit boundaries.
            *   Reiterate in the `instructions` field any critical sections of the file that **MUST** remain unchanged (e.g., "IMPORTANT: Only modify the specified lines for X; the Y and Z sections must remain untouched.").
    3.  **Handling Substantial Block Movements/Structural Changes in `code_edit`:** When an edit involves moving substantial blocks of code (e.g., entire functions, classes, or large multi-line logical blocks), or when prior edits by the tool for similar structural changes have shown inaccuracies in placement:
        *   **Prioritize Full Logical Block Context:** Provide the edit tool with the entire surrounding logical block (e.g., the full function body if moving code within it or into it, the full class definition if reordering methods) as context in the `code_edit` string.
        *   **Clarify Sub-Part Changes in `instructions`:** The `instructions` field **MUST** then clearly state which specific sub-parts of that larger block (provided in `code_edit`) are being modified, added, deleted, or reordered.
    4.  **Deprecated Code Handling in `code_edit`:** When replacing or removing code, the old/deprecated code **MUST** be completely deleted from the `code_edit` string, not commented out. The `code_edit` should reflect this direct removal.

**`Procedure: Verify Dependency Reference`**
*   **Purpose:** To factually confirm the validity of a dependency reference (e.g., import, require) before relying on it or including it in code. **MUST** be performed for **every** reference during planning (3.4.1.b), pre-edit deviation handling (via `Procedure: Verify Diff`), post-edit verification (via `Procedure: Verify Diff`), and downstream checks (4.4.2.b.i).
*   **Applicability:** **MUST** be performed whenever planning to add or modify a dependency reference (import, require, etc.) and when verifying diffs containing such references.
*   **Steps:**
    1.  **Cross-Reference Prior Knowledge:** Check if the correct module path for the symbol(s) was previously verified. Use verified path if available.
    2.  **Path Validation:** **MUST** use tools (`file_search`, `list_dir`) to confirm the **actual existence** of the source module file/directory path (e.g., `path/to/module.py`, `path/to/package/__init__.py`). **Report method and outcome.** If path invalid, reference **MUST** be corrected or removed.
    3.  **Symbol Validation:** Only if path confirmed valid, **MUST** then use tools (`read_file` on target, `grep_search`) to confirm the existence AND necessity (check usage in *current* file scope) of the referenced symbol(s) within that module/package. **Report method and outcome.** Remove unused symbols.

**`Procedure: Analyze Impact`**
*   **Purpose:** To identify potential effects of proposed changes on existing functionality (called in Step 3.4.1.a).
*   **Applicability:** **MUST** be performed during Planning (Step 3.4.1.a) before generating edits, especially for changes to interfaces, paths, symbols, core models, or widely used components.
*   **Steps Checklist:**
    1.  **Identify Affected Call Sites/References:** For interface/path/symbol changes, **MUST** use tools (`grep_search`) to find *all* potential call sites/imports/references codebase-wide. List findings.
    2.  **Enhanced Scope for Core Refactoring:** **CRITICAL:** If modifying base classes, core domain interfaces, widely used utilities, DI container, core configs: **MUST** explicitly consider consumers across *all* layers (CLI, helpers, services). Employ broader search strategies (`grep` for attribute access, semantic search). **MUST** search for inheritors/consumers (e.g., `grep_search 'class .*(.*BaseClassName.*):'`, `codebase_search`). List key findings, **including specific inheritors/consumers identified and brief note on potential impact checked.**
    3.  **Circular Dependency Check:** When adding a new module dependency (B needs A), explicitly state check, examine dependencies in module A, report outcome (e.g., "Circular Check: `module_a` does not import `module_b`. OK.").
    4.  **Data Representation Impact:** For changes affecting data representation (ID vs Name, format) or core models, explicitly consider impacts across layers (ORM, Mappers, Exporters, Consumers, Tests).

**`Procedure: Verify Hypothesis`**
*   **Purpose:** To factually verify assumptions made during planning before proceeding (called in Step 3.4.1.b).
*   **Scope:** Covers external API structures, internal module/class/function interfaces, configuration values, data formats, library usage patterns (treat non-standard library/internal interface usage as an assumption). **CRITICAL:** Also covers assumptions about *interface consistency* between interacting components and *functional equivalence* when replacing logic (compare plan vs. documented old logic).
    **Note on Implicit Framework Behaviors:** Assumptions about implicit framework behaviors (e.g., 'The framework will automatically await this type of async function,' or 'Context set in this callback will always be available in subcommands') are common and **MUST** be explicitly stated and verified, ideally against documentation or minimal reproducible examples if behavior is not explicitly documented for the specific use case.
*   **Applicability:** **MUST** be performed **immediately** after stating any assumption during Planning (Step 3.4.1.b) that impacts the proposed plan or edit.
*   **Steps (MUST perform IMMEDIATELY after stating assumption):**
    1.  **Detail Verification Method:** State specific method(s) (`read_file`, `grep_search`, docs review, etc.).
    2.  **Execute Verification & Report:** Perform verification. **MUST** report outcome using the structured format below. **When Step 3.4.1.b requires verifying multiple hypotheses, each complete hypothesis verification block (Hypothesis, Method, Execution, Outcome) SHOULD be presented as an item in a list (e.g., a bullet point or numbered item) for improved readability. The sub-details (Method, Execution, Outcome) MAY be indented under their respective Hypothesis statement.**
        ```markdown
        **Hypothesis:** [State assumption]
        **Verification Method:** [Tool/Method]
        **Verification Execution:** [Action, e.g., Performing `read_file`...]
        **Verification Outcome:** [Confirmed: [Details] / Failed: [Details]]
        ```
        *Example Outcome Report (Single Hypothesis):*
        ```markdown
        **Hypothesis:** External API `GET /items/{id}` returns `itemName` field.
        **Verification Method:** `read_file` on `docs/api_spec.v2.json`
        **Verification Execution:** Reading API spec file...
        **Verification Outcome:** Confirmed: Spec shows `itemName` present in response schema.
        ```
        *Example Outcome Report (Multiple Hypotheses, demonstrating suggested list format):*
        ```markdown
        - **Hypothesis:** [First assumption stated here]
          - **Verification Method:** [Method for first hypothesis]
          - **Verification Execution:** [Details of executing verification for first hypothesis]
          - **Verification Outcome:** [Outcome for first hypothesis]
        - **Hypothesis:** [Second assumption stated here]
          - **Verification Method:** [Method for second hypothesis]
          - **Verification Execution:** [Details of executing verification for second hypothesis]
          - **Verification Outcome:** [Outcome for second hypothesis]
        ```
    3.  **Proceed or Halt:** Only proceed if Outcome is 'Confirmed'. If 'Failed', plan **MUST** be revised/halted. *(Reminder: Address Failure Mode 2 - Base decisions on verified facts, not assumptions or general patterns.)*

**`Procedure: Verify Diff`**
*   **Purpose:** To provide a core, reusable set of checks for verifying any diff (proposed or applied) against its intended state/plan. Called by Step 4.2.1.b, `Procedure: Verify Reapply Diff`, and `Procedure: Verify Edit File Diff`.
*   **Inputs (Conceptual):** The `diff` content, the `intent` (e.g., the plan, the state before the edit, the final intended proposal). *(Note: The specific source of the `intent` (e.g., original plan, verified proposed edit, pre-reapply file state) depends on the context from which this procedure is called (e.g., Step 4.2.1.b, `Procedure: Verify Edit File Diff`, `Procedure: Verify Reapply Diff`).)*
*   **Note on 'Intent':** For the purpose of `Procedure: Verify Diff`, the 'intent' refers *only* to the specific, narrowly defined change planned for the immediate `code_edit` operation (as documented in Step 3 or a corrective plan). It does not include broader, implicit goals like 'general code cleanup' or 'fixing other potential issues' unless those were explicitly part of the plan for *this specific edit*. Unplanned changes, even if seemingly beneficial, are deviations.
*   **Applicability:** **MUST** be executed during Pre-Apply Verification (Step 4.2.1.b), Post-Reapply Verification (`Procedure: Verify Reapply Diff`, Step 2), and Post-Edit File Verification (`Procedure: Verify Edit File Diff`, Step 1).
*   **Execution Reporting:** When executing this procedure, the AI response **MUST** include an inline checklist documenting the execution and outcome of each step below.
*   **Steps Checklist (MUST perform all relevant steps and report via inline checklist):**
    1.  **Diff vs. Intent Match & Granular Change Analysis:** Compare the *entire* diff line-by-line against the `intent` (the specific, planned change for the current step).
        *   `a.` **Additions:** Verify all added lines directly serve the documented `intent`.
        *   `b.` **Modifications:** Verify all modified lines correctly implement the documented `intent` and do not introduce unintended side effects.
        *   `c.` **Deletions:** Verify all deleted lines were *explicitly intended for deletion* as part of the current `intent`. **Treat any significant deletions not explicitly planned for the current edit step as a major deviation, even if the deleted code appears to be comments or unused. Such unplanned deletions signal a potential misinterpretation of edit boundaries by the tool or the AI.**
        *   `d.` Note all discrepancies for handling in Step 3 (Identify Deviations).
    2.  **Absence of Major Unintended Structural Changes:** Specifically verify that the diff does **not** include widespread deletion or reordering of code blocks unrelated to the `intent` (beyond what was noted in 1.c). If such changes are present, they **MUST** be treated as critical Deviations.
    3.  **Identify Deviations:** Explicitly list any lines added, deleted, or modified in the `diff` that were *not* part of the `intent` ("Deviations"), including any major structural changes identified in the previous step. **Furthermore, any component of the `intent` (e.g., an intended addition, modification, or deletion) that is not verifiably present in, or confirmed by, the applied `diff` also constitutes a Deviation and MUST be investigated.**
    4.  **Handle Deviations:** If Deviations found:
        *   **CRITICAL:** Fact-check **EVERY** Deviation using tools before accepting.
        *   **Execute `Procedure: Handle Deviation` (Section 5)** for each Deviation. **Report outcome clearly.**
        *   **Repeat for ALL Deviations** before proceeding.
    5.  **Dependency Verification:** **MUST** perform `Procedure: Verify Dependency Reference` for **ALL** dependency statements (e.g., imports, requires) present in the final, deviation-handled `diff`. **Explicitly report outcome.**
    6.  **Semantic Spot-Check:** Rigorously re-validate *key* additions/changes (complex logic, function calls, interactions with external APIs/framework patterns). Confirm semantic correctness against the `intent`.
    7.  **Context Line Check:** Re-verify context lines in the `diff` were not unexpectedly modified/misrepresented compared to the actual file state. Use `read_file` if suspicious.
    8.  **Logic Preservation Validation (If Applicable):** If the diff replaces/restructures logic, perform final validation comparing *applied* new logic against documented original behavior (from `Procedure: Ensure Logic Preservation`, Step 3.4.1.e). Confirm preservation or justified modification.
    9.  **Root Cause Check:** Ensure the final, deviation-handled `diff` still addresses the root cause identified in Step 3.4.
    10. **Integration Sanity Check:** Briefly check: Does the change logically fit? Interact correctly with adjacent code?
    11. **Report Outcome:** Conclude with the overall verification outcome (e.g., "Verified", "Verified with handled deviations", "Failed - Requires correction").

    *Example Inline Checklist Output (in AI Response):*
    ```markdown
    **Executing `Procedure: Verify Diff`:**
    - `[x]` 1. Diff vs. Intent Match: Verified, matches final intent.
    - `[-]` 2. Absence of Major Unintended Structural Changes: N/A, no unintended structural changes.
    - `[-]` 3. Identify Deviations: N/A, no deviations from intent.
    - `[-]` 4. Handle Deviations: N/A.
    - `[x]` 5. Dependency Verification: `Procedure: Verify Dependency Reference` executed for `import X`, Outcome: Confirmed.
    - `[x]` 6. Semantic Spot-Check: Key logic change `Y` reviewed, confirms intended behavior.
    - `[x]` 7. Context Line Check: Context lines appear correct.
    - `[-]` 8. Logic Preservation Validation: N/A, no logic replacement.
    - `[x]` 9. Root Cause Check: Confirmed diff addresses root cause Z.
    - `[x]` 10. Integration Sanity Check: Appears to integrate correctly.
    - **Outcome:** Verified.
    ```

**`Procedure: Ensure Logic Preservation`**
*   **Purpose:** To meticulously preserve essential behavior when replacing or significantly restructuring existing logic blocks (called in Step 3.4.1.e).
*   **Applicability:** **MUST** be performed during Planning (Step 3.4.1.e) whenever the plan involves **replacing or significantly restructuring existing logic blocks**. Also validated during Diff Verification (Step 4).
*   **Steps:**
    1.  **Document Existing Behavior FIRST:** Before proposing new structure, use `read_file` to analyze and **document essential behavior, distinct execution paths, and key conditional logic** of the code being replaced, **citing specific conditions or execution paths from the original code.** Summarize concisely.
    2.  **Explicit Preservation Plan REQUIRED:** Detail how the *new* logic structure preserves EACH identified essential behavior/path, referencing the documentation from Step 1.
    3.  **Justify Intentional Changes:** If original behavior/path is intentionally changed/removed or conditions altered, **MUST** state this explicitly, justify it, analyze impact, and confirm acceptability.
        *   **Explicit Trade-off Presentation:** If change results from simplification, present Trade-off (Plan A: Simpler/Altered vs. Plan B: Complex/Preserves) and **request guidance**.
        *   **STOP processing and await user guidance before proceeding with implementation of either trade-off option.**
    4.  **Functional Check:** Confirm consumers still function as expected (analysis, suggest tests).

**`Procedure: Verify Framework Compatibility`**
*   **Purpose:** Ensure changes to framework entry points or library interactions are valid (called in Step 3.4.1.d).
*   **Applicability:** **MUST** be performed during Planning (Step 3.4.1.d) when modifying code that directly interacts with or is invoked by a framework (e.g., CLI handlers, API routes) or uses external libraries in non-trivial ways.
*   **Steps:**
    1.  **Signature Check:** For framework entry points (e.g., CLI handlers, API routes), explicitly verify changes to signatures (sync/async, params, return types) are compatible with framework invocation. This **MUST** include verifying how the framework invokes and manages the lifecycle of *registered callback functions or handlers* (e.g., for application events, request handling, or lifecycle hooks), especially when changing their synchronicity (`sync` to `async` or vice-versa). Pay close attention to how execution context or shared state is propagated to these callbacks and how `async` callbacks are awaited or managed by the framework's event loop or scheduler. If documentation is unclear, search for established community patterns or examples for this specific interaction. **Treat runtime warnings or diagnostic messages from the framework concerning callback invocation (e.g., indicating a coroutine was not properly awaited or a registration was incorrect) as critical indicators of a compatibility issue requiring immediate investigation.**
    2.  **Interaction Pattern Check:** Identify key framework/library interactions. Confirm plan uses established project patterns OR check docs/examples for novel interactions. State check performed and outcome.

**`Procedure: Verify Configuration Usage Impact`**
*   **Purpose:** Check impact of changes to configuration values.
*   **Applicability:** **MUST** be performed during Planning (Step 3.4.1.f) when proposing changes to configuration values or structure.
*   **Steps:** Use `grep_search` to identify potential usage locations codebase-wide. State findings and confirm compatibility or necessary updates in plan.

**`Procedure: Verify Reapply Diff`** (Called by Step 4.4.1.a)
*   **Purpose:** To meticulously verify the diff applied by the `reapply` tool.
*   **Trigger:** Called by Step 4.4.1.a immediately after a `reapply` tool call completes.
*   **Steps:**
    1.  **Treat Diff as New:** Approach with extreme skepticism.
    2.  **Perform Core Diff Verification:** **MUST** execute `Procedure: Verify Diff` (Section 4) on the *actual diff applied by `reapply`*. The 'intent' for this verification is the file state *before* the `reapply` call. **Emphasize extra scrutiny due to `reapply` context.**
    3.  **Structured Log:** **Immediately after `reapply` result,** **MUST** generate structured log confirming the execution and **overall outcome** of `Procedure: Verify Diff` (Step 2), explicitly mentioning the handling of any deviations. Use format similar to Step 4.2.2, noting it's post-reapply.

**`Procedure: Verify Edit File Diff`** (Called by Step 4.4.1.b)
*   **Purpose:** To meticulously verify the diff applied by the standard `edit_file` tool.
*   **Trigger:** Called by Step 4.4.1.b immediately after a standard `edit_file` tool call completes.
*   **Steps:**
    1.  **Perform Core Diff Verification:** **MUST** execute `Procedure: Verify Diff` (Section 4) on the *actual diff applied by `edit_file`*. The 'intent' for this verification is the *final intended proposal from Step 4.2 (specifically, the verified proposed `code_edit` diff after `Procedure: Verify Diff` execution in Step 4.2.1.b)* (incorporating any handled deviations from the pre-apply check).
    2.  **Discrepancy Handling:** If the overall outcome of `Procedure: Verify Diff` (Step 1) is not 'Verified' (or 'Verified with handled deviations') and cannot be justified/corrected, **trigger self-correction (Step 4.4.3)**.

---

## Exception Handling Procedures

*(This section details procedures for handling specific issues identified during the workflow. Execution of these procedures typically means **STOPPING** the standard workflow path until the issue is resolved or explicit guidance/approval is received.)*

**`Procedure: Handle Unclear Root Cause / Missing Info`**
*   **Trigger:** Execution is triggered by Step 3.5 when analysis reveals unclear root causes, missing essential information, standard conflicts that block planning, failing standard debugging methods, or specific failed dependency verifications (see `Procedure: Handle Failed Verification for Existing Dependency`).
1.  **STOP** proposing direct fix.
2.  **State Issue & Prioritize Investigation:** Clearly state the blocker (unclear cause, missing info, standard conflict, failing debug method).
3.  **Formulate Investigation Plan:** Outline specific steps to investigate (e.g., "Plan: 1. Examine logger setup. 2. Check system exception hook.").
4.  **Seek Confirmation (if needed):** Confirm broad/assumption-based investigation plans. *Example: "Traceback suppressed. Plan: [steps]. Proceed?"*
   **If confirmation is sought, STOP processing and await explicit user confirmation before executing the investigation plan.** (**BLOCKER:**)
5.  **Handle Necessary Workarounds (Use Sparingly):** Follow `Procedure: Handle Necessary Workaround` ONLY if investigation is blocked/impractical AND workaround is only viable path *now*. (See cross-reference below).
6.  **Consult on Ambiguous Missing Dependencies:** Follow `Procedure: Consult on Ambiguous Missing Dependency` ONLY if resolving a dependency error requires creating significant new structures based *only* on potentially old references, with no strong corroborating context.

**`Procedure: Handle Architectural Decisions`**
*   **Trigger:** Execution is triggered by Step 3.6 when analysis reveals architectural conflicts, significant trade-offs between valid paths, or foundational inconsistencies.
1.  **IMMEDIATELY STOP** detailed planning for original task. **Treat as mandatory **BLOCKER:**.**
2.  **State Issue/Choice:** Clearly describe the suboptimal pattern, architectural decision point, or foundational inconsistency (e.g., global state use, SRP violation choice, type mismatch).

**`Procedure: Handle Necessary Workaround`**
*   **Trigger:** Execution is triggered by Step 3.4 (Data Integrity Deviation requiring explicit approval) or as a potential outcome of `Procedure: Handle Unclear Root Cause / Missing Info` (Step 5) if root cause investigation is blocked/impractical.
*   **Pre-condition:** Root cause investigation confirmed blocked or impractical *now*, and workaround is only viable path *at this moment*.
1.  **STOP** all implementation planning/fixing. (**BLOCKER:** Step requiring user approval)

**`Procedure: Consult on Ambiguous Missing Dependency`**
*   **Trigger:** Execution is triggered as a potential outcome of `Procedure: Handle Unclear Root Cause / Missing Info` (Step 6) when resolving a dependency error requires creating significant new structures based *only* on potentially old references, with no strong corroborating context.
*   **Pre-condition:** Resolving dependency error requires creating significant new structures based *only* on potentially old references, with no strong corroborating context.
1.  **STOP** implementation planning. (**BLOCKER:** Step requiring user guidance)

**`Procedure: Handle Failed Verification for Existing Dependency`**
*   **Trigger:** Execution is triggered by Step 3.4.1.b if hypothesis verification for a dependency reference in *existing, established code* fails (module/symbol not found).
*   **Pre-condition:** Hypothesis verification for a dependency reference in *existing, established code* fails (module/symbol not found).
*   **Steps:**
    1.  **DO NOT** immediately plan creation of the missing dependency.
    2.  **State the Discrepancy:** Clearly state that established code references a module/symbol that cannot be found, specifying the reference and the file containing it.
    3.  **Perform Usage Check in Referencing File:** **MUST** use tools (`grep_search`) to search *within the referencing file* for actual usage of the specific symbol(s) (e.g., instantiation, method calls, type annotations). Report findings explicitly (e.g., "Usage Check in `file_x.py`: Searched for `MissingSymbol(...)`. Outcome: No usage found." or "Outcome: Usage found in function `Y`.").
    4.  **Perform Broader Context Search:** Execute additional searches (`grep_search` for related terms/files, `codebase_search` for conceptual links) to find context about the missing element (e.g., was it moved? refactored? deprecated? any TODOs for removal?). Report findings.
    5.  **Determine Next Action based on Context and Usage:**
        *   If context clearly indicates a simple move/rename and the symbol is still needed: Plan the fix (update reference path).
        *   If the dependency failed **AND** the Usage Check (Step 3) found **NO USAGE** in the referencing file **AND** broader context does not suggest it's critical: The default plan should be to **remove the stale dependency reference statement**. Briefly verify this removal doesn't cause downstream issues.
        *   If the dependency failed **AND** usage *was* found, OR if context suggests intentional removal but usage remains, OR if context is insufficient/ambiguous: **Execute `Procedure: Handle Unclear Root Cause / Missing Info` (Section 5)**, providing the findings from this procedure as input.

**`Procedure: Handle Deviation`**
*   **Trigger:** Called by `Procedure: Verify Diff` (Step 3) whenever deviations (unplanned changes) are identified between the `diff` being checked and the `intent`.
*   **Purpose:** To fact-check and decide on unplanned lines ("Deviations") in a proposed `code_edit` diff.
*   **Steps (For EACH Deviation):**
    1.  **Fact-check Deviation:** Use tools to confirm the existence and correctness of the deviation.
    2.  **Justify Deviation:** If deviation is intentional, justify it. If not, state it's unplanned and report it.
    3.  **Integrate or Revise:** If the deviation is verified, justified as beneficial/necessary, and accepted: **formally update the current working plan and the intended `code_edit` to incorporate this deviation.** The `code_edit` can then be considered aligned with the *updated* plan. If the deviation is not acceptable, **revise the proposed `code_edit` to remove the deviation** and align with the original (or a newly revised) plan.

**`Procedure: Request Manual Edit`**
*   **Trigger:** Called by Step 4.4.3.b.e if tool failures persist or edit application requires complex cleanup.
*   **Advisory for "Unacceptable Original Edit Application with Failed Cleanup" Trigger:**
    *   When this procedure is invoked because an automated edit achieved the primary goal but introduced unacceptable side-effects (churn, new errors in unrelated code) which a subsequent focused cleanup attempt also failed to resolve (as per Step 4.4.3.b.e.ii):
        *   The explanation in "State Tool Failure" (Step 2 below) **MUST** clearly differentiate between the successful primary change and the failed cleanup of the unacceptable side-effects.
        *   The primary goal of "Provide Specific Edit Details for Manual Application" (Step 3 below) **MUST** be to provide the user with the **original, minimal, and correct planned change** that was initially intended, presented cleanly with its necessary context, as if the side-effects had not occurred. This allows the user to apply the core intended change cleanly.
1.  **STOP** tool attempts.
2.  **State Tool Failure:** Explain the issue and the presumed incorrect file state based on last tool output.
3.  **Provide Specific Edit Details for Manual Application:**
    *   **MUST** clearly specify the target file.
    *   **MUST** clearly state whether the action is an insertion, replacement, or deletion.
    *   If an insertion or replacement, **MUST** provide the precise code block to be inserted/used as replacement.
    *   If a deletion, **MUST** clearly describe or show the exact lines to be deleted.
    *   **MUST** include sufficient surrounding context (a few lines before and after the change area) to uniquely identify the location of the edit.
    *   **MUST** present this (for insertions/replacements) as a single, clearly demarcated code block, using the language's comment style for `// ... existing code ...` where appropriate, that the user can easily copy and paste to represent the *final state* of the edited section.
    *   *Example (for insertion/replacement): "To manually apply the fix to `src/example.py`, please modify the relevant section to match the following (or insert at the indicated location):"*
        ```python
        // ... existing code ...
        // Context line before change
        // New code to be inserted / This block replaces the old code
        // Further new code if applicable
        // Context line after change
        // ... existing code ...
        ```
    *   *Example (for deletion): "To manually apply the fix to `src/example.py`, please delete the following lines:"*
        ```python
        // ... existing code ...
        // Context line before deletion
        // Line to be deleted
        // Another line to be deleted
        // Context line after deletion
        // ... existing code ...
        ```

---

## 7. Protocol for Runtime Error Diagnosis and Resolution

*   **Trigger:** When a runtime error (e.g., indicated by a traceback, failed command execution with error messages) occurs during code execution or testing, interrupting the planned workflow.
*   **Goal:** To systematically identify the root cause of the runtime error and implement a verified fix, while adhering to the project's quality standards and the core principles of this document.
*   **Core Principle Application:** Runtime error diagnosis requires the same level of care, diligence, and adherence to the standard workflow (Steps 1-6) as any other coding task. All Core Principles (Section 2) remain paramount. The steps outlined below adapt the standard workflow, particularly Section 3 (Planning), for this specific context. Reporting detail **MUST** remain consistent as per the Meta-Instruction in Section 3.

*   **Procedure:**
    1.  **Acknowledge Error & Shift Focus:** Explicitly state the observed runtime error (citing traceback/logs) and confirm that the immediate goal is now to diagnose and resolve this error.
    2.  **Apply Standard Workflow (Adapted):** Proceed through the standard Steps 1-6, adapting the "Planning" phase (Step 3) as follows:
        *   **Step 1 (Confirm Standards Awareness):** Reiterate commitment to standards.
        *   **Step 2 (Confirm Goal):** The goal is now implicitly "Diagnose and fix runtime error Z." Confirmation may be skipped unless the error context is highly ambiguous.
        *   **Step 3 (Pre-computation Standards Check - Diagnostic Adaptation):**
            *   `3.0. Assess Complexity & Ensure Sufficient Context:` **CRITICAL:** Gather all facts related to the error. Execute `Procedure: Ensure Sufficient File Context` for all files directly implicated by the traceback. Obtain full tracebacks, relevant log entries, and understand the inputs/conditions that triggered the error. **Explicitly address any information discrepancies** using the guidelines under the "Work with Facts" Core Principle (Section 2).
            *   `3.1. Search for Existing Logic:` Search codebase/documentation for similar errors, patterns, or known issues related to the components involved.
            *   `3.2. Identify Standards & Verify Alignment:` Identify the standard/principle being violated (e.g., type safety, library usage contract, logical flow). The plan's goal is to restore alignment.
            *   `3.3. Check "Work with Facts":` Base all hypotheses and plans strictly on the gathered evidence (tracebacks, logs, verified code state). Clearly state assumptions derived from evidence.
            *   `3.4. Check "Robust Solutions":` Prioritize finding the root cause over superficial patches. Plan a fix that addresses the underlying issue. Apply diagnostic sub-steps meticulously:
                *   `3.4.1.a. Analyze Impact:` Analyze the potential impact of the *proposed fix* on related code/functionality *before* applying it.
                *   `3.4.1.b. Verify Hypothesis:` **This is the core diagnostic loop.**
                    i.  Formulate a specific, testable hypothesis about the root cause based on the evidence.
                    ii. Detail the precise steps (tools, code reads, checks) to verify or refute the hypothesis.
                    iii.Execute verification and report the outcome using the standard `Procedure: Verify Hypothesis` format.
                    iv. If hypothesis is confirmed and leads to a fix, proceed to plan the fix. If refuted, formulate a new hypothesis based on findings and repeat.
                *   `3.4.1.c. Enumerate Edge Cases/Errors:` Consider edge cases for the *proposed fix* itself. Does it handle different inputs correctly? Does it introduce new potential errors?
                *   `3.4.1.d-j:` Apply other relevant checks from Step 3.4.1 (Framework Compatibility, Logic Preservation if refactoring is part of the fix, Config Impact, Testability, etc.) to the *proposed fix*.
            *   `3.5. / 3.6. Handle Blockers:` If the root cause remains unclear after initial investigation, or if fixing the error requires architectural changes, use the appropriate procedures from Section 5.
            *   `3.10 / 3.11 (Verification Summary / Preconditions):` Generate the Pre-computation Verification Summary for the *planned fix* before proceeding to Step 4. Verify preconditions for the fix edit.
        *   **Step 4 (Edit Generation & Verification Cycle):** Apply the verified fix using the standard generate-verify-apply-verify cycle for the relevant file(s). Meticulously verify the applied diff.
        *   **Step 5 (Adherence Checkpoint):** Confirm all steps (1-6, adapted) were followed for the error resolution.
        *   **Step 6 (Summarize Deferred Observations):** Note any observations made during debugging that are out of scope for the immediate fix.
    3.  **Resume Original Task (If Applicable):** Once the error is confirmed fixed and verified (completion of Step 5/6 for the fix), explicitly state this and indicate readiness to resume the original task that was interrupted, potentially re-evaluating its plan (Step 3) if the error/fix impacted it.

---

## 8. References

// ... potentially add references later ...

## Glossary of Key Terms

**Blocker:** A point in the workflow where user input is explicitly required to proceed.
**Deviation:** An unplanned change to the code that deviates from the intended plan.
**Edit:** A proposed change to the code.
**Edit Generation:** The process of creating a proposed edit.
**Edit Verification:** The process of ensuring that the proposed edit is correct and does not introduce new errors.
**Framework:** A set of libraries and tools that provide a structure for developing applications.
**Hypothesis:** A proposed explanation for a phenomenon.
**Implementation Plan:** A formal plan document outlining the steps to implement a change.
**Logic Preservation:** The process of ensuring that the original functionality of the code is preserved when changes are made.
**Plan:** The process of designing and structuring a solution to a problem.
**Post-Action Verification:** The process of verifying that the changes made to the code have not introduced new errors or regressions.
**Pre-Action Verification:** The process of verifying that the proposed changes are correct before they are applied to the code.
**Procedure:** A set of steps to follow to achieve a specific goal.
**Reapply:** The process of applying a previously verified edit to the code.
**Reusable Verification Procedure:** A procedure that can be used multiple times to verify different aspects of the code.
**Root Cause:** The underlying reason for an error or problem.
**Self-Driven Compliance:** The AI's ability to follow a set of rules and procedures without external prompting.
**Standard:** A set of rules and guidelines that are followed to ensure consistency and quality.
**Verification:** The process of confirming that something is correct or accurate.

---
