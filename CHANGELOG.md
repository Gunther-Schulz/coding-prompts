AI ASSISTANT - INSTRUCTIONS FOR UPDATING THIS CHANGELOG:
When you (the AI Assistant) are instructed to update this changelog due to changes in CLIPPY.MD:
1. For the date of the new version entry, you SHOULD use the output of the 'date +%Y-%m-%d' command. If you cannot execute this command, use the current date in YYYY-MM-DD format.
2. Place the new version entry directly below the '## [Unreleased]' heading.
3. Ensure the version number in the new entry matches the version you have updated in CLIPPY.MD.


## [Unreleased]

## [0.2.24] - 2025-05-10
**Affected Document(s):**
*   `coding-clippy/CLIPPY.md`

**Summary of Changes:**
Clarified AI's internal state management regarding proposed edits (Step 4.1) and ensured fully autonomous operation from Step 4.1 through 4.3 by removing a previously added user review pause. This addresses AI confusion about proposal vs. actual edit state and restores the intended autonomous edit cycle.

**Detailed Changes to `coding-clippy/CLIPPY.md`:**

1.  **Enhanced Step 4.1 ("Generate Proposed `code_edit` Diff Text"):**
    *   Added a new bullet point: `*   **CRITICAL AI STATE MANAGEMENT:** The generation and presentation of the \`code_edit\` text in this step is a *proposal* only. The AI **MUST NOT** internally register this action as an actual modification to the file state. The AI\'s internal understanding of the file\'s content **MUST** remain based on the last actual read or confirmed edit application. This is crucial to prevent misinterpreting subsequent tool outputs (e.g., "no changes made" from an \`edit_file\` call in Step 4.3) if the proposal happens to match the file's true current state.`

2.  **Revised Step 4.2 ("Pre-Apply Verification (Mandatory Before 4.3)"):**
    *   Removed the previously added first bullet point: `*   **Initiation:** This step is initiated by the AI in a new conversational turn, after the user has had the opportunity to review the textual proposal from Step 4.1.`
    *   The first bullet point of Step 4.2 is now: `*   The following verification steps **MUST** be successfully completed and reported *before* proceeding to Step 4.3 (Apply Edit).` (This restores it to its original state before the temporary change).

3.  **Toolkit Component Version Update:**
    *   Incremented version in `CLIPPY.MD` from `v0.2.23` to `v0.2.24` (already completed in a prior step).

## [0.2.23] - 2025-05-10

### Changed
- Enhanced Step 3.0.1 ('Verify Tool Output Congruence and Sufficiency') in `CLIPPY.MD` to mandate re-evaluation of prior planning sub-steps if new, more complete/sufficient information becomes available that could materially impact earlier conclusions.

## [0.2.22] - 2025-05-10
**Affected Document(s):**
*   `coding-clippy/CLIPPY.md`

**Summary of Changes:**
Updated `Procedure: Ensure Sufficient File Context` in `CLIPPY.md` to strengthen the bias towards attempting full file reads when partial views might be insufficient for the task, aiming to improve contextual understanding before planning or editing.

**Detailed Changes to `coding-clippy/CLIPPY.md`:**

1.  **Modified `Procedure: Ensure Sufficient File Context` (Section 4):**
    *   **Step 1 (Prioritize Complete View):** Added a sentence to explicitly state that when in doubt about the sufficiency of a partial view, the AI should default to attempting a full file read as per Step 2.
    *   **Step 2 (Attempt Full File Read):** Rephrased to reinforce that this step is the default action when Step 1 indicates a need for more comprehensive context, especially when doubt exists regarding the adequacy of partial views.

2.  **Toolkit Component Version Update:**
    *   Incremented version in `CLIPPY.MD` from `v0.2.21` to `v0.2.22`.

## [0.2.21] - 2025-05-10
**Affected Document(s):**
*   `coding-clippy/CLIPPY.md`

**Summary of Changes:**
Added a "Sustained Adherence Refocus" mandatory action at the beginning of the Planning Phase (Step 3) in `CLIPPY.md`. This requires the AI to explicitly state its commitment to re-evaluating and adhering to the full set of principles in `CLIPPY.md` before starting detailed planning for a new task or a new phase of an ongoing task. This aims to counteract focus drift during extended interactions.

**Detailed Changes to `coding-clippy/CLIPPY.md`:**

1.  **Added "Mandatory Refocus Before Planning" to Section 3 ("Pre-computation Standards Check (Planning Phase)")**:
    *   Inserted a new instructional block before Step 3.0.
    *   **Action:** Requires the AI to explicitly state: "**Sustained Adherence Refocus:** Actively re-evaluating and committing to the full set of principles and procedures within `AI_CODING_PROCESS.md` (this document) to ensure continued meticulous adherence for the upcoming planning and implementation."
    *   **Rationale:** Serves as a deliberate internal prompt for the AI to refresh its attention to the comprehensive guidelines, counteracting potential focus drift and reinforcing rigorous process execution.

2.  **Toolkit Component Version Update:**
    *   Incremented version in `CLIPPY.MD` from `v0.2.20` to `v0.2.21`.

## [0.2.20] - YYYY-MM-DD
**Affected Document(s):**
*   `coding-clippy/CLIPPY.md`

**Summary of Changes:**
Clarified AI turn management for Steps 4.1-4.3 (Propose, Pre-Verify, Apply) and reinforced that Step 4.4 (Post-Apply Verification) must verify the actual tool output diff, not the original proposal. This aims to improve AI adherence to the sequential verification process.

**Detailed Changes to `coding-clippy/CLIPPY.md`:**

1.  **Enhanced Step 4 ("Edit Generation & Verification Cycle") - "Sequential Execution and Autonomous Operation" subsection:**
    *   Restructured to define an explicit two-turn process:
        *   **Turn 1: Propose Edit (Step 4.1):** AI solely generates and presents the `code_edit` diff text.
        *   **Turn 2: Pre-Verify Proposal & Initiate Apply (Steps 4.2 & 4.3):** AI performs pre-verification on the proposal from Turn 1, then issues the tool call.

2.  **Enhanced Step 4.4 ("Post-Apply Verification (Mandatory After 4.3 Tool Call Result)")**:
    *   Added a "CRITICAL INSTRUCTION FOR AI" at the beginning to conceptually discard memory of the proposed edit and focus exclusively on the tool's actual output diff.
    *   Updated the "Purpose" and "Action" descriptions to emphasize that verification uses "ONLY the diff provided in the tool's output from Step 4.3".
    *   Updated sub-steps `4.4.1.a` (Post-Reapply Verification) and `4.4.1.b` (Post-edit_file Verification) to explicitly state that the diff being verified is from the respective tool's output.

3.  **Toolkit Component Version Update:**
    *   Incremented version in `CLIPPY.MD` from `v0.2.19` to `v0.2.20`.

## [0.2.19] - YYYY-MM-DD
### Added
- **MONOLITH_CLIPPY.md:** Added Step 3.1.1 ("Investigate Specified Refactoring Sources") to explicitly handle investigation of external source code mentioned in refactoring plans.
- **MONOLITH_CLIPPY.md:** Added rule under "Work with Facts" Core Principle: "Plan Directives Override Ambiguous Tool Output" to prioritize plan instructions over potentially incomplete tool results.

### Changed
- **MONOLITH_CLIPPY.md:** Modified Step 3.1 ("Search for Existing Logic") to reference the new Step 3.1.1 for external source investigation.

## [0.2.18] - 2024-07-17

**Affected Document(s):**
*   `coding-clippy/experimental/CLIPPY.MD` (Refactored & Moved)
*   `coding-clippy/experimental/clippy_modules/01_GENERAL_WORKFLOW.md` (New & Moved)
*   `coding-clippy/experimental/clippy_modules/02_REUSABLE_PROCEDURES.md` (New & Moved)
*   `coding-clippy/experimental/clippy_modules/03_EXCEPTION_HANDLING.md` (New & Moved)
*   `coding-clippy/experimental/clippy_modules/04_RUNTIME_ERROR_PROTOCOL.md` (New & Moved)
*   `coding-clippy/experimental/clippy_modules/05_GLOSSARY.md` (New & Moved)
*   `coding-clippy/experimental/clippy_modules/06_REFERENCES.md` (New & Moved)

**Summary of Changes:**
Major structural refactoring of the AI Coding Process documentation (`CLIPPY.MD`). The previously monolithic document has been modularized to improve AI manageability, maintainability, and clarity as the process grows in complexity. **These files were also moved to the `coding-clippy/experimental/` directory as the modularization is considered experimental and not yet fully functional.**

**Detailed Changes:**

1.  **`coding-clippy/experimental/CLIPPY.MD` Refactored & Moved:**
    *   Now serves as a shorter "umbrella" document.
    *   Retains the "Introduction & Goal" and "Core Principles & Critical Checks Summary".
    *   Adds a new "Overall Process Structure" section explaining the modular design.
    *   Updates the "Table of Contents" to point to new module files using relative links.
    *   Removes the detailed content for sections moved to modules.
    *   Toolkit Component Version updated to `v0.3.0`.

2.  **New Module Files Created in `coding-clippy/experimental/clippy_modules/` (and Moved):**
    *   `01_GENERAL_WORKFLOW.md`: Contains the detailed Steps 0-6 of the main coding workflow (previously Section 3).
    *   `02_REUSABLE_PROCEDURES.md`: Contains the detailed Reusable Verification Procedures (previously Section 4).
    *   `03_EXCEPTION_HANDLING.md`: Contains the detailed Exception Handling Procedures (previously Section 5).
    *   `04_RUNTIME_ERROR_PROTOCOL.md`: Contains the detailed Protocol for Runtime Error Diagnosis and Resolution (previously Section 7).
    *   `05_GLOSSARY.md`: Contains the Glossary of Key Terms (previously Section 6).
    *   `06_REFERENCES.md`: Contains the References section (previously Section 8).

**Reason for Changes:**
To address concerns about the increasing size and complexity of the monolithic `CLIPPY.MD` potentially hindering AI processing and adherence. Modularization aims to improve context management, attention focus, maintainability, and scalability of the AI coding process documentation.

---

## Framework v0.2.18 - 2025-05-08

**Affected Document(s):**
*   `coding-prompts/CLIPPY.md`

**Summary of Changes:**
Added new sub-step `3.0.1` ("Verify Tool Output Congruence and Sufficiency") to `CLIPPY.md`'s planning phase to ensure AI explicitly verifies information-gathering tool outputs before use. Updated Table of Contents and Pre-computation Verification Summary accordingly.

**Detailed Changes to `coding-prompts/CLIPPY.md`:**

1.  **Added Step `3.0.1. Verify Tool Output Congruence and Sufficiency (After Information Gathering)`:**
    *   Inserted after Step `3.0` in the "General Coding Workflow" (Section 3).
    *   **Trigger:** Immediately after any information-gathering tool call (`read_file`, `grep_search`, etc.) within Step 3.
    *   **Action:** Requires AI to:
        *   Verify tool output aligns with request parameters (e.g., correct lines, no unexpected truncation).
        *   Assess if output content is sufficient and clear for the immediate planning task.
        *   Take corrective action (re-run tool, adjust parameters, clarify with user) if output is incongruent or insufficient.
        *   Explicitly report the outcome of this verification.
    *   Ensures data is validated before being used in subsequent planning steps.

2.  **Updated `Pre-computation Verification Summary` (Step `3.10`):**
    *   Added a new checklist item: `- [x/-] 7. Tool Output Verification:` to confirm Step `3.0.1` was performed for all tool outputs used in planning.

3.  **Updated Table of Contents (Section 3):**
    *   Added entries for `3.0. Assess Target File Complexity & Ensure Sufficient Context (Initial Check)`.
    *   Added entries for the new `3.0.1. Verify Tool Output Congruence and Sufficiency (After Information Gathering)`.

4.  **Version Bump:** Toolkit Component Version in `CLIPPY.md` updated to `v0.2.18`.

**Reason for Changes:**
To enhance the robustness of the AI's planning phase by ensuring that all information gathered from tools is explicitly checked for correctness, completeness (congruence with the request), and sufficiency for the task at hand *before* that information is used to make decisions or formulate further plans. This aims to prevent errors arising from acting on misunderstood, incomplete, or incongruent tool outputs.

---

## Framework v0.2.17 - 2025-05-08

**Affected Document(s):**
*   `coding-prompts/CLIPPY.md`

**Summary of Changes:**
Added "Protocol for Runtime Error Diagnosis and Resolution" (Section 7) to guide methodical debugging. Added "Handling Information Discrepancies" guidance to "Work with Facts" principle (Section 2). Added "Meta-Instruction on Reporting Detail" to ensure consistent reporting verbosity (Section 3). Updated Table of Contents and section numbering. (Note: Also includes Glossary section inadvertently added during prior edit).

**Detailed Changes to `coding-prompts/CLIPPY.md`:**

1.  **Added Section 7: "Protocol for Runtime Error Diagnosis and Resolution":**
    *   Provides a structured protocol for diagnosing and fixing runtime errors, adapting the standard workflow (Steps 1-6).
    *   Emphasizes methodical investigation, hypothesis testing, and robust fixes.
    *   Requires explicit confirmation of resuming the original task post-fix.
2.  **Enhanced "Work with Facts" Core Principle (Section 2):**
    *   Added "Handling Information Discrepancies" subsection detailing how to manage conflicting data from tracebacks, logs, and tool outputs (e.g., `read_file`).
3.  **Added "Meta-Instruction on Reporting Detail" (Section 3):**
    *   Reinforces the requirement for consistent, explicit, and thorough reporting for all steps (1-6) across all task types (development, refactoring, debugging).
4.  **Updated Table of Contents & Section Numbering:** Reflected addition of Section 7 and renumbering of References to Section 8.
5.  **Retained Glossary (Section 6):** Includes Glossary section added as a side-effect of a previous edit attempt.
6.  **Version Bump:** Toolkit Component Version updated to v0.2.17.

**Reason for Changes:**
Based on collaborative review and debugging experiences, these changes enhance `CLIPPY.MD`'s robustness by: providing a formal, rigorous protocol for handling runtime errors; improving guidance on managing conflicting information sources; and ensuring consistent reporting detail regardless of task type.

---

## Framework v0.2.16 - 2025-05-07

**Affected Document(s):**
*   `coding-prompts/CLIPPY.md`
*   `coding-prompts/KNOWN_ISSUES.md`

**Summary of Changes:**
Enhanced `CLIPPY.md` to improve robustness against AI internal state inconsistencies after file reads. Added a pre-edit precondition check (Step 3.11), improved self-correction triggers (Step 4.4.3), and corrected procedure call site references. Documented related failure modes and experiences in `KNOWN_ISSUES.md`.

**Detailed Changes to `coding-prompts/CLIPPY.md`:**

1.  **Added Step `3.11 Verify Action Preconditions`:**
    *   Introduced a new mandatory step before generating an edit (Step 4.1).
    *   Requires the AI to explicitly re-verify the immediate preconditions for the planned action (e.g., existence of code to be deleted, non-existence of code to be added) against the latest file content.
    *   If preconditions fail, the AI must STOP, report the discrepancy, and revise the plan.
    *   The "IMMEDIATE NEXT ACTION" instruction to proceed to Step 4.1 was moved to after this step.

2.  **Enhanced Step `4.4.3.b.c` (Triggers for Self-Correction):**
    *   Added a new trigger (item `g.`): Self-correction is now also required if the `edit_file` or `reapply` tool reports "no changes were made" when the AI's plan expected modifications. This forces investigation into why the AI's understanding of the pre-edit state was incorrect.

3.  **Updated Procedure Call Site References:**
    *   Corrected section number references in Steps `4.4.1.a`, `4.4.1.b`, and `4.5` (item 1) to accurately point to the final de-duplicated locations of `Procedure: Verify Reapply Diff` (Section 4) and `Procedure: Verify Edit File Diff` (Section 4).

**Detailed Changes to `coding-prompts/KNOWN_ISSUES.md`:**

1.  **Revised Issue Entry ("AI's Internal File State Inconsistency..."):**
    *   Updated the existing issue regarding `read_file` to more accurately reflect the core problem observed: the AI's internal model of a file can be inconsistent or outdated even after a purported full read, leading to erroneous conclusions.
    *   Clarified the distinction between the tool's potentially misleading output display and the AI's internal state management failure.
    *   Rewritten the "Recount of `CLIPPY.md` Experience" to highlight the AI's incorrect assertions about duplicate procedures persisting after full reads, and how targeted "no change" edits were needed to force correction.
    *   Updated "Status/Mitigation" to include the need for better AI self-correction and state integrity checks.

**Reason for Changes:**
Stemming directly from a collaborative debugging and refactoring session of `CLIPPY.md`, these changes address several related issues:
*   A subtle but critical AI failure mode where the AI claimed to have read the full file but repeatedly made incorrect conclusions about its content (specifically, the presence of already-deleted duplicates). This led to inefficient loops and highlighted the need for:
    *   More rigorous pre-edit checks by the AI against the actual file state (new Step 3.11).
    *   Explicit handling of "no changes made" edits as a signal of the AI's incorrect state understanding (new trigger in 4.4.3).
    *   Clearer documentation of this failure mode and its nuances in `KNOWN_ISSUES.md`.
*   Correction of internal cross-references within `CLIPPY.md` after confirming the correct de-duplicated locations of several procedures.

---

## Framework v0.2.15 - 2025-05-07

**Affected Document(s):**
*   `coding-prompts/CLIPPY.md`

**Summary of Changes:**
Further enhanced `CLIPPY.md` planning phase (Step 3) to include proactive consideration for the complexity of newly generated code, configuration needs, and resource management.

**Detailed Changes to `coding-prompts/CLIPPY.md`:**

1.  **Enhanced Step 3.2 (Identify Standards & Verify Alignment):
    *   Added a directive for the AI to ensure that its plan for *newly generated code* (functions, methods, logic blocks) prioritizes simplicity (e.g., appropriate length, limited nesting, SRP) and includes proactive decomposition of inherently complex new units.

2.  **Added Configuration Needs Check (Step 3.4.1.i):
    *   Introduced a new sub-point requiring assessment of whether a change necessitates new configurable parameters, promoting their integration with existing config mechanisms and avoiding hardcoding.

3.  **Added Resource Management Check (Step 3.4.1.j):
    *   Introduced a new sub-point requiring the plan to detail how system resources (files, connections, etc.) will be properly acquired and consistently released, especially in error scenarios.

4.  **Version Bump:** Toolkit Component Version updated to v0.2.15.

---

## Framework v0.2.14 - 2025-05-07

**Affected Document(s):**
*   `coding-prompts/CLIPPY.md`

**Summary of Changes:**
Added light-touch considerations for Testability and Observability to the planning phase (Step 3.4.1) of `CLIPPY.md`.

**Detailed Changes to `coding-prompts/CLIPPY.md`:**

1.  **Added Testability Check (Step 3.4.1.g):
    *   Introduced a new sub-point requiring a brief assessment of whether the proposed code structure facilitates testing (e.g., dependency injection, structural clarity).

2.  **Added Observability Check (Step 3.4.1.h):
    *   Introduced a new sub-point requiring a brief assessment of whether the proposed change warrants new or updated logging or metrics for monitoring/diagnostics.

3.  **Version Bump:** Toolkit Component Version in `CLIPPY.md` updated to `v0.2.14`.

**Reason for Changes:**
To encourage proactive consideration of non-functional aspects like testability and observability during the planning phase, promoting more robust and maintainable code design without adding significant cognitive overhead to the process.

---

## Framework v0.2.13 - 2025-05-07

**Affected Document(s):**
*   `coding-prompts/CLIPPY.md`

**Summary of Changes:**
Refined recent additions to `CLIPPY.md` to enhance language/framework agnosticism, particularly within `Procedure: Verify Framework Compatibility`.

**Detailed Changes to `coding-prompts/CLIPPY.md`:**

1.  **Made `Procedure: Verify Framework Compatibility` More Agnostic (Section 4):**
    *   Replaced specific examples like "Typer/Click app callbacks, middleware" and "`ctx.obj`" with more general terms like "registered callback functions or handlers" and "execution context or shared state".
    *   Generalized the example RuntimeWarning.
    *   Removed specific library examples from Step 2 (Interaction Pattern Check) to keep it focused on the pattern itself.

2.  **Version Bump:** Toolkit Component Version in `CLIPPY.md` updated to `v0.2.13`.

**Reason for Changes:**
To ensure the coding process remains broadly applicable across different technology stacks by removing potentially overly specific examples introduced in the previous version, while retaining the core principles of the checks.

---

## Framework v0.2.12 - 2025-05-07

**Affected Document(s):**
*   `coding-prompts/CLIPPY.md`

**Summary of Changes:**
Enhanced `CLIPPY.md` to improve verification of framework interactions. This includes: emphasizing root cause analysis for framework warnings, requiring explicit verification of implicit framework behavior assumptions, and strengthening checks for framework callback compatibility (especially concerning async/sync changes and context propagation).

**Detailed Changes to `coding-prompts/CLIPPY.md`:**

1.  **Strengthened `Procedure: Verify Framework Compatibility` (Section 4):**
    *   Appended detailed clarification for verifying framework invocation of *callbacks*. This includes focusing on synchronicity changes (`sync`/`async`), context propagation (like `ctx.obj`), how `async` callbacks are awaited, and treating framework `RuntimeWarning`s (e.g., 'coroutine ... was never awaited') as critical indicators requiring immediate investigation.

2.  **Added Emphasis to "Root Cause Analysis" for Framework Warnings (Step 3.4):**
    *   Inserted a new note under "Step 3.4: Check 'Robust Solutions'" to prioritize understanding and resolving the *source of framework `RuntimeWarning`s* themselves, as they often point to the true root cause of subsequent errors.

3.  **Refined `Procedure: Verify Hypothesis` for Implicit Framework Behaviors (Section 4):**
    *   Added a note emphasizing that assumptions about implicit framework behaviors (e.g., automatic awaiting of async functions, context availability in subcommands) **MUST** be explicitly stated and verified (ideally against documentation or minimal examples).

4.  **Version Bump:** Toolkit Component Version in `CLIPPY.md` updated to `v0.2.12`.

**Reason for Changes:**
To make the AI coding process more robust when dealing with framework-specific behaviors, particularly concerning the integration of asynchronous code (like callbacks) and ensuring correct context propagation. These changes are based on lessons learned from a CLI refactoring scenario where subtle framework interactions led to debugging challenges.

---

## Framework v0.2.11 - 2025-05-07

**Affected Document(s):**
*   `coding-prompts/CLIPPY.md`

**Summary of Changes:**
Added a new "Absolute Critical Check" to `CLIPPY.md` mandating that the AI verify tool output congruence. This means the AI must actively check if a tool's actual output (e.g., from `read_file`, `grep_search`) aligns with the explicit request parameters and expected outcome, and handle discrepancies before proceeding.

**Detailed Changes to `coding-prompts/CLIPPY.md`:**

1.  **Added New "Absolute Critical Check" (Core Principles & Critical Checks Summary):**
    *   Introduced check number 6: "**Verify Tool Output Congruence:** For tools where specific outputs are expected based on inputs (e.g., `read_file` returning a specific number of lines or full content, `grep_search` finding all instances), the AI **MUST** actively verify that the tool's actual output aligns with the explicit request parameters and expected outcome. Discrepancies **MUST** be acknowledged and handled before proceeding as if the request was fully met."

2.  **Version Bump:** Document version (within `CLIPPY.md` itself, implied) updated to reflect framework alignment. (Toolkit Component Version updated to v0.2.11 in `CLIPPY.md`)

**Reason for Changes:**
To ensure the AI proactively verifies that tool outputs match specific expectations based on the inputs provided to the tool, preventing errors that arise from assuming a tool call perfectly fulfilled its intended scope without explicit output validation. This addresses potential misinterpretations of tool results.

---

## Framework v0.2.10 - 2025-05-07

**Affected Document(s):**
*   `coding-prompts/CLIPPY.md`

**Summary of Changes:**
Strengthened AI self-verification requirements within `CLIPPY.MD`'s post-edit procedures. Mandated autonomous file state verification using `read_file` when edit tool diffs are ambiguous or incomplete, particularly regarding intended deletions, before verification can proceed. Relies on the strength of the mandatory procedure (`Procedure: Verify Diff`, Step 7) rather than explicit negative constraints.

**Detailed Changes to `coding-prompts/CLIPPY.md`:**

1.  **Enhanced `Procedure: Verify Diff` (Section 4):**
    *   Modified Step 1.c (Deletions): Explicitly requires treating unconfirmed *intended* deletions as critical discrepancies, mandating investigation via Step 7.
    *   Modified Step 3 (Identify Deviations): Broadened definition of deviations to include any intended change not confirmed by the applied diff.
    *   Overhauled Step 7 ("Context Line Check") to "File State Verification and Diff Ambiguity Resolution":
        *   Added mandatory check for diff completeness regarding the *full* intent (adds, modifies, deletes).
        *   Mandated **immediate and autonomous use of `read_file`** if the diff is insufficient or suspicious.
        *   Established the content retrieved via `read_file` as the authoritative basis for completing the verification if the tool's diff was insufficient.

2.  **Reinforced "CRITICAL WARNING" (Step 4):**
    *   Added sentence emphasizing skepticism towards what a diff *doesn't* show (e.g., unconfirmed deletions) and directing AI to verify directly via the updated Step 7 of `Procedure: Verify Diff`.

3.  **Removed Explicit Guiding Principle from Step 4.4:**
    *   The "Guiding Principle: Autonomous Verification of Edit Outcomes" paragraph (previously added and then discussed) was removed from the start of Step 4.4. The process now relies on the mandatory actions defined within the updated `Procedure: Verify Diff` (Step 7) to enforce self-verification. (The removed text was archived in `CLIPPY_Optional_Additions.md`).

4.  **Updated Table of Contents:** Reflects renaming of `Procedure: Verify Diff` Step 7. (Implicit change due to Step 1c above).

5.  **Version Bump:** Document version updated to `v0.2.10`.

**Reason for Changes:**
To address a specific AI failure mode where uncertainty about an edit's outcome (due to an incomplete diff from the `edit_file` tool, particularly concerning deletions) led to premature user escalation instead of autonomous verification using available tools (`read_file`). The changes mandate self-reliance in verifying the actual file state when tool output is ambiguous, strengthening the core verification procedure itself.

---

## Framework v0.2.9 - 2025-05-07

**Affected Document(s):**
*   `coding-prompts/AI_CODING_PROCESS.md`

**Summary of Changes:**
Refactored `AI_CODING_PROCESS.md` by consolidating detailed instructions for ensuring sufficient file context and preparing robust edit tool inputs into new reusable procedures. This enhances modularity and clarity.

**Detailed Changes to `coding-prompts/AI_CODING_PROCESS.md`:**

1.  **Added `Procedure: Ensure Sufficient File Context` (Section 4):**
    *   Extracted from the detailed instructions previously in Step 3.0.
    *   This procedure now centralizes the logic for assessing context needs, attempting full file reads, handling incomplete reads (including the **BLOCKER** for user input), and the cautious use of alternatives like `grep_search`.

2.  **Added `Procedure: Prepare Robust Edit Tool Input` (Section 4):**
    *   Consolidated and extracted from detailed guidance previously in Step 3.8.b and Step 4.1.
    *   This procedure centralizes best practices for constructing the `code_edit` string and `instructions` field for the `edit_file` tool, covering context anchoring, handling complex/sensitive files, managing large block movements, and specifying sub-part changes.

3.  **Updated Step 3.0:** Now directly calls `Procedure: Ensure Sufficient File Context`.

4.  **Updated Step 3.8.b:** Now directly calls `Procedure: Prepare Robust Edit Tool Input`.

5.  **Updated Step 4.1:** Simplified to focus on formulating the `code_edit` text, referencing `Procedure: Prepare Robust Edit Tool Input` for detailed construction guidelines.

6.  **Updated Table of Contents:** Added entries for the new procedures in Section 4.

7.  **Version Bump:** Document version updated to `v0.2.9`.

**Reason for Changes:**
To improve the modularity, readability, and maintainability of `AI_CODING_PROCESS.md` by centralizing recurring complex instruction sets into named, reusable procedures. This reduces redundancy and makes the main workflow steps (3.0, 3.8.b, 4.1) more concise and focused on their primary intent, while ensuring that detailed best practices for critical operations (context gathering, edit tool preparation) are consistently applied.

---

## Framework v0.2.8 - 2025-05-07

**Affected Document(s):**
*   `coding-prompts/AI_CODING_PROCESS.md`

**Summary of Changes:**
Strengthened assumption verification procedures, introduced a mechanism for proposing preliminary refactoring of complex code sections, and enhanced the reporting and handling of unresolved edit deviations in `AI_CODING_PROCESS.md`.

**Detailed Changes to `coding-prompts/AI_CODING_PROCESS.md`:**

1.  **Enhanced Step `3.4.1.b` (Perform `Procedure: Verify Hypothesis` for ALL Assumptions):**
    *   Mandated that the structured report generated by `Procedure: Verify Hypothesis` **MUST immediately follow the statement of the hypothesis** in the AI's response text before proceeding to subsequent hypotheses or planning steps. This ensures immediate, inline reporting of verification.

2.  **Enhanced Step `4.4.3.c` (Triggers for Self-Correction):**
    *   Added a new trigger: "Missed mandatory immediate verification for a stated hypothesis (Step 3.4.1.b / `Procedure: Verify Hypothesis`) during the preceding planning phase." This reinforces the requirement for timely verification.

3.  **Added New Step `3.4.0.b` (Assess Target Code Complexity & Propose Refactoring (If Needed)):**
    *   Requires the AI to assess the complexity of a target code block before generating an edit.
    *   If the code is deemed excessively complex, the AI **MUST STOP**, propose a preliminary refactoring plan, explain the rationale (improved clarity, testability, reduced edit tool error risk), and treat this as a **BLOCKER** requiring user confirmation before proceeding with either the refactoring or the original edit.

4.  **Enhanced Step `6` (Summarize Deferred Observations):**
    *   Mandated a "Deferred Edit Issues Summary" if unintended modifications introduced during Step 4 edits could not be fully resolved.
    *   This summary **MUST** list affected files/functions, describe the unintended changes, provide a detailed analysis of likely behavioral differences, and **explicitly ask the user if they want to prioritize fixing these specific issues immediately.**
    *   The goal is to ensure unintended side effects are clearly documented, their impact analyzed, and explicitly presented to the user for a decision on immediate or deferred action.

**Reason for Changes:**
These updates aim to further improve the robustness of the AI coding process by:
*   Ensuring that all stated assumptions are immediately and explicitly verified with clear, inline reporting, reducing the chance of proceeding with unconfirmed hypotheses.
*   Providing a formal mechanism for the AI to proactively identify and propose mitigations (via refactoring) for risks associated with editing overly complex code, thereby improving the reliability of automated edits and enhancing long-term code maintainability.
*   Improving transparency and user control when automated edits result in unintended side effects that cannot be immediately corrected, by mandating detailed reporting on these deviations and offering the user an explicit choice to address them.
These changes were prompted by discussions highlighting the importance of verifying all assumptions in a timely manner, strategies to handle complex code blocks prone to edit tool errors, and the need for clear reporting and user agency when edit deviations persist.

---

## Framework v0.2.7 - 2025-05-07

**Affected Document(s):**
*   `coding-prompts/AI_CODING_PROCESS.md`

**Summary of Changes:**
Enhanced `AI_CODING_PROCESS.md` to improve the handling of provided implementation plans, reinforce adherence to framework/library best practices, and refine guidance on providing sufficient context to code editing tools for complex changes.

**Detailed Changes to `coding-prompts/AI_CODING_PROCESS.md`:**

1.  **Enhanced Step `3.4.1.b` (Perform `Procedure: Verify Hypothesis` for ALL Assumptions):**
    *   Strengthened the existing universal requirement to verify all assumptions by adding a specific directive: when a *formal implementation plan document is provided*, ALL its explicit assumptions, prescribed actions involving specific code interactions, and referenced existing code structures MUST ALSO be explicitly treated as hypotheses by the AI. These hypotheses derived directly from the formal plan MUST be immediately verified against the current codebase before code generation based on those specific parts of the plan proceeds. The AI MUST NOT assume such a provided plan's representation of existing code is perfectly current or correct and MUST report these verification outcomes. This complements the AI's ongoing duty to identify and verify its own internally formulated assumptions.

2.  **Enhanced Step `3.2` (Identify Standards & Verify Alignment - Note for AI):**
    *   Updated the "Note for AI" to mandate that if `PROJECT_STANDARDS.md` or `PROJECT_ARCHITECTURE.md` are missing or insufficient for a specific framework/library detail, the AI MUST state this. Alignment must then critically include consulting canonical documentation or widely accepted best practices for the specific framework/library. Hypotheses derived from these best practices MUST be explicitly stated and verified as per Step `3.4.1.b`.

3.  **Enhanced Step `3.8.b` (Sufficient Context - for `edit_file` reliability) and Step `4.1` (Generate Proposed `code_edit` Diff Text - context guidance):**
    *   Added guidance that when an edit involves moving substantial blocks of code, or when prior edits by the tool for similar structural changes have shown inaccuracies, the AI should prioritize providing the edit tool with the entire surrounding logical block as context (e.g., full function body, full class definition). The `instructions` field should then clearly state which sub-parts are being modified, added, deleted, or reordered.

**Reason for Changes:**
These updates stem from a CLI refactoring session where issues arose from: a plan containing incorrect assumptions about existing code interfaces, initial misplacement of Typer registrations due to insufficient emphasis on framework-specific best practices when project docs were silent, and edit tool challenges with block movements. The changes aim to make the AI more robust in verifying external plans, adhering to fundamental framework guidelines, and using edit tools effectively for complex structural changes.

---

## Framework v0.2.6 - 2025-05-07

**Affected Document(s):**
*   `coding-prompts/AI_CODING_PROCESS.md`

**Summary of Changes:**
Enhanced AI coding process to improve proactive assessment of existing code patterns, reconciliation of plans with partially modified codebases, handling of plan assumptions, and clarification of "plan" references.

**Detailed Changes to `coding-prompts/AI_CODING_PROCESS.md`:**

1.  **Added Step `3.3.1. Reconcile Plan with Current Code State`:**
    *   Requires the AI to perform a preliminary comparison between a provided implementation plan and the current file content.
    *   If parts of the plan appear already implemented, the AI must state this, confirm its understanding of the remaining scope, and may ask for user clarification if ambiguity arises.

2.  **Added Step `3.4.0.a Assess Interacting Existing Patterns`:**
    *   Mandates that when planned changes significantly interact with existing architectural patterns or critical execution paths, the AI must briefly assess these existing patterns for obvious robustness concerns that could be exacerbated by the new changes (e.g., late initializations, risky existing logic).
    *   Significant concerns directly relevant to the planned integration should be highlighted.

3.  **Enhanced Step `3.4.1.b Perform Procedure: Verify Hypothesis`:**
    *   Added a provision that if a provided implementation plan's 'Key Assumptions' section seems sparse, the AI should proactively identify, state, and verify any additional critical assumptions it's making about the existing codebase's state or structure.

4.  **Refined Trigger for `Procedure: Request Manual Edit` (Step `4.4.3.b.e.i`):**
    *   Added a qualitative consideration to the "Persistent Correction Failure" trigger: to especially consider escalating to a manual edit request if failed edits involve large structural block movements or changes in complex/sensitive files where `edit_file` consistently fails.

5.  **Added Clarifying Note in "Core Principles & Critical Checks Summary":**
    *   Inserted a "Note on 'Implementation Plan' and 'The Plan' References" to explain that "the plan" can refer to a formal provided document or the AI's own internally formulated plan (from Step 3).
    *   Clarifies that process steps explicitly conditioned on a formal plan document may be N/A otherwise.

**Reason for Changes:**
These changes were driven by a detailed refactoring session where issues arose from:
*   A plan not explicitly addressing a robustness weakness in an existing code pattern that new code interacted with.
*   Difficulties in applying a plan to a codebase that was already partially modified in line with the plan.
*   A desire to ensure the AI more proactively identifies and verifies its own implicit assumptions when a plan might be incomplete in this regard.
*   To clarify the AI_CODING_PROCESS.md's applicability whether or not a formal plan document is provided, ensuring the process remains robust and universally interpretable by the AI.

---

## Framework v0.2.5 - 2025-05-06

**Affected Document(s):**
*   `coding-prompts/AI_CODING_PROCESS.md`

**Summary of Changes:**
Strengthened the procedure for obtaining sufficient file context (Step 3.0). Mandated that if a full file read attempt is unsuccessful or provides insufficient context for a critical task, the AI **MUST** request the full file content from the user and treat this as a **BLOCKER** before proceeding with edits on that file.

**Detailed Changes to `coding-prompts/AI_CODING_PROCESS.md`:**

1.  **Revised Step 3.0 ("Assess Target File Complexity & Ensure Sufficient Context"):**
    *   Mandated prioritizing a complete file view for critical/complex files.
    *   Specified attempting `read_file` with `should_read_entire_file=True` as the first step.
    *   If the full read attempt is unsuccessful (e.g., tool indicates only partial content is returned because the file is not on an allowlist, or the returned content is still insufficient), the AI **MUST**:
        *   Clearly state the situation.
        *   Request the user to provide the full file content or critical missing sections.
        *   Treat this as a **BLOCKER**, not proceeding with planning/edits for that file based on incomplete information, until the user provides the content or gives explicit instruction to proceed with caution (with risks stated).
    *   Updated the example to reflect this new blocking scenario.
    *   Clarified that the alternative of using `grep_search` for context should be extremely rare and requires strong justification.

**Reason for Changes:**
To prevent the AI from proceeding with potentially risky edits on critical files when it has insufficient context, especially if an attempt to read the full file using `read_file` did not yield the complete content. This ensures the AI adheres to the principle of working with complete facts and prioritizes safety and accuracy by explicitly involving the user when essential information is missing. This change was prompted by an incident where the AI proceeded with an edit based on a partial file view after a full read attempt was only partially successful.

---

## Framework v0.2.4 - 2025-05-07

### PLAN_WRITING_PROCESS.md
    *   Updated `Framework Component Version` to `v0.2.4`.
    *   Added new **Step 0.1: Document Maturity & Alpha Status Acknowledgment**. This step requires explicit user confirmation to proceed if the `PLAN_WRITING_PROCESS.md` is marked with `Component Status: Alpha`.

**Overall Goal/Theme of this Framework Update:** Improve user awareness and consent when utilizing process documents marked with a non-stable (e.g., Alpha) status.

---

## Framework v0.2.3 - 2025-05-07

### PLAN_WRITING_PROCESS.md
    *   Updated `Framework Component Version` to `v0.2.3`.
    *   Added `Component Status: Alpha` to clearly indicate its current maturity level.

**Overall Goal/Theme of this Framework Update:** Enhance clarity of individual process document maturity within the unified framework versioning system.

---

## Framework v0.2.2 - 2025-05-07

**Affected Document(s):**
*   `coding-prompts/AI_CODING_PROCESS.md`

**Summary of Changes:**
Enhanced AI's self-verification procedures for code edits, particularly concerning unintended modifications by editing tools. Improved guidelines for diff scrutiny, escalation for severe tool errors, and clarity of critical warnings.

**Detailed Changes to `coding-prompts/AI_CODING_PROCESS.md`:**

1.  **Enhanced `Procedure: Verify Diff`:** Clarified 'intent' scope, mandated granular analysis of additions/modifications/deletions against specific intent, and emphasized treating unplanned deletions as major deviations. Restored full 11-point checklist.
2.  **Refined Trigger for `Procedure: Request Manual Edit`:** Broadened the "Grossly Disproportionate Modifications" condition to cover more general file corruption scenarios and refined self-correction attempts before escalation.
3.  **Improved "CRITICAL WARNING" (Step 4):** Replaced single lengthy example with three varied, shorter examples illustrating tool misapplication, plus a reinforcing conclusion.

**Reason for Changes:**
To improve AI rigor in verifying diffs and handling tool errors, driven by experiences with unintended `edit_file` modifications. Aims for clearer guidelines and more impactful warnings.

---

## Framework v0.2.1 - 2025-05-07

### AI_CODING_PROCESS.md
    * Added new Step 0 "Model Compatibility & Awareness Check" that verifies if the AI model is gemini-2.5-pro
    * If a different model is detected, requires explicit user confirmation to proceed with the process
    * Updated table of contents to include the new step

### PLAN_WRITING_PROCESS.md
    * Added new Step 0 "Model Compatibility & Awareness Check" that verifies if the AI model is gemini-2.5-pro
    * If a different model is detected, requires explicit user confirmation to proceed with the process

**Overall Goal/Theme of this Framework Update:** Enhance safety and user awareness by explicitly checking for model compatibility. Since the processes are optimized for gemini-2.5-pro, this change ensures users are explicitly informed and given an opportunity to confirm if they wish to continue with a different model that may not perform optimally with these processes.

---

## Framework v0.2.0 - 2025-05-06

### AI_CODING_PROCESS.md
    *   No direct changes to process content. Document now refers to Framework Version (effectively rolling in v0.1.54).

### PLAN_WRITING_PROCESS.md
    *   No direct changes to process content. Document now refers to Framework Version (effectively rolling in v0.1.7).

**Overall Goal/Theme of this Framework Update:** Unification of versioning for all process documents under a single framework version. Introduction of new changelog structure to reflect framework-level updates.

---

## Previous Individual Document Changelog (Historical - Pre-Framework v0.2.0)

**(This section preserves the history before unified versioning. New changes go under the unified Framework Version headings above.)**

## 2025-05-06

*   **[PLAN_WRITING_PROCESS.md v0.1.7]** Strengthened codebase analysis guidance to combat superficiality:
    *   Added explicit sub-step `1.3.d` ("Follow Initial Leads") to encourage deeper exploration during initial analysis.
    *   Augmented Step `2.3` ("Survey Existing Relevant Functionality") to mandate broader, conceptual searches if initial targeted surveys for common functionalities fail.
    *   Inserted a new "GUIDING PRINCIPLE FOR ANALYSIS: HOLISTIC CODEBASE UNDERSTANDING" at the start of Phase 2 to reinforce the need for contextual awareness.
    *   Refined the "PLANNING PRINCIPLE REMINDER: PRIORITIZE SPECIFIC CODE OVER GENERIC PATTERNS" to more strongly advocate for adapting existing sound patterns.
    *   Added a new self-assessment item `5. Sufficiency of Analysis Depth Assessment:` to the "Logic Analysis & Verification Summary" (Step `2.12`) to make the AI reflect on its thoroughness.
    *   Goal: Further mitigate the risk of AI planners performing superficial codebase analysis by providing more explicit directives and checks for deeper investigation.
*   **[PLAN_WRITING_PROCESS.md v0.1.6]** Further enhancements for AI coding synergy:
    *   Added new mandatory plan output sections (Step 3.4) to provide more comprehensive "briefing packages" for `AI_CODING_PROCESS.md`:
        *   `## Existing Relevant Functionality Survey (Ref: AI_CODING_PROCESS.md Step 3.1)`
        *   `## Key Standards & Architectural Patterns Applied (Ref: AI_CODING_PROCESS.md Step 3.2)`
        *   `## Potential Risks & Planned Mitigations`
    *   Updated Phase 2 analysis steps to explicitly prepare this data:
        *   Added new Step 2.3 (Survey Existing Relevant Functionality).
        *   Augmented Step 2.1 (Verify Standards Alignment) to collate applied standards/patterns.
        *   Added new Step 2.5 (Structure Key Impact Summary Details) for a more prescriptive summary.
        *   Augmented Step 2.9 (Define Clear, Actionable Steps) to include precise location hints for complex edits.
        *   Renamed and augmented Step 2.10 (Identify Open Questions, Risks, and Plan Mitigations).
    *   Renumbered Phase 2 steps (2.3 through 2.12) to accommodate new preparatory steps.
    *   Goal: Maximize proactive information gathering and structured output during planning to further improve efficiency and reliability when the plan is executed via `AI_CODING_PROCESS.md`.
*   **[PLAN_WRITING_PROCESS.md v0.1.5]** Enhanced plan output for AI coding synergy:
    *   Added new mandatory sections to the plan output structure (Step 3.4) to directly feed `AI_CODING_PROCESS.md`:
        *   `## Key Assumptions for Re-Verification (Ref: AI_CODING_PROCESS.md Step 3.4.1.b)`
        *   `## Logic Preservation Mapping Details (Ref: AI_CODING_PROCESS.md Step 3.4.1.e)`
        *   `## Suggested Impact Analysis Queries (For AI_CODING_PROCESS.md Steps 3.4.1.a, 4.4.2.b.i)`
        *   `## Critical Edge Cases for Implementation (Ref: AI_CODING_PROCESS.md Step 3.4.1.c)`
    *   Updated Phase 2 analysis steps (new 2.4, 2.5.b, 2.6, 2.9) to explicitly require preparing this structured information.
    *   Renumbered Phase 2 steps (2.4 through 2.10) to accommodate new preparatory steps.
    *   Goal: Make plans more directly actionable by `AI_CODING_PROCESS.md`, improving reliability and reducing ambiguity during implementation.
*   **[PLAN_WRITING_PROCESS.md v0.1.4]** Comprehensive structural review and clarification:
    *   Standardized all internal references to `PROJECT_STANDARDS.md` and `PROJECT_ARCHITECTURE.md`.
    *   Streamlined Phase 2: Corrected step numbering, removed redundant/placeholder steps.
    *   Clarified "verification" concepts, ensuring cross-references point to appropriate analytical steps rather than a non-existent dedicated verification step.
    *   Centralized definitions of mandatory guideline blocks ("No Defaults/Fallbacks," "Verify Assumptions") at the start of Phase 3 for clarity and easier reference.
    *   Updated an internal path to `learning_resources/ai_plan_writing_pitfalls.md`.
    *   Goal: Enhance clarity, internal consistency, and maintainability of the plan writing process.
*   **[AI_CODING_PROCESS.md v0.1.54]** Added Principle for Holistic Codebase Understanding:
    *   Introduced "Holistic Codebase Understanding for Integration and Reusability" to "Core Principles & Critical Checks Summary."
    *   This principle mandates striving for sufficient understanding of surrounding codebase context, reinforcing thorough searches for reusable functionality (Step 3.1) and meticulous impact analysis (Procedure: Analyze Impact).
    *   It explicitly calls out superficial codebase analysis as a deviation if it leads to poor integration, duplicated effort, or misunderstanding of architectural context.
    *   Goal: Ensure changes are not just locally correct but also fit cohesively and efficiently within the broader system, combating superficial codebase review.

*   **[AI_CODING_PROCESS.md v0.1.53]** Added Principle for Proactive Context Gathering:
    *   Introduced a new principle "Proactive Context Gathering for Robustness" to the "Core Principles & Critical Checks Summary" section.
    *   This principle encourages the AI to proactively favor obtaining more comprehensive file context (e.g., via `read_file` with `should_read_entire_file=True` when feasible) over relying on minimal partial views, especially for key files, to enhance understanding and reduce risks from incomplete context.
    *   Goal: Further improve the robustness of AI-assisted coding by promoting fuller situational awareness during planning and execution.

*   **[AI_CODING_PROCESS.md v0.1.52]** Enhanced Guidance on Sufficient File Context:
    *   Strengthened Step 3.0 ("Assess Target File Complexity") to more explicitly guide the AI to obtain fuller file context (e.g., by requesting from user or using `should_read_entire_file=True`) when initial partial reads of complex/central files prove insufficient for confident planning and editing.
    *   Added a clarifying sentence to Step 3.3 ("Check 'Work with Facts'") to reinforce the general principle that file content used for planning must be sufficiently complete to avoid guesswork, prompting clarification or more data if partial views are ambiguous.
    *   Goal: Improve proactive information gathering and reduce errors stemming from incomplete file context, based on observed successful interaction patterns.

*   **[AI_CODING_PROCESS.md v0.1.51]** Enhanced Robustness for Complex Edits & Tool Failures:
    *   Added Step 3.0 (Assess Target File Complexity) to mandate heightened scrutiny and proactive full file reads for critical/complex files from the outset of planning.
    *   Updated Step 3.8.b (Sufficient Context) and Step 4.1 (Generate Proposed `code_edit`) to require more precise context, unique anchors, and explicit `instructions` on unchangeable sections when dealing with sensitive or previously mis-edited files.
    *   Revised Step 4.4.3.b.e (Self-Correction for Tool Failure) to accelerate escalation to `Procedure: Request Manual Edit` after fewer failed attempts (2 instead of 3) or if the same pattern of disruptive modification occurs on a second attempt.
    *   Incorporated a new explicit check into `Procedure: Verify Diff` (Section 4) to verify the absence of major unintended structural changes (widespread deletions/reordering) as a critical deviation.
    *   Expanded `Procedure: Handle Failed Verification for Existing Dependency` (Section 5) with detailed investigative steps (usage check in referencing file, broader context search) before deciding on a course of action, based on details from v0.1.34 of the process.
    *   Corrected an issue where `Procedure: Verify Reapply Diff`, `Procedure: Verify Edit File Diff`, and `Procedure: Request Manual Edit` were inadvertently deleted from Section 5 during a previous automated edit (User reverted this part manually, AI confirmed). (Self-correction from prior turn)

*   **[AI_CODING_PROCESS.md v0.1.50]** Added Deferred Observations & Principle of Success:
    *   Added optional Step 6 ("Summarize Deferred Observations") to capture out-of-scope findings.
    *   Added "Principle of Successful Edit Application" definition in Core Principles section.
    *   Refined rules for "Autonomous Execution and Turn Management for Steps 4.1 to 4.3" (later superseded by v0.1.51 changes to remove optional pause).
    *   General wording refinements to combat superficial execution.

*   **[PLAN_WRITING_PROCESS.md v0.1.3]** Structural improvements & path correction:
    *   Broke down Step 1.3 (Read/Analyze State) into sub-steps (1.3.a, 1.3.b, 1.3.c).
    *   Grouped mandate/reminder blocks at the start of Phase 2.
    *   Corrected step numbering in Phase 2 (2.1-2.10).
    *   Refined mandatory header titles in Step 3.4 for presented plan clarity.
    *   Updated reference path for `ai_plan_writing_pitfalls.md`.

*   **[AI_CODING_PROCESS.md v0.1.49]** Strengthened Protection Against Superficiality:
    *   Mandated **inline checklist reporting** during execution of `Procedure: Verify Diff` to ensure granular sub-step verification.
    *   Strengthened **"Show Your Work" / Reporting Requirements** for key planning steps:
        *   Step 3.1 (Search Existing Logic): Require reporting tool, query, specific findings.
        *   Step 3.2 (Standards Alignment): Require citing specific standard/pattern name(s).
        *   Step 3.4.1.a (Impact Analysis): Require reporting specific consumers/inheritors found during enhanced scope checks.
        *   Step 3.4.1.c (Edge Cases): Require reporting *how* each case is addressed.
        *   Step 3.4.1.e (Logic Preservation): Require citing specific original conditions/paths.
    *   Added explicit **Failure Mode Reminders** within `Procedure: Verify Hypothesis` and `Procedure: Verify Diff`.
    *   Goal: Combat superficial execution by requiring more detailed proof of work during analysis and maximum transparency during critical diff verification.

*   **[AI_CODING_PROCESS.md v0.1.48]** Strengthened Skip Prevention Measures:
    *   Modified summary formats (Steps 3.10, 4.5) to require explicit justification (`[-] N/A: [justification]`) when marking steps as Not Applicable.
    *   Added explicit `Applicability:` or `Trigger:` lines to key procedures (Sections 4, 5) to clarify when they MUST be executed.
    *   Modified Step 5 (Adherence Checkpoint) to require an explicit concluding statement confirming the self-assessment was performed.
    *   Goal: Further reduce the likelihood of applicable steps being skipped unintentionally by enforcing conscious justification and clarifying trigger conditions.

*   **[README.md]** Updated references after deleting `ai_failure_modes.md` and `AI_CODING_PROCESS_CONFIRM_ADDENDUM.md`, clarified example docs.

## 2025-05-05

*   **[AI_CODING_PROCESS.md v0.1.47]** Strengthened Blocker Handling Procedures:
    *   Added explicit `STOP` and `await confirmation/guidance` requirement to `Procedure: Handle Unclear Root Cause / Missing Info` (Step 4) after seeking confirmation on investigation plans.
    *   Added explicit `STOP` and `await guidance` requirement to `Procedure: Ensure Logic Preservation` (Step 3) after presenting trade-offs for intentional logic changes.
    *   Goal: Ensure AI assistant explicitly pauses for user input in procedures where confirmation or guidance is requested before proceeding.

*   **[AI_CODING_PROCESS.md v0.1.46]** Enhanced Manual Edit Procedure: Modified `Procedure: Request Manual Edit` (Step 4) to explicitly require the AI assistant to **STOP** processing and await user confirmation after requesting a manual edit, before proceeding with verification steps (4.4). Addresses issue where AI incorrectly assumed manual edits were performed.

*   **[AI_CODING_PROCESS.md v0.1.45]** Renamed External Standard Doc References:
    *   Replaced all instances of `STANDARDS.md` with `PROJECT_STANDARDS.md`.
    *   Replaced all instances of `code_architecture_standard.md` with `PROJECT_ARCHITECTURE.md`.
    *   Goal: Use more descriptive and consistent names, reduce potential for conflicts with generic filenames.

*   **[AI_CODING_PROCESS.md v0.1.44]** Made External Standard References Optional:
    *   Modified Section 7 ("References") to use "Consult (if available)" instead of "Always refer".
    *   Allows `AI_CODING_PROCESS.md` to function standalone if `STANDARDS.md` or `code_architecture_standard.md` are not present.

*   **[AI_CODING_PROCESS.md v0.1.43]** Major Structural Refactor for Flow Clarity:
    *   Added Table of Contents (Section 1).
    *   Renamed "Handling Blockers & Deviations" to "Exception Handling Procedures" (Section 5) and explicitly framed them as off-ramps from the main workflow.
    *   Added explicit "STOP and execute Procedure X (Section 5)" instructions in Step 3 triggers.
    *   Restructured Step 4 ("Edit Generation & Verification Cycle") into clearer phases: 4.1 Generate, 4.2 Pre-Apply Verify, 4.3 Apply Edit, 4.4 Post-Apply Verify, 4.5 Generate Summary.
    *   Updated cross-references throughout to include relevant section numbers.
    *   Moved Glossary (Section 6) and References (Section 7) to the end.
    *   Goal: Improve overall document structure, navigation, and flow distinction between standard path and exception handling for AI processing.

*   **[AI_CODING_PROCESS.md v0.1.42]** Increased Granularity & Negative Checks:
    *   Restructured `Procedure: Analyze Impact` into an explicit numbered checklist for clarity.
    *   Added requirement to step 3.9.e (Diagnostics) to explicitly verify the removal of temporary code.
    *   Rephrased step 4.C.2.a (Leftover Code) as an explicit confirmation of *absence* of leftover artifacts.
    *   Goal: Enhance rigor in impact analysis and cleanup verification.

*   **[AI_CODING_PROCESS.md v0.1.41]** Minor Refinements for Clarity/Robustness:
    *   Added `## Glossary of Key Terms` section to centralize definitions (Deviation, Hypothesis, Root Cause, etc.).
    *   Added concise examples within `Procedure: Verify Hypothesis` and `Procedure: Handle Deviation`.
    *   Added a hint within `Procedure: Handle Deviation` to prioritize dependency checks.
    *   Added an explicit reminder to the main Self-Correction step (4.C.3) about avoiding excessive loops before escalating (`Procedure: Request Manual Edit`).
    *   Goal: Further enhance clarity and usability for AI processing.

*   **[AI_CODING_PROCESS.md v0.1.40]** Refactored for AI Readability/Processability:
    *   Introduced new section `## Reusable Verification Procedures` to define common checks (e.g., dependency verification, impact analysis, hypothesis verification, logic preservation) once.
    *   Introduced new section `## Handling Blockers & Deviations` to consolidate procedures for handling issues like unclear root causes, architectural decisions, necessary workarounds, and edit deviations.
    *   Streamlined main workflow (Steps 3 & 4) by replacing detailed inline descriptions with references to the new reusable procedures.
    *   Added `## Core Principles & Critical Checks Summary` section to highlight key principles and non-negotiable actions.
    *   Simplified Pre/Post-Action Verification Summary templates (3.10, 4.C.4) focusing on confirming procedure execution.
    *   Added final Adherence Checkpoint (Step 5).
    *   Goal: Improve structural clarity and modularity for AI processing while preserving the timing and rigor of all original checks.

*   **[AI_CODING_PROCESS.md v0.1.34]** Strengthened External API & Integration Checks:
    *   Enhanced Step `3.4.1.e` (Hypothesis Verification) to explicitly require treating external library API specifics (types, functions, class names) as assumptions needing verification (e.g., via documentation or existing usage patterns), especially when fixing related errors.
    *   Enhanced Step `3.4.1.g` (Verify Framework Compatibility) to require more explicitly identifying key framework/library interactions (e.g., "Typer + DI") and verifying the planned approach uses established patterns or checking for known integration issues.
    *   Enhanced Step `4.C.1.b` (Post-`edit_file` Verification) semantic spot-check to include explicitly verifying the correctness of interactions with external library APIs or framework-specific patterns introduced/modified by the edit.
    *   Aimed at preventing errors arising from unverified assumptions about external libraries or subtle framework integration issues.

*   **[AI_CODING_PROCESS.md v0.1.33]** Enhanced Core Component Refactoring Checks:
    *   Added explicit trigger warning to Step `3.4.1.b` (Enhanced Scope for Core Refactoring) when modifying base classes/core interfaces.
    *   Mandated searching for and listing inheritors/consumers as part of the `3.4.1.b` analysis.
    *   Updated Step `3.10` (Pre-computation Verification Summary) checklist to include a specific item (`2b`) for confirming the enhanced scope check (3.4.1.b) was performed when applicable.
    *   Added a new self-correction trigger (`f`) to Step `4.C.3.b.iii` to explicitly catch missed enhanced scope checks (3.4.1.b) during post-edit verification.
    *   Aimed to improve adherence and prevent errors caused by overlooking the impact of core component changes on inheriting classes.

*   **[AI_CODING_PROCESS.md v0.1.32]** Improved Structure/Readability (Step 4.C):
    *   Restructured section `4.C`
