**(Note for AI Assistant: Use `date +%F` for current date. Future entries will list changed documents under the Framework Version.)**

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
Enhanced the AI coding process to improve proactive assessment of existing code patterns, reconciliation of plans with partially modified codebases, handling of plan assumptions, and clarification of "plan" references.

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
    *   Restructured section `4.C` (Post-Edit Verification) for better clarity.
    *   Broke down `4.C.1`, `4.C.2`, and `4.C.3` into clearly numbered/lettered sub-points.
    *   Nested the specific checks within `4.C.1.a` and `4.C.1.b` using Roman numerals (i, ii, ...).
    *   Moved critical reminders/warnings closer to the relevant sub-steps.
    *   Aimed for a more hierarchical and scannable structure similar to Step 3 and 4.A/B.

*   **[AI_CODING_PROCESS.md v0.1.31]** Improved Numbering/Structure Consistency (Step 3):
    *   Updated sections `3.5` through `3.10` to use consistent numbered sub-points (`3.x.1 Trigger:`, `3.x.2 Action:`) instead of bullet points.
    *   Adjusted nested numbering within these sections accordingly (e.g., `3.5.2.a`, `3.6.2.a`).
    *   Enhanced overall structural consistency of Step 3.

*   **[AI_CODING_PROCESS.md v0.1.30]** Corrected Sub-Step Numbering:
    *   Renumbered sub-steps under `3.4.1` (Analyze Impact) to use a consistent `a` through `i` lettering (`3.4.1.a`, `3.4.1.b`, ...).
    *   Renumbered sub-steps under `4.A.1` (Granular Final Review) from `4.1.x` to `4.A.1.x` (`4.A.1.a`, `4.A.1.b`, ...).
    *   Improved hierarchical clarity and consistency in these sections.

*   **[AI_CODING_PROCESS.md v0.1.29]** Restructured Step 4 (Post-computation Checks):
    *   Grouped Step 4 checks into distinct phases: 4.A (Pre-Edit Verification), 4.B (Apply Edit), 4.C (Post-Edit Verification).
    *   Preserved all original detailed checks within the new sub-phases.
    *   Added a `CRITICAL REMINDER` before 4.A emphasizing the mandatory nature and historical importance of pre-edit diff verification.
    *   Aimed at increasing clarity and reinforcing adherence to the verify-apply-verify cycle.

## 2025-05-04

*   **[PLAN_WRITING_PROCESS.md v0.1.1]** Language-Agnostic Refactoring:
    *   Generalized language-specific examples and terminology (imports/dependencies, errors, file conventions, specific model/library mentions) throughout the document.
    *   Updated references to `code_architecture_standard.md` to emphasize it contains language/framework-specific details.
    *   Focused descriptions on the *intent* of checks (dependency validation, interface consistency, logic preservation, etc.) rather than specific language mechanisms.
    *   Set initial version number.

*   **[AI_CODING_PROCESS.md v0.1.28]** Formatting Overhaul (Readability & Consistency):
    *   Standardized emphasis formatting for keywords (`**MUST**`, `**CRITICAL:**`, `**WARNING:**`, `**NOTE:**`, `**STOP**`) throughout the document.
    *   Added vertical spacing before major steps (e.g., 3.1-3.4) and complex sub-sections (e.g., 3.4.1.b, 4.1.d) to improve visual separation.
    *   Restructured dense sections (e.g., 3.4.1 Impact Analysis) using bullet points.
    *   Reformatted Step 4.7 self-correction triggers into a bulleted list.
    *   Used Markdown code blocks for summary checklists (3.10, 4.4) for consistency.

*   **[AI_CODING_PROCESS.md v0.1.27]** Language-Agnostic Refactoring:
    *   Generalized Python-specific examples and terminology (imports, errors, file conventions, specific library mentions) in `AI_CODING_PROCESS.md` Steps 3 and 4 to make the core workflow applicable across different programming languages.
    *   Updated references to `code_architecture_standard.md` to emphasize it contains language/framework-specific details.
    *   Focused descriptions on the *intent* of checks (dependency validation, interface consistency, etc.) rather than specific language mechanisms.
    *   Kept tool usage (`grep_search`, `read_file`) descriptions as-is, relating them to the AI environment.

*   **[AI_CODING_PROCESS.md v0.1.26]** Enhanced Handling of Failed Existing Imports (Steps 3.4.1.b, 3.5):
    *   Added mandatory "Usage Check" (within the importing file) when an existing import fails verification (Step 3.4.1.b).
    *   Prioritized removing the stale import if no usage is found.
    *   Updated Step 3.5 triggers and investigation plan to explicitly consider removing the referencing code (stale import/usage) instead of always recreating the missing dependency.
    *   Added Step 3.5.5 to mandate consultation with the user if creating significant new structures (modules/interfaces) seems the only fix based solely on potentially stale references.
    *   (Addresses specific failure where AI recreated a deleted module based on a stale import in `di.py`).

*   **[AI_CODING_PROCESS.md v0.1.25]** Enhanced Hypothesis Verification Enforcement (Steps 3.4.1.b, 3.10, 4.7):
    *   Strengthened Step 3.4.1.b to mandate immediate, structured reporting of verification execution and outcome directly after stating any hypothesis.
    *   Added new Step 3.10 requiring a Pre-computation Verification Summary checklist, including explicit confirmation that all stated hypotheses were verified as required by 3.4.1.b before proceeding to edits.
    *   Added an explicit self-correction trigger in Step 4.7 to STOP if the mandatory verification reporting for a hypothesis was missed.
    *   (Addresses specific failure where AI acted on an unverified hypothesis regarding code location, leading to persistent errors).

*   **[AI_CODING_PROCESS.md v0.1.24]** Added Configuration Usage Impact Check (Step 3.4.1.f): Mandated searching for and verifying usage locations when configuration values are added, removed, or changed.

*   **[AI_CODING_PROCESS.md v0.1.23]** Added Explicit Trade-off Presentation for Logic Changes (Step 3.4.1.e): Mandated that when simplification/unification alters execution conditions, the AI must explicitly present the trade-off between the simpler/altered plan and a potentially more complex/preserving plan, requesting user guidance before proceeding.

*   **[AI_CODING_PROCESS.md v0.1.22]** Clarified Justification for Logic Changes (Step 3.4.1.e): Enhanced requirement to explicitly justify changes where refactoring (e.g., simplification/unification) alters the *conditions* under which logic executes, even if the core behavior seems preserved. Added AI responsibility to proactively identify and report these subtle shifts.

*   **[AI_CODING_PROCESS.md v0.1.21]** Refined Pre-Edit Confirmation Examples (Step 4.2): Updated the example Pre-Edit Confirmation Statements to explicitly include reporting the outcome of the Logic Preservation check (Step 3.4.1.e) when applicable, improving visibility of this check before applying edits.

*   **[AI_CODING_PROCESS.md v0.1.20]** Enhanced Emphasis (Step 3.4.1.e), removed redundant changelog: Changed prefix from `MANDATORY` to `CRITICAL MANDATORY` for the logic preservation step to underscore its importance. Removed the embedded changelog from `AI_CODING_PROCESS.md`.

*   **[AI_CODING_PROCESS.md v0.1.19]** Added Logic Preservation Checks: Added step `3.4.1.e` and enhanced steps `3.4.1.b` and `4.5.b` in `AI_CODING_PROCESS.md` to explicitly require documenting existing logic and planning/verifying its preservation during refactoring/replacement tasks.

## 2025-05-03

*   **[AI_CODING_PROCESS.md v0.1.18]** Refined manual edit request procedure: Updated the logic for requesting manual edits after repeated tool failures. Now requires performing a final `read_file` check to confirm the file's *actual* state before generating the manual edit request, addressing cases where the tool might incorrectly report failure.

*   **[AI_CODING_PROCESS.md v0.1.17]** Strengthened impact analysis, added framework check, etc.: Mandated broader codebase search and consideration of all layers when refactoring core components. Added new requirement to verify signature/invocation compatibility with frameworks. Emphasized verifying interface consistency. Modified pre-deletion check logic. Added optional smoke test.

*   **[AI_CODING_PROCESS.md v0.1.16]** Increased context for manual edits: Updated the manual edit request procedure to explicitly require including sufficient surrounding context (e.g., 5-10 lines before and after).

*   **[AI_CODING_PROCESS.md v0.1.15]** Added manual edit request procedure detail: Clarified that manual edit requests MUST include a complete copy-paste code block.

*   **[AI_CODING_PROCESS.md v0.1.14]** Added key principle note: Included prominent note emphasizing strict adherence to verification steps.

*   **[AI_CODING_PROCESS.md v0.1.13]** Added mandatory adherence checkpoint (Step 5): Introduced final checkpoint requiring confirmation that all steps were executed.

*   **[AI_CODING_PROCESS.md v0.1.12]** Suggested library doc check: Added suggestion to check library docs when verifying assumptions.

*   **[AI_CODING_PROCESS.md v0.1.11]** Added numbering to Step 3: Introduced explicit numbering to sub-points for clarity.

*   **[AI_CODING_PROCESS.md v0.1.10]** Strengthened import validation: Mandated explicit reporting of path/symbol validation outcome for imports pre-edit and re-verification post-edit.

*   **[AI_CODING_PROCESS.md v0.1.9]** Mandated explicit reporting for checks: Updated process to require AI assistant to explicitly state performance and outcome of mandatory checks.

*   **[AI_CODING_PROCESS.md v0.1.8]** Strengthened post-edit diff verification logic.
