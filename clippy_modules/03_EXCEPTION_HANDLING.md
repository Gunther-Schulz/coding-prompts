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
*   **Steps:**
    1.  **DO NOT** immediately plan creation of the missing dependency.
    2.  **State the Discrepancy:** Clearly state that established code references a module/symbol that cannot be found, specifying the reference and the file containing it.
    3.  **Perform Usage Check in Referencing File:** **MUST** use tools (`grep_search`) to search *within the referencing file* for actual usage of the specific symbol(s) (e.g., instantiation, method calls, type annotations). Report findings explicitly (e.g., "Usage Check in `file_x.py`: Searched for `MissingSymbol(...)`. Outcome: No usage found." or "Outcome: Usage found in function `Y`.").
    4.  **Perform Broader Context Search:** Execute additional searches (`grep_search` for related terms/files, `codebase_search` for conceptual links) to find context about the missing element (e.g., was it moved? refactored? deprecated? any TODOs for removal?). Report findings.
    5.  **Determine Next Action based on Context and Usage:**
        *   If context clearly indicates a simple move/rename and the symbol is still needed: Plan the fix (update reference path).
        *   If the dependency failed **AND** the Usage Check (Step 3) found **NO USAGE** in the referencing file **AND** broader context does not suggest it's critical: The default plan should be to **remove the stale dependency reference statement**. Briefly verify this removal doesn't cause downstream issues.
        *   If the dependency failed **AND** usage *was* found, OR if context suggests intentional removal but usage remains, OR if context is insufficient/ambiguous: **Execute `Procedure: Handle Unclear Root Cause / Missing Info` (Section 5)**, providing the findings from this procedure as input.

**`Procedure: Handle Deviation`**
*   **Trigger:** Called by `Procedure: Verify Diff` (Step 3) whenever deviations (unplanned changes) are identified between the `diff` being checked and the `intent`.
*   **Purpose:** To fact-check and decide on unplanned lines ("Deviations") in a proposed `code_edit` diff.
*   **Steps (For EACH Deviation):**
    1.  **Fact-check Deviation:** Use tools to confirm the existence and correctness of the deviation.
    2.  **Justify Deviation:** If deviation is intentional, justify it. If not, state it's unplanned and report it.
    3.  **Integrate or Revise:** If the deviation is verified, justified as beneficial/necessary, and accepted: **formally update the current working plan and the intended `code_edit` to incorporate this deviation.** The `code_edit` can then be considered aligned with the *updated* plan. If the deviation is not acceptable, **revise the proposed `code_edit` to remove the deviation** and align with the original (or a newly revised) plan.

**`Procedure: Request Manual Edit`**
*   **Trigger:** Called by Step 4.4.3.b.e if tool failures persist or edit application requires complex cleanup.
*   **Advisory for "Unacceptable Original Edit Application with Failed Cleanup" Trigger:**
    *   When this procedure is invoked because an automated edit achieved the primary goal but introduced unacceptable side-effects (churn, new errors in unrelated code) which a subsequent focused cleanup attempt also failed to resolve (as per Step 4.4.3.b.e.ii):
        *   The explanation in "State Tool Failure" (Step 2 below) **MUST** clearly differentiate between the successful primary change and the failed cleanup of the unacceptable side-effects.
        *   The primary goal of "Provide Specific Edit Details for Manual Application" (Step 3 below) **MUST** be to provide the user with the **original, minimal, and correct planned change** that was initially intended, presented cleanly with its necessary context, as if the side-effects had not occurred. This allows the user to apply the core intended change cleanly.
1.  **STOP** tool attempts.
2.  **State Tool Failure:** Explain the issue and the presumed incorrect file state based on last tool output.
3.  **Provide Specific Edit Details for Manual Application:**
    *   **MUST** clearly specify the target file.
    *   **MUST** clearly state whether the action is an insertion, replacement, or deletion.
    *   If an insertion or replacement, **MUST** provide the precise code block to be inserted/used as replacement.
    *   If a deletion, **MUST** clearly describe or show the exact lines to be deleted.
    *   **MUST** include sufficient surrounding context (a few lines before and after the change area) to uniquely identify the location of the edit.
    *   **MUST** present this (for insertions/replacements) as a single, clearly demarcated code block, using the language's comment style for `// ... existing code ...` where appropriate, that the user can easily copy and paste to represent the *final state* of the edited section.
    *   *Example (for insertion/replacement): "To manually apply the fix to `src/example.py`, please modify the relevant section to match the following (or insert at the indicated location):"*
        ```python
        // ... existing code ...
        // Context line before change
        // New code to be inserted / This block replaces the old code
        // Further new code if applicable
        // Context line after change
        // ... existing code ...
        ```
    *   *Example (for deletion): "To manually apply the fix to `src/example.py`, please delete the following lines:"*
        ```python
        // ... existing code ...
        // Context line before deletion
        // Line to be deleted
        // Another line to be deleted
        // Context line after deletion
        // ... existing code ...
        ```
