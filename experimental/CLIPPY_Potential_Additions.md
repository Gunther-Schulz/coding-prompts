# Optional Additions for CLIPPY.MD

## Potential Addition for Step 4.4 (Post-Apply Verification)

*(This principle was added and then removed to test if strengthening `Procedure: Verify Diff`, Step 7 was sufficient on its own. It can be re-added at the beginning of Step 4.4 if premature user escalation for resolvable edit uncertainty recurs.)*

*   **Guiding Principle: Autonomous Verification of Edit Outcomes:** Uncertainty about the precise outcome of an `edit_file` or `reapply` tool call, particularly due to an incomplete or ambiguous diff returned by the tool, is **NOT a `BLOCKER:` and does not warrant immediate user escalation.** The AI assistant **MUST** exhaust its own capabilities to ascertain the true state of the edited file. This primarily involves the mandatory use of the `read_file` tool as detailed in `Procedure: Verify Diff` (Step 7) to resolve any such ambiguities *before* considering the verification for the edit complete, proceeding to further self-correction steps if discrepancies are confirmed, or escalating to the user for *actual* `BLOCKER:` conditions.

## Proposed Addition for Step 4.2.1 (Perform Granular Final Review & Deviation Handling Loop (Pre-Edit))

*   **Context:** This new sub-step `d.` would be added within Step 4.2.1 of `CLIPPY.MD`, occurring *after* the AI has generated the proposed `code_edit` diff text (Step 4.1) but *before* confirming it's ready to be applied (Step 4.3).

*   **Proposed New Sub-step `d.`:**

    `d.` **Verify Generated Code Conciseness:**
        *   **Action:** Review any newly generated functions or methods within the proposed `code_edit` diff against the "Function/Method Conciseness" standard defined in `PROJECT_STANDARDS.MD`.
        *   **Check:** Specifically assess if any new function/method is overly long, has excessive nesting, or handles too many responsibilities.
        *   **Handle Violation:** If a violation is found:
            i.  **MUST NOT** proceed with the current `code_edit`.
            ii. **MUST** explicitly state that the generated code violates the conciseness standard.
            iii.**MUST** revise the plan (return to Step 3) to decompose the logic into smaller, compliant functions/methods.
            iv. **MUST** then re-execute Step 4.1 to generate a new, compliant `code_edit` diff.
        *   **Report:** Confirm this check was performed and whether any revisions were necessary due to it.
