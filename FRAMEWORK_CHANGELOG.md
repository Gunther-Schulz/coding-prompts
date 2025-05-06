# Framework Changelog

**(Note for AI Assistant: Use `date +%F` for current date. Always specify affected document and its version.)**

## 2025-05-06

*   **[AI_CODING_PROCESS.md v1.53]** Added Principle for Proactive Context Gathering:
    *   Introduced a new principle "Proactive Context Gathering for Robustness" to the "Core Principles & Critical Checks Summary" section.
    *   This principle encourages the AI to proactively favor obtaining more comprehensive file context (e.g., via `read_file` with `should_read_entire_file=True` when feasible) over relying on minimal partial views, especially for key files, to enhance understanding and reduce risks from incomplete context.
    *   Goal: Further improve the robustness of AI-assisted coding by promoting fuller situational awareness during planning and execution.

*   **[AI_CODING_PROCESS.md v1.52]** Enhanced Guidance on Sufficient File Context:
    *   Strengthened Step 3.0 ("Assess Target File Complexity") to more explicitly guide the AI to obtain fuller file context (e.g., by requesting from user or using `should_read_entire_file=True`) when initial partial reads of complex/central files prove insufficient for confident planning and editing.
    *   Added a clarifying sentence to Step 3.3 ("Check 'Work with Facts'") to reinforce the general principle that file content used for planning must be sufficiently complete to avoid guesswork, prompting clarification or more data if partial views are ambiguous.
    *   Goal: Improve proactive information gathering and reduce errors stemming from incomplete file context, based on observed successful interaction patterns.

*   **[AI_CODING_PROCESS.md v1.51]** Enhanced Robustness for Complex Edits & Tool Failures:
    *   Added Step 3.0 (Assess Target File Complexity) to mandate heightened scrutiny and proactive full file reads for critical/complex files from the outset of planning.
    *   Updated Step 3.8.b (Sufficient Context) and Step 4.1 (Generate Proposed `code_edit`) to require more precise context, unique anchors, and explicit `instructions` on unchangeable sections when dealing with sensitive or previously mis-edited files.
    *   Revised Step 4.4.3.b.e (Self-Correction for Tool Failure) to accelerate escalation to `Procedure: Request Manual Edit` after fewer failed attempts (2 instead of 3) or if the same pattern of disruptive modification occurs on a second attempt.
    *   Incorporated a new explicit check into `Procedure: Verify Diff` (Section 4) to verify the absence of major unintended structural changes (widespread deletions/reordering) as a critical deviation.
    *   Expanded `Procedure: Handle Failed Verification for Existing Dependency` (Section 5) with detailed investigative steps (usage check in referencing file, broader context search) before deciding on a course of action, based on details from v1.34 of the process.
    *   Corrected an issue where `Procedure: Verify Reapply Diff`, `Procedure: Verify Edit File Diff`, and `Procedure: Request Manual Edit` were inadvertently deleted from Section 5 during a previous automated edit (User reverted this part manually, AI confirmed). (Self-correction from prior turn)

*   **[AI_CODING_PROCESS.md v1.50]** Added Deferred Observations & Principle of Success:
    *   Added optional Step 6 ("Summarize Deferred Observations") to capture out-of-scope findings.
    *   Added "Principle of Successful Edit Application" definition in Core Principles section.
    *   Refined rules for "Autonomous Execution and Turn Management for Steps 4.1 to 4.3" (later superseded by v1.51 changes to remove optional pause).
    *   General wording refinements to combat superficial execution.

*   **[PLAN_WRITING_PROCESS.md v1.3]** Structural improvements & path correction:
    *   Broke down Step 1.3 (Read/Analyze State) into sub-steps (1.3.a, 1.3.b, 1.3.c).
    *   Grouped mandate/reminder blocks at the start of Phase 2.
    *   Corrected step numbering in Phase 2 (2.1-2.10).
    *   Refined mandatory header titles in Step 3.4 for presented plan clarity.
    *   Updated reference path for `ai_plan_writing_pitfalls.md`.

*   **[AI_CODING_PROCESS.md v1.49]** Strengthened Protection Against Superficiality:
    *   Mandated **inline checklist reporting** during execution of `Procedure: Verify Diff` to ensure granular sub-step verification.
    *   Strengthened **"Show Your Work" / Reporting Requirements** for key planning steps:
        *   Step 3.1 (Search Existing Logic): Require reporting tool, query, specific findings.
        *   Step 3.2 (Standards Alignment): Require citing specific standard/pattern name(s).
        *   Step 3.4.1.a (Impact Analysis): Require reporting specific consumers/inheritors found during enhanced scope checks.
        *   Step 3.4.1.c (Edge Cases): Require reporting *how* each case is addressed.
        *   Step 3.4.1.e (Logic Preservation): Require citing specific original conditions/paths.
    *   Added explicit **Failure Mode Reminders** within `Procedure: Verify Hypothesis` and `Procedure: Verify Diff`.
    *   Goal: Combat superficial execution by requiring more detailed proof of work during analysis and maximum transparency during critical diff verification.

*   **[AI_CODING_PROCESS.md v1.48]** Strengthened Skip Prevention Measures:
    *   Modified summary formats (Steps 3.10, 4.5) to require explicit justification (`[-] N/A: [justification]`) when marking steps as Not Applicable.
    *   Added explicit `Applicability:` or `Trigger:` lines to key procedures (Sections 4, 5) to clarify when they MUST be executed.
    *   Modified Step 5 (Adherence Checkpoint) to require an explicit concluding statement confirming the self-assessment was performed.
    *   Goal: Further reduce the likelihood of applicable steps being skipped unintentionally by enforcing conscious justification and clarifying trigger conditions.

*   **[README.md]** Updated references after deleting `ai_failure_modes.md` and `AI_CODING_PROCESS_CONFIRM_ADDENDUM.md`, clarified example docs.

## 2025-05-05

*   **[AI_CODING_PROCESS.md v1.47]** Strengthened Blocker Handling Procedures:
    *   Added explicit `STOP` and `await confirmation/guidance` requirement to `Procedure: Handle Unclear Root Cause / Missing Info` (Step 4) after seeking confirmation on investigation plans.
    *   Added explicit `STOP` and `await guidance` requirement to `Procedure: Ensure Logic Preservation` (Step 3) after presenting trade-offs for intentional logic changes.
    *   Goal: Ensure AI assistant explicitly pauses for user input in procedures where confirmation or guidance is requested before proceeding.

*   **[AI_CODING_PROCESS.md v1.46]** Enhanced Manual Edit Procedure: Modified `Procedure: Request Manual Edit` (Step 4) to explicitly require the AI assistant to **STOP** processing and await user confirmation after requesting a manual edit, before proceeding with verification steps (4.4). Addresses issue where AI incorrectly assumed manual edits were performed.

*   **[AI_CODING_PROCESS.md v1.45]** Renamed External Standard Doc References:
    *   Replaced all instances of `STANDARDS.md` with `PROJECT_STANDARDS.md`.
    *   Replaced all instances of `code_architecture_standard.md` with `PROJECT_ARCHITECTURE.md`.
    *   Goal: Use more descriptive and consistent names, reduce potential for conflicts with generic filenames.

*   **[AI_CODING_PROCESS.md v1.44]** Made External Standard References Optional:
    *   Modified Section 7 ("References") to use "Consult (if available)" instead of "Always refer".
    *   Allows `AI_CODING_PROCESS.md` to function standalone if `STANDARDS.md` or `code_architecture_standard.md` are not present.

*   **[AI_CODING_PROCESS.md v1.43]** Major Structural Refactor for Flow Clarity:
    *   Added Table of Contents (Section 1).
    *   Renamed "Handling Blockers & Deviations" to "Exception Handling Procedures" (Section 5) and explicitly framed them as off-ramps from the main workflow.
    *   Added explicit "STOP and execute Procedure X (Section 5)" instructions in Step 3 triggers.
    *   Restructured Step 4 ("Edit Generation & Verification Cycle") into clearer phases: 4.1 Generate, 4.2 Pre-Apply Verify, 4.3 Apply Edit, 4.4 Post-Apply Verify, 4.5 Generate Summary.
    *   Updated cross-references throughout to include relevant section numbers.
    *   Moved Glossary (Section 6) and References (Section 7) to the end.
    *   Goal: Improve overall document structure, navigation, and flow distinction between standard path and exception handling for AI processing.

*   **[AI_CODING_PROCESS.md v1.42]** Increased Granularity & Negative Checks:
    *   Restructured `Procedure: Analyze Impact` into an explicit numbered checklist for clarity.
    *   Added requirement to step 3.9.e (Diagnostics) to explicitly verify the removal of temporary code.
    *   Rephrased step 4.C.2.a (Leftover Code) as an explicit confirmation of *absence* of leftover artifacts.
    *   Goal: Enhance rigor in impact analysis and cleanup verification.

*   **[AI_CODING_PROCESS.md v1.41]** Minor Refinements for Clarity/Robustness:
    *   Added `## Glossary of Key Terms` section to centralize definitions (Deviation, Hypothesis, Root Cause, etc.).
    *   Added concise examples within `Procedure: Verify Hypothesis` and `Procedure: Handle Deviation`.
    *   Added a hint within `Procedure: Handle Deviation` to prioritize dependency checks.
    *   Added an explicit reminder to the main Self-Correction step (4.C.3) about avoiding excessive loops before escalating (`Procedure: Request Manual Edit`).
    *   Goal: Further enhance clarity and usability for AI processing.

*   **[AI_CODING_PROCESS.md v1.40]** Refactored for AI Readability/Processability:
    *   Introduced new section `## Reusable Verification Procedures` to define common checks (e.g., dependency verification, impact analysis, hypothesis verification, logic preservation) once.
    *   Introduced new section `## Handling Blockers & Deviations` to consolidate procedures for handling issues like unclear root causes, architectural decisions, necessary workarounds, and edit deviations.
    *   Streamlined main workflow (Steps 3 & 4) by replacing detailed inline descriptions with references to the new reusable procedures.
    *   Added `## Core Principles & Critical Checks Summary` section to highlight key principles and non-negotiable actions.
    *   Simplified Pre/Post-Action Verification Summary templates (3.10, 4.C.4) focusing on confirming procedure execution.
    *   Added final Adherence Checkpoint (Step 5).
    *   Goal: Improve structural clarity and modularity for AI processing while preserving the timing and rigor of all original checks.

*   **[AI_CODING_PROCESS.md v1.34]** Strengthened External API & Integration Checks:
    *   Enhanced Step `3.4.1.e` (Hypothesis Verification) to explicitly require treating external library API specifics (types, functions, class names) as assumptions needing verification (e.g., via documentation or existing usage patterns), especially when fixing related errors.
    *   Enhanced Step `3.4.1.g` (Verify Framework Compatibility) to require more explicitly identifying key framework/library interactions (e.g., "Typer + DI") and verifying the planned approach uses established patterns or checking for known integration issues.
    *   Enhanced Step `4.C.1.b` (Post-`edit_file` Verification) semantic spot-check to include explicitly verifying the correctness of interactions with external library APIs or framework-specific patterns introduced/modified by the edit.
    *   Aimed at preventing errors arising from unverified assumptions about external libraries or subtle framework integration issues.

*   **[AI_CODING_PROCESS.md v1.33]** Enhanced Core Component Refactoring Checks:
    *   Added explicit trigger warning to Step `3.4.1.b` (Enhanced Scope for Core Refactoring) when modifying base classes/core interfaces.
    *   Mandated searching for and listing inheritors/consumers as part of the `3.4.1.b` analysis.
    *   Updated Step `3.10` (Pre-computation Verification Summary) checklist to include a specific item (`2b`) for confirming the enhanced scope check (3.4.1.b) was performed when applicable.
    *   Added a new self-correction trigger (`f`) to Step `4.C.3.b.iii` to explicitly catch missed enhanced scope checks (3.4.1.b) during post-edit verification.
    *   Aimed to improve adherence and prevent errors caused by overlooking the impact of core component changes on inheriting classes.

*   **[AI_CODING_PROCESS.md v1.32]** Improved Structure/Readability (Step 4.C):
    *   Restructured section `4.C` (Post-Edit Verification) for better clarity.
    *   Broke down `4.C.1`, `4.C.2`, and `4.C.3` into clearly numbered/lettered sub-points.
    *   Nested the specific checks within `4.C.1.a` and `4.C.1.b` using Roman numerals (i, ii, ...).
    *   Moved critical reminders/warnings closer to the relevant sub-steps.
    *   Aimed for a more hierarchical and scannable structure similar to Step 3 and 4.A/B.

*   **[AI_CODING_PROCESS.md v1.31]** Improved Numbering/Structure Consistency (Step 3):
    *   Updated sections `3.5` through `3.10` to use consistent numbered sub-points (`3.x.1 Trigger:`, `3.x.2 Action:`) instead of bullet points.
    *   Adjusted nested numbering within these sections accordingly (e.g., `3.5.2.a`, `3.6.2.a`).
    *   Enhanced overall structural consistency of Step 3.

*   **[AI_CODING_PROCESS.md v1.30]** Corrected Sub-Step Numbering:
    *   Renumbered sub-steps under `3.4.1` (Analyze Impact) to use a consistent `a` through `i` lettering (`3.4.1.a`, `3.4.1.b`, ...).
    *   Renumbered sub-steps under `4.A.1` (Granular Final Review) from `4.1.x` to `4.A.1.x` (`4.A.1.a`, `4.A.1.b`, ...).
    *   Improved hierarchical clarity and consistency in these sections.

*   **[AI_CODING_PROCESS.md v1.29]** Restructured Step 4 (Post-computation Checks):
    *   Grouped Step 4 checks into distinct phases: 4.A (Pre-Edit Verification), 4.B (Apply Edit), 4.C (Post-Edit Verification).
    *   Preserved all original detailed checks within the new sub-phases.
    *   Added a `CRITICAL REMINDER` before 4.A emphasizing the mandatory nature and historical importance of pre-edit diff verification.
    *   Aimed at increasing clarity and reinforcing adherence to the verify-apply-verify cycle.

## 2025-05-04

*   **[PLAN_WRITING_PROCESS.md v1.1]** Language-Agnostic Refactoring:
    *   Generalized language-specific examples and terminology (imports/dependencies, errors, file conventions, specific model/library mentions) throughout the document.
    *   Updated references to `code_architecture_standard.md` to emphasize it contains language/framework-specific details.
    *   Focused descriptions on the *intent* of checks (dependency validation, interface consistency, logic preservation, etc.) rather than specific language mechanisms.
    *   Set initial version number.

*   **[AI_CODING_PROCESS.md v1.28]** Formatting Overhaul (Readability & Consistency):
    *   Standardized emphasis formatting for keywords (`**MUST**`, `**CRITICAL:**`, `**WARNING:**`, `**NOTE:**`, `**STOP**`) throughout the document.
    *   Added vertical spacing before major steps (e.g., 3.1-3.4) and complex sub-sections (e.g., 3.4.1.b, 4.1.d) to improve visual separation.
    *   Restructured dense sections (e.g., 3.4.1 Impact Analysis) using bullet points.
    *   Reformatted Step 4.7 self-correction triggers into a bulleted list.
    *   Used Markdown code blocks for summary checklists (3.10, 4.4) for consistency.

*   **[AI_CODING_PROCESS.md v1.27]** Language-Agnostic Refactoring:
    *   Generalized Python-specific examples and terminology (imports, errors, file conventions, specific library mentions) in `AI_CODING_PROCESS.md` Steps 3 and 4 to make the core workflow applicable across different programming languages.
    *   Updated references to `code_architecture_standard.md` to emphasize it contains language/framework-specific details.
    *   Focused descriptions on the *intent* of checks (dependency validation, interface consistency, etc.) rather than specific language mechanisms.
    *   Kept tool usage (`grep_search`, `read_file`) descriptions as-is, relating them to the AI environment.

*   **[AI_CODING_PROCESS.md v1.26]** Enhanced Handling of Failed Existing Imports (Steps 3.4.1.b, 3.5):
    *   Added mandatory "Usage Check" (within the importing file) when an existing import fails verification (Step 3.4.1.b).
    *   Prioritized removing the stale import if no usage is found.
    *   Updated Step 3.5 triggers and investigation plan to explicitly consider removing the referencing code (stale import/usage) instead of always recreating the missing dependency.
    *   Added Step 3.5.5 to mandate consultation with the user if creating significant new structures (modules/interfaces) seems the only fix based solely on potentially stale references.
    *   (Addresses specific failure where AI recreated a deleted module based on a stale import in `di.py`).

*   **[AI_CODING_PROCESS.md v1.25]** Enhanced Hypothesis Verification Enforcement (Steps 3.4.1.b, 3.10, 4.7):
    *   Strengthened Step 3.4.1.b to mandate immediate, structured reporting of verification execution and outcome directly after stating any hypothesis.
    *   Added new Step 3.10 requiring a Pre-computation Verification Summary checklist, including explicit confirmation that all stated hypotheses were verified as required by 3.4.1.b before proceeding to edits.
    *   Added an explicit self-correction trigger in Step 4.7 to STOP if the mandatory verification reporting for a hypothesis was missed.
    *   (Addresses specific failure where AI acted on an unverified hypothesis regarding code location, leading to persistent errors).

*   **[AI_CODING_PROCESS.md v1.24]** Added Configuration Usage Impact Check (Step 3.4.1.f): Mandated searching for and verifying usage locations when configuration values are added, removed, or changed.

*   **[AI_CODING_PROCESS.md v1.23]** Added Explicit Trade-off Presentation for Logic Changes (Step 3.4.1.e): Mandated that when simplification/unification alters execution conditions, the AI must explicitly present the trade-off between the simpler/altered plan and a potentially more complex/preserving plan, requesting user guidance before proceeding.

*   **[AI_CODING_PROCESS.md v1.22]** Clarified Justification for Logic Changes (Step 3.4.1.e): Enhanced requirement to explicitly justify changes where refactoring (e.g., simplification/unification) alters the *conditions* under which logic executes, even if the core behavior seems preserved. Added AI responsibility to proactively identify and report these subtle shifts.

*   **[AI_CODING_PROCESS.md v1.21]** Refined Pre-Edit Confirmation Examples (Step 4.2): Updated the example Pre-Edit Confirmation Statements to explicitly include reporting the outcome of the Logic Preservation check (Step 3.4.1.e) when applicable, improving visibility of this check before applying edits.

*   **[AI_CODING_PROCESS.md v1.20]** Enhanced Emphasis (Step 3.4.1.e), removed redundant changelog: Changed prefix from `MANDATORY` to `CRITICAL MANDATORY` for the logic preservation step to underscore its importance. Removed the embedded changelog from `AI_CODING_PROCESS.md`.

*   **[AI_CODING_PROCESS.md v1.19]** Added Logic Preservation Checks: Added step `3.4.1.e` and enhanced steps `3.4.1.b` and `4.5.b` in `AI_CODING_PROCESS.md` to explicitly require documenting existing logic and planning/verifying its preservation during refactoring/replacement tasks.

## 2025-05-03

*   **[AI_CODING_PROCESS.md v1.18]** Refined manual edit request procedure: Updated the logic for requesting manual edits after repeated tool failures. Now requires performing a final `read_file` check to confirm the file's *actual* state before generating the manual edit request, addressing cases where the tool might incorrectly report failure.

*   **[AI_CODING_PROCESS.md v1.17]** Strengthened impact analysis, added framework check, etc.: Mandated broader codebase search and consideration of all layers when refactoring core components. Added new requirement to verify signature/invocation compatibility with frameworks. Emphasized verifying interface consistency. Modified pre-deletion check logic. Added optional smoke test.

*   **[AI_CODING_PROCESS.md v1.16]** Increased context for manual edits: Updated the manual edit request procedure to explicitly require including sufficient surrounding context (e.g., 5-10 lines before and after).

*   **[AI_CODING_PROCESS.md v1.15]** Added manual edit request procedure detail: Clarified that manual edit requests MUST include a complete copy-paste code block.

*   **[AI_CODING_PROCESS.md v1.14]** Added key principle note: Included prominent note emphasizing strict adherence to verification steps.

*   **[AI_CODING_PROCESS.md v1.13]** Added mandatory adherence checkpoint (Step 5): Introduced final checkpoint requiring confirmation that all steps were executed.

*   **[AI_CODING_PROCESS.md v1.12]** Suggested library doc check: Added suggestion to check library docs when verifying assumptions.

*   **[AI_CODING_PROCESS.md v1.11]** Added numbering to Step 3: Introduced explicit numbering to sub-points for clarity.

*   **[AI_CODING_PROCESS.md v1.10]** Strengthened import validation: Mandated explicit reporting of path/symbol validation outcome for imports pre-edit and re-verification post-edit.

*   **[AI_CODING_PROCESS.md v1.9]** Mandated explicit reporting for checks: Updated process to require AI assistant to explicitly state performance and outcome of mandatory checks.

*   **[AI_CODING_PROCESS.md v1.8]** Strengthened post-edit diff verification logic.
