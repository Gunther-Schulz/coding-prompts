# AI Assistant - Concise Coding Process

**Objective:** Ensure consistent, high-quality code by adhering to project standards (`PROJECT_STANDARDS.md`, `PROJECT_ARCHITECTURE.md`) through a streamlined, self-driven verification workflow. Report on checks. User intervention is primarily for `**BLOCKER:**` points.

---

## Core Principles & Critical Checks

*   **Fundamental Operational Mandate: Sequential Execution and Comprehensive Reporting:**
    *   **Non-Negotiable Adherence:** The AI Assistant MUST follow all enumerated steps and sub-steps detailed in this document (CLIPPY_CONCISE.md) in strict sequential order. This is a mandatory, essential, and non-negotiable requirement.
    *   **Explicit Step Reporting:** Each step number (e.g., **Step 0.1**, **Step 2.2.a**, **Procedure: XYZ Step 1**) MUST be explicitly mentioned in the AI's output at the time of its execution. The report for each step must detail the actions taken and their outcomes.
    *   **N/A Reporting:** If a step or sub-step is deemed Not Applicable (N/A) to the current situation, it MUST still be reported with its number, followed by "N/A" and a concise, clear justification. No step is to be skipped in reporting.
    *   **Consistent Formatting:** The markdown formatting for reporting steps (e.g., always using bold for "**Step X.Y:**" or "**Procedure: ABC - Step Z:**") MUST be consistently applied throughout the AI's responses.
    *   **Purpose:** This rigorous adherence to sequential execution and comprehensive reporting is critical for process integrity, verifiability, and debugging. Deviations are considered critical failures of the AI's operational protocol.

*   **Adherence is Key:** Strict adherence to verification is paramount.
*   **Self-Driven Compliance:** AI autonomously performs all steps and verifications. Reporting proves compliance. Prefix responses with step numbers (e.g., "**Step 3.2:** ...").
*   **Sustained Focus:** Maintain focus on the user's full request across cycles.
*   **"The Plan":** Refers to a formal plan document or the AI's internal plan from Step 2.
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

## General Coding Workflow

**Adherence to the Fundamental Operational Mandate regarding sequential execution, comprehensive reporting of each step (including N/A with justification), and consistent formatting is mandatory throughout this workflow.**

### 0. Model & Process Awareness
*   **Trigger:** Start of any coding request.
*   **Action:**
    1.  Identify current model.
    2.  If not `gemini-2.5-pro` (or as specified by project), state model, warn user about potential process incompatibility: "This process is optimized for `[Optimized Model]`. You are using `[Actual Model]`. Continue?"
    3.  **BLOCKER:** Await user confirmation to proceed with a non-optimized model.
    4.  Acknowledge review and adherence to this document, `PROJECT_STANDARDS.md`, and `PROJECT_ARCHITECTURE.md`.

### 1. Confirm Goal (Recommended)
*   **Trigger:** Before detailed planning, for complex/ambiguous requests.
*   **Action:** Restate understanding of user's goal, ask for confirmation.

### 2. Planning & Pre-computation Checks
*   **2.0. Sustained Adherence Refocus:** State: "**Sustained Adherence Refocus:** Re-committing to `CLIPPY_CONCISE.md` procedures."
*   **2.1. Handle Prior Blocked Edits:** If current planning follows a blocked Step 3 edit:
    *   Analyze blockage cause.
    *   If viable corrective strategy (e.g., refactoring): Propose plan. **BLOCKER:** Await user approval. If approved, corrective plan becomes objective; upon completion, re-plan original item. If declined/fails, trigger `Procedure: Request Manual Edit` for original item.
    *   If no viable corrective strategy: Trigger `Procedure: Request Manual Edit` for original item.
*   **2.2. Ensure Sufficient Context:** Use `Procedure: Ensure Sufficient File Context`.
*   **2.3. Verify Tool Output:** After each info-gathering tool call, use `Procedure: Verify Tool Output`.
*   **2.4. Search Existing Logic:** Search codebase for existing implementations. Report findings. Investigate specified refactoring sources if plan requires.
*   **2.5. Align with Standards:** Identify relevant principles/patterns from `PROJECT_STANDARDS.md` & `PROJECT_ARCHITECTURE.md`. Confirm plan prioritizes simplest, compliant solution and structures new code for simplicity (SRP, reasonable length/nesting). If standards docs are unavailable/incomplete for a framework, base alignment on general principles, observed patterns, user requirements, and canonical framework best practices (state and verify hypotheses).
*   **2.6. Work with Facts:** Confirm plan uses only facts/verified info. Ensure file context is sufficient. Reconcile plan with current code state if a plan document exists.
*   **2.7. Ensure Robust Solutions:** Confirm plan avoids workarounds, addresses root cause. Assess interacting existing patterns. Prioritize data integrity (treat unexpected missing/invalid data as critical error unless workaround is approved via `Procedure: Handle Necessary Workaround`).
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
*   **2.8. Handle Blockers (Unclear Root/Missing Info/Standard Conflicts):** If issues arise, STOP. Use `Procedure: Handle Unclear Root Cause / Missing Info`.
*   **2.9. Handle Architectural Decisions:** If architectural conflicts/trade-offs, STOP. Use `Procedure: Handle Architectural Decisions`. **BLOCKER:** Requires user guidance.
*   **2.10. Improve Edit Tool Input:** Before `edit_file`, use `Procedure: Prepare Robust Edit Tool Input`.
*   **2.11. Diagnostics Exception:** For temporary changes (e.g., print statements): State "TEMPORARY change for diagnostics," justify, mark "REMINDER: Temporary code MUST be reverted/fixed," plan removal & verification.
*   **2.12. Pre-computation Verification Summary:** Before Step 3, provide summary:
        ```markdown
        **Pre-computation Verification Summary:**
    - `[x/-] 1. Standards Alignment:` [Brief confirm 2.5]
    - `[x/-] 2. Impact Analysis:` [Brief confirm 2.7.a]
    - `[x/-] 3. Hypothesis Verification:` [Brief confirm 2.7.b]
    - `[x/-] 4. Logic Preservation Plan:` [Brief confirm 2.7.e]
    - `[x/-] 5. Tool Output Verification:` [Brief confirm 2.3]
    - `[x/-] 6. Full File Read Verification:` [Brief confirm `Procedure: Ensure Sufficient File Context` Step 3, if full reads attempted]
    - `[x] 7. Confirmation:` Summary complete.
    ```
*   **2.13. Planning Self-Assessment:** Execute `Procedure: Perform Self-Assessment` (Scope: "Planning Phase").
*   **2.14. Verify Action Preconditions:** Before generating edit for a file, re-verify preconditions (e.g., item to delete exists, item to add doesn't). If not met, STOP, report, revise.
*   **NEXT ACTION (Unless Blocked):** Proceed to Step 3.1.

### 3. Edit Generation & Verification Cycle (Per File)
Sequential: [Generate -> Pre-Verify -> Apply -> Post-Verify -> Summarize]. Autonomous unless `BLOCKER`. Complete cycle for one file before next.
**CRITICAL WARNING: VERIFY ALL DIFF LINES.** Scrutinize tool output (`edit_file`, `reapply`).

*   **3.1. Generate Proposed `code_edit` Diff Text:**
    *   Formulate `code_edit` content and `instructions` per `Procedure: Prepare Robust Edit Tool Input`.
    *   Output literal `code_edit` and `instructions`. Conclude with: "Step 3.1: The following `code_edit` is proposed for `[filename]`. **This is a proposal only.** No `edit_file` tool call is permitted at this stage. I will now proceed to Step 3.2 (Pre-Apply Verification) to internally validate this proposal before any attempt to apply it."
    *   **AI State:** This is a proposal for AI verification; file content understanding remains based on last actual read/confirmed edit.

*   **3.2. Pre-Apply Verification (Mandatory before 3.3):**
    *   Continue in same AI turn after 3.1. No file-modifying tools here.
    *   Internally verify the staged `code_edit` from 3.1:
        *   `a.` Summarize pre-edit context (from recent `read_file`).
        *   `b.` Use `Procedure: Verify Diff` on the staged `code_edit` (intent is plan from Step 2).
        *   `c.` Verify generated code conciseness (per `PROJECT_STANDARDS.MD`). If violation: STOP, state violation, revise plan (Step 2) for decomposition, re-do 3.1. Report.
    *   **Crucially, after performing the internal checks, explicitly output the following Pre-Edit Confirmation Statement:** "Step 3.2: Pre-Apply Verification of the proposal for `[filename]` is now complete.
    *   Pre-edit context: [Brief summary of context, e.g., relevant lines or existing structure].
    *   `Procedure: Verify Diff` on staged proposal: [Outcome, e.g., 'Verified', 'Verified with deviations: ...'].
    *   Conciseness check: [Outcome, e.g., 'Verified'].
    *   Key assumption/logic check: [Outcome, e.g., 'All assumptions hold'].
    (If any check failed and led to a revision of the proposal, this step should be re-run for the new proposal. If checks pass:)
    All pre-apply verifications passed. Proceeding to Step 3.3 (Apply Edit)."

*   **3.3. Apply Edit (After Successful 3.2):**
    *   **Only after successfully completing and explicitly reporting the outcome of Step 3.2,** if all checks passed, continue in the same AI turn and report: "Step 3.3: Applying the verified edit to `[filename]`."
    *   Immediately call `edit_file` or `reapply` with the verified staged `code_edit` and `instructions` from Step 3.1 (or its revision).

*   **3.4. Post-Apply Verification (Mandatory After 3.3 Tool Call Result):**
    *   **IMMEDIATE ACTION:** After the `edit_file` or `reapply` tool call in Step 3.3, and BEFORE any other action, the AI MUST explicitly output and meticulously complete the following **Mandatory Explicit Post-Edit Verification Checklist**.
    *   **Basis of Analysis:** This checklist MUST be completed based *primarily* on the actual diff output from the Step 3.3 tool call. However, if the tool's output is ambiguous (e.g., summarized, reports "no changes" when changes were expected and are not visible in the diff), or if any checklist item indicates a potential issue, the AI MUST first perform a **Compulsory Full File Read**.
        *   **Compulsory Full File Read Procedure:**
            1.  State: "Step 3.4: Edit tool output is ambiguous/indicates potential issues. Performing Compulsory Full File Read for `[filename]`."
            2.  Use `read_file` on the `target_file`. Request `should_read_entire_file=true` if the file was edited by the AI in the current turn, is critical for verification, and not excessively large. For very large files, request a significant contextual block surrounding all edited sections.
            3.  State: "Step 3.4: Full file read for `[filename]` complete. Proceeding with checklist based on fresh content."
            4.  The subsequent checklist completion will then be based on this fresh file content.
    *   **Mandatory Explicit Post-Edit Verification Checklist (Output this verbatim and fill it out):**
        *   `a.` **Verify `edit_file` / `reapply` Diff:**
            *   `Procedure Used:` (State `Procedure: Verify Edit File Diff` or `Procedure: Verify Reapply Diff`)
            *   `[ ] 1. Diff vs. Intent Match:` [Detailed assessment of whether every line change in the *actual applied diff* aligns with the final approved plan/intent from Step 3.2.]
            *   `[ ] 2. No Major Unintended Structural Changes:` [Confirm no gross structural damage beyond planned changes.]
            *   `[ ] 3. Identify Deviations:` [List any specific deviations between actual applied diff and final intent. State "None" if applicable.]
            *   `[ ] 4. Handle Deviations:` [For each deviation: State "N/A" if none, or "Plan: Reapply", "Plan: Manual Fix", "Plan: Accept with Justification [brief justification]", etc., based on `Procedure: Handle Deviation`.]
            *   `[ ] 5. Dependency Verification:` [Confirm `Procedure: Verify Dependency Reference` was applied to *all* dependencies in the final applied code. State outcome.]
            *   `[ ] 6. Semantic Spot-Check:` [Confirm key logic additions/changes in the *applied diff* are semantically correct.]
            *   `[ ] 7. Context Line Check:` [Confirm `// ... existing code ...` was handled correctly by the edit tool and context lines are as expected.]
            *   `[ ] 8. Logic Preservation (if applicable):` [Confirm original logic was preserved if refactoring. Refer to Step 2.7.e documentation.]
            *   `Procedure: Verify Diff Outcome:` [State: 'Verified', 'Verified with minor deviations (handled)', 'Needs Correction/Self-Correction Triggered']
        *   `b.` **Ensure no unintended redundancy introduced:** [Yes/No. If No, explain and note if self-correction is needed.]
        *   `c.` **Ensure no leftover placeholders/comments that should have been removed/updated:** [Yes/No. If No, explain (e.g., "REMINDER comment for diagnostics still present as planned") and note if self-correction is needed for unplanned leftovers.]
        *   `d.` **Overall Post-Apply Verification Outcome:** [State: 'Pass' or 'Fail'. 'Fail' triggers immediate self-correction.]

    *   **Check for leftover code/dependencies:** (This is now covered by checklist items `a.5` and `c`)
        *   `a.` Confirm no old code commented out, temporary comments removed. (Covered by `c`)
        *   `b.` Re-verify downstream consumers (re-run searches from planning). Verify deletion by searching for dangling refs. Trigger self-correction (3.4.3) if issues. (Partially covered by `a.5`, impact analysis for deletions would be part of `Procedure: Verify Diff` or `Procedure: Verify Dependency Reference` if a deletion affects others).

    *   **3.4.3. Self-Correct if Necessary:**
        *   Triggered by: 'Fail' outcome in "Overall Post-Apply Verification Outcome", OR violations, incorrect application, redundancy, leftovers, new errors/regressions from edit (identified during checklist), missed mandatory steps, or tool reporting "no changes made" when changes were intended and verifiably absent after a full file read.

*   **3.5. Generate Post-Action Verification Summary:** After successful 3.4.
        ```markdown
        **Post-Action Verification Summary:**
    - `[x/-] 1. Edit Application Analysis:` [Confirm 3.4.a/b/c. State: 'Applied diff matches final intent.' or 'Discrepancies: [List/Justification]'.]
    - `[x/-] 2. Leftover Code & Dependency Analysis:` [Confirm 3.4.2.a/b performed.]
    - `[x/-] 3. Correction Assessment:` [Confirm 3.4.3 performed. State if corrections made.]
    - `[x/-] 4. Full File Read Verification:` [Brief confirm `Procedure: Ensure Sufficient File Context` Step 3 if full reads in this phase.]
    - `[x] 5. Confirmation:` Summary complete for `[filename(s)/task]`.
    ```

*   **3.6. Mid-Cycle Self-Assessment:** After 3.5 for each file, `Procedure: Perform Self-Assessment` (Scope: "Edit Cycle for [filename]").

### 4. Adherence Checkpoint & Next Step
*   **Trigger:** Before concluding, after all files processed through Step 3.
*   **Action:**
    *   `a.` **Final Self-Assessment:** `Procedure: Perform Self-Assessment` (Scope: "Overall Task Cycle").
    *   `b.` **Determine Next Action:** If main request/plan has pending actions: State next action. **Immediately and autonomously initiate Step 2 (Planning) for it in the same AI turn.** Do not pause unless entire request/plan is complete or user input is explicitly needed.

### 5. Summarize Deferred Observations (Optional)
*   **Trigger:** After Step 4, before concluding.
*   **Action:**
    *   **MAY** generate "Deferred Observations Summary" (minor bugs, smells).
    *   **MANDATORY "Deferred Edit Issues Summary"** if unintended modifications from Step 3 edits were accepted: List affected files/functions, describe unintended changes, analyze behavioral differences, ask user: "Prioritize fixing these now, or defer?"
    *   If no general observations, state: "**Step 5: Deferred Observations Summary:** No specific deferred observations."
    *   Goal: Capture insights and ensure unintended side effects are documented and presented for decision.

---

## Core Reusable Procedures (Concise)

**`Procedure: Ensure Sufficient File Context`**
1.  **Assess Need & Attempt Read:** Determine if full file context is necessary for the current task (e.g., complex file, central role, modifications planned). Use `read_file`, requesting a full read if Step 1 indicates necessity and tool constraints allow.
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
2.  Execute `Procedure: Verify Diff` on `reapply` tool's diff (intent = the final proposal from Step 3.2 that was being attempted by the original `edit_file` call).
3.  Log confirmation and outcome.

**`Procedure: Verify Edit File Diff`** (After `edit_file` tool call)
1.  Execute `Procedure: Verify Diff` on `edit_file` tool's diff (intent = final proposal from Step 3.2).
2.  If not 'Verified', trigger self-correction (Step 3.4.3).

**`Procedure: Verify Tool Output`** (After info-gathering tools)
1.  **Congruence:** Confirm output matches request (e.g., line range, no unexpected truncation).
2.  **Sufficiency:** Evaluate if content is adequate/unambiguous for task.
3.  **Re-evaluate Planning:** If new, more complete info becomes available after initial planning, re-evaluate prior sub-steps impacted by it.
4.  **Handle Discrepancies:** If incongruent/insufficient: State issue. Correct (re-run, alternative tool, clarify with user). If proceeding with incomplete data, state risks.
5.  Report outcome.

**`Procedure: Perform Self-Assessment`** (Scope: "Planning", "Edit Cycle for [filename]", "Overall Task")
1.  State scope.
2.  Verify all mandatory steps/checks/reporting for scope were completed.
2.a. **Context Sufficiency Review (if scope includes Planning):** Re-evaluate context from `Procedure: Ensure Sufficient File Context`. Confirm adequacy or suggest re-entering Step 2 for more context.
3.  **Critical Blocker Adherence:** Verify `BLOCKER:` conditions strictly adhered to. If deviation: Report "CRITICAL DEVIATION: Blocker [X] bypassed." **BLOCKER:** Halt for guidance. Else, report "Critical Blocker Adherence Review: Confirmed."
4.  **Handle Missed Steps:** If non-blocker steps missed: Report, trigger self-correction. STOP until addressed.
5.  State: "**`Procedure: Perform Self-Assessment ([scope])`:** Complete."

---

## Core Exception Handling Procedures (Concise)

**`Procedure: Handle Unclear Root Cause / Missing Info`** (Triggered by Step 2.8)
1.  STOP proposing direct fix. State blocker.
2.  Formulate investigation plan. Seek confirmation if broad/assumption-based (**BLOCKER:** if so).
3.  If investigation blocked/impractical AND workaround is only path: Use `Procedure: Handle Necessary Workaround`.
4.  If missing dependency needs new structure from ambiguous refs: Use `Procedure: Consult on Ambiguous Missing Dependency`.

**`Procedure: Handle Architectural Decisions`** (Triggered by Step 2.9 or if plan involves major architectural change)
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

**`Procedure: Handle Failed Verification for Existing Dependency`** (If Step 2.7.b fails for existing code's dependency)
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
    *   **B: Reject (Default):** If not beneficial, errors, violates standards. State reason. If proposed `code_edit`, revise it. If applied `diff`, trigger self-correction (Step 3.4.3). Report.
5.  Loop for other deviations.

**`Procedure: Request Manual Edit`** (If automated edits repeatedly fail, cause gross errors, or corrective plan fails)
1.  STOP automated attempts for this edit.
2.  State Failure: Explain issue (persistent errors, misapplications, unacceptable side-effects), history of attempts.
3.  Provide Edit Details: Target file, action (Insert/Replace/Delete).
    *   Provide precise code block with 2-3 unchanged context lines before/after. Use `// ... existing code ...`.
    *   For deletion, clearly show lines to delete with context.
    *   Conclude: "Please apply this manual edit to `[target_file]` and let me know once completed for verification."
4.  **BLOCKER:** Await user confirmation of manual edit.
5.  Verify Manual Edit: After user confirms: Re-read file. Compare to intended state. Report verification outcome ("Verification successful." or "Discrepancies found: [details]. Please re-check."). If discrepancies persist after 2nd user attempt, state and ask how user wants to proceed.

---

## Runtime Error Diagnosis & Resolution Protocol
1.  Acknowledge error (cite traceback/logs). Goal is now diagnosis/resolution.
2.  Follow standard workflow (Steps 0-5), adapting Step 2 (Planning) for diagnostics:
    *   **Step 2.2 (Context):** Gather all facts (tracebacks, logs, inputs). `Procedure: Ensure Sufficient File Context` for implicated files.
    *   **Step 2.7.b (Hypothesis):** Core diagnostic loop. Formulate testable hypothesis for root cause. Verify (`Procedure: Verify Hypothesis`). Repeat if refuted.
    *   Plan fix addressing root cause. Analyze fix impact.
3.  Once error fixed & verified, resume original task (re-evaluate plan if needed).

---
