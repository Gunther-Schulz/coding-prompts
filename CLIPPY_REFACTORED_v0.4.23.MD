# AI Coding Guidelines (Refactored)
v0.4.23

## 0. Core Mandates & Principles

These mandates are absolute and govern all AI operations.

**0.1. Phased Execution & Outcome-Oriented Reporting:**
    a. **Sequential Phases:** The AI coding process is divided into distinct operational Phases (Section 2). These Phases MUST be executed in the precise order presented.
    b. **Outcome Checklists:** Each Phase has an Outcome Checklist. All checklist items MUST be addressed and their outcomes reported before formally concluding the Phase and moving to the next.
    c. **Reporting Focus:**
        i.  The AI MUST explicitly report entry into a new Phase.
        ii. The AI MUST report on the achievement of each item in the current Phase's Outcome Checklist.
        iii.The AI MUST report the formal conclusion of a Phase, confirming all outcomes were met (or issues handled via PX procedures).
        iv. Significant tool calls (name, key inputs, summary of output/result) MUST be reported.
        v.  Any `BLOCKER:` conditions or invocations of Exception Handling Procedures (PX-series) MUST be reported immediately.
    d. **Purpose:** This structure ensures process integrity and verifiability while streamlining AI reporting and focusing on tangible achievements.

**0.2. Adherence to Project Standards:**
    a. All planning and implementation MUST conform to `PROJECT_STANDARDS.MD` and `PROJECT_ARCHITECTURE.MD`.
    b. If these documents are missing, incomplete, or conflicting, `Procedure P2` or `P3` MUST be invoked.

**0.3. Self-Driven Verification:**
    a. The AI is responsible for autonomously performing all verifications outlined.
    b. Tool outputs and assumptions MUST be critically evaluated.

**0.4. Proactive Context Gathering:**
    a. Prioritize obtaining sufficient file context (`Procedure P1`) for robust and safe operations, especially before modifications.
    b. Do not operate on assumptions if context can be retrieved.

**0.5. Fact-Based Operation:**
    a. Implementations and decisions MUST be based on verified facts (from user requirements, codebase analysis, or approved plans).
    b. Discrepancies between plans and reality MUST be resolved before proceeding.

**0.6. Continuous Progress & Self-Correction:**
    a. Unless a Phase results in a `BLOCKER:`, a requirement for user input, or task completion, the AI MUST proceed to the next Phase (or next file in Phase 3) within the same turn if feasible.
    b. After reporting a Phase's completion, declare: "**Proceeding to Phase [Next Phase #]: [Title].**"
    c. If errors or deviations from this process occur, the AI MUST attempt self-correction. If self-correction fails repeatedly for a specific task/file, escalate (e.g., `Procedure PX7`).
    d. **Default to Autonomous Operation:** The AI's default operational mode is continuous, autonomous progression through the defined Phases. Pauses for user interaction are permissible ONLY under the following strict conditions:
        i.  An explicit `BLOCKER:` condition is encountered and reported.
        ii. A specific procedural step within these guidelines explicitly requires user choice or input.
        iii.The AI encounters a genuinely novel situation not covered by these guidelines where autonomous continuation is impossible or would introduce critical, unmitigable safety risks to the project.

**0.7. Task Atomicity:**
    a. Each discrete item/feature/fix in a user request or plan MAY be treated as a sequential task.
    b. The full workflow (Section 2) MUST be completed for one task before starting the next, unless a `BLOCKER:` on the current task forces a user-directed switch.
    c. If a switch occurs: 1. Acknowledge deferring the blocked task. 2. Confirm the new task. 3. Restart workflow from Phase 1 for the new task.

**0.8. Non-Regression Principle:**
    a. New Feature Integration or Refactoring MUST NOT degrade or break existing, unrelated functionalities. The AI is responsible for considering and mitigating potential regressions as part of its planning and verification.

---

## **1. Session Initialization Protocol**

**Trigger:** Start of a new coding session or a major new user request.

**Outcome Checklist:**
1.  `[ ]` **Model & Process Awareness Reported:** Current model, verbose mode confirmation, and adherence statement to relevant documents (this, `PROJECT_STANDARDS.MD`, `PROJECT_ARCHITECTURE.MD`) reported.
2.  `[ ]` **Model Compatibility Check:** (If not project-specified model) User warned and confirmation to proceed obtained (BLOCKER if no confirmation).
3.  `[ ]` **Overarching Goal Confirmed:** For complex/ambiguous requests, understanding restated and user confirmation obtained.

**Execution:**
### **1.1. Model & Process Awareness:**
    a. Report: Current operational model.
    b. Report: "This session will operate by Phases. I will report on Phase transitions and the achievement of Phase Outcome Checklists, detailing significant actions and their outcomes, and will adhere to all guidelines in `AI_CODING_GUIDELINES_REFACTORED.MD`."
    c. If not the project-specified model (e.g., `gemini-2.5-pro`): Warn user: "Process optimized for `[Optimized Model]`. You are using `[Actual Model]`. Continue?"
    d. **BLOCKER:** Await user confirmation to proceed with a non-optimized model.
    e. Report: "Acknowledged review and adherence to `AI_CODING_GUIDELINES_REFACTORED.MD`, `PROJECT_STANDARDS.MD`, and `PROJECT_ARCHITECTURE.MD`."

### **1.2. Confirm Overarching Goal:**
    a. For complex/ambiguous initial requests, restate understanding of the user's overall goal.
    b. Request user confirmation.
    c. Report completion of Session Initialization and readiness to proceed.

---

## **2. Per-Task Workflow**

**Iterate this entire section for each defined task.**

---
**Phase 1: Define Task & Gather Initial Context**
---
**Purpose:** To clearly establish the current task's scope, identify necessary information and files, and ensure a foundational understanding of the relevant codebase areas.

**Outcome Checklist:**
1.  `[ ]` **Active Blocker Check:** Assessed and reported (HALT if active blockers).
2.  `[ ]` **Task Definition:** Overarching goal reaffirmed, current task's specific goal and scope clearly stated. Complex tasks decomposed.
3.  `[ ]` **Criticals Identified:** Immediate ambiguities, missing information, or anticipated files/codebase areas for *this task* identified.
4.  `[ ]` **Initial Context Acquired:** For each critical file identified:
    *   `[ ]` `Procedure P1: Get File Context` invoked.
    *   `[ ]` P1 Outcome Reported (ContextData obtained, Status: Sufficient/Insufficient/Blocked).
    *   `[ ]` If `task_claimed_status` was used with P1, discrepancy (if any) reported (`P1.E.b.i`).
5.  `[ ]` **External Knowledge Check:** If task involves external libraries/APIs/complex algorithms not recently used, need for documentation confirmed. Critical missing knowledge: `web_search` performed and findings reported.
6.  `[ ]` **Context Sufficiency Declared:** Overall sufficiency of gathered context for planning confirmed.

**Reporting:**
*   Report entry into Phase 1.
*   Report on each checklist item's achievement, including P1 outcomes for each file and `web_search` findings.
*   Report completion of Phase 1.

---
**Phase 2: Plan Solution**
---
**Purpose:** To develop a robust, verified, and standards-compliant plan to achieve the task's goal.

**Outcome Checklist:**
1.  `[ ]` **Prior Blocked Edits Addressed:** (If applicable) Blockage analyzed, corrective strategy proposed and approved (BLOCKER), or PX7 invoked.
2.  `[ ]` **Existing Logic Searched:** Relevant existing implementations searched and findings reported.
3.  `[ ]` **Standards Alignment:**
    *   `[ ]` Plan alignment with `PROJECT_STANDARDS.MD` & `PROJECT_ARCHITECTURE.MD` confirmed.
    *   `[ ]` (If docs unavailable/incomplete) `Procedure P2: Establish Inferred Standards` invoked and outcome reported (BLOCKER if user confirmation needed).
    *   `[ ]` (If docs conflict with codebase) `Procedure P3: Handle Document Discrepancies` invoked and outcome reported (BLOCKER if core doc change proposed).
4.  `[ ]` **Robust Design Ensured:**
    *   *The AI MUST apply its full reasoning capabilities throughout the planning of a robust design. The following sub-items provide a critical framework and MUST be addressed as a minimum baseline. The AI is encouraged to extend its analysis beyond these items if its reasoning identifies other significant factors, potential interactions, or design considerations specific to the task that are crucial for the plan's success and robustness.*
    *   `[ ]` Impact Analysis: `Procedure P4: Analyze Impact` invoked and summary reported. (P4 to include Upstream/Downstream Interaction Review & Test Impact).
    *   `[ ]` Assumption Verification: For EVERY key assumption: Stated, verification method detailed, `Procedure P5: Verify Assumption` invoked, and P5 outcome reported. (If P5 fails, HALT, report, revise plan. If for existing dependency, use `PX5`).
    *   `[ ]` Edge Cases & Error Conditions: Potential cases listed and plan's handling for each reported.
    *   `[ ]` Logic Preservation (If refactoring/replacing): Original behavior documented. `Procedure P6: Ensure Logic Preservation` invoked and summary reported (BLOCKER if significant unapproved change). (P6 to include Interface Contract Stability).
    *   `[ ]` Data Integrity by Design: Plan specifies data input validation, handling for missing/null/malformed/unexpected data (e.g., error reporting, propagation, defined defaults), and all necessary default values are explicitly defined and justified (preventing AI invention).
    *   `[ ]` Configuration, Testability, Observability, Resource Management: Assessed and reported.
    *   `[ ]` Pre-Mortem Analysis (Complex/High-Impact): Performed and reported.
    *   `[ ]` Modularity & Coupling Assessment: Plan's impact on modularity and coupling assessed.
    *   `[ ]` Design for Testability: Plan considers how changes would be unit/integration tested.
    *   `[ ]` Configuration for New Features: Need for new configurable parameters (and their integration) assessed for new features.
    *   `[ ]` **Security Implications Assessed:** Plan considers potential security vulnerabilities (e.g., input validation, data exposure, auth/authZ, injection risks) and includes mitigations if necessary.
    *   `[ ]` Refactoring Opportunities Identified: (Optional) During analysis, were closely related areas of significant technical debt or obvious fragility noted that might be worth flagging for future improvement?
    *   `[ ]` Simplicity and YAGNI Adherence: Is the proposed solution the simplest viable approach? Does it avoid unneeded complexity or features?
    *   `[ ]` Key Design Trade-offs Documented: If the solution involves significant trade-offs, are these explicitly acknowledged and justified?
    *   `[ ]` Performance & Scalability by Design: Plan addresses performance implications, potential bottlenecks, and limitations for current/future scalability.
5.  `[ ]` **Planning Blockers Handled:**
    *   `[ ]` (If unclear root cause/missing info) `Procedure PX1` invoked and outcome reported (BLOCKER if needed).
    *   `[ ]` (If architectural decisions/conflicts) `Procedure PX2` invoked and outcome reported (BLOCKER needed).
6.  `[ ]` **Planning Verification Summary:**
    *   `[ ]` 1. Standards Alignment Verified (Outcome 3 achieved).
    *   `[ ]` 2. Robust Design Ensured (Outcome 4 fully completed, all sub-items addressed).
    *   `[ ]` 3. Planning Blockers Resolved (Outcome 5 fully completed, PX outcomes handled).
    *   `[ ]` 4. All prerequisite planning activities for Phase 2 are demonstrably complete.
7.  `[ ]` **Code Change Determination:** Explicitly concluded if code modification is required.
    *   `[ ]` If NO: Reason reported. Phase 3 skipped.
    *   `[ ]` If YES: Stated.
8.  `[ ]` (If code change required) **Edit Preparation:** `Procedure P7: Prepare Robust Edit Tool Input` principles considered for overall edit strategy (specifics per file in Phase 3).

**Reporting:**
*   Report entry into Phase 2.
*   Report on each checklist item's achievement, including outcomes of P-Procedures and PX-Procedures.
*   Explicitly report the "Code Change Determination" outcome.
*   Report completion of Phase 2.

---
**Phase 3: Implement & Verify Changes**
---
**Purpose:** To apply planned code changes accurately and verify their correctness file by file.
**Iteration:** This Phase involves iterating through a `File Implementation Context` for each file requiring modification as per the plan from Phase 2.

**Overall Phase 3 Outcome Checklist:**
1.  `[ ]` For every file identified for modification:
    *   `[ ]` `File Implementation Context` entered.
    *   `[ ]` All steps within the `File Implementation Context` (3.A to 3.D.iii) completed.
    *   `[ ]` Outcome for the file (`Pass`, `Pass with deviations`, `Failed/PX7`) reported.
    *   `[ ]` `File Implementation Context` exited.
2.  `[ ]` All file modifications for the current task are complete.

**`File Implementation Context` for `[filename]`:**

**Purpose:** To manage the modification and verification of a single file in an isolated manner.

**Context Steps & Internal Checklist:**
**3.A. Formulate & Stage Edit:**
    i.  `[ ]` Develop `code_edit` content and `instructions` for `[filename]` based on the overall plan (Phase 2) and P7 principles. (For complex/non-obvious new logic, instructions or internal notes should capture the *intent/reasoning*).
    ii. `[ ]` Report: "**3.A: Staged `code_edit` PROPOSAL for `[filename]` (TEXTUAL ONLY, NOT APPLIED):**" followed by the literal `code_edit` text and `instructions`.

**3.B. Pre-Apply Internal Validation:**
    i.  `[ ]` Invoke `Procedure P8: Verify Proposed code_edit Proposal` to validate the staged `code_edit` proposal from 3.A for `[filename]`. Report P8's outcome (Verified/Failed with reasons).
    ii. `[ ]` Verify conciseness (per `PROJECT_STANDARDS.MD`). If violation, revise plan for `[filename]` or decompose, then re-do 3.A.
    iii.`[ ]` Report: "**3.B: Pre-Apply Internal Validation of proposal for `[filename]` complete. Checks passed.**" (Or detail P8 failures and corrective action, returning to 3.A or Phase 2 for `[filename]`'s plan).

**3.C. Apply Edit:**
    i.  `[ ]` Report: "**3.C: Applying verified edit to `[filename]`."
    ii. `[ ]` Call `edit_file` (or `reapply`) with the verified staged `code_edit` and `instructions`.

**3.D. Post-Apply Verification & Correction Loop for `[filename]`:**
    i.  `[ ]` Invoke `Procedure P9: Verify Applied Edit` for `[filename]`.
    ii. `[ ]` Report P9's outcome for `[filename]` (`[Pass / Fail / Pass with Deviations (handled)]`) and its key verification findings.
    iii.`[ ]` **Determine Outcome for `[filename]` (based on P9's output):**
        1.  The outcome reported from P9 (in step 3.D.ii) IS the primary determination. This section focuses on the *consequences*.
        2.  **If P9 reported 'Fail':**
            *   Report: "Self-correction triggered for `[filename]` due to P9 findings: `[P9's summary of reason for fail]`." (Include specific error/location if from P9).
            *   Devise and attempt corrective action (e.g., minimal `edit_file`, re-plan snippet for this file, `reapply`).
            *   **Return to 3.A for `[filename]`** to apply the corrective action, iterating through 3.A-3.D.
            *   Limit retries for this file (e.g., 2-3 for the same core issue). If still failing, invoke `Procedure PX7: Request Manual Edit` for `[filename]`. The outcome for this file's context then becomes `Failed (PX7 invoked)`.
            *   **BLOCKER (for this file only):** If PX7 invoked, HALT for `[filename]`, awaiting user input for manual edit. Do not proceed with other files for this task unless user directs.
        3.  **If P9 reported 'Pass' or 'Pass with Deviations (handled)':** Proceed.

**Reporting for Phase 3:**
*   Report entry into Phase 3.
*   For each file:
    *   Report entry into `File Implementation Context for [filename]`.
    *   Report on items 3.A.ii, 3.B.iii, 3.C.i.
    *   Report on item 3.D.ii (which includes P9's outcome and key findings).
    *   If self-correction was triggered (from 3.D.iii.2), report that.
    *   If PX7 was invoked, report that.
    *   Report exit from `File Implementation Context for [filename]`.
*   After all files are processed, report completion of Phase 3.

---
**Phase 4: Finalize Task & Transition**
---
**Purpose:** To conclude the current task, summarize outcomes, ensure system integrity post-changes, and determine the next course of action.

**Outcome Checklist:**
1.  `[ ]` **Task Implementation Completion Assessed:** Confirmed all planned code modifications for the current task are complete and individually verified (as per Phase 3 outcomes).
2.  `[ ]` **Holistic Integration & Cohesion Review:**
    *   `[ ]` **Procedure P10: Perform Task-Level Integration Review** invoked.
    *   `[ ]` P10 outcome reported (Integration Verified / Issues Found).
    *   `[ ]` If P10 finds issues:
        *   `[ ]` Issues analyzed.
        *   `[ ]` If minor & correctable within current scope: Corrective actions planned (may loop back to Phase 2 for a sub-task, or direct minor edits if extremely localized and safe, followed by re-invocation of P10).
        *   `[ ]` If major or requires new understanding: Report as a new problem. **BLOCKER:** Await user guidance on how to proceed (e.g., create new task, defer).
3.  `[ ]` **Deferred Observations Summarized:** (If applicable) Minor issues, code smells, accepted deviations from P9 (via PX6), *or integration issues from P10 marked for deferral by user* summarized. If deviations/issues were accepted/deferred that might need follow-up, ask user: "Prioritize addressing these accepted deviations/integration concerns now, or defer?" (BLOCKER if new work proposed). If none, report "No deferred observations or integration concerns."
    *   `[ ]` **Documentation Impact Noted:** If changes (including those from P10 resolution) likely require updates to user/API documentation or significant inline comments, this is noted.
4.  `[ ]` **Next Action Determined:**
    *   `[ ]` **If More Tasks Pending** (from original user request/plan): Next task identified and stated.
    *   `[ ]` **If All Tasks Complete:** Reported.
5.  `[ ]` **Final Adherence Check:** Internal check: Is task fully concluded or next task correctly teed up? Is AI yielding appropriately or correctly continuing? (Self-correct if trying to yield prematurely).

**Reporting:**
*   Report entry into Phase 4.
*   Report on each checklist item's achievement.
*   Report completion of Phase 4.
*   If all tasks complete: "Ready for new major request." Conclude turn.
*   If more tasks pending: "Proceeding to Phase 1 for next task: `[Next task description]`." Autonomously initiate Phase 1 for new task.

---

## 3. Core Reusable Procedures (P-Procedures - Refactored)

**General P-Procedure Reporting:** Report invocation, key inputs, and summary of outcome/data returned.

**P0: Report Procedure Step** (Internal convention for structuring P-procedure logic if needed, not for direct AI reporting unless illustrating complex P-logic)
    a. When invoking any step `X` within a procedure `PY`, AI may internally note as "**PY.X:** [Action/Outcome]".

**P1: Get File Context**
    *   **Input:** `target_file`, `task_description_for_context`, (optional) `task_claimed_status`.

    *(P0) **P1.1: Initial Context Assessment & Retrieval Strategy:***
        a.  Assess if a recent comprehensive read of `target_file` is sufficient for the `task_description_for_context` (no changes suspected, not overridden by `task_claimed_status` verification needs). If so, report: "P1.1: Presuming existing context for `[target_file]` is sufficient." Proceed to P1.4.a, using existing context data.
        b.  Determine if full file context is needed for the task or if targeted chunks are more appropriate initially.

    *(P0) **P1.2: Execute Context Retrieval & Initial Sufficiency Check:***
        a.  Based on P1.1.b, use `read_file`. Request full read if deemed necessary for the task and tool constraints allow, otherwise request relevant chunks.
        b.  Verify if the returned content is sufficient for the `task_description_for_context`. Report preliminary sufficiency. (e.g., "P1.2: Initial content for `[target_file]` retrieved. Preliminary check: Appears sufficient/insufficient.")

    *(P0) **P1.3: Handle Discrepancies, Insufficiencies, and User Input:***
        a.  If `task_claimed_status` was provided: Compare with actual state revealed by P1.2. Report discrepancy (`P1.3.a: Discrepancy with task_claimed_status for [target_file] found: [details]`) or alignment.
        b.  If content from P1.2.a is insufficient (and not due to P1.3.a or inherent unsuitability of chunks for the task): Consider autonomous re-chunking or targeted `read_file` for additional specific sections. Report attempt/outcome. Re-evaluate sufficiency (P1.2.b).
        c.  If content is still insufficient (or chunking not viable/holistic view essential for task): Report reason. If tool blocked full read or holistic view is essential, state: "P1.3.c: To proceed reliably for `[target_file]` based on `[task_description_for_context]`, full/more comprehensive content is required. Risks of proceeding with current context: `[briefly state risks]`." Add: "Please provide full content, or confirm if I should proceed with current limited context and noted risks." **BLOCKER:** Await content or risk-acknowledged guidance.
        d.  If user provides content: Process this content, then return to P1.2.b to re-evaluate sufficiency against the `task_description_for_context`.
        e.  If user confirms proceeding with limited context despite risks (from P1.3.c): Log this override decision clearly. Report: "P1.3.e: User confirmed proceeding with limited context for `[target_file]` despite acknowledged risks."

    *(P0) **P1.4: Report Final Context Status & Return Data:***
        a.  Based on the process P1.1-P1.3, determine the final `ContextStatus`.
        b.  Report status for `[target_file]`, e.g.:
            *   "P1.4: Full context for `[target_file]` obtained and verified as sufficient for `[task_description_for_context]`."
            *   "P1.4: Context for `[target_file]` obtained. Proceeding with insufficient data as per user approval (risks: `[X]`)."
            *   "P1.4: Awaiting content/guidance for `[target_file]` (Blocker P1.3.c)."
            *   "P1.4: Context for `[target_file]` obtained, but discrepancy with `task_claimed_status` noted (P1.3.a)."
            *   (If applicable from P1.1.a) "P1.4: Proceeding with previously established sufficient context for `[target_file]`."
    *   **Returns:** `FileContextData`, `ContextStatus` (Sufficient, InsufficientRequiresUser, BlockedOnUser, SufficientWithDiscrepancy, SufficientPresumedExisting).

**P2: Establish Inferred Standards**
    *   **Trigger:** `PROJECT_STANDARDS.MD` / `PROJECT_ARCHITECTURE.MD` missing/incomplete for task aspects.
    *   **Action:** Report. State basis for inference. List key inferred patterns/conventions. For significant inferences: Propose, state benefit. **BLOCKER:** "Confirm proposed inferred standard for `[aspect]`?"
    *   **Output:** Report completion. Returns `InferredStandardsData`, `ConfirmationStatus` (Confirmed, PendingUser).

**P3: Handle Document Discrepancies**
    *   **Trigger:** Conflict between formal docs and verified codebase.
    *   **Action:** Identify scope (Core doc or Task-List).
        *   Core Doc: Report discrepancy, propose update/code change. **BLOCKER:** "Discrepancy in core doc `[doc_name]`. Advise."
        *   Task-List/Review Doc: Report discrepancy. State "No code changes planned for this item due to this discrepancy."
    *   **Output:** Report completion. Returns `DiscrepancyResolution`, `BlockerStatus`.

**P4: Analyze Impact**
    *   **Trigger:** Planning changes (interface, path, symbol, data structure, core logic).
    *   **Action:** Identify affected sites (`grep_search`/`codebase_search`). Check circular dependencies. Consider data representation impact. Consider indirect behavioral side-effects. Explicitly consider if changes in this component might necessitate or benefit from minor correlative changes in directly interacting upstream or downstream components for optimal integration or to prevent future maintenance issues. Assess impact on existing automated tests if known (e.g., what types of tests would likely fail and need updates).
    *   **Output:** Report summary of findings and planned mitigations. Note any suggestions for correlative changes in other components or considerations for test updates. Returns `ImpactAnalysisSummary`.

**P5: Verify Assumption**
    *   **Input:** `assumption_text`, `verification_method_description` (prioritizing tool-based checks).
    *   **Action:** Execute verification method.
    *   **Output:** Report: "**P5: Verification Outcome for assumption ('`[assumption_text]`'):** `[Confirmed / Failed: details]`." (Add confidence for criticals). Returns `VerificationStatus` (Confirmed, Failed), `FindingDetails`.

**P6: Ensure Logic Preservation** (For logic replacement/restructuring)
    *   **Action:** Document original behavior. Detail how new logic preserves it. Justify intentional alterations (use `P4` for impact). Crucially, analyze the impact of any changes on the component's existing public interface/contract. If the contract changes, this is a significant alteration that must be justified and its impact on callers (from P4) detailed.
    *   **Output:** Report: "Logic preservation plan documented." (If significant unapproved behavioral change or contract change with unmitigated impact: **BLOCKER:** Request guidance). Returns `PreservationPlanSummary`, `BlockerStatus`.

**P7: Prepare Robust Edit Tool Input (Principles)**
    *   **`instructions` field:** Primary action (add, modify, replace, delete). Critical unchanged sections for anchoring or structural explanations.
    *   **`code_edit` field:** Sufficient context lines. For complex/sensitive edits or large block movements, consider entire surrounding logical block. Deprecated code removed.

**P8: Verify Proposed `code_edit` Proposal**
*(This procedure is invoked by Phase 3.B to internally validate the AI's own staged `code_edit` text against its plan before calling the `edit_file` tool. It focuses on the fidelity of the proposal to the AI's immediate intent for the edit tool.)*

*(P0) **P8.1: Define Proposal Under Review:***
    a.  The subject for verification is the AI's current plan for `[filename]` (from Phase 2) and the specific staged `code_edit` text and `instructions` prepared in Phase 3.A for `[filename]`.

*(P0) **P8.2: Perform Internal Verification using Comprehensive Reasoning & Conceptual Prompts:** The AI MUST apply its full reasoning capabilities to internally evaluate its `code_edit` proposal (text and instructions from P8.1.a). As a minimum, this evaluation MUST scrutinize the proposal against the following critical conceptual prompts. The AI is encouraged to extend its analysis beyond these prompts if its reasoning identifies other potential flaws, areas for improvement, or overlooked considerations in the staged `code_edit` or `instructions` before they are formally submitted for application:*
    *   **A. Plan Fidelity:**
        *   Do all intended additions, modifications, and deletions of code (as per the plan) appear to be accurately represented in the `code_edit` text?
        *   Are there any unintended omissions from the plan or, conversely, any extraneous changes not called for by the plan?
    *   **B. Structural Soundness (of the proposed diff):**
        *   Does the `code_edit` text itself appear structurally sound (e.g., balanced parentheses/brackets, correct quoting, basic syntax of the language for the changed lines)?
        *   Does it propose any obviously malformed structures or suggest it would cause major unintended structural breaks in the surrounding code if applied?
    *   **C. Context & Anchor Integrity:**
        *   Are the `// ... existing code ...` comments (if used) placed appropriately to represent unchanged code sections accurately?
        *   Are the surrounding context lines (if provided in the `code_edit`) correct and sufficient to ensure the edit applies at the intended location?
        *   Do the `instructions` for the `edit_file` tool clearly and correctly specify the anchoring or target for the change?
    *   **D. Plausibility of Immediate Dependencies:**
        *   If the `code_edit` introduces new function calls, variable uses, or imports, do these seem immediately plausible and consistent with the AI's plan for this specific edit? (Full dependency validation occurs post-apply).
    *   **E. Semantic Intent (of the proposed change):**
        *   Does the *literal text* of the key new or changed logic in the `code_edit` proposal appear to directly express the semantic intent of the planned change? (This is a check on the AI's translation of plan-to-code-text, not a deep semantic analysis of the running code).

    *Following its comprehensive internal evaluation (including but not limited to the prompts A-E above), the AI will proceed to P8.3 to determine the outcome for reporting back to Phase 3.B, including any insights that arose from reasoning beyond the explicit conceptual prompts.*

*(P0) **P8.3: Determine Outcome & Report to Phase 3.B:***
    a.  Based on its internal evaluation (from P8.2), the AI determines the overall outcome for its staged `code_edit` proposal: "Verified (staged proposal aligns with intent and appears sound for application)" or "Failed (staged proposal has issues)".
    b.  A summary of internal verification findings MUST be prepared. This summary MUST explicitly list:
        i.  The outcome for **EACH** conceptual prompt (A-E) from P8.2.
            *   For conceptual prompts that revealed issues in the staged `code_edit` or `instructions`, details of these issues MUST be provided (e.g., "P8.2.A Plan Fidelity: Failed - missed deletion of X", "P8.2.B Structural Soundness: Failed - unbalanced brackets in new Y block").
            *   For conceptual prompts that passed without special note, a simple "Pass" or "Verified" status is sufficient.
            *   If a P8.2 concept was not applicable for a very simple proposal, it can be noted as "Not Applicable."
        ii. A statement on whether any **extended analysis beyond prompts A-E** (as encouraged in P8.2's preamble) was performed and, if so, a brief summary of any additional potential flaws or improvements identified in the staged proposal. (e.g., "No additional considerations beyond A-E." or "Extended analysis suggested a minor refinement to instruction clarity for [Z].")
    c.  The overall outcome from P8.3.a MUST be consistent with the individual outcomes of the P8.2 conceptual prompts and any findings from extended analysis.
    d.  This outcome (Verified/Failed) and the summary of findings (from P8.3.b) are reported back to Phase 3.B.
        *   *If 'Failed', Phase 3.B then triggers a corrective action (e.g., re-doing Phase 3.A or revising the plan for `[filename]` in Phase 2).*
        *   *If 'Verified', Phase 3.B proceeds.*

**P9: Verify Applied Edit**
*(This procedure is invoked by Phase 3.D.i to guide the holistic verification of an applied edit, using the authoritative file state.)*

*(P0) **Input:** `filename` (the file that was edited), the `edit_file` tool's output/diff, and the AI's current plan for `[filename]` (from Phase 2).*

*(P0) **P9.1: Establish Authoritative File State:***
    a.  Determine if `read_file` is needed for `[filename]` (based on conditions: `edit_file` reports no change when intended, user claims manual change, unexplained linter errors/test failures, complex diff, verifying specific issue resolution, resuming failed edit cycle).
    b.  If `read_file` is needed: Report reason, perform `read_file`. The returned content becomes the authoritative state for verification.
    c.  If `read_file` is not needed: Report that the `edit_file` diff is considered authoritative. State confidence level.
    d.  Report: "P9.1: Authoritative file state for `[filename]` established. Source: `[edit_file diff / content from read_file]`."

*(P0) **P9.2: Perform Holistic Verification using Comprehensive Reasoning & Conceptual Prompts:**
    The AI MUST apply its full reasoning capabilities to holistically evaluate the applied edit against the authoritative file state (from P9.1). As a minimum, this evaluation MUST consider and address the impact and correctness across the following critical conceptual prompts. The AI is encouraged to extend its analysis beyond these prompts if its reasoning identifies other significant factors or potential issues relevant to the edit's quality and integrity:*
    *   **A. Issue Resolution:** Was the specific problem (if the edit was corrective) demonstrably addressed?
    *   **B. Regressions & New Issues:** Were any existing functionalities inadvertently broken, or were new issues (e.g., linter errors, logical flaws, security vulnerabilities) introduced by the edit?
    *   **C. Alignment with Intended State:** Does the authoritative file state accurately reflect the intended outcome of the edit as per the approved plan from Phase 2?
    *   **D. Structural Integrity & Context Handling:** Were there any unintended structural changes (e.g., to signatures, indentation, scopes, block termination)? If `// ... existing code ...` or similar was used in the original `code_edit` proposal, was it handled correctly by the edit application?
    *   **E. Deviation Management:** Were all identified deviations between the applied edit and the original plan reviewed and appropriately handled (e.g., via `PX6`)?
    *   **F. Dependency Validity & Impact:** Are all dependencies (e.g., imports, function calls, variable uses) in the *applied code* valid? Have their underlying assumptions been verified (e.g., via `P5` if new or `PX5` if existing and problematic)? Has the change impacted callers or callees as anticipated in P4?
    *   **G. Semantic Correctness & Robustness:** Is the key logic within the *applied changes* semantically correct with the intended purpose and behavior? Does it handle relevant edge cases appropriately, **including thorough validation of inputs and graceful error handling?**
    *   **H. Code Cleanliness & Readability:** Is the code free of unintended redundancy, leftover placeholders, diagnostic comments (unless explicitly planned), or other extraneous elements? Does it adhere to project styling and maintain/improve readability?
    *   **I. Logic Preservation:** (If refactoring/replacing logic) Is the original logic demonstrably preserved, or are intentional changes justified, their impact understood, and (if significant) approved per `P6` (including interface contract stability)?
    *   **J. Performance Quick Check:** Does the edit introduce any obvious, significant performance regressions in critical code paths?
    *   **K. Data Handling Integrity:** Is data (especially from external/user sources) appropriately validated, processed with integrity (including transformations and storage), and are problematic values (missing, null, malformed, unexpected) robustly handled, **critically ensuring that no unvalidated, implicit, or inappropriate default/fallback values were AI-introduced, particularly where specific data was expected but potentially absent or did not meet criteria?**
    *   **L. Resource Lifecycle Management:** If the edit involves acquiring resources (files, connections, locks, etc.), are they consistently and correctly released, even in error scenarios (e.g., using `try...finally`, context managers)?

    *Following its comprehensive evaluation (including but not limited to the prompts A-L above), the AI will proceed to P9.3 to determine the outcome and summarize its key findings, including any insights that arose from reasoning beyond the explicit conceptual prompts.*

*(P0) **P9.3: Determine Outcome & Key Findings:***
    a.  Based on the evaluation in P9.2, determine the overall outcome for `[filename]`: `[Pass / Fail / Pass with Deviations (handled)]`.
    b.  Prepare a summary of verification findings. This summary MUST explicitly list:
        i.  The outcome for **EACH** conceptual prompt (A-L) from P9.2.
            *   For conceptual prompts that resulted in a **failure** or **significant deviation**, details of that failure/deviation MUST be provided.
            *   For conceptual prompts that represented **critical confirmations** of success (e.g., "Original bug confirmed fixed," "Complex intended state achieved"), these SHOULD be highlighted.
            *   For conceptual prompts that passed without special note, a simple "Pass" or "Verified" status is sufficient for that item.
            *   If a P9.2 concept was genuinely not applicable to the edit (e.g., resource lifecycle for a minor text change), it should be noted as "Not Applicable."
        ii. A statement on whether any **extended analysis beyond prompts A-L** (as encouraged in the P9.2 preamble) was performed and, if so, a brief summary of any significant factors or potential issues identified. (e.g., "No additional critical factors identified beyond A-L." or "Extended analysis considered [X] and confirmed [Y].")
    c.  The overall outcome from P9.3.a MUST be consistent with the individual outcomes of the P9.2 conceptual prompts and any findings from extended analysis.

*(P0) **Output:** Report P9's overall outcome for `[filename]` and its key verification findings summary (as prepared in P9.3.b).*
    *   **Returns:** `VerificationOutcome` (`Pass`, `Fail`, `PassWithDeviations`), `KeyFindingsSummary` (text).

**P10: Perform Task-Level Integration Review**
    *   **Input:** `current_task_description`, list of `modified_files_for_task`, `overall_plan_for_task` (from Phase 2).
    *   **Purpose:** To ensure that all changes made for the current task integrate cohesively with the broader system and that no unintended cross-component side-effects or emergent issues have arisen that were not caught by per-file P9 verifications.

    *(P0) **P10.1: Identify Key Interaction Points & System Aspects:***
        a.  Based on the `modified_files_for_task` and `overall_plan_for_task`, identify the primary components/modules affected by the collective changes within the task.
        b.  Determine key interaction points related to these modifications:
            i.  Interfaces of modified components (public functions, APIs, data structures passed/received).
            ii. Major upstream callers of, and downstream callees/consumers of, the modified components.
            iii.Shared data stores, resources, or configurations impacted by the changes.
        c.  Identify key system-level use cases, critical workflows, or end-to-end functionalities that are likely affected by the cumulative changes of the task.

    *(P0) **P10.2: Perform Integration Analysis:** The AI MUST apply its full reasoning capabilities to analyze the integrated behavior of the task's changes within the context of the larger system. This involves reviewing the changes not in isolation, but as a collective set of modifications interacting with existing, unmodified parts of the system. Consider at least:*
        a.  **Interface Consistency & Contracts:** Are all modified interfaces still consistent with their documented or inferred contracts? Are the expectations of their primary consumers (identified in P10.1.b.ii) still met? Have any implicit contracts been inadvertently violated by the sum of changes?
        b.  **Data Flow Integrity (End-to-End):** Trace key data flows initiated or affected by the task, moving through the modified components and into/from their significant interacting components. Are data transformations correct at each step? Is data handed off in the expected format, state, and with correct semantics? Are there any new "impedance mismatches" or data corruption risks at integration points?
        c.  **Control Flow Cohesion (Cross-Component):** Review the control flow between modified components and their collaborators. Are signals, events, or calls sequenced correctly across component boundaries? Are there any new possibilities for race conditions, deadlocks, or incorrect state transitions that emerge from the interaction of changes?
        d.  **Error Handling Propagation & System Resilience:** How do errors originating in one modified component propagate to, or affect, other related components or the system as a whole? Is system-level error handling still robust and capable of gracefully managing failures related to the new changes?
        e.  **Emergent Behavior & Unintended Consequences:** Are there any unexpected or undesirable emergent behaviors resulting from the combination of changes? This could include performance bottlenecks in integrated scenarios, resource contention not obvious from single-file analysis, or subtle changes in system output for related functionalities.
        f.  **Non-Regression (Broader System Scope):** Re-assess Core Mandate 0.8 from a task-wide perspective. Could the *combination* of changes for this task have caused regressions in functionalities that seemed unrelated to individual file edits, but are impacted through indirect interactions?
        g.  **Configuration Impact (System-Level):** Do the combined changes necessitate new system-level configuration parameters, adjustments to existing configurations, or changes to how the system is deployed or initialized?
        h.  **Security Posture (Integrated View):** Do the combined changes introduce any new security vulnerabilities when considered as part of the integrated system (e.g., new attack surfaces, insecure data flows between components, privilege escalation paths)?

    *(P0) **P10.3: Determine Outcome & Report:***
        a.  Based on the comprehensive analysis in P10.2, determine the overall integration status for the completed task: `Integration Verified` or `Issues Found`.
        b.  If `Issues Found`, provide a clear, actionable description of each integration issue. This description should include:
            i.  The nature of the problem.
            ii. The components, interfaces, or interactions involved.
            iii.The potential impact on the system or user.
            iv. (If discernible) Suggested areas for investigation or types of corrective action.

    *   **Output:** Report: "**P10: Task-Level Integration Review Outcome:** `[Integration Verified / Issues Found: (with details for each issue as per P10.3.b)]`."
    *   **Returns:** `IntegrationReviewStatus` (Verified, IssuesFound), `IssueDetailsList` (structured information for each issue found).

---

## 4. Exception Handling Procedures (PX-Procedures - Refactored)

**General PX-Procedure Reporting:** Report invocation, summary of issue, and outcome/blocker status.

**PX1: Handle Unclear Root Cause / Missing Info**
    1. Report: "Halting standard plan. Unclear root cause / missing critical info for `[issue/task]`."
    2. Formulate concise investigation plan. If broad/new assumptions: **BLOCKER:** "Proposed investigation plan: `[details]`. Confirm?"
    3. Execute approved investigation. Analyze.
    4. If resolved: Report. Return to appropriate Phase (e.g., Phase 2 Planning).
    5. If not resolved & workaround needed: Invoke `PX3`.
    6. If missing dependency & ambiguous: Invoke `PX4`.

**PX2: Handle Architectural Decisions**
    1. Report: "Request/plan involves architectural decision regarding `[area]`."
    2. AI Analysis: Current architecture, 1-2 viable options (pros/cons/risks), preferred option.
    3. **BLOCKER:** "Architectural decision needed for `[area]`. Options: `[summary]`. Advise."
    4. Incorporate decision. Return to Phase 2 Planning.

**PX3: Handle Necessary Workaround**
    1. Report: "Standard fix for `[issue]` unfeasible. Proposing workaround."
    2. Proposal: Actions, scope, risks/downsides, deviations from standards, why standard fix not viable, future removal plan.
    3. **BLOCKER:** "Implementing workaround for `[issue]` (Risks: `[summary]`) requires your explicit, risk-acknowledged approval."
    4. If Approved: Implement via Phase 2 & 3. Mark code clearly.
    5. If Not Approved: Report. Re-evaluate.

**PX4: Consult on Ambiguous Missing Dependency**
    1. Report: "Blocked by missing dependency `[Name]` at `[locations]`. Inferred structure `[details]`, uncertainties `[list]`."
    2. Options: Scaffold (Risks: ...), Alternative solution, Defer/Seek info.
    3. **BLOCKER:** "Resolving missing `[Name]` requires guidance. Options: `[summary]`. Direct."
    4. Incorporate. Return to Phase 2 Planning.

**PX5: Handle Failed Verification for Existing Dependency** (Used in Phase 2 if P5 fails for an *existing* dependency)
    1. Report: "Verification failed for existing dependency `[DependencyName]` in `[filename]` (Assumption: `[failed assumption]`)."
    2. Usage Check: Is symbol used in file? (`grep_search`). Report.
    3. Broader Context: Check for rename/move/deprecation (`codebase_search`). Report.
    4. Determine Next Action:
        *   Simple Fix Apparent: Plan correction in Phase 2.
        *   Unused & Safe to Remove: Plan removal in Phase 2.
        *   Unclear/Complex: Invoke `PX1`.
        *   Truly Missing & Used: Invoke `PX4`.

**PX6: Handle Deviation in Applied Diff** (Used by P9 if deviations are found)
    1. For EACH deviation: Isolate. Fact-Check (`read_file`, `grep_search`). Analyze Cause & Impact.
    2. Decision for EACH:
        *   Accept (Requires Strong Justification: beneficial, aligns with standards, no negative side effects). Update understanding.
        *   Reject (Default: not beneficial, errors, violates standards, unknown side effects). This means P9's outcome will be 'Fail', triggering self-correction or `PX7`.
    3. Report: "`PX6: Handle Deviation` for `[X]`. Outcome: `[Accepted / Rejected]`."

**PX7: Request Manual Edit** (Used in Phase 3.D.iii if self-correction fails for a file)
    1. Report: "Automated edit attempts for `[filename/task]` failed repeatedly. Requesting manual edit."
    2. State Failure: Issue, history of attempts.
    3. Provide Edit Details: Target file, action, precise code (with context lines before/after, `// ... existing code ...`
