# AI Assistant - Standard Plan Writing Process

**ATTENTION AI ASSISTANT: This document outlines a general mandatory process you MUST follow when asked to *generate* an implementation plan. This process ensures plans are robust, context-aware, and proactively address potential integration issues *before* execution begins. It complements the project-specific plan execution process document (if one exists).**

## CRITICAL REQUIREMENT: Deep Code Comprehension & Pitfall Avoidance

**A fundamental requirement for generating accurate and useful plans is that the AI Assistant MUST perform deep reading and develop a thorough understanding of all relevant code files identified in the scope (Step 1.2). Superficial analysis, relying on assumptions, or pattern-matching without verification against the actual codebase is unacceptable and leads to flawed plans.** This deep reading MUST focus on comprehending not just syntax, but also:

*   The **specific logic and algorithms** implemented.
*   The **data structures** used (models, DTOs).
*   The **roles and relationships** between different components (classes, functions, modules).
*   The **full execution paths and side effects** of existing logic being considered for modification (as detailed in Step 1.3).

**Known Planning Pitfalls (derived from `DOCS/NOTES/ai_plan_writing_pitfalls.md`):**

Past failures in planning have often stemmed from:

1.  **Goal Fixation/Top-Down Bias:** Designing the target state before fully understanding the *current* state.
2.  **Pattern Matching Over Specific Analysis:** Applying generic code patterns instead of analyzing the *specific* implementation details of this project.
3.  **Shallow Verification/Component Isolation:** Confirming component *existence* but failing to understand their *roles and relationships*.
4.  **Incomplete Dependency Tracing/Synthesis:** Not fully tracing how existing components interact and solve relevant problems *together*.

**Mitigation via Process:** The subsequent steps in this planning process, particularly the rigorous analysis and verification mandated in **Phase 1 (Step 1.3)** and **Phase 2 (Steps 2.2, 2.4.b, 2.9, 2.10)**, are specifically designed to enforce deep code comprehension and guard against these pitfalls. Adherence is mandatory.

**Goal:** To improve plan quality by incorporating rigorous upfront analysis of the codebase, standards, and potential dependencies, minimizing errors during subsequent implementation.

---

## Plan Writing Workflow

When responding to user requests asking you to create an implementation plan, you **MUST** integrate the following steps:

### 1. Initial Request Analysis & Context Gathering

*   **Trigger:** Receiving a request to "write a plan for X".
*   **Action:**
    1.  **Acknowledge & Confirm Standards:** State understanding of the request and confirm awareness of the project's coding standards (e.g., as defined in a standards document provided by the user or found in the workspace). *Example: "Acknowledged. I will create a plan for X, adhering to the project's coding standards."*
    2.  **Identify Scope & Key Modules:** Based on the request, identify the primary files, modules, or subsystems likely to be affected. Use tools (`file_search`, `list_dir`) if the scope is broad or involves unfamiliar areas.
    3.  **Read Current State:** **CRITICAL:** *Before* proposing any specific steps, **MUST** read the relevant sections (or potentially full files if small and central) of the key modules identified in 1.2 using `read_file`. **State explicitly** which files/sections were read. *Purpose: To base the plan on verified current code, not assumptions.* *Example: "Reading current state of `module_a.py` (functions foo, bar) and `module_b.py` (class Baz) to inform planning."*
        *   **NEW REQUIREMENT:** When the scope involves refactoring existing logic blocks, 'Reading Current State' **MUST** include explicitly documenting the **core computational goal and high-level algorithm/decision points, including all significant conditional branches, the specific conditions triggering them, and the distinct logic executed within each branch** of the logic being targeted for replacement. **This analysis MUST trace the complete execution path within the targeted function/method for all significant conditional branches (e.g., based on flags like `is_reference`, error states, specific data conditions) to identify all potential outcomes and side effects (e.g., specific return values, exceptions raised, repository calls, logging actions, external interactions) associated with the logic being replaced.**
        *   **NEW REQUIREMENT:** Analysis **MUST** also include explicitly documenting the **current roles and relationships** of the key existing components identified (e.g., how `OrmGame` currently links providers to canonical teams/leagues). Avoid designing target architecture elements until this existing structure is understood and documented.

### 2. Core Analysis & Plan Drafting

*   **Trigger:** After gathering context from the current codebase (Step 1.3).
*   **Action:** Draft the plan steps while performing the following analysis:

    ---
    **PLANNING PRINCIPLE REMINDER: PRIORITIZE SPECIFIC CODE OVER GENERIC PATTERNS**

    While general design patterns are useful background, **your primary guide MUST be the specific patterns, structures, and logic discovered in *this* codebase** during Step 1.3. Avoid imposing generic patterns if the existing code provides a viable (even if imperfect) alternative structure that can be leveraged or refactored. Base your plan on verified specifics, not generalized assumptions.
    ---

    ---
    **MANDATORY DEEP ANALYSIS FOR REFACTORING (Steps 1.3, 2.4.b, 2.9):**

    When planning involves replacing or significantly altering existing code, **SUPERFICIAL ANALYSIS IS UNACCEPTABLE**. The process steps demanding analysis of existing logic, mapping changes, and verification (esp. 1.3, 2.4.b, 2.9) **MUST** be executed with extreme rigor. This includes **meticulously tracing all execution paths**, identifying **all conditional logic**, and understanding **all side effects** (database interactions, logging, API calls, etc.) of the *original* code. The plan **MUST** explicitly address how *each* of these aspects is preserved or justifiably altered. **Failure to perform this deep analysis before proposing changes is a critical process violation.**
    ---

    1.  **Verify Standards Alignment:** For each major part of the plan, explicitly reference the relevant principle or section from the project's standards document it adheres to. *Example: "Step 3 aligns with project standards (Error Handling section) by using central exceptions."*
    2.  **Analyze & Align with Existing Patterns:** **CRITICAL:** Identify and incorporate established patterns from the codebase. **MUST** verify these patterns using tools (`read_file`, `grep_search`) on relevant files (identified in 1.2/1.3). Refer to any provided architecture/conventions document for guidance. Document findings/alignment in the plan draft. **Focus not just on identifying patterns in isolation, but on understanding **how they interact and relate to each other** to form the current system's behavior.** Check for patterns like:
        *   *Dependency Injection:* How are dependencies provided? Plan **MUST** follow the established DI pattern (e.g., via a central container, constructors).
        *   *Error Handling:* Where and how are custom exceptions defined (e.g., in a central `errors` module)? Plan **MUST** use/extend central exceptions appropriately. Avoid defining local exceptions if a suitable central one exists or can be added.
        *   *Domain Model Usage:* How are domain models/entities defined and used (e.g., in a `domain` package)? Plan **MUST** leverage existing domain models/enums correctly. Avoid duplicating domain logic.
        *   *Configuration Access:* How is configuration accessed (e.g., injected config objects, global access)? Plan **MUST** use the established method.
        *   *Logging:* What logging framework and patterns are used? Plan **MUST** integrate logging consistently.
        *   *Shared Utilities/Logic:* Are there central utility modules or shared business logic functions (e.g., for mapping, validation, calculations)? Plan **MUST** utilize or extend these where applicable instead of duplicating functionality.
        *   *Other relevant patterns...*
    3.  **Perform Dependency Validation Checklist (MUST Perform & State Results):** Before finalizing steps involving new dependencies or structural changes:
        *   [ ] **Adding Imports:** If planning to add `from module_a import X` into `module_b`:
            *   [ ] Path Validation: Confirm `module_a`'s path exists and is valid relative to `module_b` (`file_search`, `list_dir`).
            *   [ ] Symbol Validation: Confirm symbol `X` exists within `module_a` (`read_file`, `grep_search`).
            *   [ ] Circularity Check: Confirm `module_a` doesn't import `module_b` (directly or indirectly) (`read_file`, `grep_search`).
            *   *Plan Impact:* If any check fails, **MUST NOT** propose the direct import. Propose an alternative (refactor, move symbol, use DI, adjust interfaces).
        *   [ ] **Moving/Renaming:** Plan **MUST** include steps to:
            *   [ ] Identify *all* potential call sites/imports (`grep_search`).
            *   [ ] Verify findings and list affected locations.
            *   [ ] Explicitly remove the original file/symbol after all references are updated.
    4.  **Ensure Clean, Forward-Looking Solutions:**
        **NEW Step 2.3.1: Generate Dependency Validation Summary (After Step 2.3 Checks):** Before proceeding, **MUST** generate a **Dependency Validation Summary**. This confirms key checks from Step 2.3 were performed. Structure using the following format:
            *   `[x/-] 1. Import Path/Symbol Checks:` *[Narrative confirming 2.3[ ] (Adding Imports - Path/Symbol Validation) performed for relevant imports. Mark `[x]` if done, `[-]` if N/A.]*
            *   `[x/-] 2. Import Circularity Checks:` *[Narrative confirming 2.3[ ] (Adding Imports - Circularity Check) performed for relevant imports. Mark `[x]` if done, `[-]` if N/A.]*
            *   `[x/-] 3. Move/Rename Impact Checks:` *[Narrative confirming 2.3[ ] (Moving/Renaming - Identify/Verify) performed if applicable. Mark `[x]` if done, `[-]` if N/A.]*
            *   `[x] 4. Confirmation:` Dependency validation summary complete. *(This point is always marked `[x]` when the summary is generated).*
        *   **Refactoring Verification:** For plans involving refactoring, **MUST** include a step to verify old code/files are fully removed (`grep_search`, `file_search`).
        *   **NEW (2.4.b): Document & Justify Logic Changes (Mandatory for Refactoring):** When planning refactoring that replaces existing logic blocks:
            *   [ ] Briefly document the essential existing logic flow/behavior (based on Step 1.3 analysis) being replaced.
            *   [ ] **MUST** explicitly state in the plan whether the proposed new logic *preserves* or *intentionally changes/simplifies* the documented existing flow.
            *   [ ] If the flow changes, **MUST** provide justification within the plan step.
            *   [ ] **NEW:** Have I documented the **algorithm/key decision points** of the existing logic being replaced (as per Step 1.3)?
            *   [ ] **NEW:** Does the proposed new logic explicitly replicate, replace, or justifiably alter **each** of those key decision points/algorithmic steps?
            *   [ ] **NEW:** Have all potential outcomes and side-effects (returns, exceptions, DB writes, logging, etc.) of the original logic block been identified and explicitly addressed (preserved or justifiably altered) in the plan's mapping?
            *   *Plan Impact:* If the flow changes, **MUST** provide justification within the plan step. Furthermore, the plan **MUST** explicitly include a point-by-point mapping stating how EACH significant conditional branch, **its associated logic, and its resulting outcomes/side effects** (as documented in Step 1.3) is handled by the proposed new logic (e.g., 'Existing condition X is preserved/replaced by mechanism Y', 'Existing branch Z logic is intentionally removed because...').
            *   **NEW VERIFICATION CHECK:** Verification **MUST** explicitly include confirming that the plan **correctly leverages the existing component roles and relationships** (as documented in Step 1.3, e.g., using `OrmGame` correctly) or provides clear justification for deviation. Check: Does the plan reuse relevant existing structures or unnecessarily invent parallel ones?
        *   **(Define Mandatory Block Text Here - See Step 3.4 for where to place it)**
            ```markdown
            ---
            **CRITICAL IMPLEMENTATION GUIDELINE: NO DEFAULTS/FALLBACKS**

            **Implementer MUST strictly adhere to the explicit error handling specified in each plan step.** DO NOT introduce default values (e.g., `0`, `None` where not explicitly allowed, `'Unknown'`, `datetime.now()`) or fallbacks for missing, invalid, or unmappable data unless that fallback mechanism is *explicitly defined and justified within the relevant plan step*. The standard procedure is to log errors/warnings and skip records/raise exceptions as detailed. Failure to adhere compromises data integrity.
            ---
            ```
    5.  **Define Clear, Actionable Steps:** Break down the proposed implementation into numbered, sequential steps. Each step should be specific (files, functions, actions). Note: Code modification steps implicitly require adherence to the execution process verification sequence (e.g., `AI_CODING_PROCESS.md` Steps 4.1, 4.2).
    6.  **Identify Open Questions / Risks:** Note ambiguities, design decisions, or potential risks.
    7.  **Multi-Part Plans:** For distinct parts, **MUST** include explicit "Action: Pause execution and notify USER..." instruction at the end of each intermediate part.
    8.  **Include Testing Considerations:** **MUST** include a dedicated section suggesting relevant tests (unit, integration) and identifying focus areas/edge cases.
    9.  **Verify Plan Assumptions vs. Implementation:** **CRITICAL:** Before proceeding to Step 3 (Review), **MUST** cross-check drafted plan steps against the *actual* codebase implementation details (`read_file`, `grep_search`). Verify assumptions about signatures, class structures, models, **key logic flows**, arguments/returns, and other interactions. If discrepancies found, **MUST** update the plan to reflect reality. **State explicitly** that this verification was performed. *Example: "Verified plan steps against `service.py`; updated Step X to match actual method signature and logic flow."*
        *   **NEW EMPHASIS:** Verification **MUST** include confirming that the algorithmic intent and key decision points of the original logic (as documented per Step 2.4.b) are correctly handled (preserved or justifiably changed) by the proposed plan steps. Verification **MUST** also include confirming that the point-by-point logic mapping from Step 2.4.b accurately reflects the implementation details and behavioral outcomes of both the original code section and the proposed changes, based on tool analysis (`read_file`, `grep_search`). **Verification MUST also explicitly confirm that the handling of all documented outcomes and side-effects (returns, exceptions, DB writes, etc.) from the original logic is correctly reflected in the proposed changes based on code analysis.** Verification is not just structural but also functional/algorithmic for each documented conditional path.
        *   **NEW VERIFICATION CHECK:** Verification **MUST** explicitly include confirming that the plan **correctly leverages the existing component roles and relationships** (as documented in Step 1.3, e.g., using `OrmGame` correctly) or provides clear justification for deviation. Check: Does the plan reuse relevant existing structures or unnecessarily invent parallel ones?

    **NEW Step 2.10: Generate Logic Analysis & Verification Summary (End of Phase 2):** Before proceeding to Phase 3 (Plan Review), **MUST** generate a **Logic Analysis & Verification Summary**. This confirms key logic checks were performed if applicable. Structure using the following format:
        *   `[x/-] 1. Existing Logic Documented:` *[Narrative confirming Step 1.3 analysis (including conditional branches **and component roles/relationships**) was performed and documented. Mark `[x]` if refactoring done, `[-]` if N/A.]*
        *   `[x/-] 2. Logic Change Mapping:` *[Narrative confirming Step 2.4.b mapping (detailing how new logic handles each documented existing branch **and its outcomes/side-effects**) was performed and included in the plan. Mark `[x]` if refactoring done, `[-]` if N/A.]*
        *   `[x/-] 3. Implementation Verification:` *[Narrative confirming Step 2.9 verification (cross-checking the mapping against the codebase) was performed. Mark `[x]` if refactoring done, `[-]` if N/A.]*
        *   `[x/-] 4. Existing Structure Integration:` *[Narrative confirming the plan was checked (Step 2.9) to ensure it correctly leverages existing component roles/relationships (from Step 1.3) or justifies deviations. Mark `[x]` if applicable, `[-]` if N/A.]*
        *   `[x] 5. Confirmation:` Logic analysis & verification summary complete. *(This point is always marked `[x]` when the summary is generated).*

### 3. Plan Review & Presentation

*   **Trigger:** After drafting plan steps (Step 2) **and verifying against implementation (Step 2.9) and generating summaries (Step 2.3.1, 2.10)**.
*   **Action:**
    1.  **Self-Review Plan:** Reread draft. Does it address the request? Is it consistent with analysis (Standards, Patterns, Dependencies)? Is it logical and clear?
    2.  **Final Checks:** Ensure all mandatory analysis (Step 2) and verification checks (esp. 2.3, 2.4 checks, 2.9) are done and reflected. Ensure necessary imports, config, DI updates are planned.
    3.  **Critical Perfection Review:** **CRITICAL:** Perform final review: "Is this the *most complete and correct* plan possible within scope? Are there *any* remaining temporary fixes, placeholders, inconsistencies, or suboptimal solutions that could be addressed now?" Revise if imperfections found.
    4.  **Present Plan:** Output the finalized plan clearly and formatted.
        *   **Mandatory Plan Structure & Content:** Generated plans **MUST** begin with the following sections, in this exact order, using the specified content:
            1.  **`!! MANDATORY PROCESS ADHERENCE !!` Block:**
                *   *Content:* Must prominently state that all implementation steps MUST strictly follow the procedures defined in the project's execution process document (e.g., `AI_CODING_PROCESS.md`). It MUST specify that adherence requires initializing the process by *reading* the process document (`AI_CODING_PROCESS.md`), the standards checklist (`STANDARDS.md`), and the detailed architecture standards (`code_architecture_standard.md`) before proceeding.
            2.  **`**CRITICAL IMPLEMENTATION GUIDELINE: NO DEFAULTS/FALLBACKS**` Block:**
                *   *Content:* Use the exact markdown text defined in Step 2.4.
            3.  **`PLANNING PRINCIPLE REMINDER: VERIFY ASSUMPTIONS` Block:**
                *   *Content:* Must remind the implementer that all assumptions made during planning MUST be explicitly stated and immediately verified using tools before proceeding with dependent steps, referencing the relevant execution process step (e.g., `AI_CODING_PROCESS.md` Step 3.4.1.b). Example:
                  ```markdown
                  ---
                  **PLANNING PRINCIPLE REMINDER: VERIFY ASSUMPTIONS**

                  **All hypotheses/assumptions made during planning (e.g., about interfaces, data structures, existing logic) MUST be explicitly stated and immediately verified using tools (`read_file`, `grep_search`) *before* proceeding with dependent steps, as per `AI_CODING_PROCESS.md` Step 3.4.1.b. Do not build upon unverified information.**
                  ---
                  ```
            4.  **`PLAN VERIFICATION CONFIRMATION (Step 2.9)` Block:**
                *   *Content:* Must explicitly confirm that the plan's steps, assumptions, and logic handling have been verified against the current codebase implementation as required by Step 2.9. Example:
                  ```markdown
                  ---
                  **PLAN VERIFICATION CONFIRMATION (Step 2.9)**

                  **Verification Complete:** This plan's steps, assumptions about existing structures, and handling of original logic (per Step 2.4.b, **including explicit verification of the point-by-point mapping for documented conditional branches per Steps 2.4.b/2.9**) have been cross-checked against the current codebase implementation (`read_file`, `grep_search`) as mandated by Step 2.9. [Optional: Briefly note if significant updates were made based on verification, e.g., "Minor adjustments were made to Step X based on verification."]
                  ---
                  ```
        *   Include the numbered plan steps, testing considerations, and open questions/risks sections after the mandatory header blocks.

---

**Reference:** Always refer to the specific project's standards document (if provided) for definitive standards. Refer to the project's plan execution process document (if provided, e.g., `AI_CODING_PROCESS.md` or similar) for the process of *executing* the plan and applying edits. Use any provided architecture/conventions document for context on existing patterns. This document governs the general *creation* of the plan itself.
