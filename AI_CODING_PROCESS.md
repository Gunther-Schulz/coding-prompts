# AI Assistant - Standard Coding Process

**ATTENTION AI ASSISTANT: This document outlines the mandatory process you **MUST** follow during general coding interactions to ensure consistent adherence to the project's standards. This process requires adherence to `PROJECT_STANDARDS.md` (Core Principles & Checklist) and the detailed guidelines in the project's specific `PROJECT_ARCHITECTURE.md` (Patterns, Structures, Examples for the relevant language/framework). This process complements, but does not replace, the detailed standards themselves. You **MUST** explicitly report the performance and outcome of mandatory checks as specified below.**

**Goal:** To improve consistency and proactively catch deviations from standards by incorporating explicit checks **and reporting** into the workflow, ensuring a strong emphasis on fundamentally robust solutions over quick fixes or workarounds.

**Version: 1.45** (Rename External Docs: PROJECT_STANDARDS, PROJECT_ARCHITECTURE)

---

## Table of Contents

1.  [Introduction & Goal](#ai-assistant---standard-coding-process)
2.  [Core Principles & Critical Checks Summary](#core-principles--critical-checks-summary)
3.  [General Coding Workflow](#general-coding-workflow)
    *   [Step 1: Confirm Standards Awareness](#1-confirm-standards-awareness-initial-step)
    *   [Step 2: Confirm Goal](#2-confirm-goal-recommended)
    *   [Step 3: Pre-computation Standards Check (Planning)](#3-pre-computation-standards-check-planning-phase)
    *   [Step 4: Edit Generation & Verification Cycle](#4-edit-generation--verification-cycle)
    *   [Step 5: Adherence Checkpoint](#5-adherence-checkpoint-final-step-in-cycle-critical)
4.  [Reusable Verification Procedures](#reusable-verification-procedures)
5.  [Exception Handling Procedures](#exception-handling-procedures)
6.  [Glossary of Key Terms](#glossary-of-key-terms)
7.  [References](#references)

---

## Core Principles & Critical Checks Summary

**Key Principle:** While this process provides structure, the primary takeaway is that **strict adherence to the existing verification and checking steps is the most crucial factor** in preventing errors and ensuring robust, maintainable code.

**Absolute Critical Checks (Non-Negotiable):**

1.  **Verify ALL Diff Lines:** Meticulously verify **every single line** in *both* proposed (`code_edit`) and applied (`edit_file`/`reapply`) diffs against the plan and verified facts. **Treat tool output with extreme skepticism.** (See Step 4).
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

    `3.1.` **Search for Existing Logic:** Before planning implementation details (especially for utilities, validation, calculations), perform a preliminary search (`grep_search`, `codebase_search`) for existing implementations of the required functionality (referencing Audit Sections 1.B, 1.C). Briefly document findings (e.g., "Found existing utility `utils.format_date`, will reuse." or "No existing specific validator found, planning new implementation.").

    `3.2.` **Identify Standards & Verify Alignment:** Explicitly identify the Core Principles or specific sections from `PROJECT_STANDARDS.md` **and relevant technical patterns/structures from the project's `PROJECT_ARCHITECTURE.md`** that are most pertinent to the request. Briefly state how the proposed plan/edit aligns with these standards. **Confirm the plan prioritizes the simplest, clearest solution that fully meets requirements and adheres to all standards.**
        *   *Example 1 (DI Focus): "Planning to add component X. This requires adhering to the Dependency Injection principles (`PROJECT_STANDARDS.md`) and specific DI configuration guidelines (`PROJECT_ARCHITECTURE.md`). Plan involves updating the DI config module per standard practice."*
        *   *Example 2 (Error Handling Focus): "Plan includes error handling for Y, following Error Handling guidelines (`PROJECT_STANDARDS.md`) and hierarchy examples (`PROJECT_ARCHITECTURE.md`), using standard logging."*

    `3.3.` **Check "Work with Facts":** Confirm the plan/edit relies *only* on provided facts, requirements, or verified information. State if clarification is needed. *Example: "Proceeding based on requirement Z. Clarify if other factors apply."*

    `3.4.` **Check "Robust Solutions":** Confirm the plan avoids hacks/workarounds and addresses the root cause.
        **Data Integrity Priority:** When encountering **unexpectedly missing or invalid data** from external sources or between internal components:
            *   **Default Action:** Treat as a **critical error**. Log clearly and raise an appropriate, specific error (e.g., `ConversionError`, `ValidationError`) to halt processing for the affected item.
            *   **Justification:** Prioritizes surfacing data quality/integration issues over potentially masking them.
            *   **Deviation (Requires Explicit Approval):** Implementing fallbacks/defaults **MUST STOP standard plan and follow `Procedure: Handle Necessary Workaround` (Section 5)**, clearly stating the data integrity compromise.

        `3.4.1.` **Analyze Impact & Verify Assumptions:**
            `a.` **Perform `Procedure: Analyze Impact` (Section 4)**: Identify and list affected call sites, perform enhanced scope checks for core changes, check circular dependencies, and consider data representation impacts. State outcome.
            `b.` **Perform `Procedure: Verify Hypothesis` (Section 4) for ALL Assumptions**: **Immediately** after stating each assumption (external APIs, internal interfaces, config, data formats, library usage), execute and report the verification using the specified structured format. **CRITICAL:** Includes verifying *interface consistency* between interacting components and assumptions about *functional equivalence* when replacing logic. **DO NOT** proceed until verification outcome is 'Confirmed'. **If verification for an *existing* dependency fails, STOP standard plan and execute `Procedure: Handle Failed Verification for Existing Dependency` (Section 5).** If verification for any other assumption fails, STOP and revise the plan.**
            `c.` **Enumerate Edge Cases/Errors (Mandatory for Logic Changes):** List key potential edge cases/errors for new/modified logic and state how the plan addresses them. *Example: "Edge Cases: Handles empty input list; checks for division by zero."*
            `d.` **Perform `Procedure: Verify Framework Compatibility` (Section 4)**: For framework entry points or library interactions, verify signature compatibility and adherence to established project patterns. State outcome.
            `e.` **Perform `Procedure: Ensure Logic Preservation` (Section 4) (Mandatory for Logic Replacement):** When replacing/restructuring logic blocks, document original behavior first, then detail how the new logic preserves it, explicitly justifying intentional changes and presenting trade-offs if necessary.
            `f.` **Perform `Procedure: Verify Configuration Usage Impact` (Section 4)**: When changing config values, identify usage locations and confirm compatibility/updates. State outcome.

        `3.4.2.` **Verification for Moves/Renames:** Use *multiple* search strategies (as detailed in `Procedure: Analyze Impact`, Section 4) to confirm impact. **DO NOT** rely on a single 'no results' search. *Example: 'Modifying `parser_x` impacts `processor_y`. Plan includes verifying compatibility.'*

        `3.4.3.` **Prefer Atomic Edits:** Break down complex changes into smaller, atomic edits where feasible.

    `3.5.` **Handle Blockers (Unclear Root Cause / Missing Info / Standard Conflicts):**
        *   **Trigger:** If Steps 3.3 or 3.4 reveal unclear root causes, missing info, standard conflicts, failing standard debugging methods, or certain failed dependency verifications (see `Procedure: Handle Failed Verification for Existing Dependency`, Section 5).
        *   **Action:** **STOP** proposing a direct fix. **Execute `Procedure: Handle Unclear Root Cause / Missing Info` (Section 5).**

    `3.6.` **Handle Architectural Decisions / Suboptimal Patterns:**
        *   **Trigger:** When analysis reveals architectural conflicts, significant trade-offs between valid paths, or foundational inconsistencies.
        *   **Action:** **IMMEDIATELY STOP** detailed planning. **Execute `Procedure: Handle Architectural Decisions` (Section 5).**

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
        *   **Action:** **MUST** generate a **Pre-computation Verification Summary**. Structure using the following format:
        ```markdown
        **Pre-computation Verification Summary:**
        - `[x/-] 1. Standards Alignment:` *[Brief confirmation 3.2 performed.]*
        - `[x/-] 2. Impact Analysis:` *[Brief confirmation `Procedure: Analyze Impact` (3.4.1.a) performed. Outcome: e.g., 'Standard impact checked', 'Core impact N/A'.]*
        - `[x/-] 3. Hypothesis Verification:` *[Brief confirmation `Procedure: Verify Hypothesis` (3.4.1.b) performed for ALL assumptions. Outcome: e.g., 'All verified', 'Assumption X confirmed'.]*
        - `[x/-] 4. Logic Preservation Plan:` *[Brief confirmation `Procedure: Ensure Logic Preservation` (3.4.1.e) performed if applicable. Mark `[x]` if done, `[-]` if N/A.]*
        - `[x/-] 5. Blocker Checks:` *[Brief confirmation blocker checks (3.5/3.6) performed and guidance sought/received via Section 5 procedures if triggered. Mark `[x]` if checked & resolved/approved, `[-]` if N/A.]*
        - `[x] 6. Confirmation:` Pre-computation verification summary complete. *(Always `[x]`)*
        ```

### 4. Edit Generation & Verification Cycle

*   **Purpose:** This step covers the critical cycle of generating a proposed code edit, verifying it rigorously *before* application, applying it, and verifying the result *after* application.
*   **Core Cycle:** [Generate -> Pre-Verify -> Apply -> Post-Verify -> Summarize]

    **CRITICAL WARNING:** VERIFY ALL DIFF LINES. Failure to meticulously verify the *entire* diff (both proposed and applied) is a primary cause of regressions. Treat tool output (`edit_file`, `reapply`) with extreme skepticism.

    #### 4.1 Generate Proposed Edit (`code_edit`)
    *   **Action:** Based on the verified plan from Step 3, formulate the `code_edit` content.
        *   Ensure `instructions` are explicit (add vs. modify/replace).
        *   Ensure `code_edit` includes sufficient unchanged context lines for anchoring.

    #### 4.2 Pre-Apply Verification (Mandatory Before 4.3)
    *   **Purpose:** To meticulously scrutinize the *proposed `code_edit` diff* against the verified plan *before* calling the `edit_file` tool.
    *   **Mandate:** **DO NOT SKIP OR RUSH.** Treat proposed diffs skeptically.
    *   **Action:** Perform the following checks on the generated `code_edit` from 4.1:

        `4.2.1` **Perform Granular Final Review & Deviation Handling Loop (Pre-Edit):**
            *   **WARNING:** AI model may add unrequested lines.
            *   `a. Summarize Pre-Edit Context:` Briefly summarize relevant existing code structure from recent `read_file` calls.
            *   `b. Review Intended Changes:` Review planned lines in `code_edit` diff. Verify alignment with plan and standards (Step 3).
            *   `c. Identify Deviations:` Compare *entire* proposed `code_edit` diff line-by-line against Step 3 plan. List all unplanned lines ("Deviations").
            *   `d. Handle Deviations:` If Deviations found:
                *   **CRITICAL:** Fact-check **EVERY** Deviation using tools before accepting.
                *   **Execute `Procedure: Handle Deviation` (Section 5)**. This includes mandatory re-verification of involved dependencies (`Procedure: Verify Dependency Reference`, Section 4) and confirmation of prior hypothesis verification.
                *   **Repeat for ALL Deviations** before exiting loop.
            *   `e. Integration Sanity Check:` Briefly check: Does the change logically fit? Interact correctly with adjacent code? State confirmation.
            *   `f. Review Final Proposed Diff:` Review final `code_edit` diff (intended + verified Deviations - removed Deviations). **CRITICAL:** Verify all types/functions/classes used are defined/imported. Add missing imports. Ensure it addresses root cause (Step 3.4). Verify calls match verified signatures (Step 3.4.1.b).

        `4.2.2` **Generate Pre-Edit Confirmation Statement:** Before proceeding to 4.3, **MUST** provide brief statement confirming 4.2.1 checks, **mentioning explicit verification of key assumptions/dependencies** and logic preservation if applicable.
            *   *Example (Refactoring): "Pre-Apply Verification (4.2) complete. No deviations. Key assumption 'API X' verified (Outcome: Confirmed). Logic Preservation: Confirmed plan preserves behavior. Proceeding to Apply Edit (4.3)."*
            *   *Example (Deviation Handling): "Pre-Apply Verification (4.2) complete. Deviations handled: removed unverified 'foo.bar' (check outcome reported). Key assumption 'Model Y' verified (Outcome: Confirmed). Logic Preservation: N/A. Proceeding to Apply Edit (4.3)."*

    #### 4.3 Apply Edit (After Successful 4.2)
    *   **Action:** Call the edit tool.
        *   `4.3.1 Apply Edit Tool:` (e.g., Call `edit_file` or `reapply`)

    #### 4.4 Post-Apply Verification (Mandatory After 4.3 Tool Call Result)
    *   **Purpose:** To meticulously verify the *actual diff applied* by the tool (`edit_file` or `reapply`) and check for side effects.
    *   **Action:** After a successful `edit_file` or `reapply` call result, explicitly check **and report the outcome of each** of the following:

        `4.4.1` **Verify Edit Application:**
            *   `a. Post-Reapply Verification:` **CRITICAL:** If modification resulted from `reapply`:
                *   **Perform `Procedure: Verify Reapply Diff` (Section 5)**. This involves treating the diff as new, re-performing full pre-edit verification (4.2.1 checks) on the applied diff, explicitly handling deviations, and logging confirmation.
            *   `b. Post-`edit_file` Verification:` **CRITICAL:** For diffs from standard `edit_file` (not `reapply`):
                *   **WARNING:** Treat Diff Output with Extreme Skepticism.
                *   **Perform `Procedure: Verify Edit File Diff` (Section 5)**. This includes: diff match, semantic spot-check, **mandatory** dependency re-verification (`Procedure: Verify Dependency Reference`, Section 4), context line check, final logic preservation validation, and discrepancy handling.
            *   `c. No Introduced Redundancy:** Check for duplicate logic, unnecessary checks, redundant mappings. Remove if found.
            *   `d. Optional Post-Refactor Smoke Test:** After significant refactoring, consider minimal runtime "smoke test" (e.g., `command --help`). Failure should trigger self-correction (4.4.3). State if performed and outcome.

        `4.4.2` **Check for Leftover Code & Dependencies:**
            *   `a. Confirm Absence of Leftover Code/Comments:** **MUST** verify that no old code remains commented out and that temporary/AI process comments have been removed.
            *   `b. Verify Downstream Consumers & Cleanup:**
                i.  **Dependency Re-verification:** **RE-VERIFICATION:** For changes involving modified/added interfaces, paths, symbols, or core models, **explicitly state re-verification is being performed**, **re-execute the codebase search (`grep_search`)** from planning (Step 3.4.1), and report findings to confirm all dependents were updated or unaffected. **Do not rely solely on initial planning search.** If missed updates found, **trigger self-correction (4.4.3)**.
                ii. **Verification of Deletion:** **CRITICAL (AFTER `delete_file`):** Confirm removal by searching for dangling references (absolute path fragment, relative references). **Report search strategies and outcome.** If lingering references found, **MUST trigger self-correction (4.4.3)**.
                iii.**Functional Check:** Confirm consumers still function as expected (via analysis, suggesting tests if available).

        `4.4.3` **Self-Correct if Necessary:**
            *   `a. Trigger:` If reviews (4.2.1, 4.4.1, 4.4.2) reveal violations, incorrect application, redundancy, or leftover cleanup issues. Also triggered by specific missed mandatory steps (see below).
            *   `b. Action:`
                i.  **STOP** proceeding with flawed logic/state. State the issue discovered.
                ii. **MUST** explicitly state flaw** based on standard/process (e.g., "Workaround violated standards." or "Applied diff mismatch.").
                iii.**Triggers for Self-Correction (STOP and Address):**
                    a.  Missed mandatory immediate verification for a hypothesis (3.4.1.b).
                    b.  Missed mandatory Pre-Edit Verification (4.2.1), Post-Reapply Verification (4.4.1.a), Post-Edit File Verification (4.4.1.b), or Dependency Re-verification (4.4.2.b.i).
                    c.  Missed mandatory Post-Action Verification Summary (4.4.3) for the preceding edit.
                    d.  Missed mandatory halt for guidance under blocker procedures (e.g., 3.6 / `Procedure: Handle Architectural Decisions`).
                    e.  Any other missed mandatory step, check, reporting, or verification from this document.
                    f.  Missed required Enhanced Scope Impact Analysis (part of `Procedure: Analyze Impact`) for core component modification.
                iv. **MUST** revise plan/next step for investigation, correction, or cleanup. State the corrective action.
                v.  **If Tool Failure Persists:** **Execute `Procedure: Request Manual Edit` (Section 5)**. **Reminder:** Avoid excessive self-correction loops (e.g., >3 attempts on the same specific fix) before triggering this escalation.

    #### 4.5 Generate Post-Action Verification Summary (After Successful 4.4)
    *   `a. Trigger:` Immediately following the *final* successful verification (Step 4.4) for the task's edits.
    *   `b. Action:` **MUST** generate a **Post-Action Verification Summary**. Confirms post-apply checks were **performed and documented**. Structure using the following format:
        ```markdown
        **Post-Action Verification Summary:**
        - `[x/-] 1. Edit Application Analysis:` *[Brief confirm checks 4.4.1.a (if `reapply`), **4.4.1.b (via `Procedure: Verify Edit File Diff`, Section 5)**, 4.4.1.c (redundancy) done. State: 'Applied diff matches final intent.' OR 'Applied diff discrepancies: [List/Justification].'.]*
        - `[x/-] 2. Leftover Code & Dependency Analysis:` *[Brief confirm check **4.4.2.a (artifacts)** and **4.4.2.b (explicit dependency re-verification/deletion checks)** done. Outcome: e.g., "Cleanup OK", "Re-verification confirmed no missed updates".]*
        - `[x/-] 3. Correction Assessment:` *[Brief confirm check 4.4.3 performed. State if corrections were needed/made. Mark `[x]` if checked & OK, `[-]` if N/A.]*
        - `[x] 4. Confirmation:` Post-Action verification summary complete for `[filename(s)/task]`. *(Always `[x]`)*
        ```

### 5. Adherence Checkpoint (Final Step in Cycle): **CRITICAL:**

*   **Trigger:** Before concluding interaction for the current task/request cycle.
*   **Action:** Perform a final mental check: Have all mandatory steps (1-4, including required sub-steps like 4.2, 4.4, 4.5), checks, verifications, and reporting requirements outlined in this document (including verification summaries) been completed for this cycle? If any were missed, **trigger self-correction (Step 4.4.3)** to address the oversight before finishing.

---

## Reusable Verification Procedures

*(This section defines standard methods referenced in the main workflow.)*

**`Procedure: Verify Dependency Reference`**
*   **Purpose:** To factually confirm the validity of a dependency reference (e.g., import, require) before relying on it or including it in code. **MUST** be performed for **every** reference during planning (3.4.1.b), pre-edit deviation handling (4.2.1.d), post-edit verification (4.4.1.b.iii), and downstream checks (4.4.2.b.i).
*   **Steps:**
    1.  **Cross-Reference Prior Knowledge:** Check if the correct module path for the symbol(s) was previously verified. Use verified path if available.
    2.  **Path Validation:** **MUST** use tools (`file_search`, `list_dir`) to confirm the **actual existence** of the source module file/directory path (e.g., `path/to/module.py`, `path/to/package/__init__.py`). **Report method and outcome.** If path invalid, reference **MUST** be corrected or removed.
    3.  **Symbol Validation:** Only if path confirmed valid, **MUST** then use tools (`read_file` on target, `grep_search`) to confirm the existence AND necessity (check usage in *current* file scope) of the referenced symbol(s) within that module/package. **Report method and outcome.** Remove unused symbols.

**`Procedure: Analyze Impact`**
*   **Purpose:** To identify potential effects of proposed changes on existing functionality (called in Step 3.4.1.a).
*   **Steps Checklist:**
    1.  **Identify Affected Call Sites/References:** For interface/path/symbol changes, **MUST** use tools (`grep_search`) to find *all* potential call sites/imports/references codebase-wide. List findings.
    2.  **Enhanced Scope for Core Refactoring:** **CRITICAL TRIGGER:** If modifying base classes, core domain interfaces, widely used utilities, DI container, core configs: **MUST** explicitly consider consumers across *all* layers (CLI, helpers, services). Employ broader search strategies (`grep` for attribute access, semantic search). **MUST** search for inheritors/consumers (e.g., `grep_search 'class .*(.*BaseClassName.*):'`, `codebase_search`). List key findings.
    3.  **Circular Dependency Check:** When adding a new module dependency (B needs A), explicitly state check, examine dependencies in module A, report outcome (e.g., "Circular Check: `module_a` does not import `module_b`. OK.").
    4.  **Data Representation Impact:** For changes affecting data representation (ID vs Name, format) or core models, explicitly consider impacts across layers (ORM, Mappers, Exporters, Consumers, Tests).

**`Procedure: Verify Hypothesis`**
*   **Purpose:** To factually verify assumptions made during planning before proceeding (called in Step 3.4.1.b).
*   **Scope:** Covers external API structures, internal module/class/function interfaces, configuration values, data formats, library usage patterns (treat non-standard library/internal interface usage as an assumption). **CRITICAL:** Also covers assumptions about *interface consistency* between interacting components and *functional equivalence* when replacing logic (compare plan vs. documented old logic).
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
    3.  **Proceed or Halt:** Only proceed if Outcome is 'Confirmed'. If 'Failed', plan **MUST** be revised/halted.

**`Procedure: Ensure Logic Preservation`**
*   **Purpose:** To meticulously preserve essential behavior when replacing or significantly restructuring existing logic blocks (called in Step 3.4.1.e).
*   **Steps:**
    1.  **Document Existing Behavior FIRST:** Before proposing new structure, use `read_file` to analyze and **document essential behavior, distinct execution paths, and key conditional logic** of the code being replaced. Summarize concisely.
    2.  **Explicit Preservation Plan REQUIRED:** Detail how the *new* logic structure preserves EACH identified essential behavior/path, referencing the documentation from Step 1.
    3.  **Justify Intentional Changes:** If original behavior/path is intentionally changed/removed or conditions altered, **MUST** state this explicitly, justify it, analyze impact, and confirm acceptability.
        *   **Explicit Trade-off Presentation:** If change results from simplification, present Trade-off (Plan A: Simpler/Altered vs. Plan B: Complex/Preserves) and **request guidance**.
        *   **STOP processing and await user guidance before proceeding with implementation of either trade-off option.**
    4.  **Functional Check:** Confirm consumers still function as expected (analysis, suggest tests).

**`Procedure: Verify Framework Compatibility`**
*   **Purpose:** Ensure changes to framework entry points or library interactions are valid (called in Step 3.4.1.d).
*   **Steps:**
    1.  **Signature Check:** For framework entry points (CLI handlers, API routes), explicitly verify changes to signatures (sync/async, params, return types) are compatible with framework invocation.
    2.  **Interaction Pattern Check:** Identify key framework/library interactions (e.g., 'Typer + DI', 'SQLAlchemy + Pydantic'). Confirm plan uses established project patterns OR check docs/examples for novel interactions. State check performed and outcome.

**`Procedure: Verify Configuration Usage Impact`**
*   **Purpose:** Check impact of changes to configuration values.
*   **Steps:** Use `grep_search` to identify potential usage locations codebase-wide. State findings and confirm compatibility or necessary updates in plan.

---

## Exception Handling Procedures

*(This section details procedures for handling specific issues identified during the workflow. Execution of these procedures typically means **STOPPING** the standard workflow path until the issue is resolved or explicit guidance/approval is received.)*

**`Procedure: Handle Unclear Root Cause / Missing Info`** (Triggered by Step 3.5)
1.  **STOP** proposing direct fix.
2.  **State Issue & Prioritize Investigation:** Clearly state the blocker (unclear cause, missing info, standard conflict, failing debug method).
3.  **Formulate Investigation Plan:** Outline specific steps to investigate (e.g., "Plan: 1. Examine logger setup. 2. Check system exception hook.").
4.  **Seek Confirmation (if needed):** Confirm broad/assumption-based investigation plans. *Example: "Traceback suppressed. Plan: [steps]. Proceed?"*
   **If confirmation is sought, STOP processing and await explicit user confirmation before executing the investigation plan.**
5.  **Handle Necessary Workarounds (Use Sparingly):** Follow `Procedure: Handle Necessary Workaround` ONLY if investigation is blocked/impractical AND workaround is only viable path *now*.
6.  **Consult on Ambiguous Missing Dependencies:** Follow `Procedure: Consult on Ambiguous Missing Dependency` if resolution requires creating significant new structures based solely on potentially old references without corroborating context.

**`Procedure: Handle Architectural Decisions`** (Triggered by Step 3.6)
1.  **IMMEDIATELY STOP** detailed planning for original task. **Treat as mandatory blocker.**
2.  **State Issue/Choice:** Clearly describe the suboptimal pattern, architectural decision point, or foundational inconsistency (e.g., global state use, SRP violation choice, type mismatch).
3.  **Outline Options:** Present potential solution paths or refactoring options for the *underlying issue*.
4.  **Analyze Options:** Briefly discuss pros/cons, referencing `PROJECT_STANDARDS.md` and `PROJECT_ARCHITECTURE.md`.
5.  **Request Guidance:** Explicitly ask user for direction on resolving the *foundational issue*. *Example: "Identified module X uses global state. Option 1: Refactor using DI (standard). Option 2: Keep global state (debt). Which approach?"* or *"Inconsistent `ItemID` type (int vs str). Option A: Change all to `string`. Option B: Keep `integer` + enforce conversion. How to resolve?"*
6.  **Await Direction:** **Do NOT** proceed with *any* implementation planning until user provides clear direction on the architectural/consistency issue.

**`Procedure: Handle Necessary Workaround`** (Triggered by Step 3.4 Data Integrity Deviation or Blocked Investigation 3.5)
*   **Pre-condition:** Root cause investigation confirmed blocked or impractical *now*, and workaround is only viable path *at this moment*.
1.  **STOP** all implementation planning/fixing.
2.  **State Blockage & Need:** Explain *why* root cause cannot be robustly addressed now. State explicitly that proceeding requires a temporary workaround violating standard principles.
3.  **Propose Workaround Concept:** Describe *intended effect* and *nature* conceptually (e.g., "Propose temporarily ignoring error X", "Propose temporarily using hardcoded value Y"). No implementation details yet.
4.  **Justify & Warn:** Explain why it seems the only *temporary* path. **CRITICAL:** State risks and technical debt introduced (e.g., "Risk: Masks issue Z.", "Debt: Requires future refactoring.").
5.  **Request Explicit Approval:** **MUST** ask user: *"Do you approve implementing this specific temporary workaround to proceed, acknowledging the stated risks and the requirement for future cleanup?"*
6.  **Await Approval:** **DO NOT** proceed unless **explicit approval** received. If approved, subsequent plan **MUST** include tracking (e.g., `TODO`) and eventually removing/replacing it with a robust solution. If denied, remain **STOP**ped.

**`Procedure: Consult on Ambiguous Missing Dependency`** (Triggered by specific case in 3.5)
*   **Pre-condition:** Resolving dependency error requires creating significant new structures based *only* on potentially old references, with no strong corroborating context.
1.  **STOP** implementation planning.
2.  **State Situation:** Explain failed resolution, lack of context, and implication (e.g., "DI references `X` from missing `Y`. No context found. Requires creating `Y` and `X`.").
3.  **Present Options:** Describe choices (e.g., "Option A: Create `Y`/`X` based on reference (Risk: Obsolete code). Option B: Assume reference is stale, plan removal + usage cleanup (Risk: If removal breaks something active).").
4.  **Request Guidance:** Ask explicitly: "How should I proceed?"
5.  **Await Direction:** **Do NOT** proceed until user provides direction.

**`Procedure: Handle Failed Verification for Existing Dependency`** (Triggered by 3.4.1.b)
*   **Pre-condition:** Hypothesis verification for a dependency reference in *existing, established code* fails (module/symbol not found).
1.  **DO NOT** immediately plan creation.
2.  **State the Discrepancy:** Explicitly state the reference failure.
3.  **Perform Usage Check:** **MUST** use `grep_search` within the *referencing file* for actual usage of the specific symbol(s). Report findings (e.g., "Usage Check: Searched `di_config.py` for `MissingSymbol`. Outcome: No usage found / Usage found in func `X`.").
4.  **Perform Broader Context Search:** Use `grep_search`/`codebase_search` for context (moved? refactored? deprecated? TODOs?). Report findings.
5.  **Proceed based on Context and Usage:**
    *   If context indicates simple move/rename: Plan fix (update path).
    *   If dependency failed **AND** Usage Check found **NO USAGE**: Default plan is to **remove the stale dependency reference statement**. Briefly verify no downstream issues.
    *   If dependency failed **AND** usage *was* found, OR context suggests intentional removal but usage remains, OR context insufficient/ambiguous: **Trigger `Procedure: Handle Unclear Root Cause / Missing Info`**.

**`Procedure: Handle Deviation`** (Called by Step 4.2.1.d)
*   **Purpose:** To fact-check and decide on unplanned lines ("Deviations") in a proposed `code_edit` diff.
*   **Steps (For EACH Deviation):**
    1.  **State Deviation:** Clearly identify the unplanned line(s).
    2.  **Fact-Check:** **MUST** perform tool-based verification. **Hint:** Often prioritize checking dependency references first.
        *   *Dependencies/Imports:* **CRITICAL:** Perform `Procedure: Verify Dependency Reference` for the specific reference involved in the deviation. **Explicitly report outcome.**
        *   *Modifying Dependencies:* If modifying existing import, re-verify necessity of *all* remaining symbols on that line using `grep_search`/`read_file`. Remove unused. State outcome.
        *   *Variables/Logic/Defaults/Deletions:* Confirm requirement based *only* on verified state/requirements. Justify unexpected deletions.
        *   *Context Line Discrepancies:* Check if context lines shown incorrectly (+/-). If so, **STOP**. Re-read file (`read_file`) to confirm state before correcting/proceeding.
        *   *Confirm Prior Hypothesis Verification:* Before accepting code dependent on prior assumption (Step 3.4.1.b), **explicitly confirm** the documented outcome was 'Confirmed'. If not, **STOP**, state violation, fix/remove code.
    3.  **Decision & Action:**
        *   If Deviation verified & necessary: State justification (including **reported verification outcome**) and keep it. *(Example: "Deviation: Added import `x.y`. Check: `Procedure: Verify Dependency Reference`. Outcome: Confirmed. Keeping.")*
        *   If Deviation cannot be factually verified/necessity confirmed: State reason and **REMOVE the Deviation / Correct dependent code / Re-plan.**
    4.  **Repeat for ALL Deviations.**

**`Procedure: Verify Reapply Diff`** (Called by Step 4.4.1.a)
*   **Purpose:** To meticulously verify the diff applied by the `reapply` tool.
*   **Steps:**
    1.  **Treat Diff as New:** Approach with extreme skepticism.
    2.  **Perform Full Re-Verification:** Re-perform **all** checks from **Step 4.2.1 (a through f)** on the *actual diff applied by `reapply`*, comparing it against the file state *before* the `reapply` call. This includes: Context Summary, Intended Change Review, Deviation Identification, **mandatory execution of `Procedure: Handle Deviation` for every deviation found in the reapply diff**, Integration Sanity Check, and Final Diff Review. **Explicitly report each check and outcome.**
    3.  **Structured Log:** **Immediately after `reapply` result,** **MUST** generate structured log confirming these re-verification checks (Steps 1-2) *including explicit outcome of deviation handling*. Use format similar to Step 4.2.2, noting it's post-reapply.

**`Procedure: Verify Edit File Diff`** (Called by Step 4.4.1.b)
*   **Purpose:** To meticulously verify the diff applied by the standard `edit_file` tool.
*   **Steps:**
    1.  **Diff Match:** Compare *entire* applied diff line-by-line against final intended proposal (from 4.2.1.f). Identify **ALL** discrepancies (subtle changes to imports, types, comments, whitespace matter).
    2.  **Targeted Semantic Spot-Check:** Rigorously re-validate *key* additions/changes (complex logic, function calls). Confirm semantic correctness, **esp. interactions with external library APIs / framework patterns.**
    3.  **Dependency Re-verification:** **MUST** perform `Procedure: Verify Dependency Reference` for **ALL** dependency statements present in the *applied* diff. **Explicitly report outcome.**
    4.  **Context Line Check:** Re-verify context lines not unexpectedly modified/misrepresented. Use `read_file` if suspicious.
    5.  **Logic Preservation Validation (If Applicable):** Final validation comparing *applied* new logic against documented original behavior (from `Procedure: Ensure Logic Preservation`). Confirm preservation or justified modification.
    6.  **Discrepancy Handling:** If any check (1-5) fails and cannot be justified/corrected, **trigger self-correction (Step 4.4.3)**.

**`Procedure: Request Manual Edit`** (Triggered by Step 4.4.3.b.v if tool failures persist)
1.  **STOP** tool attempts.
2.  **State Tool Failure:** Explain the issue and the presumed incorrect file state based on last tool output.
3.  **Final State Check:** **MUST** re-read relevant file section (`read_file`) to verify *actual* current state.
4.  **If Still Incorrect:** **MUST** request manual user intervention. Provide clear, complete, copy-pasteable code block showing *entire relevant section* in desired final state, including **sufficient surrounding unchanged lines (e.g., 5-10 lines before/after)**.
   **After requesting the manual edit, STOP processing and await explicit user confirmation that the edit has been applied before proceeding with Step 4.4 (Post-Apply Verification).**
5.  If Edit Succeeded: If re-read shows edit *did* succeed despite tool reports, state this and proceed.

---

## Glossary of Key Terms

*   **Atomic Edit:** An edit designed to address a single, verifiable change, minimizing verification scope.
*   **Assumption (Hypothesis):** A statement about the state of the codebase, external systems, or requirements that *must* be factually verified using tools before proceeding with dependent actions. Treated purely as a hypothesis until proven.
*   **Core Component:** Foundational elements like base classes, core domain interfaces, widely used utilities/helpers, the DI container, or base configurations, changes to which require enhanced impact analysis.
*   **Core Component Impact:** The specific potential effects of changes on core components and their direct/indirect consumers across all application layers.
*   **Deviation:** Any line added, deleted, or modified in a proposed (`code_edit`) or applied (`edit_file`/`reapply`) diff that was *not* part of the explicit, verified plan. Requires mandatory fact-checking.
*   **Root Cause:** The fundamental underlying reason for a bug or issue, as opposed to its superficial symptoms. Addressing the root cause is prioritized over workarounds.
*   **Standard Impact:** The potential effects of changes on standard functionality, including call sites, imports, references, configuration usage, and data representation across different components/layers (excluding the enhanced scope specific to Core Components unless applicable).
*   **Verification:** The act of using tools (`read_file`, `grep_search`, `list_dir`, etc.) or reliable sources (documentation) to factually confirm the correctness of assumptions, the state of the code, the validity of dependencies, or the outcome of an edit.

---

## References

*   Consult `PROJECT_STANDARDS.md` (if available) for definitive standard definitions.
*   Consult the project's `PROJECT_ARCHITECTURE.md` (if available) for language/framework-specific technical patterns.
