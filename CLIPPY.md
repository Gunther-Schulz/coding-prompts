# AI Assistant - Standard Coding Process

**ATTENTION AI ASSISTANT: This document outlines the mandatory process you **MUST** follow during general coding interactions to ensure consistent adherence to the project's standards. This process requires adherence to `PROJECT_STANDARDS.md` (Core Principles & Checklist) and the detailed guidelines in the project's specific `PROJECT_ARCHITECTURE.md` (Patterns, Structures, Examples for the relevant language/framework). This process complements, but does not replace, the detailed standards themselves. You **MUST** explicitly report the performance and outcome of mandatory checks as specified below.**

**Goal:** To improve consistency and proactively catch deviations from standards by incorporating explicit checks **and reporting** into the workflow, ensuring a strong emphasis on fundamentally robust solutions over quick fixes or workarounds.
**Interaction Model:** This process assumes **autonomous execution** by the AI, with user intervention primarily reserved for points explicitly marked with the literal text `**BLOCKER:**`. These points are identified within the procedures. Therefore, meticulous self-verification and clear, proactive reporting as outlined below are paramount for demonstrating adherence.

**Toolkit Component Version: Belongs to AI Collaboration Toolkit v0.2.30. See CHANGELOG.md for detailed history.**

---

## Table of Contents

1.  [Introduction & Goal](#ai-assistant---standard-coding-process)
2.  [Core Principles & Critical Checks Summary](#core-principles--critical-checks-summary)
3.  [General Coding Workflow](#general-coding-workflow)
    *   [Step 0: Model Compatibility & Awareness Check](#0-model-compatibility--awareness-check)
    *   [Step 1: Confirm Standards Awareness](#1-confirm-standards-awareness-initial-step)
    *   [Step 2: Confirm Goal (Recommended)](#2-confirm-goal-recommended)
    *   [Step 2: Confirm Goal](#2-confirm-goal-recommended)
    *   [Step 3: Pre-computation Standards Check (Planning)](#3-pre-computation-standards-check-planning-phase)
        *   [3.0. Assess Target File Complexity & Ensure Sufficient Context (Initial Check)](#30-assess-target-file-complexity--ensure-sufficient-context-initial-check)
        *   [3.0.1. Verify Tool Output Congruence and Sufficiency (After Information Gathering)](#301-verify-tool-output-congruence-and-sufficiency-after-information-gathering)
        *   [3.0.0 Handle Blocked Edit Attempts from Prior Cycle (If Applicable)](#300-handle-blocked-edit-attempts-from-prior-cycle-if-applicable)
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
    *   [`Procedure: Verify Tool Output`](#procedure-verify-tool-output)
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

**Sustained Task Focus & Context Retention:** The AI **MUST** maintain focus on completing the full scope of the user's current, explicit request or active plan (e.g., items from a provided checklist or multi-step instruction) across multiple execution cycles (e.g., refactoring steps, file edits). After completing any sub-task or procedural cycle (like a full Step 4 edit cycle), the AI is responsible for returning to the context of the main request/active plan and proactively determining and initiating the next logical action from it, unless explicitly instructed otherwise or the main request/active plan is verifiably complete.

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

**Principle of Contextual Data Utilization within Planning:** Data gathered and verified in earlier sub-steps of the Planning Phase (Step 3), such as file contents from Step 3.0 or search results from Step 3.1, **MUST** be actively utilized by subsequent sub-steps of Step 3 for the same task. These subsequent sub-steps **MUST NOT** make assumptions or perform redundant data gathering if the necessary information is already available from a prior verified step in the current planning cycle. The AI is responsible for recalling and applying this context.

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
    - **Plan Directives Override Ambiguous Tool Output:** When an implementation plan provides an explicit instruction regarding a specific source location (e.g., to read, adapt, or investigate), this instruction takes precedence over initial tool outputs that might appear incomplete or contradictory (e.g., a `list_dir` command not showing expected files). Further investigation **MUST** be performed to satisfy the plan's directive before concluding that the required source information is unavailable.
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

`3.0. Sustained Adherence Refocus (Mandatory)`
*   **Trigger:** At the very beginning of every Step 3 Planning Phase.
*   **Action:** The AI **MUST** explicitly state: "**Sustained Adherence Refocus:** Actively re-evaluating and committing to the full set of principles and procedures within `AI_CODING_PROCESS.md` (this document) to ensure continued meticulous adherence for the upcoming planning and implementation."
*   **Rationale:** This serves as a deliberate internal prompt for the AI to refresh its attention to the comprehensive guidelines in this document, counteracting potential focus drift during extended interactions and reinforcing the commitment to rigorous process execution before detailed planning begins.

`3.1. Handle Prior Blocked Edit Attempts (If Applicable)`
*   **Trigger:** Beginning of Step 3 Planning Phase.
*   **Objective:** To determine if the current planning cycle is a direct result of a previously "blocked" Step 4 edit attempt for the same user task, and to define the strategy for proceeding.

    **A. Check for Blocked Context:**
        1.  **Action:** Determine if this Step 3 cycle was initiated due to a "blocked" status from a prior Step 4 edit attempt (as per Step 4.4.3.b.e.i) for the current planned item/objective.
        2.  **Access History:** Recall the reasons and history of the prior blocked attempt.

    **B. If Prior Edit Attempt Was Blocked:**
        1.  **Diagnose Blockage Cause:**
            *   **Action:** Analyze the persistent failure details from the previous Step 4 cycle.
            *   **Consider:** Code complexity, insufficient granularity of the failed edit, or other subtle issues in the target code as likely causes.
        2.  **Evaluate Corrective Strategies (e.g., Refactoring):**
            *   **Decision Point:** Based on the diagnosis (B.1), can a specific corrective strategy (e.g., refactoring the problematic code) likely unblock the original planned item/objective?
            *   **Path 1: Corrective Strategy IS Viable:**
                a.  **Formulate Corrective Plan:** Create a preliminary plan for this corrective action (e.g., a refactoring plan). This plan may be multi-step.
                b.  **Explain & Propose:**
                    i.  Briefly explain the diagnosed cause of the previous blockage.
                    ii. Propose the corrective plan (e.g., "To address persistent edit failures in function `xyz` due to its complexity, I propose to first refactor `xyz` into `helper_A` and `helper_B`, then update `xyz` to call these helpers. This refactoring will be performed first. After its successful completion and verification, I will re-attempt the original planned edit on the simplified code.").
                c.  **BLOCKER: User Approval for Corrective Plan:**
                    i.  State: "This proposed corrective plan for `[target, e.g., function xyz]` requires your confirmation before proceeding."
                    ii. **MUST NOT** execute the corrective plan OR re-attempt the original blocked edit until the user explicitly:
                        *   Approves the corrective plan, OR
                        *   Instructs to proceed directly to `Procedure: Request Manual Edit` for the original blocked item, OR
                        *   Suggests another course of action.
                d.  **If Corrective Plan Approved by User:**
                    i.  **New Objective:** The corrective plan becomes the AI's current multi-part objective.
                    ii. **Execute Corrective Plan:** Proceed through Step 3 (planning for the *first part* of the corrective plan), then Steps 4 and 5 for each part of the corrective plan.
                    iii.**Resume Original Task:** Upon successful completion and verification of the *entire* corrective plan, **MUST** automatically re-enter Step 3 to formulate a new plan for the *original planned item/objective* (that was previously blocked), now targeting the corrected/refactored code.
                e.  **If Corrective Plan Declined by User OR Fails During Execution:**
                    i.  Condition: User declines, OR the AI's attempt to execute the approved corrective plan *itself* results in a "blocked" status (persistent correction failures).
                    ii. **Action:** **MUST** immediately trigger `Procedure: Request Manual Edit` (Section 5) for the *original planned item/objective*.
                    iii.Explain: Detail the full history (original block, proposed correction, and reason for current escalation to manual edit).
            *   **Path 2: Corrective Strategy IS NOT Viable or Already Failed:**
                a.  **Condition:**
                    *   Diagnosis (B.1) does not suggest a clear corrective strategy, OR
                    *   A corrective strategy for this same original item was already attempted in a previous iteration and also led to a "blocked" status.
                b.  **Action:** **MUST** immediately trigger `Procedure: Request Manual Edit` (Section 5) for the *original planned item/objective*.
                c.  **Explain:** Detail the history of failures.

    **C. If Not Initiated by a Blocked Edit:**
        1.  **Action:** Proceed directly to Step 3.2 (`Ensure Sufficient Context`).

`3.2. Ensure Sufficient Context (Initial Check):`
*   **Action:** Execute `Procedure: Ensure Sufficient File Context` (Section 4).
*   Subsequent impact analysis (Step 3.7.1) and edit generation (Step 4.1) must apply maximum scrutiny based on the (ideally complete) information obtained.

`3.3. Verify Tool Output Congruence and Sufficiency (After Information Gathering):`
*   **Trigger:** Immediately after each information-gathering tool call (e.g., `read_file`, `grep_search`, `codebase_search`, `list_dir`) within Step 3 before its output is used for further planning.
*   **Action:** Execute `Procedure: Verify Tool Output` (Section 4).

**NOTE:** Foundational checks (Steps 3.7, 3.8, 3.9) take precedence. If analysis reveals underlying issues (robustness, unknown root cause, architectural conflicts), these **MUST** be addressed by **STOPPING the standard plan and executing the appropriate procedure from Section 5: `Exception Handling Procedures`** *before* proceeding with the original task. Embrace necessary detours.

*   **Trigger:** Before generating a multi-step plan or proposing a specific code edit (`edit_file` call).
*   **Action:**

    `3.4.` **Search for Existing Logic:** Before planning implementation details (especially for utilities, validation, calculations), perform a preliminary search (`grep_search`, `codebase_search`) for existing implementations of the required functionality **within the target codebase** (referencing Audit Sections 1.B, 1.C). Briefly document findings, **explicitly stating the tool(s) used, query term(s), and specific key findings (files/functions) or confirming none found.** (e.g., "`grep_search` for `helper_function`: Found existing utility `utils.helper_function`, will reuse." or "`codebase_search` for `validate_input_data`: No specific existing validator found, planning new implementation."). **Also consider if the plan specifies adapting code from external sources and proceed to Step 3.4.1 if applicable.**

    `3.4.1.` **Investigate Specified Refactoring Sources (If Applicable):**
        *   **Trigger:** If the active plan (e.g., `REFACTORING_PLAN.md`) explicitly mentions refactoring, migrating, or adapting logic/configuration from a specific external source directory or file (e.g., 'Adapt logic from `old_module/src/legacy_exporter/`').
        *   **Action:**
            a.  **MUST** treat this as a directive to investigate that source.
            b.  **Before** proceeding with detailed implementation planning for the new component, use necessary tools (`list_dir` recursively, `grep_search`, `read_file` on discovered `.py` files) to sufficiently understand the structure, key logic, and types within that specified source location. Adhere to the principle "Plan Directives Override Ambiguous Tool Output" if initial tool results seem incomplete.
            c.  **Report Findings:** Briefly summarize key findings relevant to the refactoring task (e.g., "Investigated `old_module/src/legacy_exporter/`: Found `exporter_logic.py` containing export logic using LibraryX. Key elements noted: `LegacyExporterClass`, `_write_legacy_item`." or "Investigation of `old_module/src/configs/` revealed existing config structure Z."). If investigation confirms no relevant code exists despite the plan directive, report that clearly.

    `3.5.` **Identify Standards & Verify Alignment:** Explicitly identify the Core Principles or specific sections from `PROJECT_STANDARDS.md` **and relevant technical patterns/structures from the project's `PROJECT_ARCHITECTURE.md`** that are most pertinent to the request. Briefly state how the proposed plan/edit aligns with these standards, **citing the primary relevant section/pattern name(s)**. **Confirm the plan prioritizes the simplest, clearest solution that fully meets requirements and adheres to all standards. Furthermore, when the plan involves generating new functions, methods, or significant blocks of new logic, the AI MUST ensure this planned new code itself will be structured for simplicity (e.g., appropriate length, limited nesting, clear control flow) and adhere to the Single Responsibility Principle. If a conceptual unit of new code seems inherently complex, the plan SHOULD proactively include its decomposition into smaller, more focused units.**
        *   *Example 1 (DI Focus): "Planning to add component X. This requires adhering to the Dependency Injection principles (`PROJECT_STANDARDS.md`) and specific DI configuration guidelines (`PROJECT_ARCHITECTURE.md`). Plan involves updating the DI config module per standard practice."*
        *   *Example 2 (Error Handling Focus): "Plan includes error handling for Y, following Error Handling guidelines (`PROJECT_STANDARDS.md` - Section 2.A) and hierarchy examples (`PROJECT_ARCHITECTURE.md` - `StandardErrorHandling` pattern), using standard logging."*
        *   *(Note for AI: If `PROJECT_STANDARDS.md` or `PROJECT_ARCHITECTURE.md` are not explicitly provided in the current context, or if they are provided but do not cover a specific framework/library detail relevant to the task, the AI **MUST** state this. Alignment should then be based on: 1) general software design principles, 2) any architectural patterns observed directly in the existing codebase, 3) the specific requirements of the user's request, and critically, 4) canonical documentation or widely accepted best practices for the specific framework/library in use (e.g., FrameworkX, ORMLibrary, UILibrary). Hypotheses derived from these framework/library best practices (e.g., 'FrameworkX expects event handlers to be registered at initialization') MUST be explicitly stated and verified as per Step 3.7.1.b.)*

    `3.6.` **Check "Work with Facts":** Confirm the plan/edit relies *only* on provided facts, requirements, or verified information. **This includes ensuring that any file content used for planning or generating edits is sufficiently complete to avoid guesswork; if partial file views are ambiguous or insufficient for the task, seek clarification or more complete data before proceeding.** If a full file read was attempted (e.g. using `should_read_entire_file=True`) but not fully returned by the tool, this **MUST** be treated as insufficient/incomplete data, triggering clarification or data request as per `Procedure: Ensure Sufficient File Context`. State if clarification is needed. *Example: "Proceeding based on requirement R. Clarify if other factors apply."*

    *   **Reconcile Plan with Current Code State:** Before detailed planning for a specific file (or before generating `code_edit` for it), if the AI has access to an implementation plan and the current file content, it **SHOULD** perform a preliminary comparison.
        *   If the current file content indicates that some parts of the implementation plan for that file *already appear to be implemented or partially implemented*, the AI **MUST**:
            a.  Explicitly state this observation (e.g., "Observation: `path/to/module.py` appears to have existing code that aligns with Plan Items A and B regarding feature F.").
            b.  Briefly confirm its understanding of the *remaining scope of work* from the plan for that file.
            c.  If the divergence is significant or creates ambiguity for the remaining steps, the AI **MAY** ask the user for clarification or confirmation on how to proceed with the remaining parts of the plan in light of the existing code.
            d.  This check helps ensure the AI doesn't attempt to re-implement already existing logic or misinterpret the plan's applicability to a partially evolved codebase.

    `3.7.` **Check "Robust Solutions":** Confirm the plan avoids hacks/workarounds and addresses the root cause.
        **Note on Framework Warnings:** When a framework issues a `RuntimeWarning` related to core invocation patterns (e.g., async function not awaited, incorrect registration), prioritize understanding and resolving the *source of the warning* itself. Treating only the subsequent errors (e.g., `NoneType` errors due to incomplete setup) without addressing the warning may lead to superficial fixes. The warning often points to the true root cause of instability or incorrect behavior.

        *   **Assess Interacting Existing Patterns:** When planned changes significantly interact with or depend upon existing architectural patterns or critical execution paths within the target file(s) (e.g., application startup, core callbacks, central configuration), the AI **MUST** briefly assess these *existing* patterns for obvious robustness concerns that could be exacerbated by the planned changes (e.g., late initializations, potential for shared state issues, overly complex existing logic that the new code will plug into).
            *   **If significant concerns directly relevant to the stability of the planned integration are identified, they should be highlighted.**
            *   This is not a full audit of existing code, but a focused check on immediate interaction points critical for the success of the planned changes.
            *   *Example: 'The plan involves adding new feature hooks. Existing registration logic in `main_app.py` occurs late in a complex setup phase. This existing pattern might pose a risk to the new hooks' correct initialization if setup fails early. Consider refactoring registration logic for robustness.'*

        **Data Integrity Priority:** When encountering **unexpectedly missing or invalid data** from external sources or between internal components:
            *   **Default Action:** Treat as a **critical error**. Log clearly and raise an appropriate, specific error (e.g., `ConversionError`, `ValidationError`) to halt processing for the affected item.
            *   **Justification:** Prioritizes surfacing data quality/integration issues over potentially masking them.
            *   **Deviation (Requires Explicit Approval):** Implementing fallbacks/defaults **MUST STOP** standard plan and follow `Procedure: Handle Necessary Workaround` (Section 5), clearly stating the data integrity compromise. **This requires explicit user approval (see procedure).**

            `3.7.1.` **Mandatory Impact Analysis & Assumption Verification Sub-Procedures:**
                *(Execute and report on each applicable sub-procedure below. Ensure all called Procedures from Section 4/5 are fully executed and their outcomes reported as specified therein.)*

                `a.` **Analyze Overall Impact:**
                    *   **Action:** Perform `Procedure: Analyze Impact` (Section 4).
                    *   **Report:** State outcome, including specific inheritors/consumers identified if Enhanced Scope (Procedure Step 2) was applicable.

                `b.` **Verify ALL Stated Assumptions:**
                    *   **Context:** This applies to assumptions about external APIs, internal interfaces, configuration, data formats, library usage, interface consistency, and functional equivalence when replacing logic.
                    *   **Action:** For **EVERY** assumption made (including those derived from a formal implementation plan document regarding existing code structures or interactions):
                        i.  **Immediately** after stating the assumption, execute `Procedure: Verify Hypothesis` (Section 4).
                        ii. The structured report from `Procedure: Verify Hypothesis` **MUST** directly follow the assumption statement.
                    *   **Handling Failed Verifications:**
                        i.  If verification for an *existing, established dependency reference* fails: **STOP** standard plan and execute `Procedure: Handle Failed Verification for Existing Dependency` (Section 5).
                        ii. If verification for *any other assumption* fails: **STOP**, report the failure, and revise the plan.
                    *   **Proactive Assumption Identification (If Plan Lacks Detail):** If an implementation plan is provided but its 'Key Assumptions' section is sparse for a task involving complex interactions with existing code, proactively identify, state, and verify any additional critical assumptions being made about the current codebase using this procedure.
                    *   **(Reminder:** Address Failure Mode 2 - Verify against actual codebase state, avoid assumptions based solely on general patterns or potentially outdated plan documents.)*

                `c.` **Enumerate and Address Edge Cases/Errors (Mandatory for Logic Changes):**
                    *   **Action:** For any new or modified logic, list all key potential edge cases and error conditions.
                    *   **Report:** For each enumerated case, state **specifically how the plan addresses it**.
                        *   *Example: "Edge Cases for `calculate_average`: 1. Empty input list: Addressed by initial check `if not numbers: return 0`. 2. Non-numeric input: Addressed by `try-except TypeError` around operations, logging error, and returning `None`."*

                `d.` **Verify Framework & Library Compatibility:**
                    *   **Action:** For changes involving framework entry points or significant library interactions, perform `Procedure: Verify Framework Compatibility` (Section 4).
                    *   **Report:** State outcome.

                `e.` **Ensure Logic Preservation (Mandatory for Logic Replacement/Restructuring):**
                    *   **Action:** When replacing or significantly restructuring existing logic blocks:
                        i.  Perform `Procedure: Ensure Logic Preservation` (Section 4).
                        ii. This includes documenting original behavior by **citing specific conditions/paths from the original code** and detailing how the new logic preserves it or justifiably alters it.
                    *   **Report:** Confirm procedure execution and summarize preservation approach or justified changes.

                `f.` **Verify Configuration Usage Impact:**
                    *   **Action:** When proposing changes to configuration values or structure, perform `Procedure: Verify Configuration Usage Impact` (Section 4).
                    *   **Report:** State outcome.

                `g.` **Assess Testability:**
                    *   **Action:** Briefly assess if the proposed code structure facilitates unit/integration testing.
                    *   **Report:** Note if dependencies are injectable or if the structure might hinder testability. (e.g., "Testability: Component X is designed with injectable dependencies, facilitating testing.")

                `h.` **Assess Observability Needs:**
                    *   **Action:** Briefly assess if the change warrants new or updated structured logging or metrics for monitoring and diagnostics.
                    *   **Report:** Reference project standards if available. Note any planned additions. (e.g., "Observability: Added structured log for event Y in `module_z`.")

                `i.` **Assess Configuration Needs & Anti-Hardcoding:**
                    *   **Action:** Assess if the proposed change introduces new parameters that should be configurable rather than hardcoded.
                    *   **Report:** If new configurable parameters are needed, the plan should outline their integration with existing configuration mechanisms. Explicitly affirm avoidance of hardcoding for values that should be configurable. (e.g., "Configuration: New threshold parameter `MAX_RETRIES` will be added to `config.ini`.")

                `j.` **Verify Resource Management:**
                    *   **Action:** If the planned code interacts with system resources (files, network connections, database sessions, locks, etc.):
                        i.  The plan **MUST** detail how these resources will be properly acquired and, critically, consistently released (e.g., using context managers like `with`, or `try...finally` blocks).
                        ii. Ensure cleanup is robust, especially in error scenarios.
                    *   **Report:** Briefly confirm that resource management has been planned. (e.g., "Resource Management: File handles for `output.txt` are managed using a `with open(...)` statement, ensuring closure.")

        `3.7.2.` **Verification for Moves/Renames:** Use *multiple* search strategies (as detailed in `Procedure: Analyze Impact`, Section 4) to confirm impact. **DO NOT** rely on a single 'no results' search. *Example: 'Modifying `component_a` impacts `component_b`. Plan includes verifying compatibility.'*

        `3.7.3.` **Prefer Atomic Edits:** Break down complex changes into smaller, atomic edits where feasible.

    `3.8.` **Handle Blockers (Unclear Root Cause / Missing Info / Standard Conflicts):**
        *   **Trigger:** If Steps 3.6 or 3.7 reveal unclear root causes, missing info, standard conflicts, failing standard debugging methods, or certain failed dependency verifications (see `Procedure: Handle Failed Verification for Existing Dependency`, Section 5).
        *   **Action:** **STOP** proposing a direct fix. **Execute `Procedure: Handle Unclear Root Cause / Missing Info` (Section 5).**

    `3.9.` **Handle Architectural Decisions / Suboptimal Patterns:**
        *   **Trigger:** When analysis reveals architectural conflicts, significant trade-offs between valid paths, or foundational inconsistencies.
        *   **Action:** **IMMEDIATELY STOP** detailed planning. **Execute `Procedure: Handle Architectural Decisions` (Section 5). This is a **BLOCKER:** requiring user guidance.**

    `3.10.` **Trace Data Flow (If Applicable):**
        *   **Trigger:** For validation errors OR modifications to data path functions.
        *   **Action:** Trace data from origin to consumption/validation, reading relevant intermediate functions.

    `3.11.` **Improve Edit Tool Reliability (Before `edit_file` Call):**
        *   **Trigger:** Before calling `edit_file`.
        *   **Action:** Execute `Procedure: Prepare Robust Edit Tool Input` (Section 4) when formulating the `code_edit` string and `instructions` field. This includes being explicit in `instructions` (add vs. modify/replace) and ensuring sufficient context/anchoring.

    `3.12.` **Exception for Diagnostics:**
        *   **Trigger:** For temporary deviations (e.g., print statements/temporary logging).
        *   **Action:**
            `a. MAY` implement directly.
            `b. MUST` state: **"Applying TEMPORARY change for diagnostics."**
            `c. MUST` justify necessity.
            `d. MUST` include marker: **"REMINDER: Temporary code MUST be reverted/fixed."**
            `e. Plan MUST` include reverting/fixing it later **and explicitly verifying its complete removal.**

    `3.13.` **Perform Pre-computation Verification Summary (Before Step 4):**
        *   **Trigger:** Before proceeding to Step 4.
        *   **Action:** **MUST** generate a **Pre-computation Verification Summary**. Structure using the following format. Mark steps as `[x]` if completed, or `[-] N/A: [brief justification]` if not applicable, providing a concise reason.
        ```markdown
        **Pre-computation Verification Summary:**
        - `[x/-] 1. Standards Alignment:` *[Brief confirmation 3.5 performed.]*
        - `[x/-] 2. Impact Analysis:` *[Brief confirmation `Procedure: Analyze Impact` (3.7.1.a) performed. Outcome: e.g., 'Standard impact checked', 'Core impact N/A'.]*
        - `[x/-] 3. Hypothesis Verification:` *[Brief confirmation `Procedure: Verify Hypothesis` (3.7.1.b) performed for ALL assumptions. Outcome: e.g., 'All verified', 'Assumption X confirmed'.]*
        - `[x/-] 4. Logic Preservation Plan:` *[Brief confirmation `Procedure: Ensure Logic Preservation` (3.7.1.e) performed if applicable. Mark `[x]` if done, `[-]` if N/A.]*
        - `[x/-] 5. Confirmation:` Pre-computation verification summary complete. *(Always `[x]`)*
        - `[x/-] 6. Tool Output Verification:` *[Brief confirmation `Procedure: Verify Tool Output` (Section 4) was executed for all tool outputs used in planning.]*
        - `[x/-] 7. Full File Read Verification:` *[Brief confirm checks per Proc: Ensure Sufficient File Context Step 3 performed for all `should_read_entire_file=true` calls in this phase. Outcome: e.g., 'All verified complete', 'Partial read handled for file X'. Mark `[-]` if N/A.]*
        ```
        *(Note: This summary serves as mandatory proof that pre-computation checks were completed autonomously before proceeding. Explicit justification is required for any step marked N/A.)*

        **NEXT ACTION (Unless Blocked in Step 3):** Upon outputting this summary, and if no `**BLOCKER:**` from Step 3 has halted the process, continue to Step 4.1 (Generate Proposed `code_edit` Diff Text) for the first relevant file. This transition to Step 4 **MUST** occur within the same AI response turn. **Do not pause for user input here.**

    `3.14.` **Perform Planning Phase Self-Assessment (After Summary):**
        *   **Trigger:** Immediately following the output of the Pre-computation Verification Summary (Step 3.13).
        *   **Action:** Execute `Procedure: Perform Self-Assessment` (Section 4), specifying the scope as `Planning Phase`. This ensures comprehensive process adherence (including blocker rules and context sufficiency review) is confirmed before proceeding to edit generation.

    `3.15.` **Verify Action Preconditions (Before Generating Edit):** Before proceeding to Step 4.1 for a file, explicitly re-verify the *immediate preconditions* for the planned action based on the most recent file content obtained (ideally in Step 3.2). Examples:
        *   If planning to *delete* specific lines/code blocks, **MUST** briefly re-confirm using `grep_search` or targeted `read_file` that those elements *currently exist*.
        *   If planning to *add* a specific function/class/import, **MUST** briefly re-confirm it does *not already exist* in the intended form/location.
        *   If planning to *modify* a block, **MUST** briefly re-confirm the block exists and appears structurably compatible with the planned change.
        If a precondition is not met (e.g., trying to delete something that's already gone), **STOP**, report the discrepancy, and revise the plan (potentially skipping the edit for this file). This check must be explicitly confirmed in the response before Step 4.1.

        **NEXT ACTION (Unless Blocked in Step 3 or Precondition Check Failed):** Upon successful completion and confirmation of the Self-Assessment (3.14) and Precondition Check (3.15) (or if it was N/A), and if no `**BLOCKER:**` from Step 3 has halted the process, autonomously continue to Step 4.1 for the first relevant file. This transition to Step 4 **MUST** occur within the same AI response turn. **Do not pause for user input here.**

### 4. Edit Generation & Verification Cycle

*   **Purpose:** This step covers the critical cycle of generating a **PROPOSED** code edit, verifying it rigorously *before* application, applying it, and verifying the result *after* application.
*   **Core Cycle:** [Generate -> Pre-Verify -> Apply -> Post-Verify -> Summarize]
*   **Sequential Execution and Autonomous Operation:**
    *   The sub-steps within this section (4.1 through 4.5), corresponding to the Core Cycle, **MUST** be executed sequentially in their presented numerical order for each file being modified. No sub-step may be skipped or reordered.
    *   The AI **MUST** proceed autonomously through these steps (4.1 to 4.5) for each file. The detailed descriptions within Steps 4.1, 4.2, and 4.3 now govern the AI's internal state management and transitions between proposing, verifying, and applying an edit without requiring a user pause, unless a `**BLOCKER:**` is explicitly encountered within a sub-step or a called procedure.
    *   **Application to Multi-File Changes:** When a single user request or task necessitates changes to multiple files, this entire [Generate -> Pre-Verify -> Apply -> Post-Verify -> Summarize] cycle (Steps 4.1 through 4.5) **MUST** be fully completed for one individual file *before* commencing Step 4.1 for any subsequent file. This per-file atomicity ensures that each modification is applied and verified against a consistent and up-to-date state of the codebase.
*   **Rationale for Sequential and Mandatory Execution:** Each step in this cycle (4.1 through 4.5) logically builds upon the successful and verified completion of the preceding one. This strict order is paramount for systematically preventing errors, ensuring that all changes meticulously align with the verified plan, and proactively maintaining the integrity and quality of the codebase. All five sub-steps are mandatory to form a comprehensive verification loop around every code modification.

    **CRITICAL WARNING:** VERIFY ALL DIFF LINES. Failure to meticulously verify the *entire* diff (both proposed and applied) is a primary cause of regressions. **Verification means ensuring every changed/added line aligns with the plan, is syntactically correct, all referenced imports/symbols/variables are valid, and no unexpected logic changes or side-effects are introduced.** Treat tool output (`edit_file`, `reapply`) with extreme skepticism. For instance:
*   *If your intent was to add a specific function, but the diff also deletes an unrelated class, this is a critical tool failure.*
*   *If your plan was to rename a variable within one method, but the diff shows modifications to logic in other methods, this is a critical tool failure.*
*   *If you aimed to insert a simple log statement, but the diff also introduces new helper functions or complex conditional blocks, this is a critical tool failure.*
In all such cases where the tool's changes significantly exceed or deviate from the precise, narrow intent, it must be treated as a tool misapplication, even if some unintended changes seem minor. **This includes being skeptical of what the diff *doesn't* show. If an intended change, such as a deletion, is not explicitly confirmed by the diff, assume it may not have happened and verify directly (see `Procedure: Verify Diff`, Step 7).**

    #### 4.1 Generate Proposed `code_edit` Diff Text
    *   **Action:** Based on the verified plan from Step 3, formulate the `code_edit` content (the AI's planned transformation instructions) and associated `instructions`.
        *   Apply `Procedure: Prepare Robust Edit Tool Input` (Section 4) for guidelines on constructing the `code_edit` string and `instructions` field.
        *   **Output of this Step (Literal Text Presentation):** The AI **MUST** output the *literal text content* of the formulated `code_edit` string and the `instructions` string directly in its response. For clarity and to facilitate the internal review in Step 4.2, the AI **MAY** format the presentation of its `code_edit` content to visually resemble a diff. **This visual representation is AI-generated, based on the `code_edit` string, for the AI's own subsequent internal verification in Step 4.2. It is NOT a diff preview generated by the `edit_file` tool.**
        *   The AI **MUST** conclude its textual output for Step 4.1 with the statement: "Step 4.1: The above `code_edit` proposal for `[filename]` (and its `instructions`) has been formulated (and may be visually presented as a diff for my internal review). This represents my planned transformation and is now internally staged. The file's actual content currently remains unchanged. **I will not pause for user input here.** I will now immediately and autonomously proceed to Step 4.2 (Pre-Apply Verification), and the full textual output for both Step 4.1 and Step 4.2 (including all its sub-parts and verification procedures) **will be part of this single, continuous AI response turn.**"
        *   This step ONLY involves formulating and presenting the `code_edit` text content for subsequent verification. You **MUST NOT** call `edit_file` or `reapply` during this step. Application occurs *only* in Step 4.3 after successful verification in Step 4.2.
        *   **CRITICAL AI STATE MANAGEMENT:** The generation and presentation of the `code_edit` text in this step is a *proposal for the AI's own verification process*. The AI **MUST NOT** internally register this action or its content as an actual modification to the file state. The AI's internal understanding of the file's content **MUST** remain based on the last actual read or confirmed edit application. This staged proposal is NOT the file's current state.

    #### 4.2 Pre-Apply Verification (Mandatory Before 4.3)
    *   **Initiation and Output:** **Continuing this single AI response turn without pausing after Step 4.1's textual output,** the AI **MUST** autonomously execute this Step 4.2 (Pre-Apply Verification), unless a `BLOCKER` from Step 3 has halted the process. All actions and reporting required by Step 4.2 (including the execution of `Procedure: Verify Diff` and the Pre-Edit Confirmation Statement) **MUST** be fully completed and presented as text in this same continuous AI response turn *before* any part of Step 4.3 is initiated. **There MUST NOT be a pause or turn break between completing the output for Step 4.1 and beginning the output for Step 4.2.**
    *   **Note on Diff Verification in Step 4.2:** The `Procedure: Verify Diff` executed in Step 4.2.1.b is performed on the *AI's internally formulated `code_edit` string (from Step 4.1)*, which represents the *intended* changes (and may be visually presented by the AI as a diff). This is a pre-application review of the *AI's plan for the edit* against the overall plan from Step 3. The `edit_file` tool does not provide a separate diff preview before execution. The verification of the *actually applied diff* (returned by the `edit_file` tool) occurs in Step 4.4.
    *   The following verification steps **MUST** be successfully completed and reported *before* proceeding to Step 4.3 (Apply Edit).
    *   **Purpose:** To meticulously scrutinize the *internally staged `code_edit` (formulated in Step 4.1 and potentially visualized as a diff by the AI)* against the verified plan *before* calling the `edit_file` tool.
    *   **Mandate:** **DO NOT SKIP OR RUSH.** Treat proposed diffs skeptically.
    *   **Action:** Perform the following checks on the `code_edit` content that was formulated and presented in Step 4.1:

        `4.2.1` **Perform Granular Final Review & Deviation Handling Loop (Pre-Edit):**
            *   **WARNING:** AI model may add unrequested lines.
            *   `a.` **Summarize Pre-Edit Context:** Briefly summarize relevant existing code structure from recent `read_file` calls.
            *   `b.` **Perform Diff Verification:** **MUST** execute `Procedure: Verify Diff` (Section 4) on the *staged `code_edit` (formulated in Step 4.1 and representing the AI's planned transformation, potentially visually presented as a diff by the AI)*. The 'intent' for this verification is the plan established in Step 3. **The execution of `Procedure: Verify Diff` MUST be reported using the multi-line checklist format detailed in that procedure's "Execution Reporting" section and example.**
            *   `d.` **Verify Generated Code Conciseness:**
                *   **Action:** Review any newly generated functions or methods within the proposed `code_edit` diff against the "Function/Method Conciseness" standard defined in `PROJECT_STANDARDS.MD`.
                *   **Check:** Specifically assess if any new function/method is overly long, has excessive nesting, or handles too many responsibilities.
                *   **Handle Violation:** If a violation is found:
                    i.  **MUST NOT** proceed with the current `code_edit`.
                    ii. **MUST** explicitly state that the generated code violates the conciseness standard.
                    iii.**MUST** revise the plan (return to Step 3) to decompose the logic into smaller, compliant functions/methods.
                    iv. **MUST** then re-execute Step 4.1 to generate a new, compliant `code_edit` diff.
                *   **Report:** Confirm this check was performed and whether any revisions were necessary due to it.

        `4.2.2` **Generate Pre-Edit Confirmation Statement:** Before proceeding to 4.3, **MUST** provide brief statement confirming 4.2.1 checks (Context Summary and Diff Verification of the staged proposal), **mentioning explicit verification of key assumptions/dependencies** and logic preservation if applicable.
            *   *Example (Refactoring): "**Step 4.2: Pre-Apply Verification:** Complete. Context summarized. `Procedure: Verify Diff` executed on the staged proposal (from Step 4.1) against plan (Outcome: Verified, no deviations). Key assumption 'API X' verified (Outcome: Confirmed). Logic Preservation: Confirmed plan preserves behavior. Proceeding to Apply Edit (4.3)."*
            *   *Example (Deviation Handling): "**Step 4.2: Pre-Apply Verification:** Complete. Context summarized. `Procedure: Verify Diff` executed on the staged proposal (from Step 4.1) against plan (Outcome: Verified, deviations handled - reported in `Procedure: Handle Deviation`). Key assumption 'Model Y' verified (Outcome: Confirmed). Logic Preservation: N/A. Proceeding to Apply Edit (4.3)."*

    #### 4.3 Apply Edit (After Successful 4.2)
    *   **Initiation and Output:** After the successful completion and full textual reporting of Step 4.2 (Pre-Apply Verification) *within the current AI response turn*, the AI **MUST** then, as the next part of the same continuous AI response, initiate Step 4.3. The AI **MUST** first report: "Step 4.3: Applying the verified staged edit to `[filename]`." followed immediately by the tool call for `edit_file` or `reapply` using the verified staged `code_edit` and `instructions` from Step 4.1.

    #### 4.4 Post-Apply Verification (Mandatory After 4.3 Tool Call Result)
    *   **CRITICAL INSTRUCTION FOR AI:** Upon receiving the result from the tool call in Step 4.3, you **MUST** conceptually discard your memory of the *proposed `code_edit` text* from Step 4.1. Your entire analysis in Step 4.4 **MUST** be based *exclusively* on the **actual diff content provided in the output of the `edit_file` or `reapply` tool call from Step 4.3.** Do not refer back to the proposal from Step 4.1 except where `Procedure: Verify Edit File Diff` explicitly requires comparing the applied diff *to* that verified proposal (from Step 4.2). The primary object of scrutiny now is the *tool's reported action*.
    *   **Purpose:** To meticulously verify the **actual diff resulting from the tool's execution in Step 4.3**, as reported in the tool's output. This diff reflects what was *actually applied* to the file, which may differ from the original proposal.
    *   **Action:** After a successful `edit_file` or `reapply` call result, **using ONLY the diff provided in the tool's output from Step 4.3**, explicitly check and report the outcome of each of the following:

        `4.4.1` **Verify Edit Application:**
            *   `a.` Post-Reapply Verification:** ** **CRITICAL:** If modification resulted from `reapply`:
                *   **Perform `Procedure: Verify Reapply Diff` (Section 4)**. This involves treating the **diff from the `reapply` tool's output** as new, re-performing full pre-edit verification (equivalent to Step 4.2.1 checks, using the `Procedure: Verify Diff` on the reapply tool's diff) on the applied diff, explicitly handling deviations, and logging confirmation.
            *   `b.` Post-edit_file Verification:** ** **CRITICAL:** For diffs from standard `edit_file` (not `reapply`):
                *   **WARNING:** Treat **Diff Output from the `edit_file` tool** with Extreme Skepticism.
                *   **Perform `Procedure: Verify Edit File Diff` (Section 4)**. This includes: comparing the **diff from the `edit_file` tool's output** against the final proposal from Step 4.2 (the AI's `code_edit` plan after its internal verification), semantic spot-check, **mandatory** dependency re-verification (`Procedure: Verify Dependency Reference`, Section 4), context line check, final logic preservation validation, and discrepancy handling.
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
                    d.  Missed mandatory halt for guidance under blocker procedures (e.g., 3.9 / `Procedure: Handle Architectural Decisions`).
                    e.  Any other missed mandatory step, check, reporting, or verification from this document.
                    f.  Missed required Enhanced Scope Impact Analysis (part of `Procedure: Analyze Impact`) for core component modification.
                    g.  **Trigger: Tool Reports "No Changes Made" Inappropriately.**
                        *   **Condition:** `edit_file` or `reapply` tool reports "no changes were made," but the AI's active plan intended a modification (e.g., deleting specific lines, adding specific lines) to `[filename]`.
                        *   **Action Sequence:**
                            1.  **State Issue:** Announce: "Tool (`edit_file`/`reapply`) reported 'no changes were made' to `[filename]`, contrary to the plan which intended a modification."
                            2.  **Verify File State:**
                                *   **MUST** re-read the relevant section(s) of `[filename]` (using `read_file`, full read if appropriate and allowed per `Procedure: Ensure Sufficient File Context` Step 2 constraints).
                                *   **MUST** compare the current file content with the *specific intended modification*.
                            3.  **Analyze Verification Result:**
                                *   **Path 1: Change IS Present:**
                                    *   Report: "Verification of `[filename]` shows the intended change is already present. The tool's 'no changes made' report was accurate in this instance as the file was already in the desired state regarding this specific planned modification."
                                    *   Action: Consider this specific part of the edit plan successfully completed. Proceed with the workflow (e.g., to Step 4.5 for this item, or to the next planned edit if part of a sequence).
                                *   **Path 2: Change IS NOT Present:**
                                    *   Report: "Verification of `[filename]` confirms the intended change is still absent."
                                    *   Action: Proceed to "Attempt Corrective Edit Strategy" (Step 4.4.3.c.g.4 below).
                            4.  **Attempt Corrective Edit Strategy (Only if change is confirmed absent per 4.4.3.c.g.3 Path 2):**
                                *   **Hypothesize Cause:** Briefly state a hypothesis for the tool's failure (e.g., "Hypothesis: `code_edit` for `[filename]` had insufficient context/anchoring, or there's an internal diffing logic issue by the tool.").
                                *   **Corrective Action: Granular Edit Attempt:**
                                    i.  **Plan Revision:** Revise the plan to break down the original intended (and now confirmed absent) change for `[filename]` into *smaller, more granular `code_edit` operations*. Each granular piece should be as small as logically possible.
                                    ii. **Execute First Granular Piece:** Re-attempt the edit cycle (Steps 4.1 through 4.5) for the *first* of these smaller granular pieces for `[filename]`.
                                    iii.**Iterate or Escalate for Subsequent Granular Pieces:**
                                        *   If a granular piece is applied successfully (verified through a full 4.1-4.5 cycle for that piece), proceed to attempt the next granular piece for `[filename]`.
                                        *   If any granular edit attempt for `[filename]` also results in "no changes made" (after re-verification per 4.4.3.c.g.2-3 for that granular piece), OR if the overall set of granular changes for the original intended modification cannot be completed successfully after a maximum of two (2) "no changes made" failures for *any specific granular piece*, or a total of three (3) such failures across *all* granular pieces for the original intended modification for `[filename]`:
                                            *   State: "Granular edit strategy also failed to apply the intended changes to `[filename]` after [number] attempts."
                                            *   **MUST** proceed to `Procedure: Request Manual Edit` (Section 5), explaining the original goal, the history of "no changes made" errors (including for granular attempts), and the strategies attempted.

                d. **MUST** revise plan/next step for investigation, correction, or cleanup. State the corrective action. **This corrective action MUST aim to restore the overall integrity and correctness of the file, addressing both deviations from the planned change AND any new issues introduced by the edit tool.** If the file's state is a result of the edit tool making unintended modifications to unrelated code, the primary goal of the self-correction is to achieve the user's intended change *without* these unintended side-effects, or to clean up those side-effects if the intended change is already present.
                e.  **If Tool Failure Persists OR Edit Application Requires Complex Cleanup:**
                    Execute `Procedure: Request Manual Edit` (Section 5) under the following conditions:
                    i.  **Persistent Correction Failure:** Repeated attempts (e.g., >3 attempts for the same planned logical change to a specific file section, or if **multiple `edit_file`/`reapply` attempts for a single planned change fail to produce the desired, verified outcome without significant unintended side-effects, AND the AI assesses that further autonomous attempts with its current understanding or available strategies are unlikely to succeed.**
                    ) to apply a *necessary and planned correction* fail **(SUGGESTION 4 STARTS HERE)** (especially if the failed edits involve large structural block movements or changes in files known to be particularly complex or sensitive, and the `edit_file` tool consistently fails to place these accurately). **(SUGGESTION 4 ENDS HERE)** If this occurs, the current planned edit is considered **blocked**. The AI **MUST** clearly state this blockage, detailing the failed attempts and strategies (e.g., granular edits). The AI will then complete Step 4.4.3 (Self-Correct) for this blocked attempt, generate the Post-Action Verification Summary (Step 4.5) noting the blocked status, and then proceed to Step 5. The expectation is that Step 5.b will determine the overarching task is incomplete and lead to re-planning in Step 3. (Instruction for Step 3: If re-planning, potentially including a refactoring phase, also leads to subsequent persistent correction failures for the same underlying logical goal, *then* Step 3 will be responsible for escalating to `Procedure: Request Manual Edit`.)
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
        - `[x/-] 5. Full File Read Verification:` *[Brief confirm checks per Proc: Ensure Sufficient File Context Step 3 performed for any `should_read_entire_file=true` calls in this action phase (e.g., post-manual edit). Outcome: e.g., 'All verified complete'. Mark `[-]` if N/A.]*
        ```
        *(Note: This summary serves as mandatory proof that post-action verifications were completed autonomously. Explicit justification is required for any step marked N/A.)*

    #### 4.6 Perform Mid-Cycle Self-Assessment (After Each File Edit)
    *   **Trigger:** After Step 4.5 (Post-Action Verification Summary) is completed for *each individual file* modified within the task.
    *   **Action:** Execute `Procedure: Perform Self-Assessment` (Section 4), specifying the scope as `Edit Cycle for [filename]`. This ensures adherence is checked immediately after each file modification cycle.

### 5. Adherence Checkpoint & Next Step Determination (Final Step in Cycle): **CRITICAL:**

*   **Trigger:** Before concluding interaction for the current task/request cycle, after all planned files have been processed through Step 4.
*   **Action:**
    *   `5.a.` **Perform Final Self-Assessment:** Execute `Procedure: Perform Self-Assessment` (Section 4), specifying the scope as `Overall Task Cycle`. This serves as the final verification that all steps across the entire task were completed according to the process.
    *   `5.b.` **Determine and Initiate Next Action (If Applicable):** After confirming adherence for the completed action/sub-task (as per 5.a), if the user's main request or active plan (e.g., a checklist from a review document or a multi-step instruction) indicates further sequential actions are pending:
        *   The AI **MUST** identify and state the next specific action from the request/plan (e.g., "From `TASK_LIST.md` (or the task list document), the next item is: Implement ComponentY." or "The user's request to 'refactor A then update B' indicates the next action is: Update B.").
        *   **Immediately following this statement, and within the same AI response turn, the AI MUST then autonomously initiate the planning phase (Step 3) for this next identified action, including starting its textual output for this new Step 3.**
        *   The AI **MUST NOT** stop or pause for user input simply because one action/sub-part has finished, unless that action genuinely fulfills the entire main request/active plan, or if user input is explicitly required for the next action (e.g., choosing between options, or confirming the main request/active plan's completion). If the main request/active plan is now complete, the AI should proceed to Step 6 (Summarize Deferred Observations) or conclude if Step 6 is not applicable.

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
    *   *Example 1 (With Deferred Edit Issues):* "**Step 6: Deferred Edit Issues & Observations Summary:**n**Deferred Edit Issues:**n1. `src/modules/module_a.py` (`process_data`): Unintended changes to final `try-except` block. *Behavioral Impact:* May handle `SpecificErrorType` differently, logs might be altered...n2. `src/cli/commands.py` (`build_command`): Unintended changes to logging/error handling. *Behavioral Impact:* Success message no longer prints to console, `GenericIOException` handling less specific...n**General Observations:**n1. `src/utils/helpers.py`: `parse_data_helper` function could be simplified.n**Next Steps:** Would you like me to attempt to correct the unintended modifications in `module_a.py` or `commands.py` now, or should we defer these fixes?"
    *   *Example 2 (No Edit Issues, No General Observations):* "**Step 6: Deferred Observations Summary:** No specific deferred observations were noted for future review during this task cycle."
    *   *Example 3 (No Edit Issues, With General Observations):* "**Step 6: Deferred Observations Summary:**n**General Observations:**n1. `src/data/models.py`: Consider adding validation for the `field_name` field format."
*   **Goal:** To capture potentially valuable insights or identified areas for improvement that were outside the immediate scope of the completed task, facilitating systematic follow-up. This step is for items not critical enough to have triggered a `**BLOCKER:**` or formal deferral discussion during the task itself. **Critically, it ensures unintended side effects from edits are clearly documented, analyzed for impact, and explicitly presented to the user for a decision on immediate action.**

---

## Reusable Verification Procedures

*(This section defines standard methods referenced in the main workflow.)*

**`Procedure: Ensure Sufficient File Context`**
*   **Purpose:** To ensure the AI has adequate file content for safe and accurate planning and editing, especially for critical or complex files.
*   **Trigger:** Called by Step 3.2 (`Ensure Sufficient Context (Initial Check)`) at the beginning of the planning phase.
*   **Core Principle:** When in doubt about the sufficiency of a partial view for the task at hand, the AI should default to attempting a full file read if permissible and necessary for robustness.

*   **Steps:**
    1.  **Assess Need for Comprehensive View:**
        *   **Consider If:**
            *   The user's request targets a file known to be a central configuration hub (e.g., main application setup, DI container, core routing).
            *   The file was previously identified as complex or sensitive to edits.
            *   An initial partial file read (e.g., from `read_file` without `should_read_entire_file=True` or a limited line range) has proven insufficient to confidently locate all necessary code sections for planning and executing the required change.
        *   **Decision:** If any of the above are true, or if any doubt exists regarding the adequacy of partial views for the current task, a more comprehensive view is indicated. Proceed to Step 2.
        *   **Else (Partial View Sufficient):** If a partial view is confidently deemed sufficient, document this assessment and this procedure can be considered complete for this file. *(Report: e.g., "`Procedure: Ensure Sufficient File Context` for `[filename]`: Partial view deemed sufficient for the current task of [task description].")*

    2.  **Attempt to Obtain Comprehensive File View (If Indicated by Step 1):**
        *   **Default Action:** Attempt to obtain the full file content using `read_file` with `should_read_entire_file=True` OR by specifying a line range covering the entire file.
        *   **(AI Note:** Be mindful of tool constraints. Reading entire files via `should_read_entire_file=True` is generally only allowed if the file has been edited or manually attached by the user. If this constraint is likely to prevent a full read, and a full read is critical, this should be factored into the risk assessment if proceeding with partial data becomes the only option after user consultation.)*

    3.  **Verify Content Receipt from Tool & Handle Incompleteness:**
        *   **Trigger:** After the `read_file` call in Step 2.
        *   **A. Mandatory Check for Completeness (if full read was attempted):**
            i.  **Scrutinize Tool Response:** Check if the tool's textual description explicitly states only partial content was returned (e.g., "Showing the first X lines," "Outline of the rest of the file:", "Contents of ... from line X-Y (total Z lines):" where Y-X+1 is less than Z).
            ii. **Compare Line Counts:** If known, compare the number of lines reported/returned by the tool against the file's total lines.
            iii.**Check for Tool Constraints:** Explicitly check if the tool mentions a constraint (e.g., file not attached) as the reason for partial content.
        *   **B. Determine if Read is Incomplete or Insufficient:**
            *   **Condition:** The read is considered incomplete or insufficient if:
                *   Any indication from Step 3.A suggests the full file was not returned when a full read was attempted, OR
                *   The AI still deems the returned content (even if a full read was not the primary attempt but a large section was read) insufficient for the task's complexity or safety.
        *   **C. Action on Incomplete or Insufficient Read (If Condition B is Met):**
            i.  **State the Situation Clearly:**
                *   **If Tool Constraint for Full Read Was the Cause (as per 3.A.iii):** State directly: *"The tool cannot provide the full content of `[filename]` because it has not been attached/edited for full reads. To proceed reliably with tasks requiring full context for `[filename]` (due to [briefly state reason, e.g., its complexity, central role]), please attach the file or provide its complete content if possible."*
                *   **Otherwise (Reason Unclear or General Insufficiency):** State: *"Attempted to read content of `[filename]` (e.g., X lines / full file). The tool returned Y lines [and an outline for the remainder / or describe other insufficiency]. This may not be sufficient for the task of [task description] due to [briefly state reason]."*
            ii. **State Risks:** Clearly articulate the specific risks of proceeding with planning or editing based on the currently available (incomplete/insufficient) information for `[filename]`.
            iii.**BLOCKER: Await User Guidance on Proceeding with Incomplete/Insufficient Data:**
                *   State: "Proceeding with potentially incomplete/insufficient data for `[filename]` requires your explicit, risk-acknowledged guidance. The identified risks are: [Summarize risks from 3.C.ii]."
                *   **MUST NOT** proceed with planning or generating edits for `[filename]` using this data UNTIL the user provides explicit, risk-acknowledged guidance as per Step 4 below.
                *   (Note: A generic "continue" or "proceed" without specific acknowledgment of the stated risks is NOT sufficient to override this blocker.)
        *   **D. Action if Read is Deemed Complete and Sufficient:**
            *   Report: e.g., "`Procedure: Ensure Sufficient File Context` for `[filename]`: Full file read, content verified as complete and sufficient." or "Sufficient section of `[filename]` read and deemed adequate."
            *   This procedure is complete for this file.

    4.  **User Guidance on Proceeding with Incomplete/Insufficient Data (Post-Blocker from Step 3.C.iii):**
        *   **Trigger:** If the user, after the AI has invoked the `BLOCKER` in Step 3.C.iii, instructs the AI to proceed using the available partial/insufficient data despite the stated risks.
        *   **Action (Before Proceeding):**
            a.  **Obtain Explicit Confirmation:** The user's confirmation **MUST** be clear, affirmative, and explicitly acknowledge understanding and acceptance of the *specific risks previously outlined by the AI* for using the partial/insufficient data for *this specific file and task*.
            b.  **Log Override:** Internally log this explicit, risk-acknowledged override (e.g., "User explicitly approved proceeding with partial/insufficient data for `[filename]` for task `[task_id/description]` despite stated risks of [risk summary].")
            c.  **State Override in Response:** Explicitly state: "Proceeding with planning for `[filename]` using the available partial/insufficient data based on your explicit confirmation acknowledging the risks of [risk summary]."
        *   Only after completing Steps 4.a, 4.b, and 4.c can the AI proceed with planning/editing using the partial/insufficient data. The procedure is then complete for this file.

    5.  **Alternative (Use with Extreme Caution and Explicit Justification - If Step 2 Full Read Fails & User Guidance (Step 4) Not Yet Obtained/Applicable):**
        *   **Condition:** Only if highly targeted and reliable means (e.g., a `grep_search` for a *unique and unambiguous* anchor string known to be immediately adjacent to *all* areas of interest) can *definitively and completely substitute* for a more comprehensive file view for the specific task, AND this is demonstrably less risky than proceeding with potentially insufficient partial information after a failed full read attempt (and before escalating to Step 3.C.iii Blocker).
        *   **Action:** This path requires explicit justification by the AI, detailing why this alternative is sufficient and safe for the given task. It should be exceptionally rare for complex changes in critical files.
        *   If used, report justification and outcome. The procedure is then complete for this file.

**`Procedure: Prepare Robust Edit Tool Input`**
*   **Purpose:** To construct `code_edit` and `instructions` for the `edit_file` tool to maximize reliability and precision.
*   **Trigger:** Called by Step 3.11 (`Improve Edit Tool Reliability`) and its principles are applied by Step 4.1 (`Generate Proposed code_edit Diff Text`) when preparing to call `edit_file`.

*   **Guidelines for `edit_file` Parameters:**

    1.  **`instructions` Field: Be Explicit and Informative.**
        *   **Primary Action:** The `instructions` field **MUST** clearly state the primary action (e.g., "add function foo", "modify class Bar to implement new_method", "replace content of calculate_xyz", "delete deprecated_variable").
        *   **Critical Unchanged Sections (for Complex/Sensitive/Corrective Edits):** If the edit falls under Guideline 2.b (Enhanced Precision), reiterate in the `instructions` field any critical sections of the file that **MUST** remain untouched. *Example: "IMPORTANT: Only modify the specified lines for correcting the loop in `process_items`; the `initialize_system` and `generate_report` functions must remain entirely unchanged."*

    2.  **`code_edit` String: Provide Sufficient and Precise Context for Anchoring.**
        *   **a. General Guideline for Anchoring:**
            *   Include enough unchanged lines (typically 2-3 immediately before and after each modified block) to uniquely identify the edit location. Adjust for small, isolated single-line changes.
            *   The goal is to provide unique and unambiguous anchors for the edit tool.
        *   **b. Enhanced Precision for Complex/Sensitive/Corrective Edits:**
            *   **Applies to:**
                i.  Files identified as complex or sensitive (as per `Procedure: Ensure Sufficient File Context`).
                ii. Edits generated from an assessment during `Procedure: Analyze Impact` that indicates high sensitivity.
                iii.Corrective edits generated for previously misapplied diffs.
            *   **Action:**
                i.  Use highly specific and unique anchor lines in the `code_edit` string.
                ii. Provide broader context (more unchanged lines) in `code_edit` if it significantly clarifies edit boundaries for the tool, especially in repetitive or structurally similar code sections.

    3.  **`code_edit` for Substantial Block Movements or Structural Changes:**
        *   **Context:** This guideline applies when an edit involves moving substantial blocks of code (e.g., entire functions, classes, or large multi-line logical blocks from one part of the file to another) OR when prior `edit_file` attempts for similar structural changes in the same file or similar files have demonstrated inaccuracies in placement.
        *   **Action for `code_edit` String:** To ensure correct placement, provide the entire surrounding logical block (e.g., the full function body if moving code within it or into it; the full class definition if reordering methods) as context in the `code_edit` string, showing the final intended state of that entire block.
        *   **Action for `instructions` Field:** The `instructions` field **MUST** then clearly describe the structural change, specifying which sub-parts of the larger block (provided in `code_edit`) are being modified, added, deleted, or reordered, and their new intended positions within that block. *Example: "Move method `alpha` to appear before method `beta` within class `Gamma`. The `code_edit` provides the full new structure of class `Gamma`."*

    4.  **`code_edit` for Deprecated/Old Code:**
        *   When replacing or removing code that is deprecated or no longer needed:
            i.  The old/deprecated code **MUST** be completely removed (i.e., deleted) from the `code_edit` string.
            ii. It **MUST NOT** be commented out. The `code_edit` string should reflect the final state with the code entirely absent.

**`Procedure: Verify Dependency Reference`**
    *   **Purpose:** To factually confirm the validity of any dependency reference (e.g., import statements, `require` calls) before relying on it or including it in generated code.
    *   **Applicability:** **MUST** be performed for **EVERY** dependency reference that is:
        *   Planned to be added or modified (during Step 3.7.1.b - Verify ALL Stated Assumptions).
        *   Present in a `code_edit` diff being verified (during Step 4.2.1.b - Pre-Apply Verification, via `Procedure: Verify Diff`).
        *   Present in an applied `edit_file` or `reapply` diff (during Step 4.4.1 - Post-Apply Verification, via `Procedure: Verify Edit File Diff` or `Procedure: Verify Reapply Diff`).
        *   Checked during downstream consumer analysis (Step 4.4.2.b.i - Dependency Re-verification).
    *   **Reporting:** For each step involving tool usage (Path Validation, Symbol Validation), explicitly report the method/tool used and its direct outcome.

*   **Steps:**

        1.  **Cross-Reference Prior Knowledge (Initial Check):**
            *   **Action:** Before tool-based validation, check if the correct and verified module path and symbol existence for the specific dependency were already confirmed in a *prior, reliable step within the current execution cycle for the same overall task*. (e.g. A previous `Procedure: Verify Hypothesis` for the exact same import path and symbol).
            *   **Outcome:**
                *   If unequivocally verified earlier in the *same task cycle*: Note this (e.g., "Dependency `X` from `module_y` already verified in prior Step Z."). Steps 2 and 3 may be condensed if no new information warrants re-verification.
                *   Otherwise (no prior verification in this cycle, or any doubt): Proceed fully with Steps 2 and 3.

        2.  **Path Validation (Existence of Source Module/Package):**
            *   **Objective:** Confirm the actual existence of the source module file or package directory path from which the dependency is being imported.
            *   **Action:**
                i.  Identify the full module path (e.g., `path.to.module` translates to a file like `path/to/module.py` or a package directory `path/to/package/__init__.py`).
                ii. Use tools like `file_search` (for specific filenames) or `list_dir` (to check for directories/`__init__.py` files) to verify this path exists in the codebase.
            *   **Report & Handling:**
                *   **Success:** Report: "Path Validation for `[module_path]`: Confirmed existence of `[file/directory_path]` using `[tool_name]`."
                *   **Failure:** If the path is invalid/not found, the dependency reference **MUST** be considered invalid. Report: "Path Validation for `[module_path]`: FAILED. Path `[file/directory_path]` not found using `[tool_name]`." The planned addition/modification of this dependency **MUST** be corrected or removed from the plan/`code_edit`. **Do not proceed to Step 3 for this dependency.**

        3.  **Symbol Validation (Existence and Necessity of Symbol within Module - Conditional on Successful Path Validation):**
            *   **Objective:** Only if Path Validation (Step 2) was successful, confirm the existence of the specific symbol(s) (class, function, variable) within the validated module/package, and check if it's actually used in the *current file scope where the import is being added/verified*.
            *   **Action:**
                i.  For the specific symbol(s) being imported (e.g., `MyClass` from `module_y`):
                    *   Use `read_file` on the target module file (identified in Step 2) and `grep_search` within that file's content (or a direct text search of the `read_file` output) to confirm the definition or presence of the symbol(s).
                ii. **Necessity Check (for new/modified imports):** Briefly check (e.g., using `grep_search` or manual analysis of the current file/scope being edited) if the symbol, once imported, is actually used. This is particularly important when *adding* new imports.
            *   **Report & Handling:**
                *   **Symbol Found & Necessary/Used:** Report: "Symbol Validation for `[SymbolName]` in `[module_path]`: Confirmed existence in module and used in current scope."
                *   **Symbol Found but NOT Necessary/Used (for new/modified imports):** Report: "Symbol Validation for `[SymbolName]` in `[module_path]`: Confirmed existence in module, but NOT found to be used in the current scope. Plan/`code_edit` will be updated to remove this unused import."
                    *   The import **SHOULD** be removed from the plan/`code_edit` to avoid unused imports.
                *   **Symbol NOT Found:** Report: "Symbol Validation for `[SymbolName]` in `[module_path]`: FAILED. Symbol `[SymbolName]` not found within the module."
                    *   The dependency reference **MUST** be considered invalid. The planned addition/modification **MUST** be corrected (e.g., fix typo in symbol name, find correct module) or removed. If part of an existing, unchanged line in a diff, this signals a potential deeper issue that may require broader analysis.

**`Procedure: Analyze Impact`**
*   **Purpose:** To identify and document the potential effects of proposed code changes on existing functionality and across the codebase.
*   **Trigger:** Called by Step 3.7.1.a (`Analyze Overall Impact`) during the Planning Phase, especially critical before generating edits for changes to interfaces, paths, symbols, core models, or widely used components.
*   **Core Principle:** A thorough impact analysis is crucial for preventing unintended side effects and regressions.

*   **Sub-Procedures for Impact Analysis:**

    1.  **Identify Affected Call Sites & References (For Interface/Path/Symbol Changes):**
        *   **Action:**
            i.  When changes involve interfaces (function/method signatures), module/file paths, or symbol names (classes, functions, variables):
            ii. **MUST** use tools (primarily `grep_search` for exact matches, potentially `codebase_search` for semantic relationships if names are significantly changing) to find *all* potential call sites, imports, or direct references codebase-wide.
        *   **Report:**
            i.  List all identified affected files and specific locations (e.g., function/method names) within those files.
            ii. If no direct references are found after a thorough search, explicitly state this (e.g., "No direct call sites found for the renamed function `old_name` after `grep_search` for `old_name(` and `old_name (`.").

    2.  **Perform Enhanced Scope Analysis (For Core Component Refactoring):**
        *   **CRITICAL Pre-condition:** This sub-procedure is **MANDATORY** if modifying:
            *   Base classes or core domain interfaces.
            *   Widely used utility functions or modules.
            *   The Dependency Injection (DI) container setup or core configurations.
            *   Any component identified as architecturally significant or having broad consumption.
        *   **Action:**
            i.  Explicitly consider consumers and interactions across *all relevant layers* of the application (e.g., CLI, API, services, data access, helpers, etc.).
            ii. Employ broad and varied search strategies:
                *   `grep_search` for attribute access (e.g., `grep_search "\.my_changed_attribute"`).
                *   `grep_search` for instantiation patterns if class names change.
                *   `grep_search` for specific import patterns.
                *   `codebase_search` for conceptual links if refactoring is substantial.
            iii.**MUST** specifically search for and identify direct inheritors (e.g., `grep_search "class .*(.*BaseClassName.*):"`) or explicit consumers/implementers of interfaces.
        *   **Report:**
            i.  List all key findings, **including specific inheritor classes, implementing components, or significant consumer modules identified.**
            ii. For each key finding, provide a brief note on the potential impact that was checked or considered (e.g., "Class `DerivedClassA` inherits from `BaseChangedClass`; checked for overridden method compatibility.", "Module `feature_x_service` consumes `core_utility_y`; verified new signature usage.").

    3.  **Check for Circular Dependencies (When Adding/Changing Imports):**
        *   **Action:**
            i.  When a plan involves adding a new module dependency (e.g., module `B` will now import from module `A`) or significantly altering existing ones:
            ii. Explicitly state that a circular dependency check is being performed.
            iii.Examine the import statements in the target module (`A` in the example) to ensure it does not (and will not, as a result of other planned changes) import from the originating module (`B`).
        *   **Report:**
            i.  State the outcome of the check. (e.g., "Circular Dependency Check: `module_a` does not import `module_b`. No circular dependency introduced by importing `module_a` into `module_b`.")
            ii. If a potential circular dependency is identified, this **MUST** be flagged as a critical issue, and the plan **MUST** be revised to avoid it before proceeding.

    4.  **Assess Data Representation & Core Model Impact:**
        *   **Action:**
            i.  When changes affect data representation (e.g., changing an ID from an integer to a UUID, altering a date format, modifying units) or impact core data models/entities:
            ii. Explicitly consider the potential impacts across all layers and components that might produce, consume, or transmit this data. This includes (but is not limited to): ORM layers, database schemas, mappers/serializers, data exporters/importers, API contracts, front-end consumers, and tests.
        *   **Report:**
            i.  List the components/layers considered.
            ii. Summarize potential impacts identified and how the plan addresses them (e.g., "Data Impact: Change to `user_id` format from int to string. Considered ORM, API serialization, and `reporting_module`. Plan includes updating API schema and `reporting_module` data parsing.").

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
    1.  **Perform Core Diff Verification:** **MUST** execute `Procedure: Verify Diff` (Section 4) on the *actual diff applied by `edit_file`*. The 'intent' for this verification is the *final intended proposal from Step 4.2 (specifically, the AI's `code_edit` string, representing its planned transformation, after it was internally verified using `Procedure: Verify Diff` in Step 4.2.1.b)* (incorporating any handled deviations from the pre-apply check).
    2.  **Discrepancy Handling:** If the overall outcome of `Procedure: Verify Diff` (Step 1) is not 'Verified' (or 'Verified with handled deviations') and cannot be justified/corrected, **trigger self-correction (Step 4.4.3)**.

**`Procedure: Verify Tool Output`**
*   **Purpose:** To verify that the output of an information-gathering tool (e.g., `read_file`, `grep_search`) is congruent with the request and sufficient for the current task.
*   **Trigger:** Called by Step 3.0.3 immediately after an information-gathering tool call.
*   **Steps:**
    1.  **Verify Congruence:** Confirm the tool's output aligns with the explicit request parameters. Examples:
        *   `read_file`: Was the requested line range or full file content returned as expected?
        *   `grep_search`/`codebase_search`: Do the results appear complete, or is there indication of truncation (e.g., "results capped at 50 matches") that might hide relevant information?
        *   `list_dir`: Was the directory listing provided for the correct path?
    2.  **Assess Sufficiency & Clarity:** Evaluate if the *content* of the output is adequate and unambiguous for the *specific planning task or hypothesis verification at hand*.
    3.  **Re-evaluate Prior Planning with New/Sufficient Information:** **If new and more complete/sufficient information for a key file/component becomes available (e.g., after a `BLOCKER` resolution or a more successful tool call) *after* some initial planning sub-steps (e.g., 3.1 through 3.9, especially complexity assessments like 3.4.0.b) were provisionally addressed based on previously incomplete/insufficient data, the AI MUST explicitly re-evaluate any of those prior sub-steps whose conclusions would be materially impacted by the new, complete information. This re-evaluation MUST occur before proceeding as if the prior conclusions are still fully valid and before reaching the Pre-computation Verification Summary (Step 3.10).**
    4.  **Handle Discrepancies/Insufficiencies:**
        *   If output is incongruent with the request (e.g., wrong lines returned, unexpected truncation not acceptable for the task) or if the content is insufficient/ambiguous for the immediate purpose:
            *   **MUST** explicitly state the issue.
            *   **MUST** take corrective action *before* proceeding to use this data. Corrective actions may include:
                *   Re-running the tool with adjusted parameters (e.g., more specific query, different line numbers).
                *   Using an alternative tool to gather the necessary information.
                *   If ambiguity persists, requesting clarification from the user.
            *   If proceeding with potentially incomplete data is unavoidable and deemed acceptable after consideration, this **MUST** be explicitly stated along with potential risks.
    5.  **Report Outcome:** Concisely state the outcome of this verification (e.g., "`Procedure: Verify Tool Output` (`read_file` for `xyz.py`): Verified, content sufficient.", "`Procedure: Verify Tool Output` (`grep_search` for ''MyClass''): Verified, sufficient after re-run with `include_pattern='*.py'`.", "`Procedure: Verify Tool Output` (`list_dir` for `src/utils`): Verified, sufficient.").
*   **Next Step:** Only after successful verification (or explicit acknowledgment of proceeding with limitations), use the tool's output for subsequent planning sub-steps (e.g., 3.1, 3.2, etc.).

**`Procedure: Perform Self-Assessment`**
*   **Purpose:** To perform a structured self-check ensuring all mandatory steps, checks, verifications, and reporting requirements for a specific phase or the overall cycle have been completed, with critical focus on BLOCKER adherence.
*   **Trigger:** Called at the end of Step 3 (Planning), end of Step 4 (after each file edit cycle), and end of Step 5 (Overall task cycle).
*   **Input Parameter:** `scope` (String: e.g., "Planning Phase", "Edit Cycle for [filename]", "Overall Task Cycle").
*   **Steps:**
    1.  **Scope Identification:** State the scope of the self-assessment being performed (using the `scope` parameter).
    2.  **Mandatory Step Completion Check:** Review the execution history for the specified `scope`. Verify that all mandatory steps, required sub-steps, checks, verifications, and reporting requirements (including summaries like Pre-computation or Post-Action Verification Summaries relevant to the scope) outlined in this document for that `scope` have been completed.
    2.a. **Context Sufficiency Review (if scope includes Planning Phase):** If the assessment `scope` is "Planning Phase" or "Overall Task Cycle" (which includes Planning), explicitly re-evaluate the file context obtained during Step 3 (`Procedure: Ensure Sufficient File Context`). **MUST** confirm whether the level of context obtained (full or partial, based on the execution of that procedure) was demonstrably adequate for the complexity and nature of the planned changes according to the guidelines in `Procedure: Ensure Sufficient File Context`. If proceeding with partial context (even if explicitly approved via Step 3.d of the procedure), briefly reaffirm the justification and that associated risks were considered during planning. If, upon reflection, the context now seems insufficient (e.g., the plan's complexity implies a higher risk from partial context than initially assessed), report this and suggest re-entering Step 3 to gather more context before proceeding.
    3.  **Critical Blocker Adherence Review:** **MUST** specifically review the execution history within the specified `scope` to verify that all `BLOCKER:` conditions encountered (especially within `Procedure: Ensure Sufficient File Context` Step 3.c.iii / 3.d, `Procedure: Handle Architectural Decisions`, `Procedure: Handle Necessary Workaround`, `Procedure: Consult on Ambiguous Missing Dependency`, `Procedure: Request Manual Edit`) were strictly adhered to.
        *   **If a deviation is found (e.g., a `BLOCKER` was bypassed without the required explicit user confirmation/guidance):**
            *   **MUST** immediately report this as a critical process deviation: "`Procedure: Perform Self-Assessment ([scope]): CRITICAL DEVIATION FOUND: Blocker [Specify Blocker, e.g., ''Incomplete Data for file X'', ''Architectural Decision Y'] was improperly bypassed. [Briefly explain deviation]. Halting for guidance.`"
            *   **STOP** processing and await user guidance. (**BLOCKER:**)
        *   **If adherence confirmed:** Report: "`Procedure: Perform Self-Assessment ([scope]): Critical Blocker Adherence Review: Confirmed.`"
    4.  **Handle Missed Steps:** If any mandatory steps (beyond blockers checked in Step 3) relevant to the `scope` were missed:
        *   **MUST** report the specific missed step(s).
        *   **MUST** trigger self-correction (e.g., return to execute the missed step, potentially restarting the relevant phase if necessary, similar logic to Step 4.4.3). Explain the corrective action being taken. **STOP** proceeding until the oversight is addressed.
    5.  **Confirmation Statement:** After confirming completion and adherence (including Steps 3 & 4), **MUST** state: "**Procedure: Perform Self-Assessment ([scope]):** Complete. All mandatory steps confirmed executed for this scope."

---

## Exception Handling Procedures

*(This section details procedures for handling specific issues identified during the workflow. Execution of these procedures typically means **STOPPING** the standard workflow path until the issue is resolved or explicit guidance/approval is received.)*

**`Procedure: Handle Unclear Root Cause / Missing Info`**
*   **Trigger:** Execution is triggered by Step 3.8 when analysis reveals unclear root causes, missing essential information, standard conflicts that block planning, failing standard debugging methods, or specific failed dependency verifications (see `Procedure: Handle Failed Verification for Existing Dependency`).
1.  **STOP** proposing direct fix.
2.  **State Issue & Prioritize Investigation:** Clearly state the blocker (unclear cause, missing info, standard conflict, failing debug method).
3.  **Formulate Investigation Plan:** Outline specific steps to investigate (e.g., "Plan: 1. Examine logger setup. 2. Check system exception hook.").
4.  **Seek Confirmation (if needed):** Confirm broad/assumption-based investigation plans. *Example: "Traceback suppressed. Plan: [steps]. Proceed?"*
   **If confirmation is sought, STOP processing and await explicit user confirmation before executing the investigation plan.** (**BLOCKER:**)
5.  **Handle Necessary Workarounds (Use Sparingly):** Follow `Procedure: Handle Necessary Workaround` ONLY if investigation is blocked/impractical AND workaround is only viable path *now*. (See cross-reference below).
6.  **Consult on Ambiguous Missing Dependencies:** Follow `Procedure: Consult on Ambiguous Missing Dependency` ONLY if resolving a dependency error requires creating significant new structures based *only* on potentially old references, with no strong corroborating context.

**`Procedure: Handle Architectural Decisions`**
*   **Purpose:** To establish a clear protocol when a proposed change or user request involves or implies a significant architectural decision, modification to core design principles, or a choice with broad, non-obvious implications for the codebase.
*   **Core Principle:** The AI **MUST NOT** unilaterally make significant architectural decisions. It **MUST** identify such situations, present options/analysis to the user, and await explicit user guidance.
*   **Trigger:** This procedure **MUST** be invoked if any of the following conditions are met during Step 3 (Planning Phase), especially during `3.7.1.a (Analyze Overall Impact)` or `3.7.1.d (Identify Architectural Implications)`:
    *   A user request directly asks for a change that would alter fundamental structural aspects of the application (e.g., "Change the application from monolithic to microservices," "Replace the current database system," "Introduce a global state management system where none existed").
    *   A planned code modification, while seemingly localized, would necessitate changes to core abstractions, widely used interfaces, or foundational modules in a way that deviates from established patterns.
    *   Implementing a feature requires choosing between multiple design patterns or architectural approaches where the choice has significant trade-offs (e.g., performance vs. maintainability, coupling vs. complexity) and no clear precedent exists in the relevant codebase area.
    *   The AI identifies that satisfying a request as-is would violate established architectural principles observed in the codebase (e.g., layering violations, breaking encapsulation of core modules).
    *   The change impacts security, data privacy, or compliance in a non-trivial way that requires a design choice.

*   **Steps:**

    1.  **Identify and Flag:**
        *   Clearly state: "This request/planned change appears to involve an architectural decision regarding [briefly describe the area, e.g., 'data persistence strategy,' 'component communication model']."
        *   Reference the specific part of the request or plan that triggers this.

    2.  **Preliminary Analysis (AI Autonomous):**
        *   **Objective:** To provide the user with sufficient context to make an informed decision.
        *   **Actions:**
            *   Briefly describe the current architecture relevant to the decision.
            *   If multiple implementation options are apparent, outline at least two (if feasible), very briefly listing their potential pros and cons in the context of the current project. (e.g., "Option A: [Description], Pros: [Scalability], Cons: [Initial complexity]. Option B: [Description], Pros: [Simplicity], Cons: [Less flexible for future X].")
            *   Identify any immediate, obvious risks or significant impacts associated with the potential architectural change(s).
            *   Reference any existing architectural documentation or established patterns in the codebase that seem relevant (either supporting or conflicting).
        *   **Constraint:** This analysis should be concise and aimed at framing the decision, not at exhaustive research without user consent.

    3.  **Halt and Present to User:**
        *   **Action:** **MUST** halt further detailed planning or code generation related to the architecturally significant part of the task.
        *   **Report:** Present the findings from Step 2 (Preliminary Analysis).
        *   **Request:** Explicitly ask the user for guidance:
            *   "Please advise on how to proceed. Which architectural approach should be pursued?"
            *   "Do you require a more detailed comparison of options?"
            *   "Are there other architectural considerations I should be aware of for this task?"

    4.  **Await User Guidance:**
        *   Do not proceed with implementation of the architecturally sensitive parts until the user provides explicit direction.
        *   The user might:
            *   Approve an option.
            *   Ask for more detailed analysis of specific options.
            *   Provide a different architectural direction.
            *   Defer the decision or de-scope the feature.

    5.  **Incorporate User Decision:**
        *   Once guidance is received, explicitly state the chosen architectural direction in the plan. (e.g., "User has directed to proceed with Option A: [Description].")
        *   Continue with Step 3 (Planning) or Step 4 (Edit Generation) based on the user's decision, ensuring the plan and subsequent edits align with the confirmed architecture.
        *   If the user's decision significantly alters the plan, reiterate the updated plan for confirmation.

    6.  **Documentation (If Applicable and AI-Modifiable):**
        *   If the architectural decision warrants it and the AI has the capability/permission to modify project documentation (e.g., READMEs, design docs in markdown), ask the user if a brief note about the decision should be added. If yes, and feasible, include this in the edit plan.

*   **Reporting:** The AI's communication during this procedure (identification, analysis, request for guidance, and incorporation of decision) serves as the primary report.

**`Procedure: Handle Necessary Workaround`**
*   **Purpose:** To define a strict protocol for proposing, approving, and implementing a temporary workaround when a direct fix for an issue is not immediately feasible due to blocked root cause investigation, unavoidable external constraints, or critical data integrity deviations where a fallback is the only immediate option.
*   **Core Principle:** Workarounds are exceptions and **MUST** be treated with extreme caution. They should only be implemented when the benefits clearly outweigh the risks and no standard solution is presently viable. The AI **MUST NOT** unilaterally implement workarounds.
*   **Trigger:** This procedure **MUST** be invoked if:
    *   Step 3.7 (Data Integrity Priority) identifies an unexpected data issue, and implementing a fallback/default (which is a workaround) is considered.
    *   `Procedure: Handle Unclear Root Cause / Missing Info` (Step 5) concludes that root cause investigation is currently blocked or impractical, and a workaround is the only way to proceed with a critical part of the user's task.
    *   External system limitations (e.g., a known bug in a library for which no immediate fix exists) prevent a standard solution.

*   **Steps:**

    1.  **STOP All Other Implementation Planning/Fixing:** Halt any ongoing attempts to implement a direct fix or proceed with standard planning for the affected component.

    2.  **Identify and State the Need for a Workaround:**
        *   Clearly state: "A direct fix for [describe the issue, e.g., 'the data validation error for X,' 'the inability to process Y due to Z'] is currently not feasible due to [explain the blocker, e.g., 'blocked root cause investigation,' 'missing data source,' 'external library constraint']."
        *   Propose: "A temporary workaround is being considered to [state the goal of the workaround, e.g., 'allow processing to continue by using a default value,' 'bypass the problematic component to achieve partial functionality']."

    3.  **Detailed Workaround Proposal & Justification:**
        *   **Action:** Formulate a specific plan for the workaround.
        *   **Content (MUST include all points):**
            *   **Specific Actions:** Detail the exact changes the workaround involves (e.g., "Modify function `abc` to return `DEFAULT_VALUE` if `input_q` is null," "Temporarily disable module `problem_module` and reroute calls to `fallback_module`").
            *   **Scope:** Define how localized or widespread the workaround will be.
            *   **Duration:** State if the workaround is intended to be very short-term (e.g., for this session only) or if it might persist longer.
            *   **Risks & Downsides:** **CRITICALLY**, enumerate all potential negative consequences (e.g., "Risk: This will mask underlying data quality issues for field X," "Downside: Feature Y will be unavailable while this workaround is active," "Risk: Performance may be degraded by Z%").
            *   **Impact on Standards:** Explain how the workaround deviates from `PROJECT_STANDARDS.MD` or `PROJECT_ARCHITECTURE.MD`.
            *   **Alternative (Why Not Viable):** Briefly explain why a standard solution or more direct fix is not currently an option.
            *   **Future Removal Plan (If Applicable):** Suggest how or when this workaround might be removed or replaced by a proper fix (e.g., "This should be revisited once the root cause investigation for Z is complete").

    4.  **BLOCKER: Request Explicit User Approval for Workaround:**
        *   **Report:** Present the full workaround proposal and justification from Step 3.
        *   **State:** "Implementing this workaround for `[describe issue]` requires your explicit, risk-acknowledged approval. The key risks are: [Summarize key risks from Step 3.Content.Risks]."
        *   **MUST NOT** proceed with implementing the workaround until the user:
            a.  Explicitly confirms they understand and accept the stated risks AND approve the workaround.
            b.  Provides an alternative solution.
            c.  Instructs to abandon this path.
        *   (Note: A generic "continue" or "proceed" without specific acknowledgment of the stated risks for the workaround is NOT sufficient to override this blocker.)

    5.  **Implement Workaround (If Approved):**
        *   **Action:** If the user provides explicit, risk-acknowledged approval:
            i.  State: "User has approved the workaround for `[describe issue]`, acknowledging risks: [Summarize risks]."
            ii. Implement the workaround by proceeding through a standard Step 3 (Planning for the workaround edit), Step 4 (Edit Generation & Verification for the workaround), and Step 5 (Adherence Checkpoint for the workaround).
            iii.The edit **MUST** include comments clearly marking the start and end of the workaround code, explaining its purpose and temporary nature (e.g., `# START WORKAROUND: [Issue XYZ] - See [Ticket/Issue ID if available]`, `# END WORKAROUND: [Issue XYZ]`).

    6.  **Documentation of Workaround:**
        *   **Within Code:** Ensured by comments in Step 5.b.iii.
        *   **In Reporting:** The AI's proposal, justification, risk assessment, and the user's explicit approval serve as the primary documentation within the interaction log.
        *   **External (If Applicable):** If the workaround is significant or expected to persist, ask the user: "Should a brief note about this temporary workaround for `[issue]` and its approval be added to any project documentation (e.g., a known issues list, relevant module README)?" If yes, and feasible, include this in the plan.

*   **Reporting:** The AI's communication during this procedure (identification, proposal, risk assessment, request for approval, and confirmation of approval/implementation) serves as the primary report.

**`Procedure: Consult on Ambiguous Missing Dependency`**
*   **Purpose:** To establish a clear protocol when a referenced dependency (e.g., a class, module, or significant data structure) is missing, and the only available information about its potential structure comes from potentially outdated or ambiguous references, requiring user consultation before attempting to generate or scaffold it.
*   **Core Principle:** The AI **MUST NOT** create significant new code structures (e.g., entire classes, complex modules) based solely on ambiguous or potentially outdated references without explicit user guidance and confirmation of the intended structure or approach.
*   **Trigger:** This procedure **MUST** be invoked if:
    *   `Procedure: Handle Unclear Root Cause / Missing Info` (Step 6) determines that a missing dependency is the issue.
    *   The only references to the missing dependency's expected structure are from its usage in other code (which might be old or based on a previous version).
    *   There is no clear, authoritative source (like current documentation, or a nearby well-defined interface) to define the missing dependency's structure.
    *   Creating the dependency would involve more than a trivial placeholder (e.g., it implies methods, attributes, or a specific internal logic).

*   **Steps:**

    1.  **STOP All Implementation Planning:** Halt any attempts to directly implement the missing dependency or the code that relies on it.

    2.  **Gather and Present Context:**
        *   **Action:** Collect all available information about the missing dependency.
        *   **Report to User (MUST include all points):**
            *   **Missing Dependency:** Clearly identify the name and type (e.g., class, module, function) of the missing dependency (e.g., "Missing class `OldUtilityHelper` referenced in `main_processor.py`").
            *   **Referencing Locations:** List the specific file(s) and line(s) where it is referenced.
            *   **Inferred Structure (from usage):** Based on how it's used (e.g., method calls, attribute access), describe the *inferred* structure. *Example: "From its usage, `OldUtilityHelper` appears to require a method `process_item(data)` and an attribute `config_value`."*
            *   **Ambiguity/Uncertainty:** Clearly state the uncertainties. *Example: "The exact return type of `process_item(data)` is unclear from context," or "It is unknown if `OldUtilityHelper` has other essential methods or an initialization routine."
            *   **Potential Age of Reference:** If possible, indicate if the referencing code seems old or part of a legacy system.

    3.  **Outline Potential Approaches & Implications (for User Consideration):**
        *   **Option 1: Scaffold Based on Inferred Structure:**
            *   Describe: "One option is to attempt to create a basic scaffold for `[MissingDependencyName]` with the inferred methods/attributes: [list them]."
            *   Implications: "This would allow `[referencing_code]` to potentially compile/run, but the scaffolded `[MissingDependencyName]` would have placeholder logic and might not be functionally correct or complete. There's a risk of misinterpreting the original design."
        *   **Option 2: Alternative Solution (If Apparent):**
            *   If an alternative exists (e.g., using a different, existing library; refactoring the referencing code to not need the dependency), briefly describe it.
            *   Implications: Describe pros/cons of this alternative.
        *   **Option 3: Defer/Seek More Information:**
            *   Describe: "Another option is to defer this part of the task until more information about `[MissingDependencyName]` can be found, or to remove/comment out the code that relies on it if it's deemed non-critical."
            *   Implications: Impact on current task completion.

    4.  **BLOCKER: Request Explicit User Guidance:**
        *   **State:** "Resolving the missing dependency `[MissingDependencyName]` requires your guidance due to the ambiguities in its structure. Please review the inferred structure and consider the outlined options."
        *   **Ask:**
            *   "Should I attempt to scaffold `[MissingDependencyName]` based on the inferred structure (Option 1)? If so, are there any known details about its methods or attributes I should include?"
            *   "Is there an alternative solution you prefer (Option 2)?"
            *   "Should this be deferred, or should the dependent code be modified/removed (Option 3)?"
        *   **MUST NOT** proceed with any option until the user provides explicit direction.

    5.  **Incorporate User Decision:**
        *   **Action:** Once the user provides guidance (e.g., approves scaffolding with specific details, chooses an alternative, instructs to defer):
            i.  State: "User has directed to [summarize user's decision, e.g., 'scaffold `OldUtilityHelper` with method `process_item` returning a boolean', 'defer fixing `main_processor.py`']."
            ii. If the decision involves code changes (e.g., scaffolding, implementing an alternative): Proceed with a new Step 3 (Planning) for those changes, incorporating the user's specific instructions.
            iii.If the decision is to defer or remove, update the overall task plan accordingly.

*   **Reporting:** The AI's communication during this procedure (presenting context, outlining options, requesting guidance, and confirming user decision) serves as the primary report.

**`Procedure: Handle Failed Verification for Existing Dependency`**
*   **Purpose:** To define a systematic approach when a verification attempt (via `Procedure: Verify Hypothesis` during Step 3.7.1.b) for a dependency reference found in *existing, established code* fails (e.g., the module path or symbol is not found where expected).
*   **Core Principle:** A failed verification for an existing dependency signals a potential inconsistency or error in the current codebase that needs careful investigation before proceeding with plans that rely on or interact with that dependency.
*   **Trigger:** This procedure **MUST** be invoked if Step 3.7.1.b (`Verify ALL Stated Assumptions`) reports a "Failed" verification outcome for a dependency that is part of *existing, unchanged code* (i.e., not a newly planned addition or modification by the AI).

*   **Steps:**

    1.  **DO NOT Immediately Plan Creation of the Missing Element:** The primary assumption is that the existing code was once valid; therefore, the failure likely indicates a change, misconfiguration, or misunderstanding, not necessarily a need to create something new from scratch without investigation.

    2.  **State the Discrepancy Clearly:**
        *   Report: "`Procedure: Handle Failed Verification for Existing Dependency` triggered. Verification of existing dependency `[DependencyName/Path]` referenced in `[FileContainingReference]` at line `[LineNumber]` failed. Reason: `[Specific failure reason from Verify Hypothesis, e.g., 'Module path not found', 'Symbol X not found in module Y']`."

    3.  **Perform Usage Check in Referencing File:**
        *   **Objective:** To determine if the specific missing symbol (if applicable) is actually used within the file that contains the problematic dependency reference.
        *   **Action:**
            *   If the failure was a missing symbol within an otherwise found module: **MUST** use tools (`grep_search` or direct analysis of `read_file` output) to search *within the referencing file* for actual usage of the specific symbol(s) (e.g., instantiation, method calls, type annotations).
            *   If the failure was a missing module path, this step might focus on how many distinct symbols are attempted to be imported from that path in the file.
        *   **Report:** Explicitly state the tool(s) used, search term(s), and findings. *Example: "Usage Check in `file_x.py`: Searched for `MissingSymbol(...)` using `grep_search`. Outcome: No usage found." or "Outcome: Usage found in function `Y` (call to `MissingSymbol.method_a()`)."*

    4.  **Perform Broader Context Search (If Necessary):**
        *   **Objective:** To find any information about the missing dependency's history or intended state (e.g., was it moved, renamed, refactored, intentionally deprecated, or are there TODOs for its removal?).
        *   **Action:** Especially if usage *is* found (Step 3) or if the missing element is a module: Execute additional searches (`grep_search` for related terms/files, `codebase_search` for conceptual links, check commit logs if accessible and relevant) across the codebase.
        *   **Report:** Summarize findings from the broader search. *Example: "Broad Context Search: `grep_search` for `OldModuleName` found references in `docs/changelog.md` indicating it was refactored to `NewModuleName` in v2.1." or "No additional context found regarding `MissingSymbol`."

    5.  **Determine Next Action Based on Investigation:**
        *   **Path A: Simple Fix Apparent (e.g., Renamed/Moved):**
            *   **Condition:** If findings from Step 3 and 4 clearly indicate a simple, direct fix (e.g., the module was renamed and the new name is identified, a symbol was moved to a known different module).
            *   **Action:** Formulate a plan to correct the dependency reference (e.g., update import statement). This becomes the current plan. Proceed with Step 3 planning for this corrective edit.
            *   **Report:** "Conclusion: Dependency `[OldName]` appears to have been renamed to `[NewName]`. Planning to update the import statement in `[FileContainingReference]`."
        *   **Path B: Dependency Unused and Safe to Remove:**
            *   **Condition:** If the Usage Check (Step 3) found **NO actual usage** of the specific symbol(s) from the failed dependency reference *within the referencing file*, **AND** the Broader Context Search (Step 4) does not suggest it's critical or used indirectly in a way not caught by the direct usage check.
            *   **Action:** The default plan should be to **remove the stale dependency reference statement** (e.g., the unused import).
            *   **Report:** "Conclusion: `[MissingSymbol]` from `[ModulePath]` is not used in `[FileContainingReference]`. Planning to remove the import statement."
            *   **Verification for Removal:** The plan to remove **MUST** include a brief verification step (e.g., a quick `grep_search` to ensure this specific import isn't unexpectedly critical for a build tool or a very indirect mechanism, though this is rare for unused in-file symbols).
        *   **Path C: Unclear or Complex Issue (Requires Deeper Analysis):**
            *   **Condition:** If usage *was* found (Step 3) but the reason for the failure isn't a simple fix (Path A), OR if context is insufficient/ambiguous, OR if the dependency seems intentionally removed but usage persists.
            *   **Action:** **MUST** execute `Procedure: Handle Unclear Root Cause / Missing Info` (Section 5).
            *   **Report:** "Conclusion: Usage of `[MissingSymbol/Module]` was found in `[FileContainingReference]`, but the reason for its unavailability is unclear or complex. Escalating to `Procedure: Handle Unclear Root Cause / Missing Info`." Provide all findings from this current procedure as input to the next.

*   **Reporting:** The AI's communication during this procedure (stating the discrepancy, detailing search actions and findings, and concluding with the determined next action path) serves as the primary report.

**`Procedure: Handle Deviation`**
*   **Purpose:** To define a systematic process for analyzing and addressing any unplanned or unexpected lines ("Deviations") that appear in a `code_edit` diff (either a proposed diff during pre-application verification or an actually applied diff during post-application verification) when compared against the AI's specific, documented intent for that edit.
*   **Core Principle:** All changes to the codebase must be intentional and verified. Deviations from the plan require explicit analysis and justification before being accepted or require correction of the `code_edit`.
*   **Trigger:** This procedure is called by `Procedure: Verify Diff` (Step 4 of that procedure) whenever deviations (unplanned additions, deletions, or modifications) are identified between the `diff` being reviewed and the specific `intent` for that edit.

*   **Steps (MUST be performed for EACH identified Deviation):**

    1.  **Isolate and Identify the Deviation:**
        *   Clearly state the specific line(s) in the `diff` that constitute the deviation from the intended plan. (e.g., "Deviation identified: Line 42 `new_variable = unexpected_value;` was added but not planned.", "Deviation identified: Planned deletion of line 50 did not occur in the diff.")

    2.  **Fact-Check the Deviation (Mandatory Tool Use):**
        *   **Objective:** To determine if the deviation reflects an actual state of the code or a tool artifact, and to understand its immediate context.
        *   **Action:** Use appropriate tools (`read_file` for context, `grep_search` for related symbols if the deviation introduces new ones or unexpectedly modifies existing ones) to investigate the code surrounding the deviation in the *current version of the file*.
        *   **Report:** Briefly state the tools used and the findings. *Example: "Fact-check for unexpected addition on line 42: `read_file` confirms `new_variable` is not in the current version of the file before this diff. `grep_search` for `unexpected_value` found no other uses."

    3.  **Analyze Potential Cause and Impact of the Deviation:**
        *   Hypothesize the cause (e.g., AI oversight in `code_edit` generation, tool misinterpretation of context, an attempt by the AI to be 'helpful' but outside of plan).
        *   Assess the potential impact of the deviation: Is it beneficial, benign, or harmful? Does it introduce new logic, change existing behavior, or is it purely cosmetic? Does it align with project standards?

    4.  **Decision and Justification (For EACH Deviation):**
        *   **Path A: Accept Deviation (Requires Strong Justification):**
            *   **Condition:** Only if the deviation is fact-checked (Step 2), analyzed (Step 3), and deemed demonstrably beneficial, verifiably correct, aligns with project standards, AND does not introduce significant unintended side effects.
            *   **Action:**
                i.  Provide explicit justification for accepting it. *Example: "Justification for accepting added line 42: While unplanned, `new_variable = default_config.TIMEOUT;` correctly initializes a variable that would be needed by the subsequent planned logic and uses the correct project configuration source."
                ii. **CRITICAL:** The AI's internal working plan for the overall task **MUST** be immediately and explicitly updated to reflect this now-accepted change. The `code_edit` (if still in its proposed state) **MUST** also be updated to formally include this deviation, making it part of the new, verified intent.
            *   **Report:** "Deviation (e.g., added line 42) accepted. Justification: [brief summary]. Plan and proposed `code_edit` (if applicable) updated."
        *   **Path B: Reject Deviation (Default for Unjustified/Harmful Deviations):**
            *   **Condition:** If the deviation is not beneficial, introduces errors, violates standards, or its impact is unclear/negative, or if it was an unintended deletion/modification that needs to be restored/corrected.
            *   **Action:**
                i.  State reason for rejection. *Example: "Rejecting unplanned deletion of logger call on line 75; essential for debugging."
                ii. **CRITICAL:** If verifying a *proposed* `code_edit` (Step 4.2.1.b), this proposed `code_edit` **MUST** be revised to remove the deviation and accurately reflect the original (or a newly clarified) intent. The AI then re-evaluates the *revised* `code_edit`.
                iii.If verifying an *applied* `diff` (Step 4.4.1) that contains an unacceptable deviation, this signals a failed edit application. The AI **MUST** trigger self-correction (Step 4.4.3) to revert the unacceptable deviation and achieve the correct state.
            *   **Report:** "Deviation (e.g., unplanned deletion on line 75) rejected. Reason: [brief summary]. Action: [e.g., Revising proposed `code_edit` / Triggering self-correction for applied diff]."

    5.  **Loop or Conclude for This Deviation:** After processing one deviation (accepted or rejected), if other deviations were identified in the same `diff`, return to Step 1 for the next deviation. If all deviations for the current `diff` have been processed, this procedure is complete for this call.

*   **Reporting:** The AI's communication during this procedure (identifying each deviation, reporting fact-checking, stating justification for acceptance/rejection, and confirming plan/`code_edit` updates or corrective actions) serves as the primary report for each deviation handled.

**`Procedure: Request Manual Edit`**
*   **Purpose:** To provide a structured way for the AI to request user intervention when it cannot safely or reliably perform a file modification, especially after multiple `edit_file` / `reapply` failures, when dealing with highly complex/sensitive changes beyond its automated capabilities, or when an automated edit has produced unacceptable side-effects that cannot be cleanly self-corrected.
*   **Core Principle:** When automated edits are repeatedly unsuccessful or introduce significant errors, deferring to manual user application is safer and more efficient than engaging in prolonged, unpredictable self-correction loops.
*   **Trigger:** This procedure **MUST** be invoked if any of the following conditions are met:
    *   Step 4.4.3.b.e.i: Persistent Correction Failure. Repeated attempts (e.g., >3 for the same logical change, or multiple `edit_file`/`reapply` calls for a single planned change fail to achieve a verified outcome without significant unintended side-effects, and further autonomous attempts are deemed unlikely to succeed). This includes persistent "no changes made" reports when changes are intended and verified as absent (as per Step 4.4.3.c.g).
    *   Step 4.4.3.b.e.ii: Grossly Disproportionate or Corrupting Unintended Modifications by `edit_file`/`reapply` that cannot be cleanly self-corrected by a subsequent targeted edit.
    *   Step 3.1.B.1.e: If a corrective plan (e.g., for refactoring to unblock an edit) is declined by the user or itself fails during execution.
    *   Step 3.1.B.2.b: If no viable corrective strategy is identified for a previously blocked edit, or if a corrective strategy already failed.

*   **Advisory for "Unacceptable Original Edit Application with Failed Cleanup" Trigger (from Step 4.4.3.b.e.ii):**
    *   When this procedure is invoked because an automated edit achieved the primary goal but introduced unacceptable side-effects (churn, new errors in unrelated code) which a subsequent focused cleanup attempt also failed to resolve:
        *   The explanation in "State Tool Failure and Context" (Step 2 below) **MUST** clearly differentiate between the successful primary change (if any part was successful) and the failed cleanup of the unacceptable side-effects.
        *   The primary goal of "Provide Specific Edit Details for Manual Application" (Step 3 below) **MUST** be to provide the user with the **original, minimal, and correct planned change** that was initially intended, presented cleanly with its necessary context, as if the side-effects had not occurred. This allows the user to apply the core intended change cleanly.

*   **Steps:**

    1.  **STOP All Automated Tool Attempts for This Specific Edit:** Cease further `edit_file` or `reapply` calls for the problematic planned modification in the target file.

    2.  **State Tool Failure and Context:**
        *   Clearly explain the issue that led to this manual edit request (e.g., persistent "no changes made" errors, repeated misapplication of diffs by tools, introduction of unacceptable side effects).
        *   Summarize the history of automated attempts and strategies employed (e.g., "Initial `edit_file` failed. `reapply` also failed. Attempted granular `edit_file` which also reported 'no changes made'.").
        *   Describe the presumed incorrect or problematic state of the file based on the last tool output or verification step.

    3.  **Provide Specific Edit Details for Manual Application:**
        *   **MUST** clearly specify the target file (e.g., "Target File: `src/modules/utils.py`").
        *   **MUST** clearly state the action: "Action: [Insertion / Replacement / Deletion]".
        *   **If an insertion or replacement:**
            *   Provide the precise, complete code block to be inserted or to replace existing code.
            *   This block **MUST** represent the final intended state of the section.
            *   Include sufficient surrounding, unchanged context lines (typically 2-3 before and after the change area) to uniquely and unambiguously identify the location.
            *   Use the language's comment style for `// ... existing code ...` to represent unchanged spans outside the immediate context lines.
            *   Present this as a single, clearly demarcated code block. *Example (Python insertion/replacement):*
                "To manually apply the fix to `src/example.py`, please ensure the relevant section matches the following (insert at the indicated location or use this to replace the old block):"
                ```python
                # ... existing code ...
                # Context line immediately before the change
                def new_function_or_updated_block():
                    # New or corrected logic here
                    pass # Ensure complete, valid syntax
                # Context line immediately after the change
                # ... existing code ...
                ```
        *   **If a deletion:**
            *   Clearly describe or show the exact lines to be deleted.
            *   Include sufficient surrounding, unchanged context lines (before and after) to uniquely identify the lines for deletion.
            *   *Example (Python deletion):*
                "To manually apply the fix to `src/example.py`, please delete the following highlighted lines (between the context lines):"
                ```python
                # ... existing code ...
                # Context line immediately before the deletion target
                line_to_be_deleted_1
                another_line_to_be_deleted_2
                # Context line immediately after the deletion target
                # ... existing code ...
                ```
        *   **Crucial Concluding Statement:** After providing all details, the AI **MUST** explicitly state: "Please apply this manual edit to `[target_file]` and let me know once you have completed it so I can verify."

    4.  **BLOCKER: Await User Confirmation of Manual Edit:**
        *   The AI **MUST NOT** proceed with any further actions related to this file or dependent tasks until the user explicitly confirms they have applied the manual edit as requested.

    5.  **Verify Manual Edit Application (Mandatory after User Confirmation):**
        *   **Trigger:** After the user confirms they have completed the manual edit.
        *   **Action:**
            a.  **Acknowledge User Action:** Thank the user for applying the edit.
            b.  **Re-Read Modified File Content:** The AI **MUST** immediately use `read_file` to get the current content of the manually edited file. Aim for a scope that covers the edited section and its immediate surroundings. If `should_read_entire_file=True` is permissible and necessary for reliable verification (e.g., if the change was extensive or had potential widespread impact), it should be used.
            c.  **Verify Against Intended State:** The AI **MUST** then meticulously compare the re-read content against the *specific intended state* that the manual edit was supposed to achieve (as described in Step 3 of this procedure). This verification **MUST** confirm:
                *   That the specified lines were correctly inserted, replaced, or deleted.
                *   That the surrounding context lines (provided in Step 3) remain intact and correct.
                *   That no obvious unintended changes or syntax errors were introduced in the immediate vicinity of the edit.
            d.  **Report Verification Outcome:**
                *   **If Verified Successfully:** State clearly: "Verification successful. The manual edit to `[filename]` appears to be correctly applied, and the file content matches the intended state regarding this change. Resuming workflow."
                    *   The AI can then proceed, potentially re-running relevant parts of Step 4.4 (Post-Apply Verification, focusing on broader integration if needed) or Step 4.5 (Post-Action Verification Summary) and Step 5 (Adherence Checkpoint) for the overall task.
                *   **If Discrepancies Found:** State clearly: "Verification of manual edit for `[filename]` found discrepancies: [Describe discrepancies specifically, e.g., 'The intended deletion on line X seems to be still present,' or 'An unexpected modification was found at line Y within the provided context lines', 'The new function seems to be missing its closing parenthesis']. Could you please double-check the manual application against the details provided in Step 3?"
                    *   **BLOCKER (If Discrepancies Persist):** The AI **MUST NOT** proceed with the main task assuming the edit is correct. It should offer to provide the manual edit details (Step 3) again if helpful. If, after a second user confirmation of correction, verification still fails with significant discrepancies, the AI should state the persistent discrepancy and ask the user how they wish to proceed (e.g., user will investigate further, AI should abandon this specific change, etc.).

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

---

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
