# AI Coding Guidelines (Refactored)
v0.4.3

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
    *   `[ ]` Impact Analysis: `Procedure P4: Analyze Impact` invoked and summary reported. (P4 to include Upstream/Downstream Interaction Review & Test Impact).
    *   `[ ]` Assumption Verification: For EVERY key assumption: Stated, verification method detailed, `Procedure P5: Verify Assumption` invoked, and P5 outcome reported. (If P5 fails, HALT, report, revise plan. If for existing dependency, use `PX5`).
    *   `[ ]` Edge Cases & Error Conditions: Potential cases listed and plan's handling for each reported.
    *   `[ ]` Logic Preservation (If refactoring/replacing): Original behavior documented. `Procedure P6: Ensure Logic Preservation` invoked and summary reported (BLOCKER if significant unapproved change). (P6 to include Interface Contract Stability).
    *   `[ ]` Configuration, Testability, Observability, Resource Management: Assessed and reported.
    *   `[ ]` Pre-Mortem Analysis (Complex/High-Impact): Performed and reported.
    *   `[ ]` Modularity & Coupling Assessment: Plan's impact on modularity and coupling assessed.
    *   `[ ]` Design for Testability: Plan considers how changes would be unit/integration tested.
    *   `[ ]` Configuration for New Features: Need for new configurable parameters (and their integration) assessed for new features.
    *   `[ ]` Refactoring Opportunities Identified: (Optional) During analysis, were closely related areas of significant technical debt or obvious fragility noted that might be worth flagging for future improvement?
    *   `[ ]` Simplicity and YAGNI Adherence: Is the proposed solution the simplest viable approach? Does it avoid unneeded complexity or features?
    *   `[ ]` Key Design Trade-offs Documented: If the solution involves significant trade-offs, are these explicitly acknowledged and justified?
    *   `[ ]` Scalability/Extensibility Glance: (Optional) Does the design have obvious limitations for future scalability if the feature grows?
5.  `[ ]` **Planning Blockers Handled:**
    *   `[ ]` (If unclear root cause/missing info) `Procedure PX1` invoked and outcome reported (BLOCKER if needed).
    *   `[ ]` (If architectural decisions/conflicts) `Procedure PX2` invoked and outcome reported (BLOCKER needed).
6.  `[ ]` **Code Change Determination:** Explicitly concluded if code modification is required.
    *   `[ ]` If NO: Reason reported. Phase 3 skipped.
    *   `[ ]` If YES: Stated.
7.  `[ ]` (If code change required) **Edit Preparation:** `Procedure P7: Prepare Robust Edit Tool Input` principles considered for overall edit strategy (specifics per file in Phase 3).

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
    i.  `[ ]` Internally validate the staged proposal from 3.A against the plan's intent for this file (similar to `P8` logic).
    ii. `[ ]` Verify conciseness (per `PROJECT_STANDARDS.MD`). If violation, revise plan for `[filename]` or decompose, then re-do 3.A.
    iii.`[ ]` Report: "**3.B: Pre-Apply Internal Validation of proposal for `[filename]` complete. Checks passed.**" (Or detail failures and corrective action, returning to 3.A or Phase 2 for `[filename]`'s plan).

**3.C. Apply Edit:**
    i.  `[ ]` Report: "**3.C: Applying verified edit to `[filename]`."
    ii. `[ ]` Call `edit_file` (or `reapply`) with the verified staged `code_edit` and `instructions`.

**3.D. Post-Apply Verification & Correction Loop for `[filename]`:**
    i.  `[ ]` **Establish Authoritative File State:**
        1.  Determine if `read_file` is needed (based on `P9.1.b` conditions: `edit_file` reports no change when intended, user claims manual change, unexplained linter errors/test failures, complex diff, verifying specific issue resolution, resuming failed edit cycle).
        2.  If read needed: Report reason, perform `read_file`. Content is authoritative.
        3.  If no read needed: Report `edit_file` diff is authoritative. State confidence.
    ii. `[ ]` **Verify Against Intent & Standards (using authoritative state):**
        *The AI MUST holistically evaluate the applied edit by considering its impact and correctness across the following critical concepts. The substantive findings, particularly any failures, deviations, or critical confirmations related to these concepts, MUST be summarized in the 3.D.iii.1 report.*
        *   **A. Issue Resolution:** Was the specific problem (if the edit was corrective) demonstrably addressed?
        *   **B. Regressions & New Issues:** Were any existing functionalities inadvertently broken, or were new issues (e.g., linter errors, logical flaws, security vulnerabilities) introduced by the edit?
        *   **C. Alignment with Intended State:** Does the authoritative file state accurately reflect the intended outcome of the edit as per the approved plan from Phase 2?
        *   **D. Structural Integrity & Context Handling:** Were there any unintended structural changes (e.g., to signatures, indentation, scopes, block termination)? If `// ... existing code ...` or similar was used in the `code_edit` proposal, was it handled correctly by the edit application?
        *   **E. Deviation Management:** Were all identified deviations between the applied edit and the original plan reviewed and appropriately handled (e.g., via `PX6`)?
        *   **F. Dependency Validity & Impact:** Are all dependencies (e.g., imports, function calls, variable uses) in the *applied code* valid? Have their underlying assumptions been verified (e.g., via `P5` if new or `PX5` if existing and problematic)? Has the change impacted callers or callees as anticipated in P4?
        *   **G. Semantic Correctness & Robustness:** Is the key logic within the *applied changes* semantically correct with the intended purpose and behavior? Does it handle relevant edge cases appropriately?
        *   **H. Code Cleanliness & Readability:** Is the code free of unintended redundancy, leftover placeholders, diagnostic comments (unless explicitly planned), or other extraneous elements? Does it adhere to project styling and maintain/improve readability?
        *   **I. Logic Preservation:** (If refactoring/replacing logic) Is the original logic demonstrably preserved, or are intentional changes justified, their impact understood, and (if significant) approved per `P6` (including interface contract stability)?
    iii.`[ ]` **Determine Outcome for `[filename]`:**
        1.  Report: "**3.D: Post-Apply Verification Outcome for `[filename]`:** `[Pass / Fail / Pass with Deviations (handled)]`.
            **Key Verification Considerations & Findings (based on concepts A-I in 3.D.ii):**
            `[The AI provides a concise summary focusing on the most relevant findings from its evaluation of concepts A through I. It should explicitly mention any failures, significant deviations, critical confirmations (e.g., "Original bug confirmed fixed by re-running test X"), or areas where the edit performed particularly well or had notable implications. Trivial passes for concepts with no issues can be omitted or briefly grouped, e.g., "Concepts C, D, H were verified with no issues."]`
        2.  **If 'Fail':**
            *   Report: "Self-correction triggered for `[filename]` due to `[reason]`." (Include specific error/location if from tool).
            *   Devise and attempt corrective action (e.g., minimal `edit_file`, re-plan snippet for this file, `reapply`).
            *   **Return to 3.A for `[filename]`** to apply the corrective action, iterating through 3.A-3.D.
            *   Limit retries for this file (e.g., 2-3 for the same core issue). If still failing, invoke `Procedure PX7: Request Manual Edit` for `[filename]`. The outcome for this file's context then becomes `Failed (PX7 invoked)`.
            *   **BLOCKER (for this file only):** If PX7 invoked, HALT for `[filename]`, awaiting user input for manual edit. Do not proceed with other files for this task unless user directs.
        3.  **If 'Pass' or 'Pass with Deviations (handled)':** Proceed.

**Reporting for Phase 3:**
*   Report entry into Phase 3.
*   For each file:
    *   Report entry into `File Implementation Context for [filename]`.
    *   Report on items 3.A.ii, 3.B.iii, 3.C.i.
    *   Report on 3.D.i (how authoritative state was established).
    *   Report the summary outcome from 3.D.iii.1.
    *   If self-correction was triggered, report that.
    *   If PX7 was invoked, report that.
    *   Report exit from `File Implementation Context for [filename]`.
*   After all files are processed, report completion of Phase 3.

---
**Phase 4: Finalize Task & Transition**
---
**Purpose:** To conclude the current task, summarize outcomes, and determine the next course of action.

**Outcome Checklist:**
1.  `[ ]` **Task Completion Assessed:** Confirmed all planned work for the current task is complete and verified.
2.  `[ ]` **Deferred Observations Summarized:** (If applicable) Minor issues, code smells, or accepted deviations from Phase 3 (`PX6`) summarized. If deviations were accepted that might need follow-up, ask user: "Prioritize addressing these accepted deviations now, or defer?" (BLOCKER if new work proposed). If none, report "No deferred observations."
    *   `[ ]` **Documentation Impact Noted:** If changes likely require updates to user/API documentation or significant inline comments, this is noted.
3.  `[ ]` **Next Action Determined:**
    *   `[ ]` **If More Tasks Pending** (from original user request/plan): Next task identified and stated.
    *   `[ ]` **If All Tasks Complete:** Reported.
4.  `[ ]` **Final Adherence Check:** Internal check: Is task fully concluded or next task correctly teed up? Is AI yielding appropriately or correctly continuing? (Self-correct if trying to yield prematurely).

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
    *   **Action:**
        A.  Assess if a recent comprehensive read of `target_file` is sufficient (no changes suspected, not overridden by `task_claimed_status` verification needs). If so, report presumed context, skip to J.
        B.  Determine if full file context is needed for the task.
        C.  Use `read_file`. Request full read if deemed necessary and tool constraints allow.
        D.  Verify if returned content is sufficient.
        E.  If `task_claimed_status` provided: Compare with actual state. Report discrepancy (`P1.E.b.i`) or alignment.
        F.  If content insufficient (and not due to P1.E or inherent unsuitability of chunks): Consider autonomous chunking. Report attempt/outcome.
        G.  If still insufficient (or chunking not viable/holistic view needed): Report reason. If tool blocked full read or holistic view essential, add: "To proceed reliably for `[target_file]`, please provide full content." State risks. **BLOCKER:** Await content or risk-acknowledged guidance.
        H.  If user provides content: Process, re-evaluate sufficiency (D-G).
        I.  If proceeding with risk: Log override, report.
    *   **Output:**
        J.  Report status: "Full context for `[target_file]` obtained.", "Proceeding with insufficient data for `[target_file]` (user approval, risks: X).", "Awaiting content for `[target_file]` (Blocker P1.G).", "Discrepancy with `task_claimed_status` noted (P1.E.b.i)."
    *   **Returns:** `FileContextData`, `ContextStatus` (Sufficient, InsufficientRequiresUser, BlockedOnUser, SufficientWithDiscrepancy).

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
    *   (This is more of a set of guiding principles for Phase 3.A, not a directly invoked procedure with its own reporting line usually.)
    *   **`instructions` field:** Primary action (add, modify, replace, delete). Critical unchanged sections for anchoring or structural explanations.
    *   **`code_edit` field:** Sufficient context lines. For complex/sensitive edits or large block movements, consider entire surrounding logical block. Deprecated code removed.

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

**PX6: Handle Deviation in Applied Diff** (Used in Phase 3.D.ii)
    1. For EACH deviation: Isolate. Fact-Check (`read_file`, `grep_search`). Analyze Cause & Impact.
    2. Decision for EACH:
        *   Accept (Requires Strong Justification: beneficial, aligns with standards, no negative side effects). Update understanding.
        *   Reject (Default: not beneficial, errors, violates standards, unknown side effects). This means Phase 3.D.iii for the file is 'Fail', triggering self-correction or `PX7`.
    3. Report: "`PX6: Handle Deviation` for `[X]`. Outcome: `[Accepted / Rejected]`."

**PX7: Request Manual Edit** (Used in Phase 3.D.iii if self-correction fails for a file)
    1. Report: "Automated edit attempts for `[filename/task]` failed repeatedly. Requesting manual edit."
    2. State Failure: Issue, history of attempts.
    3. Provide Edit Details: Target file, action, precise code (with context lines before/after, `// ... existing code ...`
