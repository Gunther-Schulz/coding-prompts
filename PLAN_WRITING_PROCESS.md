# AI Assistant - Standard Plan Writing Process

**ATTENTION AI ASSISTANT: This document outlines a general mandatory process you MUST follow when asked to *generate* an implementation plan. This process ensures plans are robust, context-aware, and proactively address potential integration issues *before* execution begins. It complements the project-specific plan execution process document (e.g., `AI_CODING_PROCESS.md`) and requires awareness of the project's `PROJECT_STANDARDS.md` and language/framework-specific `PROJECT_ARCHITECTURE.md`.**

## CRITICAL REQUIREMENT: Deep Code Comprehension & Pitfall Avoidance

**A fundamental requirement for generating accurate and useful plans is that the AI Assistant MUST perform deep reading and develop a thorough understanding of all relevant code files identified in the scope (Step 1.2). Superficial analysis, relying on assumptions, or pattern-matching without verification against the actual codebase is unacceptable and leads to flawed plans.** This deep reading MUST focus on comprehending not just syntax, but also:

*   The **specific logic and algorithms** implemented.
*   The **data structures** used (models, DTOs).
*   The **roles and relationships** between different components (classes, functions, modules).
*   The **full execution paths and side effects** of existing logic being considered for modification (as detailed in Step 1.3).

**Known Planning Pitfalls (derived from `learning_resources/ai_plan_writing_pitfalls.md`):**

Past failures in planning have often stemmed from:

1.  **Goal Fixation/Top-Down Bias:** Designing the target state before fully understanding the *current* state.
2.  **Pattern Matching Over Specific Analysis:** Applying generic code patterns instead of analyzing the *specific* implementation details of this project.
3.  **Shallow Verification/Component Isolation:** Confirming component *existence* but failing to understand their *roles and relationships*.
4.  **Incomplete Dependency Tracing/Synthesis:** Not fully tracing how existing components interact and solve relevant problems *together*.

**Mitigation via Process:** The subsequent steps in this planning process, particularly the rigorous analysis and verification mandated in **Phase 1 (Step 1.3)** and **Phase 2 (Steps 2.2, 2.7.b, and 2.12)**, are specifically designed to enforce deep code comprehension and guard against these pitfalls. Adherence is mandatory.

**Goal:** To improve plan quality by incorporating rigorous upfront analysis of the codebase, standards, and potential dependencies, minimizing errors during subsequent implementation.

**Version: 1.6** (Further enhancements for AI coding synergy: Added new mandatory plan output sections (Existing Functionality Survey, Applied Standards/Patterns, Risks/Mitigations). Updated Phase 2 to prepare this data and for more precise edit location hints. Updated Key Impact Summary structure. Renumbered Phase 2 again.)

---

## Plan Writing Workflow

When responding to user requests asking you to create an implementation plan, you **MUST** integrate the following steps:

### 1. Initial Request Analysis & Context Gathering

*   **Trigger:** Receiving a request to "write a plan for X".
*   **Action:**
    1.  **Acknowledge & Confirm Standards:** State understanding of the request and confirm awareness of the project's coding standards (e.g., `PROJECT_STANDARDS.md`) and specific technical guidelines (`PROJECT_ARCHITECTURE.md`). *Example: "Acknowledged. I will create a plan for X, adhering to the project's `PROJECT_STANDARDS.md` and `PROJECT_ARCHITECTURE.md`."*
    2.  **Identify Scope & Key Modules:** Based on the request, identify the primary files, modules, or subsystems likely to be affected. Use tools (`file_search`, `list_dir`) if the scope is broad or involves unfamiliar areas.
    3.  **Read Current State & Perform Initial Analysis:** **CRITICAL:** *Before* proposing any specific steps, **MUST** perform the following:
        *   `a.` **Read Files:** Read the relevant sections (or potentially full files if small and central) of the key modules identified in 1.2 using `read_file`. **State explicitly** which files/sections were read. *Purpose: To base the plan on verified current code, not assumptions.* *Example: "Reading current state of `module_a.ext` (functions foo, bar) and `module_b.ext` (class Baz)."*
        *   `b.` **Analyze Existing Logic/Side Effects (If Refactoring):** When the scope involves refactoring existing logic blocks, **MUST** explicitly document:
            *   **Core computational goal** of the logic being replaced.
            *   **High-level algorithm/decision points:** Identify all significant conditional branches, the specific conditions triggering them, and the distinct logic executed within each branch.
            *   **Complete Execution Paths & Side Effects:** Trace the complete execution path for all significant conditional branches to identify **all potential outcomes and side effects** (specific return values, exceptions raised, data store interactions, logging actions, external interactions) associated with the logic being replaced.
        *   `c.` **Analyze Existing Component Roles/Relationships:** **MUST** explicitly document the **current roles and relationships** of the key existing components identified (e.g., how data model `X` currently links component `Y` to `Z`). Avoid designing target architecture elements until this existing structure is understood and documented.
    4.  **Output Existing State Analysis Summary:** **After** completing Step 1.3, **MUST** output a brief summary of the findings before proceeding to Step 2. *Example: "**Existing State Summary:** `func_to_refactor` handles cases A (returns X, logs Y) and B (raises ZError). It relies on `component_P` for data retrieval and `component_Q` for formatting."

### 2. Core Analysis & Plan Drafting

*   **Trigger:** After gathering context and summarizing the current state (Step 1).
*   **Action:** Draft the plan steps while performing the following analysis:

    ---
    **PLANNING PRINCIPLE REMINDER: PRIORITIZE SPECIFIC CODE OVER GENERIC PATTERNS (See Failure Mode 2)**

    While general design patterns are useful background, **your primary guide MUST be the specific patterns, structures, and logic discovered in *this* codebase** during Step 1.3. Avoid imposing generic patterns if the existing code provides a viable (even if imperfect) alternative structure that can be leveraged or refactored. Base your plan on verified specifics, not generalized assumptions.
    ---

    ---
    **MANDATORY DEEP ANALYSIS FOR REFACTORING (Steps 1.3.b, 2.7.b, 2.12):**

    When planning involves replacing or significantly altering existing code, **SUPERFICIAL ANALYSIS IS UNACCEPTABLE**. The process steps demanding analysis of existing logic, mapping changes, and verification (esp. 1.3.b, 2.7.b, 2.12) **MUST** be executed with extreme rigor. This includes **meticulously tracing all execution paths**, identifying **all conditional logic**, and understanding **all side effects** (database interactions, logging, API calls, etc.) of the *original* code. The plan **MUST** explicitly address how *each* of these aspects is preserved or justifiably altered. **Failure to perform this deep analysis before proposing changes is a critical process violation.**
    ---

    `2.1.` **Verify Standards Alignment:** For each major part of the plan, explicitly reference the relevant principle or section from the project's standards document (`PROJECT_STANDARDS.md`) it adheres to. **MUST cite the specific section/pattern name(s) where applicable.** *(Note for AI: If `PROJECT_STANDARDS.md` or `PROJECT_ARCHITECTURE.md` are not explicitly provided in the current context, state this. Then, base your alignment on general software design principles and any architectural patterns observed directly in the existing codebase during Step 1.3 and 2.2.)* *Example: "Step 3 aligns with `PROJECT_STANDARDS.md` (Section 5, Error Handling) by using central exceptions."* The key standards and architectural patterns identified and aligned with in this step should be collated for presentation in the 'Key Standards & Architectural Patterns Applied' section of the final plan.
    `2.2.` **Analyze & Align with Existing Patterns:** **CRITICAL:** Identify and incorporate established patterns from the codebase. **MUST** verify these patterns using tools (`read_file`, `grep_search`) on relevant files (identified in 1.2/1.3). Refer to the project's `PROJECT_ARCHITECTURE.md` for guidance. Document findings/alignment in the plan draft. **Focus not just on identifying patterns in isolation, but on understanding **how they interact and relate to each other** to form the current system's behavior.** *(Reminder: Address Failure Mode 2 - Analyze specific existing patterns, don't assume generic ones apply.)* Check for patterns like:
        *   *Dependency Injection:* How are dependencies provided? Plan **MUST** follow the established DI pattern (e.g., via a central container, constructors).
        *   *Error Handling:* Where and how are custom exceptions defined (e.g., in a central `errors` module)? Plan **MUST** use/extend central exceptions appropriately. Avoid defining local exceptions if a suitable central one exists or can be added.
        *   *Domain Model Usage:* How are domain models/entities defined and used (e.g., in a `domain` package)? Plan **MUST** leverage existing domain models/enums correctly. Avoid duplicating domain logic.
        *   *Configuration Access:* How is configuration accessed (e.g., injected config objects, global access)? Plan **MUST** use the established method.
        *   *Logging:* What logging framework and patterns are used? Plan **MUST** integrate logging consistently.
        *   *Shared Utilities/Logic:* Are there central utility modules or shared business logic functions (e.g., for mapping, validation, calculations)? Plan **MUST** utilize or extend these where applicable instead of duplicating functionality.
        *   *Other relevant patterns defined in `PROJECT_ARCHITECTURE.md`...*
    `2.3.` **Survey Existing Relevant Functionality:** Based on the request and initial analysis (Steps 1.2, 1.3, 2.2), perform a targeted survey for existing codebase functionalities that are directly relevant to the planned changes or could be reused. For each key area surveyed: document the functionality/concept searched, key files/modules investigated, and the outcome (e.g., 'to be reused as-is', 'to be modified per plan step X', 'no suitable existing logic found'). This information is for the 'Existing Relevant Functionality Survey' section of the plan output.
    `2.4.` **Perform Dependency Validation Checklist (MUST Perform & State Results):** Before finalizing steps involving new dependencies or structural changes:
        *   [ ] **Adding Dependencies:** If planning for module `B` to reference symbol `X` from module `A`:
            *   [ ] Path Validation: Confirm module `A`'s path exists and is valid relative to module `B` (`file_search`, `list_dir`).
            *   [ ] Symbol Validation: Confirm symbol `X` is accessible (e.g., exported) from module `A` (`read_file`, `grep_search`).
            *   [ ] Circularity Check: Confirm module `A` doesn't depend on module `B` (directly or indirectly) (`read_file`, `grep_search`).
            *   *Plan Impact:* If any check fails, **MUST NOT** propose the direct reference. Propose an alternative (refactor, move symbol, use DI, adjust interfaces).
        *   [ ] **Moving/Renaming:** If moving/renaming symbols/files, plan **MUST** include steps to:
            *   [ ] Identify *all* potential call sites/references (`grep_search`).
            *   [ ] Verify findings and list affected locations.
            *   [ ] Explicitly remove the original file/symbol after all references are updated.
    `2.5.` **Structure Key Impact Summary Details:** Consolidate findings from impact analysis (Steps 1.3, 2.2, 2.4) into a structured summary for the 'Key Impact Summary' section of the plan. This MUST include: Directly Modified Files/Components, Key Identified Upstream Dependencies (Callers/Consumers Potentially Affected), Key Identified Downstream Dependencies (Callees/Services Relied Upon), and Circular Dependency Check Outcome (for new inter-module dependencies).
    `2.6.` **Collate Key Assumptions for Re-Verification:** During the analysis in Steps 1.3, 2.1, 2.2, 2.3, 2.4 and 2.5, the AI planner will have made and verified several assumptions about existing code, interfaces, and dependencies. This step requires collating the *most critical* of these assumptions that the *coding AI (following AI_CODING_PROCESS.md)* should explicitly re-verify before implementing plan steps.
        *   **Action:** Identify and list key assumptions (e.g., about specific API responses, function signatures, module responsibilities, data formats) that, if incorrect, would significantly derail implementation. For each assumption, briefly note its original verification. This list is for inclusion in the final plan output under the heading 'Key Assumptions for Re-Verification'.
    `2.7.` **Ensure Clean, Forward-Looking Solutions:**
        *   `a.` **Refactoring Verification:** For plans involving refactoring, **MUST** include a step to verify old code/files are fully removed (`grep_search`, `file_search`).
        *   `b.` **Document & Justify Logic Changes (Mandatory for Refactoring):** When planning refactoring that replaces existing logic blocks:
            *   [ ] Briefly document the essential existing logic flow/behavior (based on Step 1.3 analysis) being replaced.
            *   [ ] **MUST** explicitly state in the plan whether the proposed new logic *preserves* or *intentionally changes/simplifies* the documented existing flow.
            *   [ ] If the flow changes, **MUST** provide justification within the plan step.
            *   [ ] **NEW:** Have I documented the **algorithm/key decision points** of the existing logic being replaced (as per Step 1.3)?
            *   [ ] **NEW:** Does the proposed new logic explicitly replicate, replace, or justifiably alter **each** of those key decision points/algorithmic steps?
            *   [ ] **NEW:** Have all potential outcomes and side-effects (returns, exceptions, data store writes, logging, etc.) of the original logic block been identified and explicitly addressed (preserved or justifiably altered) in the plan's mapping?
            *   *Plan Impact:* If the flow changes, **MUST** provide justification within the plan step. Furthermore, the plan **MUST** explicitly include a point-by-point mapping stating how EACH significant conditional branch, **its associated logic, and its resulting outcomes/side effects** (as documented in Step 1.3) is handled by the proposed new logic (e.g., 'Existing condition X is preserved/replaced by mechanism Y', 'Existing branch Z logic is intentionally removed because...'). The point-by-point mapping generated here MUST be structured clearly for direct inclusion in the final plan under the 'Logic Preservation Mapping Details' section, serving as a direct guide for `AI_CODING_PROCESS.md` Step 3.4.1.e.
            *   **NEW VERIFICATION CHECK:** Verification **MUST** explicitly include confirming that the plan **correctly leverages the existing component roles and relationships** (as documented in Step 1.3) or provides clear justification for deviation. Check: Does the plan reuse relevant existing structures or unnecessarily invent parallel ones?
    `2.8.` **Document Suggested Analysis Queries:** During impact analysis (especially in Steps 1.3, 2.2, 2.4, 2.5 and 2.7), specific search queries (`grep_search`, `codebase_search`) may have been used to identify dependencies, usage patterns, or potential call sites.
        *   **Action:** If any such queries yielded particularly insightful results or would be valuable for the coding AI to re-run for detailed impact analysis or re-verification (as per `AI_CODING_PROCESS.md` Steps 3.4.1.a, 4.4.2.b.i), list them. This is for inclusion in the final plan output under 'Suggested Impact Analysis Queries'.
    `2.9.` **Define Clear, Actionable Steps:** Break down the proposed implementation into numbered, sequential steps. Each step should be specific (files, functions, actions). Note: Code modification steps implicitly require adherence to the execution process verification sequence (e.g., `AI_CODING_PROCESS.md` Steps 4.1, 4.2). For plan steps involving modifications to complex files or where precise edit location is critical, strive to include explicit textual anchors or unique locational hints within the description of the action (e.g., 'insert after comment X', 'modify function Y immediately following the Z variable declaration'). This aids `AI_CODING_PROCESS.md` Steps 3.8.b and 4.1.
    `2.10.` **Identify Open Questions, Risks, and Plan Mitigations:** Note ambiguities, design decisions, or potential risks. For each significant risk identified, briefly outline how the plan attempts to mitigate it, or what checks are built in. This information is for the 'Potential Risks & Planned Mitigations' section of the plan output. If no extraordinary risks are identified, note that standard implementation risks are addressed by adherence to `AI_CODING_PROCESS.md`.
    `2.11.` **Include Testing Considerations & Critical Edge Cases:** **MUST** include a dedicated section suggesting relevant tests (unit, integration) and identifying focus areas/edge cases, **including briefly how key edge cases will be handled/tested.** In addition to suggesting relevant tests, this step **MUST** explicitly identify and list the 1-3 most critical edge cases (or types of edge cases) that the planned logic must handle correctly. This list is for inclusion in the final plan output under the 'Critical Edge Cases for Implementation' section, directly informing `AI_CODING_PROCESS.md` Step 3.4.1.c.

    `2.12.` **Generate Logic Analysis & Verification Summary (End of Phase 2):** Before proceeding to Phase 3 (Plan Review), **MUST** generate a **Logic Analysis & Verification Summary**. This confirms key logic checks were performed if applicable. Structure using the following format. Mark steps as `[x]` if completed, or `[-] N/A: [brief justification]` if not applicable.
        ```markdown
        **Logic Analysis & Verification Summary:**
            *   `[x/-] 1. Existing Logic Documented:` *[Narrative confirming Step 1.3 analysis (including conditional branches **and component roles/relationships**) was performed and documented. Mark `[x]` if refactoring done, `[-]` if N/A.]*
            *   `[x/-] 2. Logic Change Mapping:` *[Narrative confirming Step 2.7.b mapping (detailing how new logic handles each documented existing branch **and its outcomes/side-effects**) was performed and included in the plan. Mark `[x]` if refactoring done, `[-]` if N/A.]*
            *   `[x/-] 3. Plan-to-Code Alignment Check:` *[Narrative confirming that the proposed logic changes and their mappings (from Step 2.7.b) were checked for consistency and feasibility against the analyzed current codebase state (from Step 1.3). Mark `[x]` if refactoring done, `[-]` if N/A.]*
            *   `[x/-] 4. Existing Structure Integration Check:` *[Narrative confirming the plan was checked (as part of the analysis in Steps 1.3.c and 2.7.b) to ensure it correctly leverages existing component roles/relationships or justifies deviations. Mark `[x]` if applicable, `[-]` if N/A.]*
            *   `[x] 5. Confirmation:` Logic analysis & verification summary complete. *(This point is always marked `[x]` when the summary is generated).*
        ```

### 3. Plan Review & Presentation

---

**Mandatory Guideline Blocks for Plan Output:**
The following guideline blocks **MUST** be included verbatim at the beginning of the presented plan as specified in Step 3.4.

```markdown
---
**CRITICAL IMPLEMENTATION GUIDELINE: NO DEFAULTS/FALLBACKS**

**Implementer MUST strictly adhere to the explicit error handling specified in each plan step.** DO NOT introduce default values (e.g., `0`, null/empty/undefined where not explicitly allowed, `'Unknown'`, current timestamp) or fallbacks for missing, invalid, or unmappable data unless that fallback mechanism is *explicitly defined and justified within the relevant plan step*. The standard procedure is to log errors/warnings and skip records/raise exceptions as detailed. Failure to adhere compromises data integrity.
---
```

```markdown
### Guideline: Verify Assumptions During Implementation

**All hypotheses/assumptions made during planning (e.g., about interfaces, data structures, existing logic) MUST be explicitly stated and immediately verified using tools (`read_file`, `grep_search`) *before* proceeding with dependent steps, as per `AI_CODING_PROCESS.md` Step 3.4.1.b. Do not build upon unverified information.**
```

---

*   **Trigger:** After drafting plan steps (Step 2), performing the analyses and checks within it (esp. 1.3, 2.2, 2.3, 2.4), and generating the Logic Analysis & Verification Summary (Step 2.8).
*   **Action:**
    1.  **Self-Review Plan:** Reread draft. Does it address the request? Is it consistent with analysis (Standards, Patterns, Dependencies)? Is it logical and clear?
    2.  **Final Checks:** Ensure all mandatory analysis (Step 2) and verification checks (esp. 2.3, 2.4 checks, 2.8) are done and reflected. Ensure necessary dependency updates, config, DI updates are planned.
    3.  **Critical Perfection Review:** **CRITICAL:** Perform final review: "Is this the *most complete and correct* plan possible within scope? Are there *any* remaining temporary fixes, placeholders, inconsistencies, or suboptimal solutions that could be addressed now?" Revise if imperfections found.
    4.  **Present Plan:** Output the finalized plan clearly and formatted.
        *   **Mandatory Plan Structure & Content:** Generated plans **MUST** begin with the following sections, in this exact order, using the specified **HEADINGS** and content:
            1.  **`## !! Implementation Process Note !!` Heading & Block:**
                *   *Content:* Must prominently state that all implementation steps MUST strictly follow the procedures defined in the project's execution process document (e.g., `AI_CODING_PROCESS.md`). It MUST specify that adherence requires initializing the process by *reading* the process document (`AI_CODING_PROCESS.md`), the standards checklist (`PROJECT_STANDARDS.md`), and the detailed architecture standards (`PROJECT_ARCHITECTURE.md`) before proceeding.
            2.  **`## Guideline: No Defaults/Fallbacks During Implementation` Heading & Block:**
                *   *Content:* Include the 'CRITICAL IMPLEMENTATION GUIDELINE: NO DEFAULTS/FALLBACKS' block as defined in the 'Mandatory Guideline Blocks for Plan Output' section at the start of Phase 3.
            3.  **`## Guideline: Verify Assumptions During Implementation` Heading & Block:**
                *   *Content:* Include the 'Guideline: Verify Assumptions During Implementation' block as defined in the 'Mandatory Guideline Blocks for Plan Output' section at the start of Phase 3.
            4.  **`## Plan Verification Status` Heading & Block:**
                *   *Content:* Must explicitly confirm that the plan's steps, assumptions, and logic handling have been developed through cross-checking against the current codebase implementation (as detailed in Steps 1.3, 2.2, and 2.7). Example:
                  ```markdown
                  ### Plan Verification Status

                  **Verification Complete:** This plan's steps, assumptions about existing structures, and handling of original logic (per Step 2.7.b, including explicit verification of the point-by-point mapping for documented conditional branches) have been developed through cross-checking against the current codebase implementation (using tools like `read_file`, `grep_search` as detailed in Steps 1.3, 2.2, and 2.7).
                  ```
            5.  **`## Key Impact Summary` Heading & Block:**
                *   *Content:* Must provide a brief, structured summary of the key impacts as prepared in Step 2.5 (covering directly modified components, upstream/downstream dependencies, and circular dependency checks). Example:
                  ```markdown
                  ### Key Impact Summary

                  *   **Key Dependencies:** Plan introduces dependency from `ModuleA` to `ModuleB.funcX`. Verification confirmed validity.
                  *   **Affected Components:** Changes to `ClassY` are expected to affect `ConsumerZ` (update planned in Step N).
                  *   **Core Refactoring:** N/A.
                  ```
            6.  **`## Key Assumptions for Re-Verification (Ref: AI_CODING_PROCESS.md Step 3.4.1.b)` Heading & Block:**
                *   *Content:* Must include the structured list of critical assumptions (and their original planner verification notes) collated in Step 2.4, intended for re-verification by the coding AI.
            7.  **`## Logic Preservation Mapping Details (Ref: AI_CODING_PROCESS.md Step 3.4.1.e)` Heading & Block:**
                *   *Content:* Must include the detailed point-by-point mapping of how new logic preserves or justifiably alters original logic, as prepared in Step 2.7.b. This section is critical if the plan involves refactoring. If no refactoring of existing logic, state "N/A: No existing logic blocks are being refactored."
            8.  **`## Suggested Impact Analysis Queries (For AI_CODING_PROCESS.md Steps 3.4.1.a, 4.4.2.b.i)` Heading & Block:**
                *   *Content:* Must include any specific `grep_search` or `codebase_search` queries documented in Step 2.8 that are recommended for the coding AI to re-run for its own impact analysis or re-verification. If no specific queries were noted as high-value for re-running, state "No specific high-value queries were flagged for re-running."
            9.  **`## Critical Edge Cases for Implementation (Ref: AI_CODING_PROCESS.md Step 3.4.1.c)` Heading & Block:**
                *   *Content:* Must include the list of 1-3 most critical edge cases (or types of edge cases) identified in Step 2.11 that the planned logic must handle correctly.
            10. **`## Existing Relevant Functionality Survey (Ref: AI_CODING_PROCESS.md Step 3.1)` Heading & Block:**
                *   *Content:* Must include the documented survey of existing relevant functionalities from Step 2.3, detailing searches and outcomes (reuse, modify, new).
            11. **`## Key Standards & Architectural Patterns Applied (Ref: AI_CODING_PROCESS.md Step 3.2)` Heading & Block:**
                *   *Content:* Must include the collated list of key `PROJECT_STANDARDS.md` sections and `PROJECT_ARCHITECTURE.md` patterns that the plan adheres to, as prepared in Step 2.1.
            12. **`## Potential Risks & Planned Mitigations` Heading & Block:**
                *   *Content:* Must include the identified significant risks and their planned mitigations from Step 2.10. If no extraordinary risks were identified, state that standard risks are covered by adherence to `AI_CODING_PROCESS.md`.
        *   Include the numbered plan steps, testing considerations, and open questions/risks sections **after** these mandatory header blocks.

---

**Reference:** Always refer to the specific project's standards document (`PROJECT_STANDARDS.md`) for definitive standards. Refer to the project's plan execution process document (e.g., `AI_CODING_PROCESS.md` or similar) for the process of *executing* the plan and applying edits. Use the project's language/framework-specific `PROJECT_ARCHITECTURE.md` document for context on existing patterns and technical guidelines. This document governs the general *creation* of the plan itself.
