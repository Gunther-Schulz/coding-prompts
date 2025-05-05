# AI Coding Process Changelog

**(Note for AI Assistant: When adding a new entry, always use the current date. You can get the correct date in YYYY-MM-DD format by running the command `date +%F` in the terminal.)**

## v1.34 - 2025-05-05

*   **Strengthened External API & Integration Checks:**
    *   Enhanced Step `3.4.1.e` (Hypothesis Verification) to explicitly require treating external library API specifics (types, functions, class names) as assumptions needing verification (e.g., via documentation or existing usage patterns), especially when fixing related errors.
    *   Enhanced Step `3.4.1.g` (Verify Framework Compatibility) to require more explicitly identifying key framework/library interactions (e.g., "Typer + DI") and verifying the planned approach uses established patterns or checking for known integration issues.
    *   Enhanced Step `4.C.1.b` (Post-`edit_file` Verification) semantic spot-check to include explicitly verifying the correctness of interactions with external library APIs or framework-specific patterns introduced/modified by the edit.
    *   Aimed at preventing errors arising from unverified assumptions about external libraries or subtle framework integration issues.

## v1.33 - 2025-05-05

*   **Enhanced Core Component Refactoring Checks:**
    *   Added explicit trigger warning to Step `3.4.1.b` (Enhanced Scope for Core Refactoring) when modifying base classes/core interfaces.
    *   Mandated searching for and listing inheritors/consumers as part of the `3.4.1.b` analysis.
    *   Updated Step `3.10` (Pre-computation Verification Summary) checklist to include a specific item (`2b`) for confirming the enhanced scope check (3.4.1.b) was performed when applicable.
    *   Added a new self-correction trigger (`f`) to Step `4.C.3.b.iii` to explicitly catch missed enhanced scope checks (3.4.1.b) during post-edit verification.
    *   Aimed to improve adherence and prevent errors caused by overlooking the impact of core component changes on inheriting classes.

## v1.32 - 2025-05-05

*   **Improved Structure/Readability (Step 4.C):**
    *   Restructured section `4.C` (Post-Edit Verification) for better clarity.
    *   Broke down `4.C.1`, `4.C.2`, and `4.C.3` into clearly numbered/lettered sub-points.
    *   Nested the specific checks within `4.C.1.a` and `4.C.1.b` using Roman numerals (i, ii, ...).
    *   Moved critical reminders/warnings closer to the relevant sub-steps.
    *   Aimed for a more hierarchical and scannable structure similar to Step 3 and 4.A/B.

## v1.31 - 2025-05-05

*   **Improved Numbering/Structure Consistency (Step 3):**
    *   Updated sections `3.5` through `3.10` to use consistent numbered sub-points (`3.x.1 Trigger:`, `3.x.2 Action:`) instead of bullet points.
    *   Adjusted nested numbering within these sections accordingly (e.g., `3.5.2.a`, `3.6.2.a`).
    *   Enhanced overall structural consistency of Step 3.

## v1.30 - 2025-05-05

*   **Corrected Sub-Step Numbering:**
    *   Renumbered sub-steps under `3.4.1` (Analyze Impact) to use a consistent `a` through `i` lettering (`3.4.1.a`, `3.4.1.b`, ...).
    *   Renumbered sub-steps under `4.A.1` (Granular Final Review) from `4.1.x` to `4.A.1.x` (`4.A.1.a`, `4.A.1.b`, ...).
    *   Improved hierarchical clarity and consistency in these sections.

## v1.29 - 2025-05-05

*   **Restructured Step 4 (Post-computation Checks):**
    *   Grouped Step 4 checks into distinct phases: 4.A (Pre-Edit Verification), 4.B (Apply Edit), 4.C (Post-Edit Verification).
    *   Preserved all original detailed checks within the new sub-phases.
    *   Added a `CRITICAL REMINDER` before 4.A emphasizing the mandatory nature and historical importance of pre-edit diff verification.
    *   Aimed at increasing clarity and reinforcing adherence to the verify-apply-verify cycle.

## v1.28 - 2025-05-04

*   **Formatting Overhaul (Readability & Consistency):**
    *   Standardized emphasis formatting for keywords (`**MUST**`, `**CRITICAL:**`, `**WARNING:**`, `**NOTE:**`, `**STOP**`) throughout the document.
    *   Added vertical spacing before major steps (e.g., 3.1-3.4) and complex sub-sections (e.g., 3.4.1.b, 4.1.d) to improve visual separation.
    *   Restructured dense sections (e.g., 3.4.1 Impact Analysis) using bullet points.
    *   Reformatted Step 4.7 self-correction triggers into a bulleted list.
    *   Used Markdown code blocks for summary checklists (3.10, 4.4) for consistency.

## v1.27 - 2025-05-04

*   **Language-Agnostic Refactoring:**
    *   Generalized Python-specific examples and terminology (imports, errors, file conventions, specific library mentions) in `AI_CODING_PROCESS.md` Steps 3 and 4 to make the core workflow applicable across different programming languages.
    *   Updated references to `code_architecture_standard.md` to emphasize it contains language/framework-specific details.
    *   Focused descriptions on the *intent* of checks (dependency validation, interface consistency, etc.) rather than specific language mechanisms.
    *   Kept tool usage (`grep_search`, `read_file`) descriptions as-is, relating them to the AI environment.

## v1.26 - 2025-05-04

*   **Enhanced Handling of Failed Existing Imports (Steps 3.4.1.b, 3.5):**
    *   Added mandatory "Usage Check" (within the importing file) when an existing import fails verification (Step 3.4.1.b).
    *   Prioritized removing the stale import if no usage is found.
    *   Updated Step 3.5 triggers and investigation plan to explicitly consider removing the referencing code (stale import/usage) instead of always recreating the missing dependency.
    *   Added Step 3.5.5 to mandate consultation with the user if creating significant new structures (modules/interfaces) seems the only fix based solely on potentially stale references.
    *   (Addresses specific failure where AI recreated a deleted module based on a stale import in `di.py`).

## v1.25 - 2025-05-04

*   **Enhanced Hypothesis Verification Enforcement (Steps 3.4.1.b, 3.10, 4.7):**
    *   Strengthened Step 3.4.1.b to mandate immediate, structured reporting of verification execution and outcome directly after stating any hypothesis.
    *   Added new Step 3.10 requiring a Pre-computation Verification Summary checklist, including explicit confirmation that all stated hypotheses were verified as required by 3.4.1.b before proceeding to edits.
    *   Added an explicit self-correction trigger in Step 4.7 to STOP if the mandatory verification reporting for a hypothesis was missed.
    *   (Addresses specific failure where AI acted on an unverified hypothesis regarding code location, leading to persistent errors).

## v1.24 - 2025-05-04

*   **Added Configuration Usage Impact Check (Step 3.4.1.f):** Mandated searching for and verifying usage locations when configuration values are added, removed, or changed.

## v1.23 - 2025-05-04

*   **Added Explicit Trade-off Presentation for Logic Changes (Step 3.4.1.e):** Mandated that when simplification/unification alters execution conditions, the AI must explicitly present the trade-off between the simpler/altered plan and a potentially more complex/preserving plan, requesting user guidance before proceeding.

## v1.22 - 2025-05-04

*   **Clarified Justification for Logic Changes (Step 3.4.1.e):** Enhanced requirement to explicitly justify changes where refactoring (e.g., simplification/unification) alters the *conditions* under which logic executes, even if the core behavior seems preserved. Added AI responsibility to proactively identify and report these subtle shifts.

## v1.21 - 2025-05-04

*   **Refined Pre-Edit Confirmation Examples (Step 4.2):** Updated the example Pre-Edit Confirmation Statements to explicitly include reporting the outcome of the Logic Preservation check (Step 3.4.1.e) when applicable, improving visibility of this check before applying edits.

## v1.20 - 2025-05-04

*   **Enhanced Emphasis (Step 3.4.1.e):** Changed prefix from `MANDATORY` to `CRITICAL MANDATORY` for the logic preservation step to underscore its importance.
*   **Removed Redundant Changelog:** Removed the embedded changelog from `AI_CODING_PROCESS.md` as this file is the canonical source.

## v1.19 - 2025-05-04

*   **Added Logic Preservation Checks:** Added step `3.4.1.e` and enhanced steps `3.4.1.b` and `4.5.b` in `AI_CODING_PROCESS.md` to explicitly require documenting existing logic and planning/verifying its preservation during refactoring/replacement tasks.

## v1.18 - 2025-05-03

*   **Refined Manual Edit Request Procedure (Step 4.7):** Updated the logic for requesting manual edits after repeated tool failures. Now requires performing a final `read_file` check to confirm the file's *actual* state before generating the manual edit request, addressing cases where the tool might incorrectly report failure.

## v1.17 - 2025-05-03

*   **Strengthened Impact Analysis Scope (Step 3.4.1):** Mandated broader codebase search (beyond imports) and consideration of all layers when refactoring core components (DI, base config, etc.).
*   **Added Framework Compatibility Check (Step 3.4.1.d):** Added new requirement to verify signature/invocation compatibility with frameworks (Typer, etc.) for entry point functions.
*   **Emphasized Caller/Callee Verification (Step 3.4.1.b):** Added emphasis on verifying interface consistency (types, access methods) from both perspectives, especially post-refactoring.
*   **Modified Pre-Deletion Check Logic (Step 4.6.2):** Removed the separate pre-deletion check (old 4.6.3). Enhanced the post-move/rename/delete verification (4.6.2) to mandate searching for dangling references *after* deletion and trigger self-correction if found, prioritizing deletion over blocking.
*   **Added Optional Smoke Test (Step 4.5.d):** Added suggestion to perform minimal runtime check (smoke test) after significant refactoring to catch immediate integration errors.

## v1.16 - 2025-05-03

*   **Increased Context for Manual Edits (Step 4.7):** Updated the manual edit request procedure to explicitly require including sufficient surrounding context (e.g., 5-10 lines before and after) in the provided copy-paste block.

## v1.15 - 2025-05-03

*   **Added Manual Edit Request Procedure (Step 4.7):** Clarified that when requesting manual user edits after repeated tool failures, the request MUST include a complete copy-paste code block showing the desired state of the relevant code section.

## v1.14 - 2025-05-03

*   **Added Key Principle Note:** Included a prominent note near the beginning emphasizing that strict adherence to the verification and checking steps is the most crucial factor for success.

## v1.13 - 2025-05-03

*   **Added Mandatory Adherence Checkpoint (Step 5):** Introduced a final checkpoint step requiring explicit confirmation that all mandatory process steps (1-4) were executed and reported within the response cycle.
*   **Strengthened Process Self-Correction Trigger (Step 4.7):** Added an explicit trigger to STOP and self-correct if *any* mandatory process step (check, reporting, verification) was missed, skipped, or incompletely performed.

## v1.12 - 2025-05-03

*   **Suggest Library Doc Check (Step 3.4.1.b):**
    *   Added a suggestion to explicitly consider checking library documentation or examples when formulating assumptions about the usage of library functions/classes, especially for less common patterns, to verify API contracts.

## v1.11 - 2025-05-03

*   **Added Numbering to Step 3 (Pre-computation):**
    *   Introduced explicit numbering (3.1, 3.2, 3.4.1, etc.) to sub-points within Step 3 for improved clarity and referencing, mirroring the structure of Step 4.

## v1.10 - 2025-05-03

*   **Strengthened Import Validation (Steps 4.1.d & 4.5.b):**
    *   Enhanced pre-edit check (4.1.d) to mandate cross-referencing prior knowledge for import paths and explicitly reporting path/symbol validation outcome for *every* import in the diff context.
    *   Enhanced post-edit check (4.5.b) to mandate re-verification of path existence and symbol existence for *all* imports present in the *applied* diff, explicitly reporting the outcome.
    *   (Addresses specific failure where an incorrect import path was assumed based on naming conventions and not caught by validation).

## v1.9 - 2025-05-03

*   **Mandated Explicit Reporting for Checks (Steps 3 & 4):**
    *   Updated process to require the AI assistant to explicitly state the performance and outcome of mandatory verification and analysis checks (e.g., dependency checks, assumption verification, deviation handling, post-edit validation) in the conversation log.
    *   Aimed at increasing transparency and reducing errors caused by skipped or implicitly performed checks.

## v1.8 - 2025-05-03

*   **Strengthened Post-`
