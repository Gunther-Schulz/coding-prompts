## Protocol for Runtime Error Diagnosis and Resolution

*   **Trigger:** When a runtime error (e.g., indicated by a traceback, failed command execution with error messages) occurs during code execution or testing, interrupting the planned workflow.
*   **Goal:** To systematically identify the root cause of the runtime error and implement a verified fix, while adhering to the project's quality standards and the core principles of this document.
*   **Core Principle Application:** Runtime error diagnosis requires the same level of care, diligence, and adherence to the standard workflow (Steps 1-6) as any other coding task. All Core Principles (Section 2) remain paramount. The steps outlined below adapt the standard workflow, particularly Section 3 (Planning), for this specific context. Reporting detail **MUST** remain consistent as per the Meta-Instruction in Section 3.

*   **Procedure:**
    1.  **Acknowledge Error & Shift Focus:** Explicitly state the observed runtime error (citing traceback/logs) and confirm that the immediate goal is now to diagnose and resolve this error.
    2.  **Apply Standard Workflow (Adapted):** Proceed through the standard Steps 1-6, adapting the "Planning" phase (Step 3) as follows:
        *   **Step 1 (Confirm Standards Awareness):** Reiterate commitment to standards.
        *   **Step 2 (Confirm Goal):** The goal is now implicitly "Diagnose and fix runtime error Z." Confirmation may be skipped unless the error context is highly ambiguous.
        *   **Step 3 (Pre-computation Standards Check - Diagnostic Adaptation):**
            *   `3.0. Assess Complexity & Ensure Sufficient Context:` **CRITICAL:** Gather all facts related to the error. Execute `Procedure: Ensure Sufficient File Context` for all files directly implicated by the traceback. Obtain full tracebacks, relevant log entries, and understand the inputs/conditions that triggered the error. **Explicitly address any information discrepancies** using the guidelines under the "Work with Facts" Core Principle (Section 2).
            *   `3.1. Search for Existing Logic:` Search codebase/documentation for similar errors, patterns, or known issues related to the components involved.
            *   `3.2. Identify Standards & Verify Alignment:` Identify the standard/principle being violated (e.g., type safety, library usage contract, logical flow). The plan's goal is to restore alignment.
            *   `3.3. Check "Work with Facts":` Base all hypotheses and plans strictly on the gathered evidence (tracebacks, logs, verified code state). Clearly state assumptions derived from evidence.
            *   `3.4. Check "Robust Solutions":` Prioritize finding the root cause over superficial patches. Plan a fix that addresses the underlying issue. Apply diagnostic sub-steps meticulously:
                *   `3.4.1.a. Analyze Impact:` Analyze the potential impact of the *proposed fix* on related code/functionality *before* applying it.
                *   `3.4.1.b. Verify Hypothesis:` **This is the core diagnostic loop.**
                    i.  Formulate a specific, testable hypothesis about the root cause based on the evidence.
                    ii. Detail the precise steps (tools, code reads, checks) to verify or refute the hypothesis.
                    iii.Execute verification and report the outcome using the standard `Procedure: Verify Hypothesis` format.
                    iv. If hypothesis is confirmed and leads to a fix, proceed to plan the fix. If refuted, formulate a new hypothesis based on findings and repeat.
                *   `3.4.1.c. Enumerate Edge Cases/Errors:` Consider edge cases for the *proposed fix* itself. Does it handle different inputs correctly? Does it introduce new potential errors?
                *   `3.4.1.d-j:` Apply other relevant checks from Step 3.4.1 (Framework Compatibility, Logic Preservation if refactoring is part of the fix, Config Impact, Testability, etc.) to the *proposed fix*.
            *   `3.5. / 3.6. Handle Blockers:` If the root cause remains unclear after initial investigation, or if fixing the error requires architectural changes, use the appropriate procedures from Section 5.
            *   `3.10 / 3.11 (Verification Summary / Preconditions):` Generate the Pre-computation Verification Summary for the *planned fix* before proceeding to Step 4. Verify preconditions for the fix edit.
        *   **Step 4 (Edit Generation & Verification Cycle):** Apply the verified fix using the standard generate-verify-apply-verify cycle for the relevant file(s). Meticulously verify the applied diff.
        *   **Step 5 (Adherence Checkpoint):** Confirm all steps (1-6, adapted) were followed for the error resolution.
        *   **Step 6 (Summarize Deferred Observations):** Note any observations made during debugging that are out of scope for the immediate fix.
    3.  **Resume Original Task (If Applicable):** Once the error is confirmed fixed and verified (completion of Step 5/6 for the fix), explicitly state this and indicate readiness to resume the original task that was interrupted, potentially re-evaluating its plan (Step 3) if the error/fix impacted it.
