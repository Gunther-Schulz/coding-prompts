# AI Coding Guidelines
v0.3.0

## 0. Core Mandates & Principles

These mandates are absolute and govern all AI operations.

**0.1. Strict Sequential Execution & Comprehensive Reporting:**
    a. **Sequentiality:** All numbered steps and sub-steps within this document MUST be executed in the precise order presented.
    b. **Completeness:** No step or sub-step is optional unless explicitly stated.
    c. **Reporting:** (Modified for Verbosity Control)
        *   **Verbosity Control:** This document defines three verbosity levels for AI reporting: `verbose`, `standard`, and `quiet`. The active `[VERBOSITY_LEVEL]` is determined by user instruction at the start of a task/session, defaulting to `standard` if unspecified.
        *   **Reporting Rules (conditional on `[VERBOSITY_LEVEL]`):**

            *   **If `[VERBOSITY_LEVEL] = verbose`:**
                i.  The AI MUST explicitly report on the execution of each numbered step/sub-step.
                ii. The report for each step MUST detail the actions taken and their outcomes.
                iii. If a step is Not Applicable (N/A), it MUST be reported with its number, "N/A", and a concise justification.
                iv. Use consistent markdown for reporting (e.g., bold for step numbers).

            *   **If `[VERBOSITY_LEVEL] = standard`:**
                i.  **Internal Accountability:** The AI MUST internally track and verify the execution of each numbered step/sub-step and procedure, maintaining detailed logs for auditability against this document.
                ii. **User-Facing Summaries:** The AI's output to the user should prioritize clarity and conciseness, focusing on:
                    1.  Start and completion of major phases or sub-tasks.
                    2.  Key decisions made and their justifications.
                    3.  Critical findings (e.g., discrepancies, failed verifications).
                    4.  Proposed code changes or plans.
                    5.  `BLOCKER:` conditions requiring user input.
                    6.  Overall outcomes of significant actions (e.g., file edits, context gathering).
                iii. **Explicit Reporting of Critical Items:** Specific items like `BLOCKER:` notifications, `code_edit` proposals, and outcomes of verification checklists (e.g., 2.4.e.ii) MUST still be explicitly reported as detailed in their respective sections.
                iv. If a step is Not Applicable (N/A), its omission in user-facing output is acceptable if not critical; internal logs must note the N/A and justification.
                v.  Use consistent markdown for reporting key user-facing items (e.g., bold for step numbers of major phases being reported).

            *   **If `[VERBOSITY_LEVEL] = quiet`:**
                i.  **Internal Accountability:** (Same as `standard`) The AI MUST internally track and verify the execution of each numbered step/sub-step and procedure, maintaining detailed logs for auditability against this document.
                ii. **User-Facing Summaries (Critical Only):** The AI's output to the user should be limited to:
                    1.  `BLOCKER:` conditions requiring user input.
                    2.  Direct questions to the user whose input is required to proceed.
                    3.  Final overall outcome/summary upon completion of a sub-task or the entire request.
                    4.  `code_edit` proposals (textual, before application).
                    5.  Brief notification of successful edit application or failure if self-correction is triggered.
                    6.  Explicitly identified critical findings that prevent progress or indicate significant plan changes (e.g., major discrepancies from `P3`, critical assumption failures from `P5` that halt a plan).
                iii. All other detailed step reporting, including interim phase transitions not tied to a critical output, is handled via internal accountability logs.

            *   **If `[VERBOSITY_LEVEL] = very_quiet`:**
                i.  **Internal Accountability:** The AI MUST internally track and verify the execution of *all* numbered steps, sub-steps, and procedures with the same rigor as `standard` and `quiet` levels, maintaining detailed logs for auditability. This comprehensive internal tracking is what enables the AI to proceed silently with minimal user-facing output (as defined in `ii` and `iii` below) while ensuring full process adherence.
                ii. **User-Facing Summaries (Absolute Minimum):** The AI's output to the user should be strictly limited to:
                    1.  `BLOCKER:` conditions requiring user input.
                    2.  Direct questions to the user whose input is required to proceed.
                    3.  `code_edit` proposals (textual, before application).
                    4.  Brief notification of edit application *failure* if self-correction is triggered (success is implied by progression).
                    5.  A single, minimal notification (e.g., "All tasks complete.") when the entire user request is finished.
                iii. All other detailed step reporting, including sub-task summaries, successful edit notifications, and critical findings that don't immediately lead to a `BLOCKER` or direct question, are handled via internal accountability logs. The AI proceeds silently between the essential interaction points defined above.
    d. **Purpose:** This rigor ensures process integrity, verifiability, and reliability, while providing clear and actionable information to the user, tailored by the chosen verbosity level.

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
    a. Unless a step results in a `BLOCKER:`, a requirement for user input, or task completion, the AI MUST proceed to the next step within the same turn.
    b. After reporting a major step's completion, declare: "**Proceeding to Step [Next Step #]: [Title].**"
    c. If errors or deviations from this process occur, the AI MUST attempt self-correction. If self-correction fails repeatedly for a specific task, escalate (e.g., `Procedure PX7`).
    d. **Default to Autonomous Operation:** The AI's default operational mode is continuous, autonomous progression through the defined steps. Pauses for user interaction are permissible ONLY under the following strict conditions:
        i.  An explicit `BLOCKER:` condition is encountered and reported.
        ii. A specific procedural step within these guidelines explicitly requires user choice or input (e.g., `PX2`, `PX3`, `PX4`, `PX7`).
        iii.The AI encounters a genuinely novel situation not covered by these guidelines where autonomous continuation is impossible or would introduce critical, unmitigable safety risks to the project.
    General review of intermediate outputs (e.g., code proposals, plans) by the user is not a condition for pausing unless one of these explicit criteria is met.

**0.7. Sub-Task Atomicity:**
    a. Each discrete item/feature/fix in a user request or plan MUST be treated as a sequential sub-task.
    b. The full workflow (Section 2) MUST be completed for one sub-task before starting the next, unless a `BLOCKER:` on the current sub-task forces a user-directed switch.
    c. If a switch occurs: 1. Acknowledge deferring the blocked task. 2. Confirm the new task. 3. Restart workflow from Step 2.1 for the new task.

---

## 1. Session Initialization Protocol

**Trigger:** Start of a new coding session or a major new user request.

**1.1. Model & Process Awareness:**
    a. Report: Current operational model.
    b. Report: "Active `[VERBOSITY_LEVEL]` is `[current setting]`."
    c. If not the project-specified model (e.g., `gemini-2.5-pro`): Warn user: "Process optimized for `[Optimized Model]`. You are using `[Actual Model]`. Continue?"
    d. **BLOCKER:** Await user confirmation to proceed with a non-optimized model.
    e. Report: "Acknowledged review and adherence to `AI_CODING_GUIDELINES.MD`, `PROJECT_STANDARDS.MD`, and `PROJECT_ARCHITECTURE.MD`."

**1.2. Confirm Overarching Goal:**
    a. For complex/ambiguous initial requests, restate understanding of the user's overall goal.
    b. Request user confirmation.

---

## 2. Per-Sub-Task Workflow

**Iterate this entire section for each defined sub-task.**
**Invoke `Procedure P12: Phase Transition Guard` at every `--- [Phase Gate: ...] ---` point.**

---
**[Phase Gate: Start of Sub-Task]**
(Current Phase: None, Next Phase: 2.1. Define Sub-Task)
---
**2.1. Define Sub-Task & Initial Setup**
    a. **Blocker Check:**
        i.  Assess for unresolved `BLOCKER:`s from previous turns.
        ii. If Blocker(s) Exist: Report "**2.1.a: Active Blocker(s) Detected.**" Reiterate each. **HALT.** Await user input.
        iii.If No Blockers: Report "**2.1.a: No active blockers impede current focus.**"
    b. **Reaffirm Overarching Goal:** Briefly restate the confirmed overall goal (from 1.2) and this sub-task's contribution.
    c. **Define Current Sub-Task:** Clearly state the specific, actionable goal and scope of the *current* sub-task.
    d. **Decomposition:** If the sub-task is complex, report its decomposition into smaller logical steps for planning.
    e. **Identify Criticals:** Report immediate ambiguities, missing information, or anticipated files/codebase areas for *this sub-task*. Plan to address these during Step 2.2 or 2.3. Special attention should be given if the task description (e.g., from a review document or issue) claims a component is "missing," "incomplete," or "a stub," but preliminary observations (e.g., quick `file_search`, `grep_search` for imports) suggest it might already exist or has a different status. If such a suspicion arises, this MUST be noted, and **`Procedure P1` (when invoked for that component in Step 2.2.b) MUST be called with the `task_claimed_status` parameter populated** (e.g., with "missing", "incomplete") so `P1.4` can perform its specific verification.

---
**[Phase Gate: After Setup, Before Context Gathering]**
(Current Phase: 2.1. Define Sub-Task, Next Phase: 2.2. Gather Context)
---
**2.2. Gather Context & Knowledge**
    a. **Identify Files:** Based on 2.1.e, list specific files/modules requiring access.
    b. **File Context:** For each identified file, invoke `Procedure P1: Ensure Sufficient File Context`.
    c. **External Knowledge:** If the sub-task involves external libraries, APIs, or complex algorithms not recently used:
        i.  Confirm necessary documentation/knowledge is accessible.
        ii. If critical knowledge is missing, perform `web_search`. Report findings.
    d. **Report Sufficiency:** Summarize context obtained and confirm its adequacy for planning.

---
**[Phase Gate: After Context, Before Planning]**
(Current Phase: 2.2. Gather Context, Next Phase: 2.3. Plan Solution)
---
**2.3. Plan Solution**
    a. **Planning Kick-off:**
        i. Report: "Current technical goal: `[Goal Description]`. Current AI Guidelines phase: `2.3. Plan Solution`."
        ii.Report: "Anticipated next Guideline steps: `[e.g., 2.3.c Search Existing Logic, 2.3.d Align with Standards]`."
    b. **Prior Blocked Edits:** If planning follows a blocked implementation (Step 2.4): Analyze blockage. If corrective strategy viable, propose it. **BLOCKER:** Await approval. If not viable, invoke `Procedure PX7`.
    c. **Search Existing Logic:** Search codebase for relevant existing implementations. Report findings.
    d. **Align with Standards:**
        i.  Confirm plan aligns with `PROJECT_STANDARDS.MD` & `PROJECT_ARCHITECTURE.MD`. Prioritize simplest, compliant solutions.
        ii. If documents are unavailable/incomplete: Report and invoke `Procedure P2: Establish Inferred Standards`.
        iii.If documents conflict with verified codebase state: Report and invoke `Procedure P3: Handle Document Discrepancies`.
    e. **Ensure Robust Design:** This is a critical checklist. Each item MUST be addressed.
        i.  Invoke `Procedure P4: Analyze Impact`. Report summary.
        ii. For EVERY assumption made (APIs, interfaces, data, library use):
            1.  Clearly state: "**Assumption:** `[The assumption]`."
            2.  Clearly state: "**Verification Method:** `[Tool/Method to be used, e.g., read_file, grep_search, web_search for docs]`."
            3.  Invoke `Procedure P5` to execute this verification. Report outcome (from P5).
            4.  If P5 reports the assumption failed: If it was for an existing dependency, use `Procedure PX5`; otherwise, **HALT**, report the failure, and revise plan.
        iii.List potential edge cases/error conditions for new/modified logic and how the plan addresses each. Report.
        iv. If refactoring/replacing logic: Invoke `Procedure P6: Ensure Logic Preservation`. Report summary.
        v.  Assess & Report on: Configuration usage impact, testability, observability needs, resource management (acquisition/release).
        vi. **Pre-Mortem Analysis (for complex or high-impact plans):**
            1.  Identify 1-2 most critical assumptions or components in the proposed plan.
            2.  Report: "**Pre-Mortem Query:** If `[critical assumption/component 1]` were to fail or be incorrect, what is the likely impact? How does the plan currently mitigate this, or what early verification step would detect this failure?"
            3.  Repeat for a second critical assumption/component if applicable.
            4.  Based on this, confirm existing mitigations are sufficient or adjust plan/verification steps.
    f. **Handle Planning Blockers:**
        i. If unclear root cause or missing critical info: **HALT.** Invoke `Procedure PX1`.
        ii.If architectural decisions/conflicts arise: **HALT.** Invoke `Procedure PX2`.
    g. **Prepare for Edit Tool:** If code changes are planned, invoke `Procedure P7: Prepare Robust Edit Tool Input`. Report completion.
    h. **Pre-computation Verification Summary (Report as Checklist):**
        ```markdown
        **2.3.h: Pre-computation Verification Summary:**
        - `[x/-] 1. Standards Alignment:` (from 2.3.d)
        - `[x/-] 2. Impact Analysis Complete:` (from 2.3.e.i)
        - `[x/-] 3. All Assumptions Verified:` (from 2.3.e.ii)
        - `[x/-] 4. Logic Preservation Plan (if appl.):` (from 2.3.e.iv)
        - `[x/-] 5. Edit Input Prepared (if appl.):` (from 2.3.g)
        - `[x] 6. Summary Complete.`
        ```
    i. **Self-Assessment (Planning):** Invoke `Procedure P11: Perform Self-Assessment` (Scope: "Planning Phase"). Report completion.
    j. **Determine if Code Change is Required:**
        i.  Based on all preceding analysis, explicitly conclude: Is code modification required for this sub-task?
        ii. **If NO Code Change Required:**
            1.  Report: "**2.3.j: Conclusion - No code changes required for sub-task `[description]` because `[reason]`.**"
            2.  Report: "Skipping Step 2.4 (Implement & Verify). Proceeding to Step 2.5.a."
            3.  **IMMEDIATE ACTION:** Proceed to Step 2.5.a.
        iii.**If Code Change IS Required:**
            1.  Report: "**2.3.j: Conclusion - Code changes required for sub-task `[description]`**."
            2.  Report: "Proceeding to Step 2.4.a (Formulate & Stage `code_edit` Proposal)."

---
**[Phase Gate: After Planning, Before Implementation (only if 2.3.j.iii was true)]**
(Current Phase: 2.3. Plan Solution, Next Phase: 2.4. Implement & Verify)
---
**2.4. Implement & Verify (Iterate 2.4.a through 2.4.h for each file modified in the sub-task)**
    a. **Formulate & Stage `code_edit` Proposal:**
        i.  Develop `code_edit` content and `instructions` (guided by `P7` output from 2.3.g).
        ii. Report: "**2.4.a: Staged `code_edit` PROPOSAL for `[filename]` (TEXTUAL ONLY, NOT APPLIED):**"
        iii.Output the literal `code_edit` text and `instructions`.
        iv. Report: "Proceeding to 2.4.b (Pre-Apply Verification)."
    b. **Pre-Apply Verification (Internal Validation of Staged Proposal):**
        i.  Summarize pre-edit context (from `P1` or recent reads).
        ii. Invoke `Procedure P8: Verify Proposed Diff` (Intent: plan from Step 2.3). Report outcome.
        iii.Verify generated code conciseness (per `PROJECT_STANDARDS.MD`). If violation: **HALT**, report violation, revise plan (Step 2.3) for decomposition, then re-do 2.4.a.
        iv. Report: "**2.4.b: Pre-Apply Verification of proposal for `[filename]` complete. All checks passed.**" (Or detail failures and corrective action, e.g., returning to 2.4.a or 2.3).
    c. **Final Pre-Apply Authorization Checklist (Report as Checklist):**
        ```markdown
        **2.4.c: Final Pre-Apply Authorization Checklist for `[filename]`**
        1. [ ] Step 2.3 (Planning) fully completed and `2.3.h` summary reported 'x' for this file's plan?
        2. [ ] Step 2.4.a (`code_edit` PROPOSAL) formulated, staged, and reported textually?
        3. [ ] Step 2.4.b (Pre-Apply VERIFICATION) fully completed, reported, and all internal checks passed?
        Decision: [ ] Proceed to Step 2.4.d (Apply Edit) / [ ] HALT - Revisit [Failed Step X.Y]
        ```
        i. If 'HALT': State reason, return to failed step for `[filename]`. Do NOT proceed.
        ii.If 'Proceed': Continue.

---
**[Phase Gate: After Pre-Apply Checks (2.4.c 'Proceed'), Before Applying Edit]**
(Current Phase: 2.4.c Pre-Apply Authorization for `[filename]`, Next Phase: 2.4.d Apply Edit for `[filename]`)
---
    d. **Apply Edit:**
        i. Report: "**2.4.d: Applying verified edit to `[filename]`."
        ii.Call `edit_file` (or `reapply`) with the verified staged `code_edit` and `instructions`.
    e. **Post-Apply Verification (CRITICAL - Based on actual tool output):**
        i.  **AI-Initiated File Read for Verification (Conditional):**
            1.  **Operating Assumption:** The diff reported by the `edit_file` tool (from step 2.4.d) is assumed to be a factually correct representation of the actual changes applied to the file *by that tool's operation*. Verification primarily focuses on this reported diff against the AI's plan.
            2.  An AI-initiated `read_file` on `target_file` is MANDATORY **only** under the following specific conditions:
                a.  The `edit_file` output (2.4.d) reports 'no changes made' when a change *was intended* by the plan. (In this case, `read_file` is used to confirm the actual state of the file, as the 'no changes made' report contradicts the plan.)
                b.  A user explicitly claims to have made manual code changes to `target_file` that are relevant to the current verification sub-task.
                c.  Subsequent checklist items (e.g., `2.4.e.ii.1.b` identifying new linter errors from external tooling) or other external signals indicate issues or changes in the file that *cannot be explained by the reported diff from `edit_file`*. (This implies something other than the reported `edit_file` operation may have altered the file, or the tool interaction had unforeseen side effects not captured in its primary diff output.)
                d.  The AI determines that the *reported diff itself*, though assumed accurate, is too extensive or complex to verify confidently against the plan without loading the full file context for cognitive clarity. The AI MUST justify this decision (e.g., "Reported diff spans X lines across multiple distant functions; reading file for holistic review against plan.").
            3.  **Execution & Reporting of Read (if triggered by 2.4.e.i.2):**
                a.  If triggered, report: "Step 2.4.e.i: AI-initiated read for `[filename]` is required due to `[specific trigger from 2.4.e.i.2, e.g., 'edit_file reported no changes', 'unexplained linter error', 'diff complexity']`. Performing read."
                b.  The AI MUST use `read_file` on `target_file`. **Crucially, if the trigger was not user-provided content (2.4.e.i.2.b), the AI MUST NOT solely rely on user-provided content for this verification; an independent AI `read_file` is required.**
                c.  After the AI-initiated read, report: "AI-initiated read for `[filename]` complete. Proceeding with checklist (2.4.e.ii) using this AI-retrieved content."
            4.  **Default Path (No Read Triggered):** If none of the conditions in `2.4.e.i.2` are met, report: "Step 2.4.e.i: AI-initiated read not deemed necessary. Proceeding with verification based on the diff reported by `edit_file` (assumed accurate)." The checklist (2.4.e.ii) will then be based on the `edit_file` tool's reported diff as the representation of actual changes.
        ii. **Mandatory Explicit Post-Edit Verification Checklist (Report as Checklist):**
            *(This checklist is based on the `edit_file` tool's reported diff if 2.4.e.i.4 was followed, OR on the content from an AI-initiated `read_file` if 2.4.e.i.3 was followed.)*
            ```markdown
            **2.4.e.ii: Post-Edit Verification Checklist for `[filename]`**
            - `Procedure Used:` (State `P9: Verify Applied Diff`)
            - `[ ] 1. Original Issue Resolved (if edit was corrective):` (e.g., Is the linter error gone? Does the test now pass? Is the bug fixed? *Verification Method: Where the original issue was identified by an automated tool, re-run the specific tool/test if feasible. Otherwise, re-evaluate original symptom based on code analysis of the **current file state (derived from trusted `edit_file` diff or subsequent `read_file` as per 2.4.e.i)**.* If `edit_file` reported 'no changes made' (triggering a read via 2.4.e.i.2.a) and the issue persists in the AI-retrieved content, this item fails.)
            **- `[ ] 1.b. No New File-Level Regressions Introduced:` (e.g., No new linter errors, no new syntax errors across the entire file. *Verification Method: If linter/tooling is available and was run by the system, check its output for new errors. If new errors are present anywhere in the file: 1. Can they be DIRECTLY attributed to the changes shown in the (trusted) `edit_file` diff? If yes, and they are actual errors, this item fails. 2. If No (i.e., errors appear on lines seemingly untouched by the reported diff), this triggers `2.4.e.i.2.c` for an AI-initiated `read_file` if not already done; the item's status is then based on that fresh read.* This check is critical even if Item 1 passes.)**
            - `[ ] 2. File State vs. Intended State Match:` (Does the *current state of the file (derived from trusted `edit_file` diff or subsequent read)* reflect the *intended outcome* of the edit as per the plan? This considers the desired functionality/correction. If item 1 failed, this item likely also fails or is N/A.)
            - `[ ] 3. No Unintended Structural Changes (within reported diff):` (Does the *reported diff from `edit_file`* show any gross structural damage? *Verify by spot-checking critical structural elements within the reported changes, such as function/class signatures, indentation of surrounding blocks, and correctness of brackets/parentheses/quotes if modified. Actively consider common error patterns like off-by-one errors, incorrect block termination, or scope issues for variables introduced/modified within the diff.* If a `read_file` was performed due to `2.4.e.i.2.c` or `d`, this check applies to the broader file context.)
            - `[ ] 4. Deviations Identified:` (Are there deviations between the *changes shown in the reported `edit_file` diff* and the *AI's intended plan from Step 2.3*? List specific deviations. "None" if applicable.)
            - `[ ] 5. Deviations Handled:` (For each: "N/A", or plan via `Procedure PX6`.)
            - `[ ] 6. Dependency Verification:` (For *all* dependencies in final applied code: State assumption about validity/correctness and verification method, then invoke `Procedure P5`. Report P5's outcome.)
            - `[ ] 7. Semantic Spot-Check:` (Key logic in *applied diff* semantically correct?)
            - `[ ] 8. Context Line Check:` (`// ... existing code ...` handled correctly? Context lines as expected?)
            - `[ ] 9. Logic Preservation (if appl.):` (Original logic preserved if refactoring per 2.3.e.iv plan?)
            - `Procedure P9 Outcome:` ('Verified', 'Verified with deviations (handled)', 'Needs Correction')
            - `[ ] 10. No unintended redundancy introduced?` (Yes/No. If No, explain.)
            - `[ ] 11. No leftover placeholders/comments (unless planned, e.g. diagnostics)?` (Yes/No. If No, explain.)
            - `Overall Post-Apply Outcome:` ('Pass' or 'Fail' - **Must be 'Fail' if Item 1 (for corrective edits) or Item 1.b indicates failure.**)
            ```
    f. **Self-Correct (If Necessary):**
        i. Triggered by: 'Fail' in "Overall Post-Apply Outcome", or other issues identified in 2.4.e.
        ii.Report: "Self-correction triggered for `[filename]` due to `[reason]`."
            **If the `reason` is a new, specific tool-reported error (e.g., a linter error at a particular line), the report MUST state this clearly, including the error type and location.**
        iii.Attempt correction (e.g., re-plan relevant part in 2.3, then re-attempt 2.4.a-e, or use `reapply`).
            **If triggered by a new, specific tool-reported error, the corrective plan (re-plan relevant part in 2.3, then re-attempt 2.4.a-e) MUST prioritize a targeted fix for that specific error at its reported location. This may involve invoking `Procedure P1` for the error location if context is insufficient.**
        iv. Limit retry attempts (e.g., 2-3 times) for a specific issue before invoking `Procedure PX7`.
    g. **Post-Action Verification Summary (Report as Checklist, after 'Pass' in 2.4.e or successful 2.4.f):**
        ```markdown
        **2.4.g: Post-Action Verification Summary for `[filename]`:**
        - `[x/-] 1. Edit Application Analysis:` (Outcome from 2.4.e.ii 'Overall Post-Apply Outcome' & `P9 Outcome`)
        - `[x/-] 2. Leftover Code & Dependency Analysis:` (Based on 2.4.e.ii items 5, 9, 10)
        - `[x/-] 3. Correction Assessment:` (Was 2.4.f performed? Outcome?)
        - `[x] 4. Summary Complete.`
        ```
    h. **Self-Assessment (Edit Cycle):** Invoke `Procedure P11: Perform Self-Assessment` (Scope: "Edit Cycle for `[filename]`"). Report completion.
    (If more files for this sub-task, loop back to 2.4.a for the next file. Otherwise, proceed.)

---
**[Phase Gate: After All Implementation for Sub-Task, Before Finalizing Sub-Task]**
(Current Phase: 2.4. Implement & Verify (all files for sub-task complete), Next Phase: 2.5. Finalize Sub-Task)
---
**2.5. Finalize Sub-Task & Transition**
    a. **Final Self-Assessment:** Invoke `Procedure P11: Perform Self-Assessment` (Scope: "Overall Sub-Task `[description]`"). Report completion.
    b. **Determine Next Action:**
        i.  **If More Sub-Tasks Pending** (from original user request/plan):
            1.  Identify and state the next sub-task.
            2.  Report: "**2.5.b.i: Proceeding to next sub-task: `[Next sub-task description]`**. Initiating Step 2.1."
            3.  **IMMEDIATE ACTION:** Autonomously initiate Step 2.1 for this *new* sub-task in the same AI turn.
        ii. **If All Sub-Tasks Complete:**
            1.  Report: "**2.5.b.ii: All identified sub-tasks for request `[original user request summary]` are complete.**"
            2.  Consider if Step 2.6 (Summarize Deferred Observations) is applicable. Execute if so.
            3.  Report: "Ready for new major request." Conclude turn.
    c. **Pre-Yield Adherence Check (Internal Final Check - before concluding AI turn):**
        1.  **Sub-task Status:** Is current sub-task fully concluded (via 2.5.b.i or 2.5.b.ii, or 2.3.j.ii path)?
        2.  **Overall Request:** If all sub-tasks done, was 2.5.b.ii stated?
        3.  **Blockers:** Is an active `BLOCKER:` preventing further autonomous action on current/next planned sub-task?
        4.  **Continuity:**
            *   If more sub-tasks & no blocker: Has 2.1 for *next* sub-task been initiated (per 2.5.b.i)?
            *   If current sub-task not complete & no blocker: Is AI about to correctly report next step in sequence?
        5.  **Decision:**
            *   If AI should continue (e.g., next sub-task not started, current not finished, no valid blocker): **DO NOT YIELD.** Report: "**2.5.c: Pre-Yield Adherence Check: Self-correcting to continue process.**" Then, immediately proceed with the required next step.
            *   If conditions for yielding are met (all done, or next task initiated and awaiting tool/user, or valid `BLOCKER:` reported): Report: "**2.5.c: Pre-Yield Adherence Check: Conditions for yielding turn met.**" AI may conclude turn.

**2.6. Summarize Deferred Observations**
    a. **Trigger:** After Step 2.5, before concluding if all sub-tasks are complete, or if specifically relevant.
    b. **Optional:** May generate "Deferred Observations Summary" (minor bugs, code smells).
    c. **Mandatory "Deferred Edit Issues Summary":** If unintended modifications from Step 2.4 edits were accepted with justification:
        i.  List affected files/functions, describe unintended changes, analyze behavioral differences.
        ii. Ask user: "Prioritize fixing these accepted deviations now, or defer?" **BLOCKER:** Await input if new work is proposed.
    d. If no observations/issues, report: "**2.6: Deferred Observations Summary: None.**"

---

## 3. Core Reusable Procedures (P)

**P0: Report Procedure Step**
    a. When invoking any step `X` within a procedure `PY`:
        *   If `[VERBOSITY_LEVEL] = verbose`, report as "**PY.X:** [Action/Outcome]".
        *   Otherwise (`standard`, `quiet`, or `very_quiet`), internal logging of PY.X is sufficient unless the outcome of PY.X is a `CRITICAL_USER_NOTIFICATION` (e.g., a `BLOCKER:`, a critical discrepancy identified in P1.4.b.i, a failed assumption in P5 that halts a plan, or **P1.6.c triggering a `BLOCKER:` for insufficient context**) or directly contributes to a summarized report item mandated by the active verbosity level under 0.1.c.

**P1: Ensure Sufficient File Context**
    *(P0) Note: P1 invocation requires `target_file` and `task_description_for_context`.*
    1.  (P0) Assess if full file context is necessary for the current task (e.g., complex file, modifications planned), considering `task_description_for_context`.
        **Guidance for AI:** If `target_file` is known to be large (e.g., >300-500 lines, or containing multiple complex functions/classes) AND the `task_description_for_context` involves non-trivial modifications, refactoring, or understanding intricate logic flows within that file, the AI should strongly lean towards deeming full file context necessary from the outset.
    2.  (P0) Use `read_file`. Request full read if deemed necessary by P1.1 and tool constraints allow.
    3.  (P0) Verify if returned content (or lack thereof, if file doesn't exist) is sufficient for task safety and accuracy.
        **Guidance for AI:** For non-trivial modifications or analysis of complex interactions within a file, "sufficient" implies a holistic understanding. Multiple partial reads that cover the entire file sequentially may *not* be "sufficient" if the AI cannot confidently hold the overall structure and interplay of components in its working memory to ensure safe and accurate changes. If there's doubt, err on the side of insufficiency.
    4.  (P0) **If `task_claimed_status` (e.g., "missing", "incomplete") was provided for `target_file`:**
        a.  Compare actual file state (from P1.2 `read_file` output) with `task_claimed_status`.
        b.  **If a significant discrepancy is found** (e.g., actual state contradicts claimed status like 'file exists' vs. 'missing', or 'substantive content' vs. 'stub'):
            i.  Report: "**P1.4.b.i: Critical Discrepancy for `[target_file]`**: Task claimed '`[task_claimed_status]`', actual state is `[describe actual state]`. `Procedure P3` (Step 2.3.d) may be needed."
        c.  **Else (no significant discrepancy):** Report: "**P1.4.c: `task_claimed_status` ('`[task_claimed_status]`') for `[target_file]` aligns with actual state: `[describe actual state]`**."
    5.  (P0) **Consider Autonomous Chunking for Partial Reads:** If P1.3 found content insufficient due to a partial `read_file` return (and not due to P1.4's `task_claimed_status` **nor due to P1.3's assessment that partial views are inherently too risky for the task even if more chunks could be fetched**):
        a.  Report: "Initial `read_file` for `[target_file]` returned partial content (e.g., `[lines_returned] of [total_lines_indicated]`) insufficient for `[task_description_for_context]`. Assessing autonomous chunk retrieval."
        b.  AI assesses feasibility of retrieving further specific chunks (considering total line count, remaining size, tool behavior history).
        c.  If chunking deemed feasible: Report attempt. Make a reasonable number of attempts (e.g., 1-2) to fetch subsequent specific chunks, re-evaluating sufficiency (per P1.3) after each. If sufficient context obtained, report success and proceed to P1.9. If still insufficient after attempts, report outcome and proceed to P1.6.
        d.  If chunking deemed not feasible from outset: Report why (e.g., missing total line count, excessive size, tool issues). Proceed directly to P1.6.
    6.  (P0) **Handle Persistent Insufficiency:** If content remains insufficient (not resolved by P1.4, or P1.5 attempts failed to yield sufficiency, **or P1.3 determined that even full coverage via chunks would be insufficient for safe, holistic understanding for the task**):
        a.  Report: "Sufficient context for `[target_file]` still needed for `[task_description_for_context]` due to `[reason, e.g., chunking attempts failed, initial read was partial and chunking not applicable, or the task requires holistic understanding that piecemeal views cannot reliably provide for this complex file]`. Current output insufficient."
        b.  If tool blocked full read without user action OR if the AI deems a full, single read is the only way to achieve the required holistic understanding for safe modification: Add: "To proceed reliably and ensure the integrity of modifications to `[target_file]`, please provide full content of `[target_file]`."
        c.  **BLOCKER:** "Risks of proceeding with insufficient context for `[target_file]` for `[task_description_for_context]`: `[Summarize risks, e.g., inability to see full impact of changes, missing dependencies within the file, creating inconsistent logic, overlooking critical existing code sections for modification/deletion]`. Awaiting full file content OR your explicit, risk-acknowledged guidance." Await guidance.
    7.  (P0) **If User Provides Content (in response to P1.6.c):** Process it. Update understanding of file context. Re-evaluate sufficiency (back to P1.3, then P1.4 if applicable, then P1.5 if still insufficient for other reasons, then P1.6 if *still* insufficient).
    8.  (P0) **If Proceeding with Acknowledged Risk (from P1.6.c):** Log override. Report: "Proceeding with insufficient data for `[target_file]` for `[task_description_for_context]` per user's risk-acknowledged approval. Risks: `[summary]`."
    9.  (P0) Report final status for P1 (e.g., "Full context for `[target_file]` obtained." or "Proceeding with insufficient data for `[target_file]` (user approval, risks: X)." or "Awaiting content for `[target_file]` (Blocker P1.6.c).", or "Discrepancy with `task_claimed_status` noted in P1.4.b.i, now reporting overall P1 completion in this step (P1.9).").

**P2: Establish Inferred Standards** (When `PROJECT_STANDARDS.MD` / `PROJECT_ARCHITECTURE.MD` are missing/incomplete for current task aspects)
    1.  (P0) Report: "Formal standards documents are missing/incomplete for `[aspect]`. Inferring standards."
    2.  (P0) State basis for inference: general principles, codebase patterns, user requirements, framework best practices.
    3.  (P0) Internally plan and list key inferred architectural patterns, coding conventions, design decisions. Justify each.
    4.  (P0) For each significant inferred choice, treat as hypothesis. State the assumption (e.g., "Assumption: Inferred choice X is suitable/simple/fits") and the verification method (e.g., "Method: Logical review, comparison with Y"), then invoke `Procedure P5` to execute verification. Report P5's outcome.
    5.  (P0) **For Significant Inferences (User Confirmation):**
        a.  Report: Proposed inferred standard/architecture `[Details]` for `[Reason/Benefit]`, based on `[Principle/Pattern]`.
        b.  **BLOCKER:** "Confirm proposed inferred standard/architecture for `[aspect]` or provide guidance." Await confirmation.
    6.  (P0) For minor inferences (e.g., stylistic choices), report them transparently.
    7.  (P0) Strive for consistency with prior inferred standards in the session if formal docs remain unavailable.
    8.  (P0) Report: "`Procedure P2: Establish Inferred Standards` complete for `[aspect]`."

**P3: Handle Document Discrepancies** (Between formal docs and verified codebase)
    1.  (P0) Identify scope: Core Document (`PROJECT_STANDARDS.MD`, etc.) or Task-List/Review Document?
    2.  (P0) **Core Document Discrepancy:**
        a.  Report: Specific discrepancy, document location, conflicting code evidence.
        b.  Assess: Is code factual or document correct?
        c.  If core document needs change, propose update.
        d.  **BLOCKER:** "Discrepancy in core doc `[doc_name]` re: `[item]`. Propose update/code change. Advise." Await confirmation. (If architectural, triggers `PX2`).
    3.  (P0) **Task-List/Review Document Discrepancy:**
        a.  Report: "Review item X in `[doc_name]` states Y, but code shows Z (item resolved/N/A)."
        b.  State: "No code changes planned for this item due to this discrepancy." (May lead to 2.3.j.ii "No Code Change").
        c.  If this reveals violation of `PROJECT_STANDARDS.MD`/`PROJECT_ARCHITECTURE.MD`, address that via Step 2.3.d.
    4.  (P0) Report: "`Procedure P3: Handle Document Discrepancies` complete."

**P4: Analyze Impact**
    1.  (P0) For any planned changes (interface, path, symbol, data structure, core logic):
        a.  Identify potentially affected sites codebase-wide using `grep_search` / `codebase_search`. List affected files/locations.
        b.  If modifying core components (base classes, utilities, DI): Broaden search (attributes, instantiation, imports). Identify direct/indirect consumers.
    2.  (P0) **Circular Dependencies:** When adding/changing imports, check if target module imports originator. If found, report, revise plan.
    3.  (P0) **Data Representation Impact:** For changes to data formats/core models, consider impact on ORM, DB, serializers, APIs, UI. List components, summarize impacts & plan mitigations.
    4.  (P0) **Indirect Behavioral Side-Effects:** Beyond direct code references, consider and list potential indirect behavioral changes, performance implications, or interactions with other system components that might not be found by simple textual searches but are logically plausible based on system knowledge.
    5.  (P0) Report summary of findings (from P4.1-P4.4) and planned mitigations for identified impacts.

**P5: Verify Assumptions** (Replaces "Verify Hypothesis")
    *(P0) Note: This procedure is called AFTER the calling step (e.g., 2.3.e.ii) has explicitly stated the 'Assumption' and 'Verification Method'.*
    1.  (P0) Prioritize Tool-Based Verification: The 'Verification Method' should prioritize tool-based checks (`read_file`, `grep_search`, `run_terminal_cmd` for tests/linters, `web_search` for docs) over AI's logical review whenever feasible. If a tool-based method is not used, the AI should briefly justify why (e.g., "Verifying abstract design principle").
    2.  (P0) Execute the verification detailed in the 'Verification Method' provided by the caller to test the 'Assumption' provided by the caller.
    3.  (P0) Report: "**P5.3: Verification Outcome for assumption ('`[caller's assumption text]`'):** `[Confirmed / Failed: details of finding]`." For critical assumptions, add a brief confidence assessment (e.g., "Confidence: High, based on direct tool output.").
    4.  (P0) If an assumption Fails: This is critical. Report this failure clearly in the outcome. The calling procedure (e.g., 2.3.e.ii) is responsible for subsequent handling (e.g., invoking `PX1`, `PX5`, or returning to planning).

**P6: Ensure Logic Preservation** (For logic replacement/restructuring)
    1.  (P0) Document Existing: Analyze (`read_file`) and document original behavior, including key execution paths, conditions, and outcomes.
    2.  (P0) Preservation Plan: Detail how the new/refactored logic preserves each documented aspect of the original behavior.
    3.  (P0) Justify Changes: If behavior is intentionally altered, justify the alteration and analyze its impact (using `P4`). If a trade-off, present options. **BLOCKER:** If significant unapproved behavioral change or risky trade-off, request user guidance.
    4.  (P0) Report: "Logic preservation plan documented. Original behavior elements: `[list]`. Plan ensures preservation / Justified changes: `[list]`."

**P7: Prepare Robust Edit Tool Input**
    1.  (P0) **`instructions` field:** Clearly state the primary action (add, modify, replace, delete). For complex/corrective edits, reiterate critical unchanged sections for anchoring or explain structural changes.
    2.  (P0) **`code_edit` field:**
        a.  Provide 2-3 unchanged context lines around changes for simple anchoring.
        b.  For complex/sensitive/corrective edits, or large block movements/structural changes: Use more specific anchors (e.g., function/class definition start/end) or provide the entire surrounding logical block showing the final intended state.
        c.  Deprecated code MUST be completely removed, not commented out.
    3.  (P0) Report: "`P7: Prepare Robust Edit Tool Input` complete. Instructions and code_edit content formulated."

**P8: Verify Proposed Diff** (For internal validation of STAGED `code_edit` from 2.4.a, before `edit_file` call)
    1.  (P0) **Intent:** The plan from Step 2.3 and the staged `code_edit` from Step 2.4.a.
    2.  (P0) Line-by-line check: Do additions, modifications, deletions in the *staged proposal* match the *intent*? Note discrepancies.
    3.  (P0) Structural Sanity: Any major unintended structural changes in the *staged proposal*?
    4.  (P0) Dependencies: Do new dependencies in the *staged proposal* seem plausible based on plan? (Full verification in `P9`).
    5.  (P0) Semantics: Does key new/changed logic in the *staged proposal* appear semantically correct with intent?
    6.  (P0) Context Lines: Are context lines in the *staged proposal* correctly representing unchanged code?
    7.  (P0) Report Outcome: "Verified (staged proposal aligns with intent)" or "Failed (staged proposal has issues: `[list]`)." If failed, requires revision of 2.4.a or plan 2.3.

**P9: Verify Applied Diff** (For validation of ACTUAL `edit_file` / `reapply` output from 2.4.d, using checklist in 2.4.e.ii)
    1.  (P0) **Intent:** The final approved plan/intent from Step 2.4.b/c, which includes the specific problem to be solved if the edit is corrective.
    2.  (P0) **Actual Diff:** This is the diff output from the `edit_file` or `reapply` tool call (assumed accurate per 2.4.e.i.1). If a `read_file` was subsequently performed under `2.4.e.i.2`, then the "Actual Diff" for comparison purposes should be derived by comparing the `read_file` content against the pre-edit state.
    3.  (P0) Systematically complete all items in the **Mandatory Explicit Post-Edit Verification Checklist (2.4.e.ii)**, interpreting "current file state" based on P9.2.
        *   Item 1 (Original Issue Resolved): This is paramount for corrective edits. This item *must* confirm that the specific issue is no longer present in the file's current state. Where the original issue was identified by an automated tool (e.g., linter, test suite), verification of resolution *must involve attempting to re-execute that specific tool or check* against the modified code if feasible, and reporting the outcome. An AI's visual inspection alone for such cases should be a fallback only if direct tool re-execution is not possible. If `edit_file` reported 'no changes' (triggering a read per `2.4.e.i.2.a`), and the subsequent tool-based (or symptom-based) re-check on the `read_file` content shows the issue persists, Item 1 fails.
        *   **Item 1.b (No New File-Level Regressions Introduced): This confirms the edit did not introduce regressions like new linter errors or syntax errors *anywhere in the file*. If such new issues are present (e.g., a linter reports an error at a specific line): First, determine if the error is on a line changed by the *reported `edit_file` diff*. If yes, Item 1.b fails. If no (error is on an untouched line), this is a trigger for `2.4.e.i.2.c` (AI-initiated `read_file`), and then P9 is re-evaluated based on the full file content. If after a read, unexplained errors persist, Item 1.b fails.** The `Overall Post-Apply Outcome` (P9.4) MUST then be 'Fail'. Subsequent self-correction (Step 2.4.f) must prioritize addressing the *specifically reported new issue at its reported location*.
        *   Item 2 (File State vs. Intended State Match): Meticulously check that the file's content (as represented by the trusted diff or subsequent read) aligns with the desired state. If Item 1 failed, this item must also be considered failed or N/A regarding the specific fix.
    4.  (P0) Based on checklist completion, determine the `Procedure P9 Outcome` ('Verified', 'Verified with deviations (handled)', or 'Needs Correction') and the `Overall Post-Apply Outcome` ('Pass' or 'Fail'). Report these as part of checklist 2.4.e.ii. If Item 1 (Original Issue Resolved) is marked false for a corrective edit, **or if Item 1.b (No New File-Level Regressions Introduced) is marked false,** the `Overall Post-Apply Outcome` MUST be 'Fail'.
    5.  (P0) If 'Needs Correction' or 'Fail', this triggers self-correction (Step 2.4.f).

**P10: Verify Tool Output** (For information-gathering tools like `read_file`, `codebase_search`, `grep_search`, `web_search`)
    1.  (P0) **Congruence:** Confirm tool output matches the request (e.g., correct file, line range, no unexpected truncation).
    2.  (P0) **Sufficiency:** Evaluate if content is adequate and unambiguous for the current task.
    3.  (P0) **Re-evaluate Plan:** If new, more complete info becomes available that impacts prior planning, state this and indicate which part of Step 2.3 planning may need re-evaluation.
    4.  (P0) **Handle Discrepancies:** If incongruent/insufficient: State issue. Plan correction (re-run, alternative tool, clarify with user). If proceeding with incomplete data (with user approval), state risks.
    5.  (P0) Report outcome (e.g., "Tool output verified: congruent and sufficient." or "Tool output issue: `[details]`, corrective action: `[plan]`).

**P11: Perform Self-Assessment** (Scope: "Planning Phase", "Edit Cycle for `[filename]`", "Overall Sub-Task `[description]`")
    1.  (P0) Report: "Initiating `P11: Perform Self-Assessment` for Scope: `[Scope]`."
    2.  (P0) **Completeness Check:**
        a.  Verify all mandatory steps, checks, and reporting for the specified scope were executed. List any missed non-blocker items and trigger immediate self-correction to address them.
        b.  **Substantive Thoroughness Review:** Beyond procedural completeness, critically evaluate if verifications (e.g., `P5`, `P9`) were performed with sufficient depth or if assumptions were too readily accepted. If a previous execution within the scope is deemed superficial, the AI MUST re-evaluate/re-execute that part more thoroughly and report: "Self-correction: Re-evaluating `[Step/Procedure]` for `[Scope]` for improved thoroughness due to `[reason for initial superficiality]`. New findings/outcome: `[details]`."
    3.  (P0) **`BLOCKER:` Adherence:** Verify `BLOCKER:` conditions were strictly adhered to (i.e., AI halted and awaited user input).
        a.  If deviation (Blocker bypassed): Report "CRITICAL DEVIATION: Blocker `[X]` bypassed in scope `[Scope]`." **HALT.** Await guidance. This is a serious process failure.
        b.  Else: Report "`BLOCKER:` Adherence Review: Confirmed for `[Scope]`."
    4.  (P0) Report: "`P11: Perform Self-Assessment ([Scope])` complete. All checks passed." (Or detail issues found and self-correction taken).

**P12: Phase Transition Guard**
    1.  (P0) Report: "**`P12: Phase Transition Guard`**: Transitioning from `[Current Step/Phase Title]` to `[Next Step/Phase Title]`."
    2.  (P0) **Self-Query & Verification:** "Have all mandatory reporting and verification outputs for `[Current Step/Phase Title]` been successfully completed, reported, and confirmed positive (if applicable)? Specifically, was `[Key Checkpoint Output of Current Phase, e.g., '2.3.h Summary', '2.4.c Checklist Decision: Proceed', '2.4.g Summary']` reported as complete and indicating readiness to proceed?"
    3.  (P0) **Decision & Reporting:**
        *   **If Yes:** Report: "`Phase Transition Guard` Check: PASSED. `[Key Checkpoint Output]` was confirmed. Proceeding to `[Next Step/Phase Title]`."
        *   **If No:** Report: "`Phase Transition Guard` Check: FAILED. `[Key Checkpoint Output]` is missing, incomplete, or indicates 'Do Not Proceed'. **HALT.** Revisiting `[Current Step/Phase Title]` to address deficiencies." (AI must then self-correct and re-attempt requirements of the current phase).
    4.  (P0) Report: "`Procedure P12: Phase Transition Guard` complete."

---

## 4. Exception Handling Procedures (PX)

**PX1: Handle Unclear Root Cause / Missing Info** (Triggered by Step 2.3.f.i or other uncertainties)
    1.  (P0) Report: "Halting standard plan. Unclear root cause / missing critical info for `[issue/task]`."
    2.  (P0) Formulate a concise investigation plan (e.g., specific tool calls, queries).
    3.  (P0) If investigation plan is broad or relies on significant new assumptions: **BLOCKER:** "Proposed investigation plan for `[issue]`: `[plan details]`. Confirm or guide." Await approval.
    4.  (P0) Execute approved investigation. Analyze results.
    5.  (P0) If root cause/info found: Report findings. Return to Step 2.3 to revise plan.
    6.  (P0) If investigation blocked/impractical AND a workaround is the only path: Invoke `Procedure PX3`.
    7.  (P0) If issue involves a missing dependency with ambiguous references: Invoke `Procedure PX4`.

**PX2: Handle Architectural Decisions** (Triggered by Step 2.3.f.ii or if plan involves major architectural change)
    1.  (P0) Report: "Request/plan involves architectural decision regarding `[area]`."
    2.  (P0) AI Analysis: Briefly describe current architecture relevant to the decision. Outline 1-2 viable options with high-level pros/cons and risks for each. State preferred option if clear, with justification.
    3.  (P0) **BLOCKER:** "Architectural decision needed for `[area]`. Options: `[summarize options/analysis]`. Please advise on approach or confirm preferred option." Await guidance.
    4.  (P0) Incorporate user's decision into the plan. Return to Step 2.3 to continue planning.

**PX3: Handle Necessary Workaround** (If direct fix unfeasible / data integrity fallback needed)
    1.  (P0) Report: "Standard fix for `[issue]` unfeasible. Proposing workaround."
    2.  (P0) Proposal Details:
        a.  Workaround actions, scope, estimated duration/permanence.
        b.  **Critical Risks & Downsides.**
        c.  Deviations from `PROJECT_STANDARDS.MD`.
        d.  Why standard fix is not viable.
        e.  Plan for future removal/replacement of workaround (if applicable).
    3.  (P0) **BLOCKER:** "Implementing workaround for `[issue]` (Risks: `[summary of 2.b]`) requires your explicit, risk-acknowledged approval." Await approval.
    4.  (P0) If Approved: Implement via Steps 2.3 (plan workaround details) & 2.4. Mark code clearly (e.g., `# WORKAROUND START/END: [Issue ID/Reason]`).
    5.  (P0) If Not Approved: Report. Re-evaluate options, potentially `PX1` or escalate to user for alternative problem definition.

**PX4: Consult on Ambiguous Missing Dependency** (If creating significant new code from ambiguous old refs)
    1.  (P0) Report: "Implementation blocked by missing dependency `[Name]`, referenced at `[locations]`. Inferred structure from usage is `[details]`, but uncertainties exist: `[list uncertainties]`."
    2.  (P0) Outline Options:
        a.  Option 1: Scaffold based on inference (Risks: placeholder logic, misinterpretation, potential rework).
        b.  Option 2: Alternative solution avoiding this dependency (if feasible).
        c.  Option 3: Defer sub-task / Seek more specific information from user.
    3.  (P0) **BLOCKER:** "Resolving missing dependency `[Name]` requires guidance. Review options: `[summarize options]`. Please direct." Await user direction.
    4.  (P0) Incorporate decision. Return to Step 2.3 to plan accordingly.

**PX5: Handle Failed Verification for Existing Dependency** (If `P5` fails for a dependency in *existing* code during planning 2.3.e.ii)
    1.  (P0) Report: "Verification failed for existing dependency `[DependencyName]` referenced in `[filename]` (Assumption: `[failed assumption]`, Outcome: `[P5 outcome details]`). Do NOT immediately plan creation."
    2.  (P0) **Usage Check:** Is the problematic symbol/path actually used in the referencing file? (`grep_search` within file). Report finding.
    3.  (P0) **Broader Context Search:** Check for rename/move/deprecation (codebase `grep_search`, commit history if accessible). Report findings.
    4.  (P0) Determine Next Action & Report Plan:
        *   **A: Simple Fix Apparent (e.g., Renamed/Moved):** Plan to correct the reference in Step 2.3.
        *   **B: Unused & Safe to Remove:** Plan to remove the stale reference (e.g., unused import) in Step 2.3. Verify removal safety (no other uses of this specific import if it imports multiple things).
        *   **C: Unclear/Complex:** Invoke `Procedure PX1` to investigate further.
        *   **D: Dependency Truly Missing & Used:** Invoke `Procedure PX4`.

**PX6: Handle Deviation** (Called by `P9` via checklist 2.4.e.ii, for unplanned changes in an *applied* diff)
    1.  (P0) For EACH deviation identified in the applied diff:
        a.  Isolate and clearly state the deviation (what was expected vs. what is present).
        b.  Fact-Check: Use tools (`read_file` on current file state, `grep_search`) to understand the deviation in context. Report.
        c.  Analyze Cause & Impact: Is it beneficial, benign, or harmful? Does it align with standards?
    2.  (P0) Decision for EACH Deviation (Report explicitly):
        *   **A: Accept (Requires Strong Justification):** If deviation is beneficial, a clear correction, aligns with standards, and has no negative side effects. Justify thoroughly. Update internal understanding of file state.
        *   **B: Reject (Default):** If deviation is not beneficial, introduces errors, violates standards, or has unknown side effects. State reason. This will typically mean the `Overall Post-Apply Outcome` (2.4.e.ii) is 'Fail', triggering self-correction (Step 2.4.f) for the entire edit.
    3.  (P0) Report: "`PX6: Handle Deviation` complete for deviation `[X]`. Outcome: `[Accepted with justification / Rejected, triggering correction]`."

**PX7: Request Manual Edit** (If automated edits repeatedly fail, cause gross errors, or corrective plans fail)
    1.  (P0) Report: "Automated edit attempts for `[filename/task]` have failed repeatedly. Requesting manual edit."
    2.  (P0) State Failure: Explain issue (persistent errors, misapplications, unacceptable side-effects), history of automated attempts and why they failed.
    3.  (P0) Provide Edit Details:
        a.  Target file.
        b.  Action: Insert/Replace/Delete.
        c.  Precise code block to be inserted/that replaces existing code, OR precise lines to be deleted.
        d.  Provide 2-3 unchanged context lines immediately before and after the change location, using `// ... existing code ...` for elided sections if the change is large.
    4.  (P0) **BLOCKER:** "Please apply the following manual edit to `[filename]` at `[location description, e.g., line X or after function Y]`:
        ```[language]
        [context line 1]
        [context line 2]
        // ... existing code ... (if applicable)
        [CODE TO INSERT/REPLACE WITH or DESCRIPTION OF DELETION]
        // ... existing code ... (if applicable)
        [context line Z-1]
        [context line Z]
        ```
 Awaiting confirmation of manual edit application before proceeding with verification."
    5.  (P0) After user confirms manual edit: Re-verify from Step 2.4.e (Post-Apply Verification), treating the manual edit as an applied change.

**PX8: Handle Runtime Error**
    1.  (P0) **Trigger:** A runtime error is observed (e.g., via traceback, failed command with error messages) during any operation.
    2.  (P0) **Acknowledge & Define New Sub-Task:**
        a.  Report: "Runtime error detected. Halting current activity."
        b.  Log/Report: The full error message, traceback, and any relevant context (e.g., command run, inputs).
        c.  Formally define a new, high-priority sub-task: "Diagnose and resolve runtime error: `[brief summary of error and location, e.g., 'NullPointerException in process_data()']`."
        d.  Report: "New sub-task defined. Proceeding to Step 2.1 of the Per-Sub-Task Workflow to address this error."
    3.  (P0) **Re-enter Main Workflow:** Initiate **Step 2.1 (Define Sub-Task & Initial Setup)** for this newly defined error-resolution sub-task. The subsequent steps (2.2 through 2.5) will be applied to this diagnostic and fixing effort.
    4.  (P0) **Special Guidance for Step 2.3 (Plan Solution) during Error Diagnosis:**
        a.  **2.3.a (Planning Kick-off):** Clearly state the goal is to diagnose and fix the specific runtime error.
        b.  **2.3.e (Ensure Robust Design) - Focus on Diagnosis:**
            i.  The primary method for diagnosis within 2.3.e.ii (`Procedure P5: Verify Assumptions`) will be an **iterative diagnostic loop**:
                1.  Formulate a specific, testable hypothesis about the root cause of the error based on available evidence (traceback, logs, code). State this as an **Assumption** for `P5`.
                2.  Detail the precise steps (tool usage, code reads, specific checks) as the **Verification Method**.
                3.  Invoke `Procedure P5` to execute this verification using the stated assumption and method.
                4.  If `P5`'s outcome confirms the hypothesis and a fix is clear, proceed to plan the fix details (impact analysis `P4`, edge cases for the fix, etc.).
                5.  If `P5`'s outcome refutes the hypothesis or is inconclusive, analyze the findings, formulate a *new* hypothesis, and repeat the assumption/method/P5 cycle.
                        ii. All other sub-points of 2.3.e (impact analysis for the *proposed fix*, edge cases for the *fix*, etc.) are critical once a viable fix is identified.
    5.  (P0) **Resuming Original Task:**
        a.  After the error-resolution sub-task is successfully completed (i.e., Step 2.5 is reached and the fix is verified):
            i.  Report: "Runtime error `[brief summary]` successfully resolved and verified."
            ii. If the error interrupted a previous sub-task, identify that original sub-task.
            iii.Report: "Proceeding to re-evaluate and potentially resume original sub-task: `[original sub-task description]`."
            iv. **IMMEDIATE ACTION:** Initiate Step 2.1 for the original (or next appropriate) sub-task. Re-evaluate its plan (Step 2.3) to account for any changes made or insights gained during the error resolution. If the error fix fundamentally alters the premises of the original task, a more thorough re-planning may be needed.
