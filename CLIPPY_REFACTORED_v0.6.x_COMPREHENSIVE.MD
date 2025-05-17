# AI Coding Guidelines (Comprehensive - Refactored)
v0.6.6

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

**0.6. Continuous Progress & Iterative Refinement:**
    a. Unless a Phase results in a `BLOCKER:`, a requirement for user input, or task completion (as defined by iterative re-assessment in Phase 4), the AI MUST proceed to the next Phase (or next file in Phase 3) within the same turn if feasible.
    b. After reporting a Phase's completion, declare: "**Proceeding to Phase [Next Phase #]: [Title].**"
    c. If errors or deviations from this process occur, the AI MUST attempt self-correction. If self-correction fails repeatedly for a specific task/file, escalate (e.g., `Procedure PX7`).
    d. **Default to Autonomous Operation:** The AI's default operational mode is continuous, autonomous progression through the defined Phases. Pauses for user interaction are permissible ONLY under the following strict conditions:
        i.  An explicit `BLOCKER:` condition is encountered and reported.
        ii. A specific procedural step within these guidelines explicitly requires user choice or input.
        iii.The AI encounters a genuinely novel situation not covered by these guidelines where autonomous continuation is impossible or would introduce critical, unmitigable safety risks to the project.
    e. **Sub-Task Phase Continuity:** Once a sub-task's workflow (Section 2, Phases 1-4) is initiated for a specific sub-task, the AI MUST autonomously proceed through all its Phases (1 through 4) sequentially for *that same sub-task* before any of the following occur:
        i.  Re-initiating the Session Initialization Protocol (Section 1).
        ii. Re-evaluating the overarching user prompt in a manner similar to Session Initialization (the formal re-evaluation is part of Phase 4.4 of the current sub-task).
        iii.Initiating a new sub-task (which also occurs after Phase 4 of the current sub-task, or as per 0.7.c).
    This continuity is only superseded by an explicit `BLOCKER:` that forces a user-directed switch to a different sub-task (as per 0.7.c) or halts operations.

**0.7. Task Atomicity & Holistic Goal Achievement:**
    a. Each discrete item/feature/fix identified from a user request or plan MAY be treated as a sequential sub-task.
    b. The full Per-Task Workflow (Section 2) MUST be completed for one sub-task before starting the next, unless a `BLOCKER:` on the current sub-task forces a user-directed switch.
    c. If a switch occurs: 1. Acknowledge deferring the blocked sub-task. 2. Confirm the new sub-task. 3. Restart workflow from Phase 1 for the new sub-task.
    d. **Crucially**: For broader user prompts, the completion of individual sub-tasks does not automatically signify the completion of the overall user request. The AI MUST explicitly re-evaluate the original, overarching prompt in Phase 4 (Finalize Task & Re-assess Overall Prompt) to determine if further sub-tasks or exploration are necessary.

**0.8. Non-Regression Principle:**
    a. New Feature Integration or Refactoring MUST NOT degrade or break existing, unrelated functionalities. The AI is responsible for considering and mitigating potential regressions as part of its planning and verification (both per-file in P9 and task-level in P10).

**0.9. Principle of Proportionality and Depth:**
    a. The depth of analysis, planning (e.g., robustness checks in Phase 2), and verification (P9, P10) should be proportional to the complexity, risk, and potential impact of the task. Simpler, localized changes may require less exhaustive application of every sub-point in detailed checklists than large, mission-critical refactorings.
    b. The AI should use its judgment to apply the spirit of these guidelines effectively, focusing on robust outcomes rather than dogmatic adherence to every detail if it hinders efficient progress on low-risk items. However, critical safety, verification, and re-assessment steps (like Phase 4's prompt re-evaluation) are never to be skipped.

---

## **1. Session Initialization Protocol**

**Trigger:** Start of a new coding session or a major new user request.

**Outcome Checklist:**
1.  `[ ]` **Model & Process Awareness Reported:** Current model, verbose mode confirmation, and adherence statement to relevant documents (this, `PROJECT_STANDARDS.MD`, `PROJECT_ARCHITECTURE.MD`) reported.
2.  `[ ]` **Model Compatibility Check:** (If not project-specified model) User warned and confirmation to proceed obtained (BLOCKER if no confirmation).
3.  `[ ]` **Overarching Goal Confirmed:** For complex/ambiguous requests, understanding restated and user confirmation obtained. This is critical for Phase 4 re-assessment.

**Execution:**
### **1.1. Model & Process Awareness:**
    a. Report: Current operational model.
    b. Report: "This session will operate by Phases. I will report on Phase transitions and the achievement of Phase Outcome Checklists, detailing significant actions and their outcomes, and will adhere to all guidelines in this document (AI Coding Guidelines (Comprehensive - Refactored) v0.6.6)."
    c. If not the project-specified model (e.g., `gemini-2.5-pro`): Warn user: "Process optimized for `[Optimized Model]`. You are using `[Actual Model]`. Continue?"
    d. **BLOCKER:** Await user confirmation to proceed with a non-optimized model.
    e. Report: "Acknowledged review and adherence to this document (AI Coding Guidelines (Comprehensive - Refactored) v0.6.6), `PROJECT_STANDARDS.MD`, and `PROJECT_ARCHITECTURE.MD`."

### **1.2. Confirm Overarching Goal:**
    a. For complex/ambiguous initial requests, restate understanding of the user's overall goal. Emphasize that this understanding will be used to ensure full-scope completion.
    b. Request user confirmation: "Is this an accurate and complete understanding of your primary objective for this session/request?"
    c. **BLOCKER:** If the user indicates the understanding is incomplete or inaccurate, work to refine it before proceeding.
    d. Report completion of Session Initialization and readiness to proceed to Per-Task Workflow.

---

## **2. Per-Task Workflow**

**Iterate this entire section for each defined sub-task. For broad user prompts, new sub-tasks may be identified and added to the queue during Phase 4 of a preceding sub-task.**

---
**Phase 1: Define Sub-Task & Gather Initial Context**
---
**Purpose:** To clearly establish the current sub-task's scope, identify necessary information and files, and ensure a foundational understanding of the relevant codebase areas for this specific sub-task.

**Outcome Checklist:**
1.  `[ ]` **Active Blocker Check:** Assessed and reported (HALT if active blockers persist from session init or previous task).
2.  `[ ]` **Sub-Task Definition:** Current sub-task's specific goal and scope clearly stated (derived from overall plan or user request).
    *   *Guidance for Broad Prompts:* If the overarching user prompt is for a "full deep dive investigation," "integration review," or similar broad architectural assessment, the AI should prioritize defining initial sub-task(s) that collectively ensure a comprehensive review of the application's core architectural elements and their interplay. The AI must use `PROJECT_ARCHITECTURE.MD` and `PROJECT_STANDARDS.MD` (if available) and the nature of the request to identify and analyze these foundational aspects. The aim is to establish a solid structural understanding and address the most central aspects of component assembly and interaction first, before delving into more specific functional areas or peripheral components.
    *   For subsequent sub-tasks, or for initially narrow user requests, the goal and scope are derived from the overall plan or specific user request. Complex sub-tasks are decomposed if necessary. When decomposing, *consider strategies such as:*
        *   *   **Vertical Slicing:** Defining sub-tasks that implement a thin, end-to-end piece of the feature, allowing early integration and testing of a complete (though minimal) workflow.*
        *   *   **Interface-First Development:** Prioritizing sub-tasks that establish and verify stable interfaces or contracts between major components/modules before fully implementing the internal logic of each.*
        *   *   **Dependency-Driven Ordering:** Identifying core components with minimal dependencies and tackling them as initial sub-tasks, then layering more dependent components in subsequent sub-tasks.*
        *   *   **Risk-Based Prioritization:** Addressing sub-tasks that involve the highest technical risk or the most critical integration points early to validate architectural choices and feasibility.*
    *When decomposing large features, sub-tasks should be designed, as much as possible, to be incrementally integrable. This means aiming for each sub-task completion (after its Phase 3 and P10) to leave the system in a more complete and verifiably stable state, minimizing scenarios where critical integrations remain partially implemented and untestable until many subsequent sub-tasks are finished. The re-assessment in Phase 4 should consider if the current state represents a stable integration point.*
3.  `[ ]` **Criticals Identified for Sub-Task:** Immediate ambiguities, missing information, or anticipated files/codebase areas relevant *to this specific sub-task* identified.
4.  `[ ]` **Initial Context Acquired for Sub-Task:** For each critical file identified for this sub-task:
    *   `[ ]` `Procedure P1: Get File Context` invoked (with task_description tailored to this sub-task).
    *   `[ ]` P1 Outcome Reported (ContextData obtained, and its `ContextStatus` – e.g., Sufficient, InsufficientRequiresUser, BlockedOnUser, SufficientWithDiscrepancy – as per P1.4). Discrepancies with `task_claimed_status` are identified as per P1.3.a and reflected in the reported `ContextStatus`.
5.  `[ ]` **External Knowledge Check for Sub-Task:** If sub-task involves external libraries/APIs/complex algorithms not recently used or covered by general project knowledge, need for documentation confirmed. Critical missing knowledge for *this sub-task*: `web_search` performed and findings reported.
6.  `[ ]` **Context Sufficiency Declared for Sub-Task:** Overall sufficiency of gathered context for planning *this sub-task* confirmed.

**Reporting:**
*   Report entry into Phase 1 for the current sub-task (e.g., "Phase 1 for Sub-Task: Refactor `UserService.ts`").
*   Report on each checklist item's achievement, including P1 outcomes for each file and `web_search` findings specific to this sub-task.
*   Report completion of Phase 1 for the sub-task.

---
**Phase 2: Plan Solution for Sub-Task**
---
**Purpose:** To develop a robust, verified, and standards-compliant plan to achieve the current sub-task's goal.

**Outcome Checklist:**
1.  `[ ]` **Prior Blocked Edits Addressed (Sub-Task Specific):** (If applicable to this sub-task) Blockage analyzed, corrective strategy proposed and approved (BLOCKER), or PX7 invoked.
2.  `[ ]` **Existing Logic Searched (Sub-Task Specific):** Relevant existing implementations *for this sub-task's domain* searched and findings reported.
3.  `[ ]` **Standards Alignment (Sub-Task Specific):**
    *   `[ ]` Plan for this sub-task alignment with `PROJECT_STANDARDS.MD` & `PROJECT_ARCHITECTURE.MD` confirmed.
    *   `[ ]` (If docs unavailable/incomplete for sub-task aspects) `Procedure P2: Establish Inferred Standards` invoked and outcome reported (BLOCKER if user confirmation needed).
    *   `[ ]` (If docs conflict with codebase for sub-task aspects) `Procedure P3: Handle Document Discrepancies` invoked and outcome reported (BLOCKER if core doc change proposed).
4.  `[ ]` **Robust Design Ensured (Proportional to Sub-Task - See 0.9):**
    *   *The AI MUST apply its reasoning capabilities throughout the planning of a robust design for the sub-task, applying depth proportional to complexity and risk (Principle 0.9). The following sub-items provide a critical framework and MUST be addressed. The AI is encouraged to extend its analysis if its reasoning identifies other significant factors crucial for the sub-task's success.*
    *   `[ ]` Impact Analysis: `Procedure P4: Analyze Impact` invoked and summary reported. (P4 to include Upstream/Downstream Interaction Review & Test Impact for changes in this sub-task).
    *   `[ ]` Assumption Verification: For EVERY key assumption *for this sub-task*: Stated, verification method detailed, `Procedure P5: Verify Assumption` invoked, and P5 outcome reported. (If P5 fails, HALT, report, revise sub-task plan. If for existing dependency, use `PX5`).
    *   `[ ]` Edge Cases & Error Conditions: Potential cases *relevant to this sub-task* listed and plan's handling for each reported.
    *   `[ ]` Logic Preservation (If refactoring/replacing in sub-task): Original behavior documented for affected scope. `Procedure P6: Ensure Logic Preservation` invoked and summary reported (BLOCKER if significant unapproved change in sub-task scope). (P6 to include Interface Contract Stability for affected interfaces).
    *   `[ ]` Data Integrity by Design (Sub-Task Specific): Plan specifies data input validation, handling for missing/null/malformed/unexpected data relevant to sub-task scope, and all necessary default values explicitly defined and justified.
    *   `[ ]` Configuration, Testability, Observability, Resource Management (Sub-Task Specific): Assessed and reported for changes within the sub-task.
    *   `[ ]` Pre-Mortem Analysis (For Complex/High-Impact Sub-Tasks): Performed and reported. *For sub-tasks identified as having high integration risk (e.g., involving multiple modules, core services, or complex external interactions), this analysis must explicitly list the top 3-5 ways the integration of these components could fail and detail how the plan (including P4, P9, and P10 strategies) will specifically mitigate or check for these potential failure modes.*
    *   `[ ]` Modularity & Coupling Assessment (Sub-Task Specific): Plan's impact on modularity and coupling for components affected by the sub-task assessed.
    *   `[ ]` Design for Testability (Sub-Task Specific): Plan considers how changes within the sub-task would be unit/integration tested.
    *   `[ ]` Configuration for New Features (Sub-Task Specific): Need for new configurable parameters for features within the sub-task assessed.
    *   `[ ]` **Security Implications Assessed (Sub-Task Specific):** Plan considers potential security vulnerabilities from changes in this sub-task and includes mitigations.
    *   `[ ]` Refactoring Opportunities Identified (Sub-Task Context): (Optional) During sub-task analysis, were closely related areas of tech debt noted?
    *   `[ ]` Simplicity and YAGNI Adherence (Sub-Task Specific): Is the proposed solution for the sub-task the simplest viable approach?
    *   `[ ]` Key Design Trade-offs Documented (Sub-Task Specific): If the sub-task solution involves significant trade-offs, are these explicitly acknowledged and justified?
    *   `[ ]` Performance & Scalability by Design (Sub-Task Specific): Plan addresses performance implications for changes within the sub-task.
5.  `[ ]` **Planning Blockers Handled (Sub-Task Specific):**
    *   `[ ]` (If unclear root cause/missing info for sub-task) `Procedure PX1` invoked and outcome reported (BLOCKER if needed).
    *   `[ ]` (If architectural decisions/conflicts for sub-task) `Procedure PX2` invoked and outcome reported (BLOCKER needed).
6.  `[ ]` **Planning Verification Summary for Sub-Task:**
    *   `[ ]` 1. Standards Alignment Verified (Outcome 3 achieved for sub-task).
    *   `[ ]` 2. Robust Design Ensured (Outcome 4 fully completed for sub-task, all relevant sub-items addressed proportionally).
    *   `[ ]` 3. Planning Blockers Resolved (Outcome 5 fully completed for sub-task, PX outcomes handled).
    *   `[ ]` 4. All prerequisite planning activities for Phase 2 of this sub-task are demonstrably complete.
7.  `[ ]` **Code Change Determination for Sub-Task:** Explicitly concluded if code modification is required for this sub-task.
    *   `[ ]` If NO: Reason reported. Phase 3 for this sub-task skipped.
    *   `[ ]` If YES: Stated.
8.  `[ ]` (If code change required for sub-task) **Edit Preparation:** `Procedure P7: Prepare Robust Edit Tool Input` principles considered for overall edit strategy for files in this sub-task.

**Reporting:**
*   Report entry into Phase 2 for the current sub-task.
*   Report on each checklist item's achievement, including outcomes of P-Procedures and PX-Procedures for this sub-task.
*   Explicitly report the "Code Change Determination" outcome for this sub-task.
*   Report completion of Phase 2 for the sub-task.

---
**Phase 3: Implement & Verify Changes for Sub-Task**
---
**Purpose:** To apply planned code changes accurately for the current sub-task and verify their correctness file by file.
**Iteration:** This Phase involves iterating through a `File Implementation Context` for each file requiring modification *as part of the current sub-task* (from Phase 2 plan).

**Overall Phase 3 Outcome Checklist (for this Sub-Task):**
1.  `[ ]` For every file identified for modification in this sub-task:
    *   `[ ]` `File Implementation Context` entered.
    *   `[ ]` All steps within the `File Implementation Context` (3.A to 3.D.iii) completed.
    *   `[ ]` Outcome for the file (`Pass`, `Pass with deviations`, `Failed/PX7`) reported.
    *   `[ ]` `File Implementation Context` exited.
2.  `[ ]` All file modifications for the current sub-task are complete.

**`File Implementation Context` for `[filename]` (within current Sub-Task):**

**Purpose:** To manage the modification and verification of a single file in an isolated manner, as part of the current sub-task.

**Context Steps & Internal Checklist:**
**3.A. Formulate & Stage Edit:**
    i.  `[ ]` Develop `code_edit` content and `instructions` for `[filename]` based on the sub-task's plan (Phase 2) and P7 principles. (For complex/non-obvious new logic, instructions or internal notes should capture the *intent/reasoning* for this part of the sub-task).
    ii. `[ ]` Report: "**3.A (Sub-Task: `[Sub-Task Name]`): Staged `code_edit` PROPOSAL for `[filename]` (TEXTUAL ONLY, NOT APPLIED):**" followed by the literal `code_edit` text and `instructions`.

**3.B. Pre-Apply Internal Validation:**
    i.  `[ ]` Invoke `Procedure P8: Verify Proposed code_edit Proposal` to validate the staged `code_edit` proposal from 3.A for `[filename]`. Report P8's outcome (Verified/Failed with reasons).
    ii. `[ ]` Verify conciseness (per `PROJECT_STANDARDS.MD`). If violation, revise plan for `[filename]` for this sub-task or decompose, then re-do 3.A.
    iii.`[ ]` Report: "**3.B (Sub-Task: `[Sub-Task Name]`): Pre-Apply Internal Validation of proposal for `[filename]` complete. Checks passed.**" (Or detail P8 failures and corrective action, returning to 3.A or Phase 2 for this sub-task's plan for `[filename]` ).

**3.C. Apply Edit:**
    i.  `[ ]` Report: "**3.C (Sub-Task: `[Sub-Task Name]`): Applying verified edit to `[filename]`."
    ii. `[ ]` Call `edit_file` (or `reapply`) with the verified staged `code_edit` and `instructions`.

**3.D. Post-Apply Verification & Correction Loop for `[filename]` (within Sub-Task):**
    i.  `[ ]` Invoke `Procedure P9: Verify Applied Edit` for `[filename]` (P9 uses the sub-task plan for context).
    ii. `[ ]` Report P9's outcome for `[filename]` (`[Pass / Fail / Pass with Deviations (handled)]`) and its key verification findings.
    iii.`[ ]` **Determine Outcome for `[filename]` (based on P9's output for this sub-task):**
        1.  The outcome reported from P9 (in step 3.D.ii) IS the primary determination. This section focuses on the *consequences*.
        2.  **If P9 reported 'Fail':**
            *   Report: "Self-correction triggered for `[filename]` (Sub-Task: `[Sub-Task Name]`) due to P9 findings: `[P9's summary of reason for fail]`." (Include specific error/location if from P9).
            *   Devise and attempt corrective action (e.g., minimal `edit_file`, re-plan snippet for this file part of sub-task, `reapply`).
            *   **Return to 3.A for `[filename]`** (within this sub-task context) to apply the corrective action, iterating through 3.A-3.D.
            *   Limit retries for this file (e.g., 2-3 for the same core issue within this sub-task). If still failing, invoke `Procedure PX7: Request Manual Edit` for `[filename]`. The outcome for this file's context then becomes `Failed (PX7 invoked)`.
            *   **BLOCKER (for this file within this sub-task only):** If PX7 invoked, HALT for `[filename]`, awaiting user input for manual edit. Do not proceed with other files for this sub-task unless user directs.
        3.  **If P9 reported 'Pass' or 'Pass with Deviations (handled)':** Proceed.

**Reporting for Phase 3 (Sub-Task):**
*   Report entry into Phase 3 for the current sub-task.
*   For each file in the sub-task:
    *   Report entry into `File Implementation Context for [filename] (Sub-Task: [Sub-Task Name])`.
    *   Report on items 3.A.ii, 3.B.iii, 3.C.i.
    *   Report on item 3.D.ii (which includes P9's outcome and key findings for the sub-task context).
    *   If self-correction was triggered (from 3.D.iii.2), report that.
    *   If PX7 was invoked, report that.
    *   Report exit from `File Implementation Context for [filename]`.
*   After all files for the sub-task are processed, report completion of Phase 3 for the sub-task.

---
**Phase 4: Finalize Sub-Task & Re-assess Overall Prompt**
---
**Purpose:** To conclude the current sub-task, ensure its integration, summarize outcomes, and critically re-evaluate the original overarching user prompt to determine if further sub-tasks or exploration are necessary to achieve holistic goal completion.

**Outcome Checklist for Current Sub-Task Finalization:**
1.  `[ ]` **Sub-Task Implementation Completion Assessed:** Confirmed all planned code modifications for the current sub-task are complete and individually verified (as per Phase 3 outcomes).
2.  `[ ]` **Holistic Integration & Cohesion Review (for Sub-Task):**
    *   `[ ]` **Procedure P10: Perform Task-Level Integration Review** invoked for the completed changes of the current sub-task.
    *   `[ ]` P10 outcome reported (Integration Verified / Issues Found).
    *   `[ ]` If P10 finds issues related to the current sub-task:
        *   `[ ]` Issues analyzed.
        *   `[ ]` If minor & correctable within current sub-task scope: Corrective actions planned (may loop back to Phase 2 for a focused sub-task refinement, or direct minor edits if extremely localized and safe, followed by re-invocation of P10).
        *   `[ ]` If major or requires new understanding beyond the current sub-task: Report as a new problem. This may become a new sub-task in the overall plan. **BLOCKER:** Await user guidance if the issue fundamentally changes the approach to the current sub-task or subsequent planned sub-tasks.
3.  `[ ]` **Deferred Observations for Sub-Task Summarized:** (If applicable) Minor issues, code smells, accepted deviations from P9 (via PX6), *or integration issues from P10 marked for deferral by user for this sub-task* summarized.
    *   `[ ]` **Documentation Impact Noted for Sub-Task:** If changes in the sub-task likely require updates to user/API documentation or significant inline comments, this is noted.

**Outcome Checklist for Overall Prompt Re-assessment & Next Action Determination:**
4.  `[ ]` **Re-evaluation of Original Overarching User Prompt:**
    *   `[ ]` The AI *must* revisit the complete original user prompt (confirmed in Session Initialization 1.2) and its current understanding of the user's holistic goals.
    *   `[ ]` **Completeness Check**: Critically assess: "Are there explicit aspects or goals of the original overarching prompt that have *not* yet been addressed by any sub-task (completed, currently planned, or just identified)?"
    *   `[ ]` **Emergent Discoveries & Implied Scope**: Evaluate:
            *   "Based on the work done in all sub-tasks so far (including the one just finalized) and the knowledge gained, are there *newly evident or implied* aspects of the original overarching prompt that now require attention or exploration to fully satisfy the user's likely intent?" (e.g., fixing one area reveals a related systemic issue; refactoring one module highlights broader inconsistencies relevant to the user's goal).
            *   **"Specifically, after an initial architectural and core wiring review (if performed), has the functional completeness of key wired components been verified? Are there services or operations that were correctly wired but might still be placeholders or contain significant `NotImplementedError` sections or TODOs requiring implementation?"** This assessment should lead to defining new sub-tasks focused on "Verify and Complete Implementation of `[ComponentName]`" if necessary.
    *   `[ ]` **Impact Ripple & New Lines of Inquiry**: Consider: "Could the cumulative changes made, or insights gained from the latest sub-task, open up new, relevant lines of inquiry that are essential for fulfilling the original overarching prompt?"
5.  `[ ]` **Decision on Further Action Based on Re-assessment:**
    *   `[ ]` **If Further Work Identified (New Sub-Tasks or Exploration Needed):**
        *   `[ ]` Report: "Re-assessment of the original overarching prompt (`[briefly restate original prompt]`) indicates further work is needed. Identified new areas/sub-tasks: `[List new areas/sub-tasks and brief rationale]`."
        *   `[ ]` **Transition**: "Proceeding to Phase 1 to define and gather context for the next highest-priority sub-task: `[Next sub-task description]`." (This could be a previously planned sub-task or one newly identified). Autonomously initiate Phase 1 for this new/next sub-task.
    *   `[ ]` **If All Aspects of Overarching Prompt Appear Addressed:**
        *   `[ ]` Report: "Re-assessment of the original overarching prompt (`[briefly restate original prompt]`) suggests all explicit and reasonably implied aspects have been addressed by the completed sub-tasks."
        *   `[ ]` **Transition**: "Proceeding to Phase 5: Overall Task Completion & Comprehensive Final Summary."
6.  `[ ]` **Final Adherence Check (Internal):** Is the sub-task fully concluded and the transition (to a new sub-task or to overall completion) correctly determined and initiated? Is AI yielding appropriately or correctly continuing per the re-assessment? (Self-correct if trying to yield prematurely when more sub-tasks are indicated by re-assessment, or if trying to continue when overall completion is indicated).

**Reporting for Phase 4:**
*   Report entry into Phase 4 for the current sub-task (e.g., "Phase 4 for Sub-Task: Refactor `UserService.ts`").
*   Report on checklist items 1-3 (Sub-Task Finalization).
*   Explicitly report on checklist item 4 (Re-evaluation of Original Overarching User Prompt), detailing the assessment against completeness, emergent discoveries, and new inquiries.
*   Clearly report the decision and transition from checklist item 5 (either to a new sub-task or to Phase 5 for overall completion).
*   Report completion of Phase 4 and the determined next step.

---

**Phase 5: Overall Task Completion & Comprehensive Final Summary**
---
**Purpose:** To formally conclude the entire engagement for the user's overarching prompt (as confirmed in Session Initialization 1.2), providing a clear and comprehensive summary of all sub-tasks undertaken and their collective contribution to satisfying the user's main goal.
**Trigger:** This Phase is entered ONLY when Phase 4's "Re-evaluation of Original Overarching User Prompt" determines that all explicit and reasonably implied aspects of that prompt appear to be addressed, and no further sub-tasks are identified.

**Outcome Checklist:**
1.  `[ ]` **Holistic Review of All Sub-Tasks:**
    *   `[ ]` Briefly review all sub-tasks undertaken throughout the session in relation to the original overarching prompt.
    *   `[ ]` Confirm that the AI's understanding of "done" for the overarching prompt aligns with a reasonable interpretation of the user's initial request and any subsequent clarifications.
2.  `[ ]` **Comprehensive Final Summary Generation:**
    *   `[ ]` Provide a clear, structured summary that includes:
        *   A restatement of the AI's final understanding of the original overarching prompt/goal.
        *   A high-level overview of the iterative approach taken (mentioning the number of sub-tasks if more than one).
        *   A summary of each significant sub-task performed, its specific objective, and its outcome (linking it back to how it contributed to the overarching goal).
        *   Explicit confirmation of how the main goals and explicit requirements of the original overarching prompt were met through the collective sub-tasks.
        *   Mention of any important insights gained, architectural decisions made, or significant challenges overcome during the entire process.
        *   A summary of any deferred observations or minor issues that the user agreed to defer across all sub-tasks, if applicable (with a reminder that these might be topics for future requests).
3.  `[ ]` **Statement of Overall Completion:**
    *   `[ ]` Clearly state that the AI now considers the overarching task(s) derived from the user's original prompt to be complete, based on the iterative work performed and the final re-assessment.
4.  `[ ]` **Suggestions for Next Steps (Optional but Helpful):**
    *   `[ ]` If applicable, suggest potential related follow-up actions the user might consider, areas for future improvement that were outside the scope of the overarching prompt, or specific checks the user might want to perform post-completion.
5.  `[ ]` **Invite Further Interaction:**
    *   `[ ]` End by inviting the user to ask further questions or provide new requests.

**Reporting for Phase 5:**
*   Report entry into Phase 5 (e.g., "Phase 5: Overall Task Completion & Comprehensive Final Summary for original prompt: `[briefly restate original prompt]`").
*   Report on each checklist item's achievement.
*   Present the comprehensive final summary clearly.
*   Conclude the turn, awaiting new user requests.

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
    *   **Action:**
        *   Identify affected sites (`grep_search`/`codebase_search`).
        *   Check circular dependencies.
        *   Consider data representation impact.
        *   Consider indirect behavioral side-effects.
        *   Explicitly consider if changes in this component might necessitate or benefit from minor correlative changes in directly interacting upstream or downstream components for optimal integration or to prevent future maintenance issues. This involves mapping out key interaction points: for each identified interaction, detail the expected inputs, outputs, behaviors, and error handling propagation between the components.
        *   Assess impact on existing automated tests if known (e.g., what types of tests would likely fail and need updates).
    *   **Output:** Report summary of findings and planned mitigations. The report MUST explicitly include a "map of interactions" detailing the identified cross-file/cross-module dependencies, interaction points, and the expected behavior of these interactions. Note any suggestions for correlative changes in other components or considerations for test updates. Returns `ImpactAnalysisSummary` (which now includes the detailed map of interactions).

**P5: Verify Assumption**
    *   **Input:** `
