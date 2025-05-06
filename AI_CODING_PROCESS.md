# AI Assistant - Standard Coding Process

**ATTENTION AI ASSISTANT: This document outlines the mandatory process you **MUST** follow during general coding interactions to ensure consistent adherence to the project's standards. This process requires adherence to `PROJECT_STANDARDS.md` (Core Principles & Checklist) and the detailed guidelines in the project's specific `PROJECT_ARCHITECTURE.md` (Patterns, Structures, Examples for the relevant language/framework). This process complements, but does not replace, the detailed standards themselves. You **MUST** explicitly report the performance and outcome of mandatory checks as specified below.**

**Goal:** To improve consistency and proactively catch deviations from standards by incorporating explicit checks **and reporting** into the workflow, ensuring a strong emphasis on fundamentally robust solutions over quick fixes or workarounds.
**Interaction Model:** This process assumes **autonomous execution** by the AI, with user intervention primarily reserved for explicit `**BLOCKER:**` points identified within the procedures. Therefore, meticulous self-verification and clear, proactive reporting as outlined below are paramount for demonstrating adherence.

**Version: 1.49** (Combat Superficial Execution)

---

## Table of Contents

1.  [Introduction & Goal](#ai-assistant---standard-coding-process)
2.  [Core Principles & Critical Checks Summary](#core-principles--critical-checks-summary)
3.  [General Coding Workflow](#general-coding-workflow)
    *   [Step 1: Confirm Standards Awareness](#1-confirm-standards-awareness-initial-step)
    *   [Step 2: Confirm Goal](#2-confirm-goal-recommended)
    *   [Step 3: Pre-computation Standards Check (Planning)](#3-pre-computation-standards-check-planning-phase)
    *   [Step 4: Edit Generation, Application, and Verification Cycle](#4-edit-generation--application--and-verification-cycle)
    *   [Step 5: Adherence Checkpoint](#5-adherence-checkpoint-final-step-in-cycle-critical)
4.  [Reusable Verification Procedures](#reusable-verification-procedures)
5.  [Exception Handling Procedures](#exception-handling-procedures)
6.  [Glossary of Key Terms](#glossary-of-key-terms)
7.  [References](#references)

---

## Core Principles & Critical Checks Summary

**Key Principle:** While this process provides structure, the primary takeaway is that **strict adherence to the existing verification and checking steps is the most crucial factor** in preventing errors and ensuring robust, maintainable code.
**Self-Driven Compliance Principle:** The AI is responsible for proactively initiating and completing **all** required steps and verifications outlined herein **without external prompting** (except when a `**BLOCKER:**` explicitly requires user input). The structured reporting and summaries mandated by this process serve as the primary **proof** of this self-driven compliance. Furthermore, to provide clear proof of step execution, the AI **MUST** prefix relevant sections of its responses with the corresponding step number and name (e.g., '**Step 3.2: Identify Standards & Verify Alignment**').

**Absolute Critical Checks (Non-Negotiable):**

1.  **Verify ALL Diff Lines:** Meticulously verify **every single line** in *both* proposed (`code_edit`) and applied (`edit_file`/`reapply`) diffs against the plan and verified facts. **Treat tool output with extreme skepticism.** See clarification in Step 4 warning.
2.  **Fact-Check Dependencies & Assumptions:** **MUST** factually verify the existence and correctness of **all** dependency references (imports, requires) and critical assumptions (API structures, interfaces) using tools **immediately** when planned (Step 3) and **again** after edits (Step 4). **Do not assume validity.** (See `Procedure: Verify Dependency Reference`, `Procedure: Verify Hypothesis`).
3.  **Address Root Cause:** Prioritize investigating and fixing the underlying cause of issues over applying superficial patches or workarounds. (See Step 3.4, Section 5: `Exception Handling Procedures`).
4.  **Preserve Logic:** When refactoring, meticulously document and ensure the preservation of essential original logic, explicitly justifying any intentional changes. (See `Procedure: Ensure Logic Preservation`).
5.  **Handle Blockers/Deviations Systematically:** Follow the defined procedures when encountering unclear root causes, architectural issues, necessary workarounds, or unexpected deviations in proposed/applied edits. (See Section 5: `Exception Handling Procedures`, Step 4.2.1.d).

---

## General Coding Workflow

When responding to user requests involving code analysis, planning, or modification, you **MUST** integrate the following steps **and explicitly report their execution and outcome in your responses**:

### 1. Confirm Standards Awareness (Initial Step)

*   **Trigger:** At the very beginning of handling any coding-related request.
*   **Action:** Explicitly state that you have read and understood the requirements outlined in `PROJECT_STANDARDS.md` and the project's specific `PROJECT_ARCHITECTURE.md`. *Example: "Acknowledged. I have reviewed and will adhere to the principles in `PROJECT_STANDARDS.md` and the detailed guidelines in `PROJECT_ARCHITECTURE.md`."*

### 2. Confirm Goal (Recommended)

*   **Trigger:** Before detailed planning, especially for complex or potentially ambiguous requests.
*   **Action:** Briefly restate your understanding of the user's ultimate goal for this change and ask for confirmation.
*   *Example: "To confirm my understanding, the goal is to refactor X to achieve Y, primarily by implementing pattern Z. Is this correct?"*

### 3. Pre-computation Standards Check (Planning Phase)

**NOTE:** Foundational checks (Steps 3.4, 3.5, 3.6) take precedence. If analysis reveals underlying issues (robustness, unknown root cause, architectural conflicts), these **MUST** be addressed by **STOPPING the standard plan and executing the appropriate procedure from Section 5: `Exception Handling Procedures`** *before* proceeding with the original task. Embrace necessary detours.

*   **Trigger:** Before generating a multi-step plan or proposing a specific code edit (`edit_file` call).
*   **Action:**

    `3.1.` **Search for Existing Logic:** Before planning implementation details (especially for utilities, validation, calculations), perform a preliminary search (`grep_search`, `codebase_search`) for existing implementations of the required functionality (referencing Audit Sections 1.B, 1.C). Briefly document findings, **explicitly stating the tool(s) used, query term(s), and specific key findings (files/functions) or confirming none found.** (e.g., "`grep_search` for `format_date`: Found existing utility `utils.format_date`, will reuse." or "`codebase_search` for `validate_user_input`: No specific existing validator found, planning new implementation.").

    `3.2.` **Identify Standards & Verify Alignment:** Explicitly identify the Core Principles or specific sections from `PROJECT_STANDARDS.md` **and relevant technical patterns/structures from the project's `PROJECT_ARCHITECTURE.md`** that are most pertinent to the request. Briefly state how the proposed plan/edit aligns with these standards, **citing the primary relevant section/pattern name(s)**. **Confirm the plan prioritizes the simplest, clearest solution that fully meets requirements and adheres to all standards.**
        *   *Example 1 (DI Focus): "Planning to add component X. This requires adhering to the Dependency Injection principles (`PROJECT_STANDARDS.md`) and specific DI configuration guidelines (`PROJECT_ARCHITECTURE.md`). Plan involves updating the DI config module per standard practice."*
        *   *Example 2 (Error Handling Focus): "Plan includes error handling for Y, following Error Handling guidelines (`PROJECT_STANDARDS.md` - Section 2.A) and hierarchy examples (`PROJECT_ARCHITECTURE.md` - `StandardErrorHandling` pattern), using standard logging."*

    `3.3.` **Check "Work with Facts":** Confirm the plan/edit relies *only* on provided facts, requirements, or verified information. State if clarification is needed. *Example: "Proceeding based on requirement Z. Clarify if other factors apply."*

    `3.4.` **Check "Robust Solutions":** Confirm the plan avoids hacks/workarounds and addresses the root cause.
        **Data Integrity Priority:** When encountering **unexpectedly missing or invalid data** from external sources or between internal components:
            *   **Default Action:** Treat as a **critical error**. Log clearly and raise an appropriate, specific error (e.g., `ConversionError`, `ValidationError`) to halt processing for the affected item.
            *   **Justification:** Prioritizes surfacing data quality/integration issues over potentially masking them.
            *   **Deviation (Requires Explicit Approval):** Implementing fallbacks/defaults **MUST STOP** standard plan and follow `Procedure: Handle Necessary Workaround` (Section 5), clearly stating the data integrity compromise. **This requires explicit user approval (see procedure).**

        `3.4.1.` **Analyze Impact & Verify Assumptions:**
            `a.` **Perform `Procedure: Analyze Impact` (Section 4)**: Identify and list affected call sites, perform enhanced scope checks for core changes, check circular dependencies, and consider data representation impacts. State outcome. **When reporting on enhanced scope checks (Procedure Step 2), MUST include specific inheritors/consumers identified.**
            `b.` **Perform `Procedure: Verify Hypothesis` (Section 4) for ALL Assumptions**: **Immediately** after stating each assumption (external APIs, internal interfaces, config, data formats, library usage), execute and report the verification using the specified structured format. **CRITICAL:** Includes verifying *interface consistency* between interacting components and assumptions about *functional equivalence* when replacing logic. **DO NOT** proceed until verification outcome is 'Confirmed'. **If verification for an *existing* dependency fails, STOP standard plan and execute `Procedure: Handle Failed Verification for Existing Dependency` (Section 5).** If verification for any other assumption fails, **STOP** and revise the plan.** *(Reminder: Address Failure Mode 2 - Verify against actual codebase state, avoid assumptions based solely on general patterns)*
            `c.` **Enumerate Edge Cases/Errors (Mandatory for Logic Changes):** List key potential edge cases/errors for new/modified logic and state **specifically how the plan addresses each enumerated case**. *Example: "Edge Cases: 1. Empty input list: Handled by initial check `if not input_list: return`. 2. Division by zero: Handled by pre-check `if divisor != 0`."*
            `d.` **Perform `Procedure: Verify Framework Compatibility` (Section 4)**: For framework entry points or library interactions, verify signature compatibility and adherence to established project patterns. State outcome.
            `e.` **Perform `Procedure: Ensure Logic Preservation` (Section 4) (Mandatory for Logic Replacement):** When replacing/restructuring logic blocks, document original behavior first ( **citing specific conditions or execution paths from the original code.** ), then detail how the new logic preserves it, explicitly justifying intentional changes and presenting trade-offs if necessary.
            `f.` **Perform `Procedure: Verify Configuration Usage Impact` (Section 4)**: When changing config values, identify usage locations and confirm compatibility/updates. State outcome.

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
        *   **Action:**
            `a. Explicit Instructions:` Be explicit in `instructions` (add vs. modify/replace).
            `b. Sufficient Context:` Ensure `code_edit` includes enough unchanged lines for anchoring.

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

### 4. Edit Generation, Application, and Verification Cycle

**Overarching Principle for Section 4:** The following sub-processes (4.A through 4.E) form a strict, unalterable sequence. Each sub-process **MUST** be fully completed, and its defined "Completion Artifact" **MUST** be generated and confirmed, before the next sub-process in the sequence can be initiated.

**CRITICAL WARNING (Applies throughout Section 4):** VERIFY ALL DIFF LINES. Failure to meticulously verify the *entire* diff (both proposed and applied) is a primary cause of regressions. **Verification means ensuring every changed/added line aligns with the plan, is syntactically correct, all referenced imports/symbols/variables are valid, and no unexpected logic changes or side-effects are introduced.** Treat tool output (`edit_file`, `reapply`) with extreme skepticism.

---

#### 4.A: Formulate Proposed Edit Package

*   **Purpose:** To translate the verified plan from Step 3 into a concrete, proposed code modification.
*   **Actions:**
    1.  Based on the verified plan from Step 3, formulate the `code_edit` content.
    2.  Ensure `instructions` for the `edit_file` tool are explicit (add vs. modify/replace).
    3.  Ensure `code_edit` includes sufficient unchanged context lines for anchoring.
    4.  This sub-process ONLY involves formulating the textual content. **DO NOT** call `edit_file` or `reapply` tools here.
*   **Completion Artifact:** `Proposed Code Edit Package`. This package conceptually contains:
    *   The complete `code_edit` string.
    *   The `instructions` string for the edit tool.
    *   Confirmation that sufficient context lines have been included.
*   **Transition Gate:** Proceed to Sub-process 4.B **only when** the `Proposed Code Edit Package` is fully formulated and internally confirmed.

---

#### 4.B: Scrutinize Proposed Edit (Pre-Application Verification)

*   **Purpose:** To meticulously verify the `Proposed Code Edit Package` against the plan *before* any attempt to apply it to the file system.
*   **Input Requirement:** `Proposed Code Edit Package` (from Sub-process 4.A).
*   **Actions:**
    1.  `4.B.1.` **Perform Granular Final Review:**
        *   `a.` Summarize relevant existing code structure from recent `read_file` calls (Pre-Edit Context).
        *   `b.` **MUST** execute `Procedure: Verify Diff` (Section 4, or as per its definition if moved to Section 5) on the *proposed `code_edit` diff* within the `Proposed Code Edit Package`. The 'intent' for this verification is the plan established in Step 3.
*   **Completion Artifact:** `Pre-Application Verification Report`. This report **MUST** conclude with one of the following explicit statuses:
    *   **Status:** `VERIFIED_PROPOSAL_OK_TO_APPLY`.
        *   *Report Details Example: "Sub-process 4.B Complete. Pre-Edit Context summarized. `Procedure: Verify Diff` executed on proposed edit against plan (Outcome: Verified, no deviations). Key assumption 'API X' verified (Outcome: Confirmed). Logic Preservation: Confirmed. **Status: VERIFIED_PROPOSAL_OK_TO_APPLY.**"*
    *   **Status:** `PROPOSAL_REQUIRES_REVISION`.
        *   *Report Details Example: "Sub-process 4.B Complete. `Procedure: Verify Diff` found deviations [details]. **Status: PROPOSAL_REQUIRES_REVISION.**"*
*   **Transition Gate:**
    *   If `Status: VERIFIED_PROPOSAL_OK_TO_APPLY`, proceed to Sub-process 4.C.
    *   If `Status: PROPOSAL_REQUIRES_REVISION`, **STOP**. Re-evaluate and return to Sub-process 4.A (to reformulate the edit) or to Step 3 (to revise the plan), clearly stating the reason for revision.

---

#### 4.C: Apply Edit to File System

*   **Purpose:** To execute the code modification on the target file using the appropriate tool.
*   **Input Requirement:** `Pre-Application Verification Report` (from Sub-process 4.B) with `Status: VERIFIED_PROPOSAL_OK_TO_APPLY`.
*   **Actions:**
    1.  `4.C.1.` **Execute Edit Tool:** Call the `edit_file` (or `reapply`) tool, providing the `code_edit` and `instructions` from the `Proposed Code Edit Package` (formulated in 4.A).
*   **Completion Artifact:** `Applied Edit Tool Result`. This is the actual diff or status returned by the `edit_file` or `reapply` tool.
*   **Transition Gate:** Proceed to Sub-process 4.D **only when** the `Applied Edit Tool Result` is available and indicates the tool call was accepted (even if the diff itself might later be found to be flawed). If the tool call itself fails catastrophically (e.g., tool error, file not found by tool), treat as a blocker and investigate (potentially involving `Procedure: Handle Unclear Root Cause / Missing Info`).

---

#### 4.D: Verify Applied Edit (Post-Application Verification)

*   **Purpose:** To meticulously verify the *actual modification made* by the tool to the file system and to check for any unintended side effects or deviations from the final approved proposal.
*   **Input Requirement:** `Applied Edit Tool Result` (from Sub-process 4.C).
*   **Actions:**
    1.  `4.D.1.` **Verify Edit Application Integrity:**
        *   `a.` If modification resulted from `reapply`: **Perform `Procedure: Verify Reapply Diff`** (Section 5).
        *   `b.` For diffs from standard `edit_file`: **Perform `Procedure: Verify Edit File Diff`** (Section 5). This includes using `Procedure: Verify Diff` where the 'intent' is the *final, verified proposal* that resulted in `VERIFIED_PROPOSAL_OK_TO_APPLY` status from 4.B.
        *   `c.` Check for and address any introduced redundancy (duplicate logic, unnecessary checks, etc.).
        *   `d.` Optional: After significant refactoring, consider a minimal runtime "smoke test."
    2.  `4.D.2.` **Check for Leftover Code & Dependency Integrity:**
        *   `a.` Confirm absence of old code commented out, temporary/AI process comments.
        *   `b.` Verify downstream consumers and perform dependency re-verification (as per original 4.4.2.b.i).
        *   `c.` For `delete_file` operations, verify complete removal and absence of dangling references.
    3.  `4.D.3.` **Self-Correct if Necessary:**
        *   `a.` Trigger: If reviews in 4.D.1 or 4.D.2 reveal violations, incorrect application, redundancy, leftover cleanup issues, or missed mandatory steps from this document.
        *   `b.` Action: **STOP** proceeding. State the issue and its basis in standards/process. Revise plan for investigation, correction, or cleanup. State the corrective action. If tool failure persists after reasonable attempts (e.g., >3 on the same specific fix), **Execute `Procedure: Request Manual Edit`** (Section 5).
*   **Completion Artifact:** `Post-Application Verification Report`. This report **MUST** conclude with one of the following explicit statuses:
    *   **Status:** `APPLIED_EDIT_CONFIRMED_CORRECT`.
    *   **Status:** `APPLIED_EDIT_DISCREPANCY_HANDLED_AND_RESOLVED`. (Used if self-correction in 4.D.3 was successful).
    *   **Status:** `APPLIED_EDIT_FAILURE_UNRESOLVED`. (Used if discrepancies persist or `Procedure: Request Manual Edit` is triggered).
*   **Transition Gate:**
    *   If `Status: APPLIED_EDIT_CONFIRMED_CORRECT` or `Status: APPLIED_EDIT_DISCREPANCY_HANDLED_AND_RESOLVED`, proceed to Sub-process 4.E.
    *   If `Status: APPLIED_EDIT_FAILURE_UNRESOLVED`, **STOP**. The task cannot proceed to summary without resolution or manual intervention as per procedure.

---

#### 4.E: Generate Final Action Summary

*   **Purpose:** To provide a final, structured summary confirming the successful completion of all verifications for the applied edit.
*   **Input Requirement:** `Post-Application Verification Report` (from Sub-process 4.D) with `Status: APPLIED_EDIT_CONFIRMED_CORRECT` or `Status: APPLIED_EDIT_DISCREPANCY_HANDLED_AND_RESOLVED`.
*   **Actions:**
    1.  `4.E.1.` **MUST** generate a **Post-Action Verification Summary**. This confirms all post-application checks (Sub-process 4.D) were performed, documented, and successfully resolved. Structure using the format previously defined in 4.5.b, adapting references as needed:
        ```markdown
        **Post-Action Verification Summary:**
        - `[x/-] 1. Edit Application Analysis:` *[Brief confirm checks from 4.D.1 performed. State: 'Applied diff matches final intent.' OR 'Applied diff discrepancies handled and resolved: [Details/Justification].'.]*
        - `[x/-] 2. Leftover Code & Dependency Analysis:` *[Brief confirm checks from 4.D.2 performed. Outcome: e.g., "Cleanup OK", "Re-verification confirmed no missed updates".]*
        - `[x/-] 3. Correction Assessment:` *[Brief confirm self-correction (4.D.3) performed. State if corrections were made and resolved. Mark `[x]` if checked & OK/Resolved, `[-]` if N/A.]*
        - `[x] 4. Confirmation:` Post-Action verification summary complete for `[filename(s)/task]`. *(Always `[x]`)*
        ```
*   **Completion Artifact:** The fully generated `Post-Action Verification Summary` (as markdown text).
*   **Transition Gate:** Section 4 is complete. Proceed to Step 5: Adherence Checkpoint.

---

## Reusable Verification Procedures

*(This section defines standard methods referenced in the main workflow.)*

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
    2.  **Enhanced Scope for Core Refactoring:** **CRITICAL TRIGGER:** If modifying base classes, core domain interfaces, widely used utilities, DI container, core configs: **MUST** explicitly consider consumers across *all* layers (CLI, helpers, services). Employ broader search strategies (`grep` for attribute access, semantic search). **MUST** search for inheritors/consumers (e.g., `grep_search 'class .*(.*BaseClassName.*):'`, `codebase_search`). List key findings, **including specific inheritors/consumers identified and brief note on potential impact checked.**
    3.  **Circular Dependency Check:** When adding a new module dependency (B needs A), explicitly state check, examine dependencies in module A, report outcome (e.g., "Circular Check: `module_a` does not import `module_b`. OK.").
    4.  **Data Representation Impact:** For changes affecting data representation (ID vs Name, format) or core models, explicitly consider impacts across layers (ORM, Mappers, Exporters, Consumers, Tests).

**`Procedure: Verify Hypothesis`**
*   **Purpose:** To factually verify assumptions made during planning before proceeding (called in Step 3.4.1.b).
*   **Scope:** Covers external API structures, internal module/class/function interfaces, configuration values, data formats, library usage patterns (treat non-standard library/internal interface usage as an assumption). **CRITICAL:** Also covers assumptions about *interface consistency* between interacting components and *functional equivalence* when replacing logic (compare plan vs. documented old logic).
*   **Applicability:** **MUST** be performed **immediately** after stating any assumption during Planning (Step 3.4.1.b) that impacts the proposed plan or edit.
*   **Steps (MUST perform IMMEDIATELY after stating assumption):**
    1.  **Detail Verification Method:** State specific method(s) (`read_file`, `grep_search`, docs review, etc.).
    2.  **Execute Verification & Report:** Perform verification. **MUST** report outcome structured:
        ```markdown
        **Hypothesis:** [State assumption]
        **Verification Method:** [Tool/Method]
        **Verification Execution:** [Action, e.g., Performing `read_file`...]
        **Verification Outcome:** [Confirmed: [Details] / Failed: [Details]]
        ```
        *Example Outcome Report:*
        ```markdown
        **Hypothesis:** External API `GET /items/{id}` returns `itemName` field.
        **Verification Method:** `read_file` on `docs/api_spec.v2.json`
        **Verification Execution:** Reading API spec file...
        **Verification Outcome:** Confirmed: Spec shows `itemName` present in response schema.
        ```
    3.  **Proceed or Halt:** Only proceed if Outcome is 'Confirmed'. If 'Failed', plan **MUST** be revised/halted. *(Reminder: Address Failure Mode 2 - Base decisions on verified facts, not assumptions or general patterns.)*

**`Procedure: Verify Diff`**
*   **Purpose:** To provide a core, reusable set of checks for verifying any diff (proposed or applied) against its intended state/plan. Called by Step 4.2.1.b, `Procedure: Verify Reapply Diff`, and `Procedure: Verify Edit File Diff`.
*   **Inputs (Conceptual):** The `diff` content, the `intent` (e.g., the plan, the state before the edit, the final intended proposal).
*   **Applicability:** **MUST** be executed during Pre-Apply Verification (Step 4.2.1.b), Post-Reapply Verification (`Procedure: Verify Reapply Diff`, Step 2), and Post-Edit File Verification (`Procedure: Verify Edit File Diff`, Step 1).
*   **Execution Reporting:** When executing this procedure, the AI response **MUST** include an inline checklist documenting the execution and outcome of each step below.
*   **Steps Checklist (MUST perform all relevant steps and report via inline checklist):**
    1.  **Diff vs. Intent Match:** Compare *entire* diff line-by-line against the `intent`. Note discrepancies. *(Reminder: Address Failure Mode 1 - Ensure diff matches intent, do not assume AI generated only planned changes)*
    2.  **Identify Deviations:** Explicitly list any lines added, deleted, or modified in the `diff` that were *not* part of the `intent` ("Deviations").
    3.  **Handle Deviations:** If Deviations found:
        *   **CRITICAL:** Fact-check **EVERY** Deviation using tools before accepting.
        *   **Execute `Procedure: Handle Deviation` (Section 5)** for each Deviation. **Report outcome clearly.**
        *   **Repeat for ALL Deviations** before proceeding.
    4.  **Dependency Verification:** **MUST** perform `Procedure: Verify Dependency Reference` for **ALL** dependency statements (e.g., imports, requires) present in the final, deviation-handled `diff`. **Explicitly report outcome.**
    5.  **Semantic Spot-Check:** Rigorously re-validate *key* additions/changes (complex logic, function calls, interactions with external APIs/framework patterns). Confirm semantic correctness against the `intent`.
    6.  **Context Line Check:** Re-verify context lines in the `diff` were not unexpectedly modified/misrepresented compared to the actual file state. Use `read_file` if suspicious.
    7.  **Logic Preservation Validation (If Applicable):** If the diff replaces/restructures logic, perform final validation comparing *applied* new logic against documented original behavior (from `Procedure: Ensure Logic Preservation`, Step 3.4.1.e). Confirm preservation or justified modification.
    8.  **Root Cause Check:** Ensure the final, deviation-handled `diff` still addresses the root cause identified in Step 3.4.
    9.  **Integration Sanity Check:** Briefly check: Does the change logically fit? Interact correctly with adjacent code?
    10. **Report Outcome:** Conclude with the overall verification outcome (e.g., "Verified", "Verified with handled deviations", "Failed - Requires correction").

   *Example Inline Checklist Output (in AI Response):*
   ```markdown
   **Executing `Procedure: Verify Diff`:**
   - `[x]` 1. Diff vs. Intent Match: Verified, matches final intent.
   - `[-]` 2. Identify Deviations: N/A, no deviations from intent.
   - `[-]` 3. Handle Deviations: N/A.
   - `[x]` 4. Dependency Verification: `Procedure: Verify Dependency Reference` executed for `import X`, Outcome: Confirmed.
   - `[x]` 5. Semantic Spot-Check: Key logic change `Y` reviewed, confirms intended behavior.
   - `[x]` 6. Context Line Check: Context lines appear correct.
   - `[-]` 7. Logic Preservation Validation: N/A, no logic replacement.
   - `[x]` 8. Root Cause Check: Confirmed diff addresses root cause Z.
   - `[x]` 9. Integration Sanity Check: Appears to integrate correctly.
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
    1.  **Signature Check:** For framework entry points (CLI handlers, API routes), explicitly verify changes to signatures (sync/async, params, return types) are compatible with framework invocation.
    2.  **Interaction Pattern Check:** Identify key framework/library interactions (e.g., 'Typer + DI', 'SQLAlchemy + Pydantic'). Confirm plan uses established project patterns OR check docs/examples for novel interactions. State check performed and outcome.

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
    1.  **Perform Core Diff Verification:** **MUST** execute `Procedure: Verify Diff` (Section 4) on the *actual diff applied by `edit_file`*. The 'intent' for this verification is the *final intended proposal* from Step 4.2.1.f (incorporating any handled deviations from the pre-apply check).
    2.  **Discrepancy Handling:** If the overall outcome of `Procedure: Verify Diff` (Step 1) is not 'Verified' (or 'Verified with handled deviations') and cannot be justified/corrected, **trigger self-correction (Step 4.4.3)**.

**`Procedure: Request Manual Edit`** (Triggered by Step 4.4.3.b.v if tool failures persist)
1.  **STOP** tool attempts.

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
1.  **DO NOT** immediately plan creation.

**`Procedure: Handle Deviation`**
*   **Trigger:** Called by `Procedure: Verify Diff` (Step 3) whenever deviations (unplanned changes) are identified between the `diff` being checked and the `intent`.
*   **Purpose:** To fact-check and decide on unplanned lines ("Deviations") in a proposed `code_edit` diff.
*   **Steps (For EACH Deviation):**
    1.  **Fact-check Deviation:** Use tools to confirm the existence and correctness of the deviation.
    2.  **Justify Deviation:** If deviation is intentional, justify it. If not, state it's unplanned and report it.
    3.  **Handle Deviation:** If deviation is intentional, implement it. If not, revise the plan.

**`Procedure: Verify Reapply Diff`**
*   **Purpose:** To meticulously verify the diff applied by the `reapply` tool.
*   **Trigger:** Called by Step 4.4.1.a immediately after a `reapply` tool call completes.
*   **Steps:**
    1.  **Treat Diff as New:** Approach with extreme skepticism.
    2.  **Perform Core Diff Verification:** **MUST** execute `Procedure: Verify Diff` (Section 4) on the *actual diff applied by `reapply`*. The 'intent' for this verification is the file state *before* the `reapply` call. **Emphasize extra scrutiny due to `reapply` context.**
    3.  **Structured Log:** **Immediately after `reapply` result,** **MUST** generate structured log confirming the execution and **overall outcome** of `Procedure: Verify Diff` (Step 2), explicitly mentioning the handling of any deviations. Use format similar to Step 4.2.2, noting it's post-reapply.

**`Procedure: Verify Edit File Diff`**
*   **Purpose:** To meticulously verify the diff applied by the standard `edit_file` tool.
*   **Trigger:** Called by Step 4.4.1.b immediately after a standard `edit_file` tool call completes.
*   **Steps:**
    1.  **Perform Core Diff Verification:** **MUST** execute `Procedure: Verify Diff` (Section 4) on the *actual diff applied by `edit_file`*. The 'intent' for this verification is the *final intended proposal* from Step 4.2.1.f (incorporating any handled deviations from the pre-apply check).
    2.  **Discrepancy Handling:** If the overall outcome of `Procedure: Verify Diff` (Step 1) is not 'Verified' (or 'Verified with handled deviations') and cannot be justified/corrected, **trigger self-correction (Step 4.4.3)**.

**`Procedure: Request Manual Edit`**
*   **Trigger:** Called by Step 4.4.3.b.v if repeated attempts to apply an edit via tools (`edit_file`, `reapply`) fail or produce incorrect results, and self-correction loops are exhausted.
1.  **STOP** tool attempts.
2.  **State Tool Failure:** Explain the issue and the presumed incorrect file state based on last tool output.
