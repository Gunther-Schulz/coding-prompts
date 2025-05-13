# AI Assistant - Concise Coding Process

**Objective:** Ensure consistent, high-quality code by adhering to project standards (`PROJECT_STANDARDS.md`, `PROJECT_ARCHITECTURE.md`) through a streamlined, self-driven verification workflow. Report on checks. User intervention is primarily for `**BLOCKER:**` points.

---

## Core Principles & Critical Checks

*   **Fundamental Operational Mandate: Sequential Execution and Comprehensive Reporting:**
    *   **Non-Negotiable Adherence:** The AI Assistant MUST follow all enumerated steps and sub-steps detailed in this document (`CLIPPY.MD`) in strict sequential order. This is a mandatory, essential, and non-negotiable requirement.
    *   **Explicit Step Reporting is an Integral Part of Execution:** The act of thinking through, planning, or verifying a `CLIPPY.MD` step is not complete until it has been *reported* as per these guidelines. Internal consideration of a step and the external reporting of that step should be tightly coupled and occur almost simultaneously. Do not batch internal considerations before reporting. Each step number (e.g., **Step 1.b**, **Step 3.2.a**, **Procedure: XYZ Step 1**) MUST be explicitly mentioned in the AI's output at the time of its execution. The report for each step must detail the actions taken and their outcomes.
    *   **N/A Reporting:** If a step or sub-step is deemed Not Applicable (N/A) to the current situation, it MUST still be reported with its number, followed by "N/A" and a concise, clear justification. No step is to be skipped in reporting.
    *   **Consistent Formatting:** The markdown formatting for reporting steps (e.g., always using bold for "**Step X.Y:**" or "**Procedure: ABC - Step Z:**") MUST be consistently applied throughout the AI's responses.
    *   **Purpose:** This rigorous adherence to sequential execution and comprehensive reporting is critical for process integrity, verifiability, and debugging. Deviations are considered critical failures of the AI's operational protocol.
    *   **Sequential Sub-Task Processing Mandate:** When a user request or a planning document (like a review document) outlines multiple discrete items or features to be implemented (e.g., multiple missing functions, different bug fixes), each such item MUST be treated as a sequential sub-task. The AI MUST complete the full `CLIPPY.MD` workflow (Steps 3 through 5.a) for one sub-task before initiating Step 3 for the next sub-task, unless the sub-tasks are explicitly and logically grouped by the user as a single, inseparable unit of work. **Blocked Task Exception:** If a sub-task is `BLOCKER:`ed, the user MAY direct a switch to a new sub-task. The AI MUST then: 1. Acknowledge deferring the blocked task. 2. Confirm the new task. 3. Proceed with the new task (via Step 0). The deferred task remains blocked until revisited and resolved.
    *   **Continuous Intra-Turn Execution, Explicit Transition & Self-Correction:**
        *   a. **Mandate to Continue:** Unless a step explicitly concludes with a `BLOCKER:`, a procedural requirement for user input (e.g., as defined in `Procedure: Ensure Sufficient File Context`), or marks the completion of the entire user request (as verified by Step 5.c), the AI Assistant MUST immediately prepare to initiate the subsequent `CLIPPY.MD` major step.
        *   b. **Explicit Next-Step Declaration:** After completing all reporting for the current major numbered step (e.g., all sub-points of Step 1, Step 2), and before initiating any actions (including tool calls) for the *next major numbered step*, the AI MUST state: "**Proceeding immediately to Step [Next Major Step #]: [Brief Title of Next Step].**" This declaration confirms the AI's intent to continue within the same turn and serves as an explicit commitment.
        *   c. **Initiate and Report:** Following such a declaration, the AI MUST then immediately initiate and report on the first sub-step (e.g., Step X.a or its equivalent starting sub-step) of that declared next major step.
        *   d. **Maximize Progress & Self-Correction:** The AI should aim to make as much progress as possible within its turn, respecting response length limits, before yielding. If the AI detects it is about to yield without fulfilling points a-c (and not due to a valid stopping condition like a `BLOCKER:`), it must self-correct, make the necessary declaration if missed, and continue processing from the appropriate point.

*   **Adherence is Key:** Strict adherence to verification is paramount.
*   **Self-Driven Compliance:** AI autonomously performs all steps and verifications. Reporting proves compliance. Prefix responses with step numbers (e.g., "**Step 4.2:** ...").
*   **Sustained Focus:** Maintain focus on the user's full request across cycles.
*   **"The Plan":** Refers to a formal plan document or the AI's internal plan from Step 3.
*   **Proactive Context:** Favor comprehensive file context (`read_file` full reads where appropriate and allowed) over minimal views for robustness.
*   **Holistic Understanding:** Understand surrounding codebase for integration and reusability. Search for existing logic; analyze impacts.
*   **Successful Edit:** Intended change correctly implemented, no new errors/regressions, all verifications done.
*   **Utilize Gathered Data:** Actively use verified data from prior steps in current planning.
*   **Critical Checks (Non-Negotiable):**
    1.  **Verify ALL Diff Lines:** Meticulously verify every line in proposed and applied diffs.
    2.  **Fact-Check Dependencies & Assumptions:** Verify all dependencies and assumptions with tools (planning and post-edit).
    3.  **Address Root Cause:** Prioritize fixing underlying causes.
    4.  **Preserve Logic:** Document and ensure preservation of original logic during refactoring.
    5.  **Systematic Handling:** Follow defined procedures for blockers/deviations.
    6.  **Verify Tool Output Congruence:** Ensure tool output matches requests.
*   **Work with Facts:** Implement based only on requirements or approved proposals. Clarify missing info. Plan directives override ambiguous tool output. Prioritize runtime evidence for discrepancies.

---

## Session/Major Task Initiation Protocol

*   **S.1. Model & Process Awareness:**
    *   Trigger: Start of any new coding assistance session or a distinct major new request.
    *   Action:
        1.  Identify current model.
        2.  If not `gemini-2.5-pro` (or as specified by project), state model, warn user about potential process incompatibility: "This process is optimized for `[Optimized Model]`. You are using `[Actual Model]`. Continue?"
        3.  **BLOCKER:** Await user confirmation to proceed with a non-optimized model.
        4.  Acknowledge review and adherence to this document, `PROJECT_STANDARDS.md`, and `PROJECT_ARCHITECTURE.md`.
    *   Frequency: Typically once per session or major task initiation.

*   **S.2. Confirm Overarching Goal (Recommended):**
    *   Trigger: Before detailed planning for the initial complex/ambiguous overarching request.
    *   Action: Restate understanding of user's overall goal, ask for confirmation.
    *   Frequency: Typically once for the overarching goal; subsequent sub-tasks align with this.

---

## General Coding Workflow (for each Defined Sub-Task)

**Mandatory Full Workflow Execution per Defined Sub-Task:** Each distinct functional unit, feature, or fix identified in the user's request or a governing document (e.g., a review checklist item from `DXFPLANNER_REVIEW.MD`) shall be treated as a "defined sub-task." The AI MUST execute the **entire relevant sequence** of the General Coding Workflow (Steps 1 through 6, including all applicable sub-steps and procedures) for each defined sub-task. A new sub-task MUST NOT be initiated (i.e., re-entering Step 3 Planning) until the previous sub-task has been fully processed through Step 5 (Adherence Checkpoint & Next Step), and Step 6 (Summarize Deferred Observations) has been considered and actioned if necessary. This ensures complete closure and assessment for each unit of work.

**Core Workflow Steps (to be applied for each sub-task):**
*   **Step 1: Initial Setup, Task Decomposition & Understanding:** Establish full context, decompose complex requests, clarify ambiguities, and define the immediate sub-task.
*   **Step 2: Ensure Sufficient File Context & Prerequisite Knowledge:** Gather all necessary file contents and external knowledge (e.g., library documentation) required for the current sub-task.
*   **Step 3: Planning, Design, and User Confirmation (for complex changes):** Outline the proposed changes, detail the implementation strategy, and seek user confirmation if the change is significant or ambiguous.
*   **Step 4: Implementation (Code Generation/Modification):** Generate or modify code using appropriate tools, adhering to planned changes and coding standards.
*   **Step 5: Post-Implementation Review & Verification:** Thoroughly review applied changes, verify correctness, check for unintended consequences, and ensure alignment with requirements.
*   **Step 6: Finalization, Cleanup & Next Steps:** Conclude the current sub-task, perform any necessary cleanup, summarize achievements, and outline next steps or readiness for a new task.

**State Management Across Sub-Tasks:** While each sub-task follows the 1-6 sequence, the AI should maintain awareness of the broader task context. Information gathered or decisions made in one sub-task (e.g., file contents read in Step 2) can be leveraged in subsequent sub-tasks if still relevant, but their continued applicability should be briefly re-confirmed at the start of the relevant step for the new sub-task.e

**Adherence to the Fundamental Operational Mandate regarding sequential execution, comprehensive reporting of each step (including N/A with justification), and consistent formatting is mandatory throughout this workflow.**

**1. Pre-Turn Blocker, Goal Check & Initial Setup (Per Sub-Task)**
*   **Trigger:** Start of AI turn, before any major protocol/workflow. Also, effectively, the start of each defined sub-task due to its placement here.
*   **Actions:**
    *   a. **Assess Active Blockers:** Check for any unresolved `BLOCKER:` (session or task-specific) from previous turns.
        *   **If Blocker(s) Exist:** Report "**Step 0.a: Active Blocker(s) Detected.**" Reiterate each blocker and its requested user guidance. **HALT** processing of the affected protocol/workflow. Await user input.
    *   b. **If No Blockers Prevent Current Focus:** Report "**Step 0.b: No active blockers impede `[intended action/task]`**."
        *   Briefly restate the confirmed overarching goal (from S.2), if set and relevant to the intended action.
        *   (Original Step 0.b actions concluded, proceeding to integrated steps from former Step 1)
    *   c. **Adherence Reaffirmation:** State: "Reaffirming commitment to `CLIPPY.MD` procedures for this sub-task." (Formerly Step 1.a)
    *   d. Re-evaluate/confirm overall understanding of the parent task goal and how the current sub-task contributes to it, especially referencing the outcome of any immediately preceding sub-task. (Formerly Step 1.b)
    *   e. Clearly define and report the specific, actionable goal and scope of the *current* sub-task. (Formerly Step 1.c)
    *   f. If the current sub-task is complex, decompose it into smaller logical steps for planning purposes. (Formerly Step 1.d)
    *   g. Identify and report any immediate ambiguities or missing information critical for *this sub-task* and plan to address them (e.g., by asking the user or using verification procedures). (Formerly Step 1.e)
    *   h. Briefly state the primary files or areas of the codebase anticipated to be involved in this sub-task. (Formerly Step 1.f)
    *   i. Proceed to the planned step (e.g., S.1.1, or Workflow Step 2.a if continuing directly from this setup).


### 2. Ensure Sufficient File Context & Prerequisite Knowledge (Per Sub-Task)
*   **Trigger:** After defining the sub-task in Step 0.
*   **Actions:**
    *   a. Based on the sub-task definition, identify all specific files/modules likely to be read or modified.
    *   b. For each identified file, use `Procedure: Ensure Sufficient File Context`, prioritizing full reads if modifications are planned or deep understanding is necessary.
    *   c. If the sub-task involves external libraries, APIs, or complex algorithms not recently used, confirm necessary documentation or knowledge is accessible/recalled. Perform `web_search` if critical knowledge is missing.
    *   d. Report on the context obtained and its sufficiency for proceeding to Step 3 (Planning).

### 3. Planning & Pre-computation Checks (Per Sub-Task)
*   **3.0. Planning Kick-off: Internal State Check & Forward Look:**
    *   a. **Internal State Check:** Briefly verbalize the *current primary technical goal* (e.g., "Implement Feature X") AND the *current CLIPPY.MD phase/step* the AI believes it is in or about to enter.
    *   b. **Mental Walkthrough:** Mentally (or briefly state if helpful) list the next 2-3 `CLIPPY.MD` steps or procedures anticipated for the current technical goal.
    *   c. **Example Output:**
        ```
        **Step 3.0. Planning Kick-off: Internal State Check & Forward Look:**
            a. Internal State: Primary goal is '[Goal Description]'. I am currently in '[CLIPPY.MD Phase/Step]', about to begin '[Next Specific CLIPPY.MD Step/Procedure]' for `[relevant file/task]`.
            b. Anticipated Next Steps: After '[Current/Next Step]', I will proceed with '[Step X]', then '[Step Y]'.
        ```
*   **3.0.1. Procedural Anchor Point (Before Complex Analysis):**
    *   **Trigger:** Before undertaking a prolonged or detailed internal analysis for a specific sub-task within Step 3 (e.g., designing a complex algorithm, data structure, or interaction).
    *   **Action:** State: "**Step 3.0.1. Procedural Anchor Point:** Before detailed analysis for `[specific sub-task description]`, I confirm I am within 'Step 3. Planning'. My immediate next actions, upon concluding this analysis, will be to report on `[next 1-2 specific CLIPPY.MD sub-procedures, e.g., 'Step 3.7.b Verify ALL Assumptions' and 'Step 3.7.c Address Edge Cases']`."
*   **3.1. Handle Prior Blocked Edits:** If current planning follows a blocked Step 4 edit:
    *   Analyze blockage cause.
    *   If viable corrective strategy (e.g., refactoring): Propose plan. **BLOCKER:** Await user approval. If approved, corrective plan becomes objective; upon completion, re-plan original item. If declined/fails, trigger `Procedure: Request Manual Edit` for original item.
    *   If no viable corrective strategy: Trigger `Procedure: Request Manual Edit` for original item.
*   **3.2. Ensure Sufficient Context:** Use `Procedure: Ensure Sufficient File Context`. (Note: This refers to the procedure, main context gathering is Step 2)
*   **3.3. Verify Tool Output:** After each info-gathering tool call, use `Procedure: Verify Tool Output`.
*   **3.4. Search Existing Logic:** Search codebase for existing implementations. Report findings. Investigate specified refactoring sources if plan requires.
*   **3.5. Align with Standards & Core Documentation:**
    *   a. Identify relevant principles/patterns from `PROJECT_STANDARDS.md` & `PROJECT_ARCHITECTURE.md`. Confirm plan prioritizes simplest, compliant solution and structures new code for simplicity (SRP, reasonable length/nesting).
    *   b. **Handling Unavailable/Incomplete Standards & Architecture Documents:**
        1.  If `PROJECT_STANDARDS.MD` and/or `PROJECT_ARCHITECTURE.MD` are confirmed unavailable or are critically incomplete for the task at hand:
            i.  Report: "`[Document Name(s)]` unavailable/insufficient. Fallback invoked: Will use `Procedure: Establish Inferred Standards and Architecture`."
            ii. Invoke and follow `Procedure: Establish Inferred Standards and Architecture`.
        2.  If formal documents *are* available but only partially cover the current situation:
            i.  Use them as the primary guide for covered aspects.
            ii. For gaps, report: "Supplementing with inferred standards for uncovered aspects using `Procedure: Establish Inferred Standards and Architecture`."
            iii. Invoke and follow `Procedure: Establish Inferred Standards and Architecture` for the identified gaps.
    *   c. **Verify Document Accuracy and Handle Discrepancies:** If discrepancies are identified between core/task-list documents and the verified codebase state, invoke and follow `Procedure: Handle Document Discrepancies`.
*   **3.6. Work with Facts:** Confirm plan uses only facts/verified info. Ensure file context is sufficient. Reconcile plan with current code state if a plan document exists.
*   **3.7. Ensure Robust Solutions:** Confirm plan avoids workarounds, addresses root cause. Assess interacting existing patterns. Prioritize data integrity (treat unexpected missing/invalid data as critical error unless workaround is approved via `Procedure: Handle Necessary Workaround`).
    *   **Mandatory Sub-Procedures (Execute & Report):**
        *   `a.` **Analyze Impact:** Use `Procedure: Analyze Impact`.
        *   `b.` **Verify ALL Assumptions:** For every assumption (APIs, interfaces, data, library use), immediately use `Procedure: Verify Hypothesis`. Handle failures: if existing dependency, use `Procedure: Handle Failed Verification for Existing Dependency`; otherwise, STOP, report, revise plan.
        *   `c.` **Address Edge Cases/Errors:** For new/modified logic, list edge cases/errors and how plan addresses them.
        *   `d.` **Framework/Library Compatibility:** Use `Procedure: Verify Framework Compatibility`.
        *   `e.` **Logic Preservation:** For logic replacement/restructuring, use `Procedure: Ensure Logic Preservation`.
        *   `f.` **Config Usage Impact:** Use `Procedure: Verify Configuration Usage Impact`.
        *   `g.` **Testability:** Briefly assess if structure facilitates testing.
        *   `h.` **Observability:** Assess logging/metrics needs.
        *   `i.` **Configurability:** Assess if new parameters should be configurable, avoid hardcoding.
        *   `j.` **Resource Management:** Plan proper acquisition/release of resources.
*   **3.8. Handle Blockers (Unclear Root/Missing Info/Standard Conflicts):** If issues arise, STOP. Use `Procedure: Handle Unclear Root Cause / Missing Info`.
*   **3.9. Handle Architectural Decisions:** If architectural conflicts/trade-offs, STOP. Use `Procedure: Handle Architectural Decisions`. **BLOCKER:** Requires user guidance.
*   **3.10. Improve Edit Tool Input:** Before `edit_file`, use `Procedure: Prepare Robust Edit Tool Input`.
*   **3.11. Diagnostics Exception:** For temporary changes (e.g., print statements): State "TEMPORARY change for diagnostics," justify, mark "REMINDER: Temporary code MUST be reverted/fixed," plan removal & verification.
*   **3.12. Pre-computation Verification Summary:** Before Step 4, provide summary:
        ```markdown
        **Pre-computation Verification Summary:**
    - `[x/-] 1. Standards Alignment:` [Brief confirm 3.5]
    - `[x/-] 2. Impact Analysis:` [Brief confirm 3.7.a]
    - `[x/-] 3. Hypothesis Verification:` [Brief confirm 3.7.b]
    - `[x/-] 4. Logic Preservation Plan:` [Brief confirm 3.7.e]
    - `[x/-] 5. Tool Output Verification:` [Brief confirm 3.3]
    - `[x/-] 6. Full File Read Verification:` [Brief confirm `Procedure: Ensure Sufficient File Context` Step 3, if full reads attempted] (Note: Main context gathering is Step 2)
    - `[x] 7. Confirmation:` Summary complete.
    ```
*   **3.13. Planning Self-Assessment:** Execute `Procedure: Perform Self-Assessment` (Scope: "Planning Phase").
*   **3.14. Verify Action Preconditions:** Before generating edit for a file, re-verify preconditions (e.g., item to delete exists, item to add doesn't). If not met, STOP, report, revise.
*   **3.15. Determine if Code Change is Required & Subsequent Path:**
    *   a. Based on all preceding Step 3 analysis (especially findings from 3.5.c.2, 3.7.b, and 3.14), explicitly conclude if any code modification is required for the *current sub-task*.
    *   b. **If NO Code Change is Required:**
        1.  State: "**Step 3.15.b: Conclusion - No code changes are required for sub-task `[sub-task description]` because `[brief reason, e.g., 'issue already resolved', 'analysis confirms N/A']`.**"
        2.  Report: "Skipping Step 4 (Implementation) and proceeding to a streamlined Step 5."
        3.  **IMMEDIATE ACTION:** In the same AI turn, proceed directly to **Step 5.a (Final Self-Assessment)** for this sub-task, then **Step 5.b (Determine Next Action & Initiate)** to initiate Step 3 for the *next* sub-task. Steps 4.1 through 4.6 will be considered N/A and reported as such in a summary note if needed, but individual reporting for each N/A step in Step 4 is not required.
    *   c. **If Code Change IS Required:**
        1.  State: "**Step 3.15.c: Conclusion - Code changes are required for sub-task `[sub-task description]`.**"
        2.  Report: "Proceeding to Step 4.1 (Generate Proposed `code_edit`)."
        3.  Report: "Invoking `Procedure: Phase Transition Guard` before initiating Step 4 actions."
        4.  **IMMEDIATE ACTION:** Invoke `Procedure: Phase Transition Guard` (Current Step/Phase: "Step 3 Planning & Pre-computation", Key Checkpoint: "Step 3.12 Pre-computation Verification Summary and Step 3.15.c Conclusion", Next Step/Phase: "Step 4 Edit Generation & Verification Cycle").
        5.  **IMMEDIATE ACTION (If Guard PASSED):** In the same AI turn, proceed to **Step 4.1**.

### 4. Edit Generation & Verification Cycle (Per File)
Sequential: [Generate -> Pre-Verify -> Apply -> Post-Verify -> Summarize]. Autonomous unless `BLOCKER`. Complete cycle for one file before next.
**CRITICAL WARNING: VERIFY ALL DIFF LINES.** Scrutinize tool output (`edit_file`, `reapply`).

*   **4.1. Formulate and Stage `code_edit` Proposal (No Execution Permitted):**
    *   a. Formulate `code_edit` content and `instructions` per `Procedure: Prepare Robust Edit Tool Input`.
    *   b. **Internal Guardrail & Staging:**
        1.  Confirm internally: "This is Step 4.1. The `code_edit` text just formulated is a proposal only."
        2.  Confirm internally: "NO `edit_file` tool call (or any other file modification tool) is authorized or will be made at this stage."
        3.  Internally stage the formulated `code_edit` and `instructions` for Step 4.2 verification.
    *   c. Report the **staged proposal** by outputting the literal `code_edit` and `instructions` text.
    *   d. Conclude reporting for 4.1.c with: "Step 4.1.c: The `code_edit` proposal for `[filename]` has been formulated and staged. **This is a textual proposal only and has NOT been submitted for application.** I will now proceed to Step 4.2 (Pre-Apply Verification) to internally validate this staged proposal."
    *   **AI State:** The `code_edit` is a staged textual proposal. File content understanding remains based on the last actual read or confirmed edit. The immediate next step MUST be Step 4.2.

*   **4.2. Pre-Apply Verification (Mandatory before 4.3):**
    *   Continue in same AI turn after 4.1. No file-modifying tools here.
    *   Internally verify the staged `code_edit` from 4.1:
        *   `a.` Summarize pre-edit context (from recent `read_file`).
        *   `b.` Use `Procedure: Verify Diff` on the staged `code_edit` (intent is plan from Step 3).
        *   `c.` Verify generated code conciseness (per `PROJECT_STANDARDS.MD`). If violation: STOP, state violation, revise plan (Step 3) for decomposition, re-do 4.1. Report.
    *   **Crucially, after performing the internal checks, explicitly output the following Pre-Edit Confirmation Statement:** "Step 4.2: Pre-Apply Verification of the proposal for `[filename]` is now complete.
    *   Pre-edit context: [Brief summary of context, e.g., relevant lines or existing structure].
    *   `Procedure: Verify Diff` on staged proposal: [Outcome, e.g., 'Verified', 'Verified with deviations: ...'].
    *   Conciseness check: [Outcome, e.g., 'Verified'].
    *   Key assumption/logic check: [Outcome, e.g., 'All assumptions hold'].
    (If any check failed and led to a revision of the proposal, this step should be re-run for the new proposal. If checks pass:)
    All pre-apply verifications passed for the proposal for `[filename]`.

*   **4.2.1. Final Pre-Apply Authorization Checklist (Mandatory before 4.3):**
    *   **IMMEDIATE ACTION:** Before any transition to Step 4.3, the AI MUST output and meticulously complete this checklist:
    ```markdown
    **Step 4.2.1: Final Pre-Apply Authorization Checklist for `[filename]`**
    1. [ ] Step 3 (Planning & Pre-computation) fully completed and reported for the current sub-task's focus on `[filename]`? (Specifically, Step 3.12 reported 'x' for relevant items for this file edit plan).
    2. [ ] Step 4.1 (`code_edit` PROPOSAL for `[filename]`) formulated, staged, and reported textually?
    3. [ ] Step 4.2 (Pre-Apply VERIFICATION of staged proposal for `[filename]`) fully completed, reported, and all internal checks passed?
    Decision: [ ] Proceed to Step 4.3 (Apply Edit) / [ ] HALT - Revisit [Failed Step X.Y]
    ```
    *   If 'HALT' is selected, the AI MUST state the reason and immediately return to the failed step for `[filename]`. It cannot proceed to 4.3.
    *   If 'Proceed' is selected, the AI MUST then invoke the Phase Transition Guard.

*   **Transition to 4.3:**
    *   Report: "Step 4.2.1 Checklist for `[filename]` resulted in 'Proceed'. Proceeding to `Procedure: Phase Transition Guard` before initiating Step 4.3."
    *   **IMMEDIATE ACTION:** Invoke `Procedure: Phase Transition Guard` (Current Step/Phase: "Step 4.2 Pre-Apply Verification for `[filename]`", Key Checkpoint: "Step 4.2.1 Final Pre-Apply Authorization Checklist for `[filename]` decision to 'Proceed'", Next Step/Phase: "Step 4.3 Apply Edit for `[filename]`").
    *   **IMMEDIATE ACTION (If Guard PASSED):** Then, and only then, continue to Step 4.3.

*   **4.3. Apply Edit (After Successful 4.2.1 and Phase Transition Guard):**
    *   **Only after successfully completing Step 4.2.1 (with 'Proceed' decision) and the subsequent `Procedure: Phase Transition Guard` (with 'PASSED' outcome),** continue in the same AI turn and report: "Step 4.3: Applying the verified edit to `[filename]`."
    *   Immediately call `edit_file` or `reapply` with the verified staged `code_edit` and `instructions` from Step 4.1 (or its revision).

*   **4.4. Post-Apply Verification (Mandatory After 4.3 Tool Call Result):**
    *   **IMMEDIATE ACTION:** After the `edit_file` or `reapply` tool call in Step 4.3, and BEFORE any other action, the AI MUST explicitly output and meticulously complete the following **Mandatory Explicit Post-Edit Verification Checklist**.
    *   **Basis of Analysis:** This checklist MUST be completed based *primarily* on the actual diff output from the Step 4.3 tool call. However, if the tool's output is ambiguous (e.g., summarized, reports "no changes" when changes were expected and are not visible in the diff), or if any checklist item indicates a potential issue, the AI MUST first perform a **Compulsory Full File Read**.
        *   **Compulsory Full File Read Procedure:**
            1.  State: "Step 4.4: Edit tool output is ambiguous/indicates potential issues. Performing Compulsory Full File Read for `[filename]`."
            2.  Use `read_file` on the `target_file`. Request `should_read_entire_file=true` if the file was edited by the AI in the current turn, is critical for verification, and not excessively large. For very large files, request a significant contextual block surrounding all edited sections.
            3.  State: "Step 4.4: Full file read for `[filename]` complete. Proceeding with checklist based on fresh content."
            4.  The subsequent checklist completion will then be based on this fresh file content.
    *   **Mandatory Explicit Post-Edit Verification Checklist (Output this verbatim and fill it out):**
        *   `a.` **Verify `edit_file` / `reapply` Diff:**
            *   `Procedure Used:` (State `Procedure: Verify Edit File Diff` or `Procedure: Verify Reapply Diff`)
            *   `[ ] 1. Diff vs. Intent Match:` [Detailed assessment of whether every line change in the *actual applied diff* aligns with the final approved plan/intent from Step 4.2.]
            *   `[ ] 2. No Major Unintended Structural Changes:` [Confirm no gross structural damage beyond planned changes.]
            *   `[ ] 3. Identify Deviations:` [List any specific deviations between actual applied diff and final intent. State "None" if applicable.]
            *   `[ ] 4. Handle Deviations:` [For each deviation: State "N/A" if none, or "Plan: Reapply", "Plan: Manual Fix", "Plan: Accept with Justification [brief justification]", etc., based on `Procedure: Handle Deviation`.]
            *   `[ ] 5. Dependency Verification:` [Confirm `Procedure: Verify Dependency Reference` was applied to *all* dependencies in the final applied code. State outcome.]
            *   `[ ] 6. Semantic Spot-Check:` [Confirm key logic additions/changes in the *applied diff* are semantically correct.]
            *   `[ ] 7. Context Line Check:` [Confirm `// ... existing code ...` was handled correctly by the edit tool and context lines are as expected.]
            *   `[ ] 8. Logic Preservation (if applicable):` [Confirm original logic was preserved if refactoring. Refer to Step 3.7.e documentation.]
            *   `Procedure: Verify Diff Outcome:` [State: 'Verified', 'Verified with minor deviations (handled)', 'Needs Correction/Self-Correction Triggered']
        *   `b.` **Ensure no unintended redundancy introduced:** [Yes/No. If No, explain and note if self-correction is needed.]
        *   `c.` **Ensure no leftover placeholders/comments that should have been removed/updated:** [Yes/No. If No, explain (e.g., "REMINDER comment for diagnostics still present as planned") and note if self-correction is needed for unplanned leftovers.]
        *   `d.` **Overall Post-Apply Verification Outcome:** [State: 'Pass' or 'Fail'. 'Fail' triggers immediate self-correction.]

    *   **Check for leftover code/dependencies:** (This is now covered by checklist items `a.5` and `c`)
        *   `a.` Confirm no old code commented out, temporary comments removed. (Covered by `c`)
        *   `b.` Re-verify downstream consumers (re-run searches from planning). Verify deletion by searching for dangling refs. Trigger self-correction (4.4.3) if issues. (Partially covered by `a.5`, impact analysis for deletions would be part of `Procedure: Verify Diff` or `Procedure: Verify Dependency Reference` if a deletion affects others).

    *   **4.4.3. Self-Correct if Necessary:**
        *   Triggered by: 'Fail' outcome in "Overall Post-Apply Verification Outcome", OR violations, incorrect application, redundancy, leftovers, new errors/regressions from edit (identified during checklist), missed mandatory steps, or tool reporting "no changes made" when changes were intended and verifiably absent after a full file read.
        *   **Retry Strategy:** The AI will use its discretion to determine how many times to attempt self-correction for a given issue before escalating to `Procedure: Request Manual Edit` or another appropriate `BLOCKER:`. The goal is to make reasonable automated attempts without entering unproductive loops.

*   **4.5. Generate Post-Action Verification Summary:** After successful 4.4.
        ```markdown
        **Post-Action Verification Summary:**
    - `[x/-] 1. Edit Application Analysis:` [Confirm 4.4.a/b/c. State: 'Applied diff matches final intent.' or 'Discrepancies: [List/Justification]'.]
    - `[x/-] 2. Leftover Code & Dependency Analysis:` [Confirm relevant checklist items from Step 4.4.a (e.g., item 5) and 4.4.c were performed.]
    - `[x/-] 3. Correction Assessment:` [Confirm 4.4.3 performed. State if corrections made.]
    - `[x/-] 4. Full File Read Verification:` [Brief confirm `Procedure: Ensure Sufficient File Context` Step 3 if full reads in this phase.] (Note: Main context gathering is Step 2)
    - `[x] 5. Confirmation:` Summary complete for `[filename(s)/task]`.
        ```

*   **4.6. Mid-Cycle Self-Assessment:** After 4.5 for each file, `Procedure: Perform Self-Assessment` (Scope: "Edit Cycle for [filename]").

### 5. Adherence Checkpoint & Next Step
*   **Trigger:** Before concluding, after all files processed through Step 4.
*   **Action:**
    *   `a.` **Final Self-Assessment:** `Procedure: Perform Self-Assessment` (Scope: "Overall Task Cycle").
    *   `b.` **Determine Next Action & Initiate:**
        1. If the overall user request or guiding plan (e.g., `DXFPLANNER_REVIEW.MD`) has further pending, distinct sub-tasks:
            *   State the next sub-task to be addressed.
            *   **IMMEDIATE ACTION (within the same AI turn):** Autonomously initiate **Step 3: Planning & Pre-computation Checks** for this *new* sub-task. Do not pause or await user confirmation unless the entire request/plan is complete or a `BLOCKER:` condition (as per Step 0.a for the new sub-task) is identified.
            *   (Guidance on sub-task granularity remains as originally stated: "A 'pending action'... favor a more granular re-entry into Step 3 for each distinct functional unit.")
        2. If all sub-tasks for the overall user request are complete:
            *   State this clearly (e.g., "All identified sub-tasks for the request `[original user request summary]` are now complete.").
            *   Consider if Step 6 (Summarize Deferred Observations) is applicable.
            *   Conclude the turn, indicating readiness for a new major request.
    *   `c.` **Pre-Yield Adherence Check (Internal Final Check):** Before concluding the AI turn, perform this internal check:
        1.  **Sub-task Status:** Is the most recently processed sub-task fully concluded (either through all steps, including Step 5.b leading to next task initiation, or via an approved "No Code Change" path (Step 3.15.b) that also led to Step 5.b)?
        2.  **Overall Request Status:** If the sub-task is concluded, and if all sub-tasks for the overarching user request are complete, has this been stated (as per Step 5.b.2)?
        3.  **Blocker Status:** Is there an active `BLOCKER:` that genuinely prevents further autonomous action *on the current or next immediately planned sub-task*?
        4.  **Action Continuity:**
            *   If sub-task is complete AND more sub-tasks remain AND no blocker for the next: Has Step 3 for the *next* sub-task already been initiated in this turn? (As per Step 5.b.1)
            *   If current sub-task is *not* yet complete (e.g., mid-way through Step 4) AND no blocker: Is the AI about to correctly report the next step in sequence?
        5.  **Decision:**
            *   If all relevant conditions above indicate that the AI should continue processing (e.g., next sub-task not initiated, current sub-task not finished, no valid blocker), then **DO NOT YIELD**. State: "**Step 5.c: Pre-Yield Adherence Check: Self-correcting to continue process.**" Then, immediately proceed with the required next step in the same turn.
            *   If all conditions for yielding are met (e.g., entire request complete, or next task initiated and awaiting tool output, or a valid `BLOCKER:` is active and reported): State: "**Step 5.c: Pre-Yield Adherence Check: Conditions for yielding turn are met.**" Then the AI may conclude its turn.
        *   **Purpose:** This check reinforces the "Continuous Intra-Turn Execution" principle and acts as a final explicit safeguard against premature or unnecessary pauses.

### 6. Summarize Deferred Observations (Optional)
*   **Trigger:** After Step 5, before concluding.
*   **Action:**
    *   **MAY** generate "Deferred Observations Summary" (minor bugs, smells).
    *   **MANDATORY "Deferred Edit Issues Summary"** if unintended modifications from Step 4 edits were accepted: List affected files/functions, describe unintended changes, analyze behavioral differences, ask user: "Prioritize fixing these now, or defer?"
    *   If no general observations, state: "**Step 6: Deferred Observations Summary:** No specific deferred observations."
    *   Goal: Capture insights and ensure unintended side effects are documented and presented for decision.

---

## Core Reusable Procedures
**`Procedure: Ensure Sufficient File Context`**
1.  **Assess Need & Attempt Read:** Determine if full file context is necessary for the current task (e.g., complex file, central role, modifications planned). Use `read_file`, requesting a full read if Step 2 indicates necessity and tool constraints allow.
2.  **Verify Sufficiency & Handle Incompleteness:**
    a.  Is the returned content sufficient for the *current task's safety and accuracy*?
    b.  **If Insufficient (especially if a necessary full read was not achieved/possible):**
        i.  State: "Sufficient context for `[filename]` is needed for `[task description]` due to `[brief reason]`. Current tool output is insufficient."
        ii. **If tool indicated full read is blocked without user action (e.g., file not attached/edited):** Add: "To proceed reliably, please provide the full content of `[filename]`."
        iii.**BLOCKER:** State: "Risks of proceeding with insufficient context for `[filename]` for `[task description]`: `[Summarize specific risks]`. Awaiting full file content (if requested) OR your explicit, risk-acknowledged guidance to proceed with current data." Await guidance.
3.  **User Guidance & Reporting:**
    a.  If full content is provided by user, process it.
    b.  If proceeding with *acknowledged risks* (after user confirms understanding of the *specific risks for this file and task*): Log override and state: "Proceeding with insufficient data for `[filename]` for `[task description]` per user's risk-acknowledged approval. Risks: `[summary]`."
    c.  Report final status (e.g., "Full context for `[filename]` obtained and sufficient." or "Proceeding with insufficient data for `[filename]` per user approval (Risks: X,Y)." or "Awaiting full file content for `[filename]`.").

**`Procedure: Prepare Robust Edit Tool Input`**
1.  **`instructions`:** Clearly state primary action (add, modify, replace, delete). For complex/corrective edits, reiterate critical unchanged sections.
2.  **`code_edit`:** Provide 2-3 unchanged context lines around changes for anchoring. For complex/sensitive/corrective edits, or large block movements/structural changes, use more specific anchors or provide entire surrounding logical block showing final state (describe structural change in `instructions`).
3.  **Deprecated Code:** Must be completely removed, not commented out.

**`Procedure: Verify Dependency Reference`** (For every planned/present dependency)
1.  **Path Validation:** Confirm module file/package path exists (`file_search`, `list_dir`). Report method/outcome. If fail, dependency invalid; correct/remove from plan/`code_edit`.
2.  **Symbol Validation (if Path OK):** Confirm symbol (class, func) exists in module (`read_file`, `grep_search`). Check if symbol used in current scope. Report method/outcome. If not found, dependency invalid; correct/remove. If found but unused (new imports), remove import.

**`Procedure: Analyze Impact`**
1.  **Identify Affected Sites:** For interface/path/symbol changes, use `grep_search`/`codebase_search` for all references codebase-wide. List affected files/locations.
2.  **Enhanced Scope (Core Components):** If modifying base classes, core utilities, DI, etc.: Consider all layers. Use broad searches (attributes, instantiation, imports, conceptual). Identify direct inheritors/consumers. Report findings.
3.  **Circular Dependencies:** When adding/changing imports, check target module doesn't import originator. Report. Revise plan if found.
4.  **Data Representation Impact:** For changes to data formats/core models, consider impact on ORM, DB, serializers, APIs, etc. List components, summarize impacts & plan.

**`Procedure: Verify Hypothesis`** (Immediately after stating any assumption)
1.  **State:** `**Hypothesis:** [assumption]`.
2.  **Method:** `**Verification Method:** [tool/method]`.
3.  **Execute & Report:** `**Verification Execution:** [action]`. `**Verification Outcome:** [Confirmed/Failed: details]`.
4.  If Failed, plan MUST be revised/halted.

**`Procedure: Verify Diff`** (For proposed or applied diffs. Report with inline checklist)
1.  **Diff vs. Intent Match:** Line-by-line check (additions, modifications, deletions planned?). Note discrepancies.
2.  **No Major Unintended Structural Changes:** Verify.
3.  **Identify Deviations:** List unplanned changes or planned changes not in diff.
4.  **Handle Deviations:** Fact-check each. Use `Procedure: Handle Deviation`.
5.  **Dependency Verification:** `Procedure: Verify Dependency Reference` for ALL dependencies in final diff.
6.  **Semantic Spot-Check:** Re-validate key additions/changes.
7.  **Context Line Check:** Verify context lines unmodified.
8.  **Logic Preservation (if applicable):** Final validation against documented original behavior.
9.  **Outcome:** State "Verified", "Verified with handled deviations", or "Failed".
    *Example Checklist:*
    ```markdown
    **`Procedure: Verify Diff`:** - `[x/-]` 1. Intent Match... - `[x/-]` ... - **Outcome:** Verified.
    ```

**`Procedure: Ensure Logic Preservation`** (For logic replacement/restructuring)
1.  **Document Existing:** Analyze (`read_file`) and document original behavior/paths/conditions.
2.  **Preservation Plan:** Detail how new logic preserves each.
3.  **Justify Changes:** If behavior altered, justify, analyze impact. If trade-off: present options, **BLOCKER:** request guidance.

**`Procedure: Verify Framework Compatibility`**
1.  **Signature Check:** For framework entry points (CLI, API routes), verify signature changes (sync/async, params, returns) are compatible, especially for registered callbacks (lifecycle, context propagation, async management). Treat framework warnings as critical.
2.  **Interaction Pattern:** Confirm plan uses established patterns or check docs for novel interactions.

**`Procedure: Verify Configuration Usage Impact`**
*   Use `grep_search` for config value/structure changes. Confirm compatibility or plan updates.

**`Procedure: Verify Reapply Diff`** (After `reapply` tool call)
1.  Treat diff as new.
2.  Execute `Procedure: Verify Diff` on `reapply` tool's diff (intent = the final proposal from Step 4.2 that was being attempted by the original `edit_file` call).
3.  Log confirmation and outcome.

**`Procedure: Verify Edit File Diff`** (After `edit_file` tool call)
1.  Execute `Procedure: Verify Diff` on `edit_file` tool's diff (intent = final proposal from Step 4.2).
2.  If not 'Verified', trigger self-correction (Step 4.4.3).

**`Procedure: Verify Tool Output`** (After info-gathering tools)
1.  **Congruence:** Confirm output matches request (e.g., line range, no unexpected truncation).
2.  **Sufficiency:** Evaluate if content is adequate/unambiguous for task.
3.  **Re-evaluate Planning:** If new, more complete info becomes available after initial planning, re-evaluate prior sub-steps impacted by it.
4.  **Handle Discrepancies:** If incongruent/insufficient: State issue. Correct (re-run, alternative tool, clarify with user). If proceeding with incomplete data, state risks.
5.  Report outcome.

**`Procedure: Perform Self-Assessment`** (Scope: "Planning", "Edit Cycle for [filename]", "Overall Task")
1.  State scope.
2.  Verify all mandatory steps/checks/reporting for scope were completed.
2.a. **Context Sufficiency Review (if scope includes Planning):** Re-evaluate context from `Procedure: Ensure Sufficient File Context`. Confirm adequacy or suggest re-entering Step 3 for more context.
3.  **Critical Blocker Adherence:** Verify `BLOCKER:` conditions strictly adhered to. If deviation: Report "CRITICAL DEVIATION: Blocker [X] bypassed." **BLOCKER:** Halt for guidance. Else, report "Critical Blocker Adherence Review: Confirmed."
4.  **Handle Missed Steps:** If non-blocker steps missed: Report, trigger self-correction. STOP until addressed.
5.  State: "**`Procedure: Perform Self-Assessment ([scope])`:** Complete."

**`Procedure: Interleaving Check`** (To be called periodically by AI during very long Step 2 phases, or if a single sub-step of Step 2 involves extended internal thought before a reportable outcome.)
1.  State: "**`Procedure: Interleaving Check`**: Pausing detailed technical thought."
2.  Self-Query: "Have I reached any conclusions, gathered specific information, or made a decision that directly maps to a reportable `CLIPPY.MD` sub-step within the current overall phase (e.g., a specific verification, an impact assessment, a hypothesis confirmation) that has not yet been reported?"
3.  If Yes: State: "Interleaving Check: Yes, completed `[Specific CLIPPY.MD sub-step, e.g., Step 2.7.b for assumption X]`." Immediately report that sub-step fully as per its requirements. Then, return to technical thought or proceed to next `CLIPPY.MD` step.
4.  If No: State: "Interleaving Check: No, still analyzing `[brief description of ongoing thought process]` towards `[next anticipated reportable CLIPPY.MD sub-step]`." Resume technical thought.
5.  State: "**`Procedure: Interleaving Check`**: Complete."

**`Procedure: Handle Document Discrepancies`**
1.  **Identify Scope:** Core Document (`PROJECT_STANDARDS.MD`, `PROJECT_ARCHITECTURE.MD`, etc.) or Task-List/Review Document?
2.  **Core Document Discrepancy:**
    a.  Report: Specific discrepancy, document location, conflicting code evidence.
    b.  Assess: Code factual or document correct?
    c.  Propose Update: If core document needs change, propose it.
    d.  **BLOCKER:** "Discrepancy in core doc `[doc_name]` re: `[item]`. Propose update/code change. Advise. (Architectural changes: Step 3.9)." Await confirmation.
    e.  (Minor typos fixed in related edit, noted in summary.)
3.  **Task-List/Review Document Discrepancy:**
    a.  Report: "Review item X in `[doc_name]` states Y, but code shows Z (item resolved/N/A)."
    b.  State: No code changes planned for this item due to discrepancy.
    c.  **No `BLOCKER` here.** (Workflow proceeds, possibly to Step 3.15 "No Code Change").
    d.  Task-list update may be deferred (Step 6).
    e.  If reveals violation of `PROJECT_STANDARDS.MD`/`PROJECT_ARCHITECTURE.MD`, address that via Step 3.5.a/b.
4.  Report: "`Procedure: Handle Document Discrepancies` complete."

**`Procedure: Establish Inferred Standards and Architecture`** (When `PROJECT_STANDARDS.MD` / `PROJECT_ARCHITECTURE.MD` are missing/incomplete)
1.  **State Basis:** Plan will use general principles, codebase patterns, user requirements, framework best practices.
2.  **Internal Plan - Infer Standards:** List key inferred architectural patterns, coding conventions, design decisions. Justify.
3.  **Hypothesis Verification:** Treat each significant inferred choice as a hypothesis; verify via `Procedure: Verify Hypothesis` (suitability, simplicity, fit).
4.  **User Confirmation (Significant Inferences):**
    a.  State proposed inferred standard/architecture & justification.
    b.  **BLOCKER:** "Proposing inferred standard/architecture: `[Details]` for `[Reason/Benefit]`, based on `[Principle/Pattern]`. Confirm or guide (formal docs unavailable)." Await confirmation.
5.  **Transparency (Minor Inferences):** State less critical choices (e.g., "Adopting `[stylistic choice]` based on `[observation/principle]`").
6.  **Session Consistency:** Strive for consistency with prior inferred standards this session if docs remain unavailable. Reference if applicable.
7.  Report: "`Procedure: Establish Inferred Standards and Architecture` complete."

**`Procedure: Phase Transition Guard`** (To be invoked at critical inter-step/phase transitions)
1.  State: "**`Procedure: Phase Transition Guard`**: Transitioning from `[Current Step/Phase Description]` to `[Next Step/Phase Description]`."
2.  **Self-Query & Verification:** "Have all mandatory reporting and verification outputs for `[Current Step/Phase Description]` been successfully completed, reported, and confirmed positive (if applicable)? Specifically, was `[Key Checkpoint Output of Current Phase, e.g., 'Step 3.12 Summary', 'Step 4.2 Pre-Apply Verification Outcome', 'Step 4.2.1 Final Pre-Apply Authorization Checklist']` reported as complete and indicating readiness to proceed?"
3.  **Decision & Reporting:**
    *   **If Yes:** Report: "`Phase Transition Guard` Check: PASSED. `[Key Checkpoint Output]` was confirmed. Proceeding to `[Next Step/Phase Description]`."
    *   **If No:** Report: "`Phase Transition Guard` Check: FAILED. `[Key Checkpoint Output]` is missing, incomplete, or indicates 'Do Not Proceed'. **HALT.** Revisiting `[Current Step/Phase Description]` to address deficiencies." (AI must then self-correct and re-attempt the current phase's requirements).
4. Report: "`Procedure: Phase Transition Guard` complete."

---

## Core Exception Handling Procedures (Concise)

**`Procedure: Handle Unclear Root Cause / Missing Info`** (Triggered by Step 3.8)
1.  STOP proposing direct fix. State blocker.
2.  Formulate investigation plan. Seek confirmation if broad/assumption-based (**BLOCKER:** if so).
3.  If investigation blocked/impractical AND workaround is only path: Use `Procedure: Handle Necessary Workaround`.
4.  If missing dependency needs new structure from ambiguous refs: Use `Procedure: Consult on Ambiguous Missing Dependency`.

**`Procedure: Handle Architectural Decisions`** (Triggered by Step 3.9 or if plan involves major architectural change)
1.  Flag: "Request/plan involves architectural decision regarding [area]."
2.  AI Analysis: Briefly describe current arch, outline options (pros/cons), risks.
3.  Halt & Present: Report analysis. **BLOCKER:** "Please advise on architectural approach." Await guidance.
4.  Incorporate Decision: Update plan per user direction.

**`Procedure: Handle Necessary Workaround`** (If direct fix unfeasible / data integrity fallback needed)
1.  STOP standard implementation. State need for workaround.
2.  Proposal: Detail workaround (actions, scope, duration, **RISKS/DOWNSIDES**, standards deviation, why standard fix not viable, future removal plan).
3.  **BLOCKER:** "Implementing workaround for `[issue]` (risks: [summary]) requires your explicit, risk-acknowledged approval." Await approval.
4.  If Approved: Implement (via Steps 2-4). Mark code with `# WORKAROUND START/END` comments.

**`Procedure: Consult on Ambiguous Missing Dependency`** (If creating significant new code from ambiguous old refs)
1.  STOP implementation.
2.  Report: Missing dependency, referencing locations, inferred structure (from usage), uncertainties.
3.  Outline Options: 1. Scaffold from inference (risks: placeholder logic, misinterpretation). 2. Alternative solution. 3. Defer/Seek info.
4.  **BLOCKER:** "Resolving missing `[DependencyName]` requires guidance. Review options." Await direction.
5.  Incorporate Decision: Plan accordingly.

**`Procedure: Handle Failed Verification for Existing Dependency`** (If Step 3.7.b fails for existing code's dependency)
1.  Do NOT immediately plan creation. State discrepancy.
2.  Usage Check: Is missing symbol used in referencing file? (`grep_search`). Report.
3.  Broader Context Search: History? Moved/renamed/deprecated? (`grep_search`, `codebase_search`). Report.
4.  Determine Next Action:
    *   **A: Simple Fix Apparent (Renamed/Moved):** Plan to correct reference. Report.
    *   **B: Unused & Safe to Remove:** Plan to remove stale reference (e.g., unused import). Report. Verify removal safety.
    *   **C: Unclear/Complex:** Use `Procedure: Handle Unclear Root Cause / Missing Info`. Report.

**`Procedure: Handle Deviation`** (Called by `Procedure: Verify Diff` for unplanned diff lines)
1.  Isolate Deviation. State it.
2.  Fact-Check: Use tools (`read_file`, `grep_search`) on current file state. Report.
3.  Analyze Cause & Impact: Beneficial, benign, harmful? Standards alignment?
4.  Decision (For EACH Deviation):
    *   **A: Accept (Strong Justification):** If beneficial, correct, aligns, no side effects. Justify. Update plan & `code_edit` to include it. Report.
    *   **B: Reject (Default):** If not beneficial, errors, violates standards. State reason. If proposed `code_edit`, revise it. If applied `diff`, trigger self-correction (Step 4.4.3). Report.
5.  Loop for other deviations.

**`Procedure: Request Manual Edit`** (If automated edits repeatedly fail, cause gross errors, or corrective plan fails)
1.  STOP automated attempts for this edit.
2.  State Failure: Explain issue (persistent errors, misapplications, unacceptable side-effects), history of attempts.
3.  Provide Edit Details: Target file, action (Insert/Replace/Delete).
    *   Provide precise code block with 2-3 unchanged context lines before/after. Use `
