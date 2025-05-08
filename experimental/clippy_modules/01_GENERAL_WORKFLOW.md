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

**3.0.1. Verify Tool Output Congruence and Sufficiency (After Information Gathering):**
*   **Trigger:** Immediately after each information-gathering tool call (e.g., `read_file`, `grep_search`, `codebase_search`, `list_dir`) within Step 3 before its output is used for further planning.
*   **Action:**
    1.  **Verify Congruence:** Confirm the tool's output aligns with the explicit request parameters. Examples:
        *   `read_file`: Was the requested line range or full file content returned as expected?
        *   `grep_search`/`codebase_search`: Do the results appear complete, or is there indication of truncation (e.g., "results capped at 50 matches") that might hide relevant information?
        *   `list_dir`: Was the directory listing provided for the correct path?
    2.  **Assess Sufficiency & Clarity:** Evaluate if the *content* of the output is adequate and unambiguous for the *specific planning task or hypothesis verification at hand*.
    3.  **Handle Discrepancies/Insufficiencies:**
        *   If output is incongruent with the request (e.g., wrong lines returned, unexpected truncation not acceptable for the task) or if the content is insufficient/ambiguous for the immediate purpose:
            *   **MUST** explicitly state the issue.
            *   **MUST** take corrective action *before* proceeding to use this data. Corrective actions may include:
                *   Re-running the tool with adjusted parameters (e.g., more specific query, different line numbers).
                *   Using an alternative tool to gather the necessary information.
                *   If ambiguity persists, requesting clarification from the user.
            *   If proceeding with potentially incomplete data is unavoidable and deemed acceptable after consideration, this **MUST** be explicitly stated along with potential risks.
    4.  **Report Outcome:** Concisely state the outcome of this verification (e.g., "Output of `read_file` for `xyz.py` verified, content is sufficient for planning.", "Initial `grep_search` for '''MyClass''' was truncated; re-running with `include_pattern='*.py'` for better focus. Output verified and sufficient.", "Output of `list_dir` for `src/utils` verified and sufficient.").
*   **Next Step:** Only after successful verification (or explicit acknowledgment of proceeding with limitations), use the tool's output for subsequent planning sub-steps (e.g., 3.1, 3.2, etc.).

**NOTE:** Foundational checks (Steps 3.4, 3.5, 3.6) take precedence. If analysis reveals underlying issues (robustness, unknown root cause, architectural conflicts), these **MUST** be addressed by **STOPPING the standard plan and executing the appropriate procedure from Section 5: `Exception Handling Procedures`** *before* proceeding with the original task. Embrace necessary detours.

*   **Trigger:** Before generating a multi-step plan or proposing a specific code edit (`edit_file` call).
*   **Action:**

    `3.1.` **Search for Existing Logic:** Before planning implementation details (especially for utilities, validation, calculations), perform a preliminary search (`grep_search`, `codebase_search`) for existing implementations of the required functionality (referencing Audit Sections 1.B, 1.C). Briefly document findings, **explicitly stating the tool(s) used, query term(s), and specific key findings (files/functions) or confirming none found.** (e.g., "`grep_search` for `format_date`: Found existing utility `utils.format_date`, will reuse." or "`codebase_search` for `validate_user_input`: No specific existing validator found, planning new implementation.").

    `3.2.` **Identify Standards & Verify Alignment:** Explicitly identify the Core Principles or specific sections from `PROJECT_STANDARDS.md` **and relevant technical patterns/structures from the project's `PROJECT_ARCHITECTURE.md`** that are most pertinent to the request. Briefly state how the proposed plan/edit aligns with these standards, **citing the primary relevant section/pattern name(s)**. **Confirm the plan prioritizes the simplest, clearest solution that fully meets requirements and adheres to all standards. Furthermore, when the plan involves generating new functions, methods, or significant blocks of new logic, the AI MUST ensure this planned new code itself will be structured for simplicity (e.g., appropriate length, limited nesting, clear control flow) and adhere to the Single Responsibility Principle. If a conceptual unit of new code seems inherently complex, the plan SHOULD proactively include its decomposition into smaller, more focused units.**
        *   *Example 1 (DI Focus): "Planning to add component X. This requires adhering to the Dependency Injection principles(`PROJECT_STANDARDS.md`) and specific DI configuration guidelines(`PROJECT_ARCHITECTURE.md`). Plan involves updating the DI config module per standard practice."*
        *   *Example 2 (Error Handling Focus): "Plan includes error handling for Y, following Error Handling guidelines(`PROJECT_STANDARDS.md` - Section 2.A) and hierarchy examples(`PROJECT_ARCHITECTURE.md` - `StandardErrorHandling` pattern), using standard logging."*
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
        - `[x/-] 7. Tool Output Verification:` *[Brief confirmation that all tool outputs used in planning were verified for congruence and sufficiency as per Step 3.0.1.]*
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
    *   *Example 1 (With Deferred Edit Issues):* "**Step 6: Deferred Edit Issues & Observations Summary:**\n**Deferred Edit Issues:**\n1. `src/commands/analyze.py` (`run_analysis`): Unintended changes to final `try-except` block. *Behavioral Impact:* May handle `FormatterError` differently, logs might be altered...\n2. `src/commands/docs.py` (`generate_cli`): Unintended changes to logging/error handling. *Behavioral Impact:* Success message no longer prints to console, `OSError` handling less specific...\n**General Observations:**\n1. `src/utils/parser.py`: `parse_data` function could be simplified.\n**Next Steps:** Would you like me to attempt to correct the unintended modifications in `analyze.py` or `docs.py` now, or should we defer these fixes?"
    *   *Example 2 (No Edit Issues, No General Observations):* "**Step 6: Deferred Observations Summary:** No specific deferred observations were noted for future review during this task cycle."
    *   *Example 3 (No Edit Issues, With General Observations):* "**Step 6: Deferred Observations Summary:**\n**General Observations:**\n1. `src/models/user.py`: Consider adding validation for the `email` field format."
*   **Goal:** To capture potentially valuable insights or identified areas for improvement that were outside the immediate scope of the completed task, facilitating systematic follow-up. This step is for items not critical enough to have triggered a `**BLOCKER:**` or formal deferral discussion during the task itself. **Critically, it ensures unintended side effects from edits are clearly documented, analyzed for impact, and explicitly presented to the user for a decision on immediate action.**

---
