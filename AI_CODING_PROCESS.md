# AI Assistant - Standard Coding Process

**ATTENTION AI ASSISTANT: This document outlines the mandatory process you **MUST** follow during general coding interactions to ensure consistent adherence to the project's standards. This process requires adherence to `STANDARDS.md` (Core Principles & Checklist) and the detailed guidelines in the project's specific `code_architecture_standard.md` (Patterns, Structures, Examples for the relevant language/framework). This process complements, but does not replace, the detailed standards themselves. You **MUST** explicitly report the performance and outcome of mandatory checks as specified below.**

**Goal:** To improve consistency and proactively catch deviations from standards by incorporating explicit checks **and reporting** into the workflow, ensuring a strong emphasis on fundamentally robust solutions over quick fixes or workarounds.

**Version: 1.32** (Improved structure and readability of Step 4.C)

**Key Principle:** While this process provides structure, the primary takeaway is that **strict adherence to the existing verification and checking steps is the most crucial factor** in preventing errors and ensuring robust, maintainable code.

---

## General Coding Workflow

When responding to user requests involving code analysis, planning, or modification, you **MUST** integrate the following checks **and explicitly report their execution and outcome in your responses**:

### 1. Confirm Standards Awareness (Initial Step)

*   **Trigger:** At the very beginning of handling any coding-related request.
*   **Action:** Explicitly state that you have read and understood the requirements outlined in `STANDARDS.md` and the project's specific `code_architecture_standard.md`. *Example: "Acknowledged. I have reviewed and will adhere to the principles in `STANDARDS.md` and the detailed guidelines in `code_architecture_standard.md`."*

### 2. Confirm Goal (Recommended)

*   **Trigger:** Before detailed planning, especially for complex or potentially ambiguous requests.
*   **Action:** Briefly restate your understanding of the user's ultimate goal for this change and ask for confirmation.
*   *Example: "To confirm my understanding, the goal is to refactor X to achieve Y, primarily by implementing pattern Z. Is this correct?"*

### 3. Pre-computation Standards Check (Planning Phase)

**NOTE:** Foundational checks (especially Steps 3.4, 3.5, 3.6) take precedence. If analysis reveals underlying issues (robustness, unknown root cause, architectural conflicts), these **MUST** be addressed or explicitly discussed for guidance *before* proceeding with the original, narrower task (e.g., fixing an immediate error symptom). Embrace necessary detours.

*   **Trigger:** Before generating a multi-step plan or proposing a specific code edit (`edit_file` call).
*   **Action:**

    `3.1.` **Search for Existing Logic:** Before planning implementation details (especially for utilities, validation, calculations), perform a preliminary search (`grep_search`, `codebase_search`) for existing implementations of the required functionality (referencing Audit Sections 1.B, 1.C). Briefly document findings (e.g., "Found existing utility `utils.format_date`, will reuse." or "No existing specific validator found, planning new implementation.").

    `3.2.` **Identify Standards & Verify Alignment:** Explicitly identify the Core Principles or specific sections from `STANDARDS.md` **and relevant technical patterns/structures from the project's `code_architecture_standard.md`** that are most pertinent to the request. Briefly state how the proposed plan/edit aligns with these standards. **Confirm the plan prioritizes the simplest, clearest solution that fully meets requirements and adheres to all standards.**
        *   *Example 1 (DI Focus): "Planning to add component X. This requires adhering to the Dependency Injection principles outlined in `STANDARDS.md` and the specific DI configuration guidelines detailed in `code_architecture_standard.md`. The plan involves updating the designated DI configuration module accordingly, which is the standard, simplest approach here."*
        *   *Example 2 (Error Handling Focus): "The plan includes error handling for Y. This follows the Error Handling guidelines in `STANDARDS.md` and the hierarchy examples in `code_architecture_standard.md` (Error Handling section), using the standard logging framework's method for detailed reporting (e.g., logging exceptions with stack traces)."*

    `3.3.` **Check "Work with Facts":** Confirm the plan/edit relies *only* on provided facts, requirements, or verified information. State if clarification is needed. *Example: "Proceeding based on requirement Z. Clarify if other factors apply."*

    `3.4.` **Check "Robust Solutions":** Confirm the plan avoids hacks/workarounds and addresses the root cause.
        **Data Integrity Priority:** When encountering **unexpectedly missing or invalid data** from external sources or between internal components (e.g., missing required fields, type mismatches, unexpected null/empty values where data is expected based on verified interfaces):
            *   **Default Action:** Treat this as a **critical error** requiring investigation. The default plan should be to **log the issue clearly and raise an appropriate, specific error** (e.g., `ConversionError`, `ValidationError`, `ValueError`, or a project-defined error) to halt processing for the affected record/item, rather than implementing a fallback or default value.
            *   **Justification:** This prioritizes surfacing potential data quality issues or integration problems for root cause analysis over masking them with potentially inaccurate workarounds.
            *   **Deviation (Requires Explicit Approval):** Implementing a fallback or using default values (e.g., current timestamp for a missing timestamp, `0` for a missing required number) **MUST** be treated as a **deviation requiring explicit user approval** following the procedure outlined in **Step 3.5.4 (Handle Necessary Workarounds)**, clearly stating the data integrity compromise.

        `3.4.1.` **Analyze Impact:** Identify potential impacts on existing functionality.

            `3.4.1.a Identify Affected Call Sites:` For changes modifying interfaces, paths, or symbol names, **MUST** use tools (`grep_search`) to identify *all* potentially affected call sites/imports/references codebase-wide; list these in the plan.
            `3.4.1.b Enhanced Scope for Core Refactoring:` When refactoring core components (DI container, base configurations, widely used services/models), impact analysis **MUST** explicitly consider potential consumers across *all* application layers (e.g., CLI commands, helpers, other services) even if not directly imported/referenced. Broader search strategies (`grep` for specific attribute access patterns, semantic search for conceptual usage) **MUST** be employed beyond simple import/reference checks.
            `3.4.1.c Circular Dependency Check:` **MUST** When adding a new dependency reference between modules (e.g., module `B` referencing symbol `X` from module `A`), explicitly state that you are checking for circular dependencies, perform the check (examining dependencies in module `A`), and report the outcome (e.g., "Checking for circular dependency between `module_b` and `module_a`... Confirmed: `module_a` does not depend on `module_b`.") before proceeding. State the outcome of dependency checks in the plan.
            `3.4.1.d Data Representation Impact:` **NOTE:** For changes affecting data representation (e.g., ID vs Name, format changes) or core interfaces/models, impact analysis **MUST** explicitly consider potentially related components across different layers (e.g., ORM definitions, Mappers, Exporters, Consumers, Tests).

        `3.4.1.e - Mandatory Hypothesis Identification & Immediate Verification:** **CRITICAL:** The plan **MUST** explicitly identify *all* necessary hypotheses (termed 'Assumptions' for this process) the proposed change relies upon (e.g., structure of external API responses, existence/interface of other internal modules/classes/functions, configuration values, data formats). **For each assumption, the following steps are **MUST** occur *immediately* after the assumption is stated, *before* any subsequent plan step relies on the assumption's outcome:**
            1.  **Detail Verification Method:** The plan **MUST** detail the specific verification method(s) to be used (`read_file`, `grep_search`, etc.).
            2.  **Execute Verification & Report:** The verification **MUST** be performed immediately using the specified tool/method. The outcome **MUST** be explicitly reported in a structured format directly following the hypothesis statement:
                ```markdown
                **Hypothesis:** [State the assumption clearly]
                **Verification Method:** [Tool/Method, e.g., `read_file` on `file.ext`]
                **Verification Execution:** [State action, e.g., Performing `read_file`...]
                **Verification Outcome:** [State outcome, e.g., Confirmed: [Details] / Failed: [Details]]
                ```
            3.  **Proceed or Halt:** Only proceed with subsequent planning steps relying on the assumption if the **Verification Outcome** was 'Confirmed'. If 'Failed', the plan **MUST** be revised or halted.

        **Handling Failed Verifications for Existing Dependencies:** If hypothesis verification for a dependency reference present in existing, established code* (e.g., DI configuration, core modules) fails with a module resolution or symbol import error, **DO NOT** immediately plan creation.** Instead:
            1.  **State the Discrepancy:** Explicitly state that established code references a module/symbol that cannot be found.
            2.  **Perform Usage Check:** **MUST** use tools (`grep_search`) to search within the *referencing file* for actual usage of the specific symbol(s) being referenced from the missing source (e.g., instantiation, method calls, type annotations). Report findings explicitly (e.g., "Usage Check: Searched `di_config.xyz` for `MissingSymbol(...)` or `: MissingSymbol`. Outcome: No usage found." or "Outcome: Usage found in function `X`.").
            3.  **Perform Broader Context Search:** Execute additional searches (`grep_search` for related terms/files, `codebase_search` for conceptual links) to find context about the missing element (e.g., was it moved? refactored? deprecated? any TODOs for removal?). Report findings.
            4.  **Proceed based on Context and Usage:**
                *   If context clearly indicates a simple move/rename, plan the fix (update reference path).
                *   If the dependency failed **AND** the Usage Check (Step 2) found **NO USAGE** in the referencing file, the default plan should be to **remove the stale dependency reference statement**. Briefly verify this removal doesn't cause downstream issues (e.g., if the unused reference was providing a type hint relied upon elsewhere, although this is less common for *failing* dependencies).
                *   If the dependency failed **AND** usage *was* found, OR if context suggests intentional removal but usage remains, OR if context is insufficient/ambiguous, **trigger Step 3.5 (Handle Unclear Root Cause)**.

        **CRITICAL:** When planning interactions *between components* (e.g., function/method calls, data handoffs), the assumption about the *consistency* of the interface (signatures, data structures, expected return types/values, **types and access methods expected after refactoring**) **MUST** be explicitly verified using this structured approach by checking *both* the calling and called components *before* finalizing the interaction plan.** Proposing an interaction *without* verifying the full interface alignment based on implementation files is a process violation.** **CRITICAL:** An 'Assumption' is treated solely as a hypothesis pending mandatory factual validation using tools. **DO NOT** proceed with subsequent planning steps or code generation that rely on an assumption *until* its corresponding verification step (1-3 above) has been successfully completed and explicitly documented with the outcome (Confirmed/Failed) within this planning phase.** *Suggestion:* When making an assumption about the usage of a specific library function/class, especially involving less common patterns or arguments, explicitly suggest a quick verification against official documentation or trusted examples if unsure about the exact API contract or standard usage pattern. **(Enhanced - Logic Replacement): When replacing logic (per 3.4.1.e), any assumption about the functional equivalence of the new logic (i.e., that it fully covers necessary cases from the old logic) **MUST** be explicitly stated and verified by comparing the plan against the documented behavior of the old logic.**

        `3.4.1.f - Enumerate Edge Cases/Errors (Mandatory for Logic Changes):` For changes involving new logic, algorithms, or significant modifications, **explicitly list key potential edge cases** or specific error conditions anticipated (beyond generic exceptions). Briefly state how the plan addresses them (e.g., specific checks, custom exceptions, validation logic). *Example: "Edge Cases: Handles empty input list by returning default; checks for division by zero before calculation."*

        `3.4.1.g - Verify Framework Compatibility:** When refactoring functions that serve as entry points for frameworks (e.g., command-line argument parsers, API route handlers), **explicitly verify** that changes to function signatures (sync/async, parameters, return types) are compatible with how the framework invokes them. This may require checking framework documentation or known patterns (e.g., ensuring appropriate handling of asynchronous operations if the framework expects it). State the check performed and outcome.

        **`3.4.1.h - CRITICAL: Preserve Core Logic during Refactoring (Mandatory Logic Replacement Check):`** **MUST** Whenever planning a refactor that involves **replacing or significantly restructuring existing logic blocks** (e.g., replacing complex conditional structures, rewriting core algorithms):
            *   **Document Existing Behavior FIRST:** Before proposing the new structure, you **MUST** first use tools (`read_file`) to meticulously analyze and **document the essential behavior, distinct execution paths, and key conditional logic** of the **code section being replaced**. This documented behavior needs to be summarized concisely in your response.
            *   **Explicit Preservation Plan REQUIRED:** The refactoring plan **MUST** then explicitly detail **how the *new* logic structure** (e.g., the new function call, rule loop, simplified structure) **preserves EACH identified essential behavior and conditional path** from the original code, referencing the documented behavior.
            *   **Justify Intentional Changes:** If any original behavior or path is *intentionally* being changed or removed, or if the conditions under which specific logic executes are altered (even as a result of simplification or unification), this **MUST** be explicitly stated, justified (explaining the reason for the change and confirming its acceptability), and its impact analyzed separately as part of the overall impact analysis. **AI Assistant Responsibility: You **MUST** be vigilant in identifying these subtle behavioral shifts during planning and explicitly report them for confirmation *before* proceeding.**
                *   **Explicit Trade-off Presentation:** If such an alteration is identified as a consequence of simplification/unification, the AI **MUST** explicitly present the trade-off: Describe Plan A (Simpler/Altered Behavior) with justification, briefly outline Plan B (More Complex/Preserves Behavior), and **request guidance** on which approach to proceed with before finalizing the plan.

            *   **Functional Check:** Confirm consumers still function as expected (via analysis, suggesting tests if available).

        **`3.4.1.i - Verify Configuration Usage Impact:`** When adding, removing, renaming, or significantly changing the type/structure of configuration values (e.g., in config files or dedicated configuration modules), **MUST** use tools (`grep_search`) to identify potential usage locations throughout the codebase. Briefly state findings and confirm compatibility or necessary updates in the plan.

        `3.4.2.` **Verification:** For moves/renames, **MUST** use *multiple* search strategies (as detailed in 3.4.1) to confirm impact. **DO NOT** rely on a single search yielding 'no results'.**
          *Example: 'Modifying `parser_x` impacts `processor_y`. Plan includes verifying compatibility.'*

        `3.4.3.` **Prefer Atomic Edits:** Where feasible, break down complex changes into smaller, atomic edits. Each edit should ideally address a single, verifiable change to minimize verification scope and complexity.

    `3.5.` **Handle Unclear Root Cause / Missing Info / Debugging Blockers:**
        `3.5.1 Trigger:` If Steps 3.3 or 3.4 reveal unclear root cause, missing info, standard conflicts, **or failing standard debugging methods.** Also triggered if Step 3.4.1.b verification for an existing dependency fails and usage *is* found, or context suggests intentional removal/ambiguity.
        `3.5.2 Action:` **STOP** proposing a direct fix.

            `3.5.2.a Prioritize Investigation:` State the issue and that investigation is the next step.

            `3.5.2.b Formulate Investigation Plan:` Outline steps to investigate the root cause *or* debugging blocker (e.g., "Investigate traceback suppression", "Investigate `func_a` for missing data"). **For failed existing dependencies with usage:** Investigation plan **MUST** include assessing whether the *referencing code itself* (e.g., the dependency reference statement and its usage in the DI config) is stale and should be removed, rather than assuming the missing module needs creation.

            `3.5.2.c Seek Confirmation/Clarification (if needed):` Confirm investigation plan if broad or based on assumptions. *Example: "Traceback suppressed. Plan: 1. Examine logger. 2. Check system exception hook mechanism. Proceed?"* or *"Dependency reference to used symbol `X` fails from `missing_module`. Plan: Read usages in DI config, search context for `missing_module`. Proceeding."*

            `3.5.2.d Handle Necessary Workarounds (Requires Explicit User Approval - Use Sparingly):`
                `i. Trigger:` Only when root cause investigation (Step 3.5.2.b) is confirmed to be **blocked** (e.g., requires external information/access unavailable), **impractical** in the immediate context (e.g., requires disproportionate effort/time diverting significantly from the primary goal), **AND** a temporary workaround appears to be the *only* viable path to proceed *at this moment*.
                `ii. Action:` **STOP** all implementation planning or proposing fixes.
                    a.  **State the Blockage & Need:** Clearly explain *why* the root cause cannot be addressed robustly *right now* (citing the specific blocker or impracticality) and state explicitly that proceeding requires a temporary workaround that **violates standard principles**.
                    b.  **Propose Workaround Concept:** Briefly describe the *intended effect* and *nature* of the proposed workaround conceptually (e.g., "Propose temporarily ignoring specific benign error X during initialization", "Propose temporarily using hardcoded value Y for configuration"). **Do NOT** provide detailed implementation steps yet.
                    c.  **Justify & Warn:** Briefly explain why this specific workaround seems the only *temporary* path forward. **CRITICAL:** Explicitly state the risks and technical debt** introduced (e.g., "Risk: This will mask the underlying library issue Z.", "Debt: This requires adding tracking and mandatory future refactoring to address the root cause properly.").
                    d.  **Request Explicit Approval:** **MUST** ask the user directly: *"Do you approve implementing this specific temporary workaround to proceed, acknowledging the stated risks and the requirement for future cleanup?"*
                    e.  **Await Approval:** **DO NOT** proceed with planning or implementing the workaround unless **explicit user approval** is received for *this specific instance*. If approval is granted, any subsequent plan **MUST** include steps for tracking the workaround (e.g., adding `TODO` comments, linking to an issue) and eventually removing/replacing it with a robust solution. If approval is denied, remain **STOP**ped and await alternative direction from the user.

            `3.5.2.e Consult User on Ambiguous Missing Dependencies:`
                `i. Trigger:` Halt and request guidance if the **only viable path** to resolve a dependency/module resolution error appears to be **creating significant new structures** (e.g., entire modules, core interfaces) based *solely* on references found in potentially old/established code (like DI setup), especially when broader context search (per Step 3.4.1.b-3) doesn't strongly corroborate the need for this structure.
                `ii. Action:` **STOP** implementation planning.
                    a.  **State the Situation:** Clearly explain the failed dependency resolution, the lack of context, and the implication (e.g., "DI config references `X` from missing module `Y`. Standard checks failed to find `Y` or strong context for its necessity. Resolving this requires creating module `Y` and interface `X`.").
                    b.  **Present Options:** Describe the choices: "Option A: Create missing module/interface `Y`/`X` based on the reference in DI config (Risk: Might recreate obsolete code). Option B: Assume the reference in DI config is stale and plan to remove it and its usages (Requires verifying this cleanup won't break active components)."
                    c.  **Request Guidance:** Ask explicitly: "How should I proceed?"
                    d.  **Await Direction:** **Do NOT** proceed until user provides direction.

    `3.6.` **Handle Architectural Decisions / Suboptimal Patterns:** **CRITICAL:**
        `3.6.1 Trigger:` When analysis (Step 3) reveals:
            a.  An existing architectural pattern that conflicts with `code_architecture_standard.md` or project standards.
            b.  Multiple valid implementation paths exist for the core request, each with significant architectural trade-offs (e.g., performance vs. maintainability, complexity vs. explicitness, changes impacting multiple core components).
            c.  **Foundational inconsistencies** (e.g., conflicting type definitions vs. usage, data representation mismatches between layers).
        `3.6.2 Action:` **IMMEDIATELY STOP** detailed implementation planning or proposing fixes for the original, narrower task. **Treat this as a mandatory blocker.**

            `3.6.2.a State the Issue/Choice:` Clearly describe the suboptimal pattern, architectural decision point, or foundational inconsistency identified.

            `3.6.2.b Outline Options:` Present the potential solution paths or refactoring options to address the *underlying issue*.

            `3.6.2.c Analyze Options:** Briefly discuss the pros and cons of each option, explicitly referencing relevant sections of `STANDARDS.md` and `code_architecture_standard.md`.

            `3.6.2.d Request Guidance:` Explicitly ask the user for direction on which path to proceed with *to resolve the foundational issue*. *Example: "Identified that module X uses global state, violating standards. Option 1: Refactor using DI (preferred by standards, more effort). Option 2: Keep global state (simpler now, maintains tech debt). Which approach should I plan for?"* or *"The request can be implemented by modifying Interface A or creating New Interface B. Modifying A is simpler but broadens its responsibility. Creating B adheres better to SRP but adds complexity. How should I proceed?"* or *"Identified `ItemID` defined as `integer` but used as `string`. Option A: Change alias/model/usage to `string`. Option B: Keep `integer` and enforce conversion. How should we resolve this type inconsistency?"*

            `3.6.2.e Await Direction:** Do NOT** proceed with implementation planning for *any* specific option (neither the original task nor the foundational fix) until the user provides clear direction on how to address the **identified architectural/consistency issue**.

    `3.7.` **Trace Data Flow:**
        `3.7.1 Trigger:` For validation errors OR modifications to data path functions.
        `3.7.2 Action:` Trace data from origin to consumption/validation, reading relevant intermediate functions.

    `3.8.` **Improve Edit Tool Reliability (Before `edit_file` Call):**
        `3.8.1 Trigger:` Before calling `edit_file`.
        `3.8.2 Action:`
            `a. Explicit Instructions:` Be explicit in `instructions` (add vs. modify/replace). *Example: "Replace `foo` method..." or "Insert new `bar` function..."*
            `b. Sufficient Context:` Ensure `code_edit` includes enough unchanged lines for anchoring.

    `3.9.` **Exception for Diagnostics:**
        `3.9.1 Trigger:` For temporary deviations (e.g., print statements/temporary logging).
        `3.9.2 Action:`
            `a. MAY` implement directly.
            `b. MUST` state: **"Applying TEMPORARY change for diagnostics."**
            `c. MUST` justify necessity.
            `d. MUST` include marker: **"REMINDER: Temporary code MUST be reverted/fixed."**
            `e. Plan MUST` include reverting/fixing it later.

    `3.10.` **Perform Pre-computation Verification Summary (Before Step 4):**
        `3.10.1 Trigger:` Before proceeding to Step 4.
        `3.10.2 Action:` **MUST** generate a **Pre-computation Verification Summary**. This confirms key checks from Step 3 were performed. Structure using the following format:
        ```markdown
        **Pre-computation Verification Summary:**
        - `[x/-] 1. Standards Alignment:` *[Narrative confirming 3.2 performed. Mark `[x]` if done.]*
        - `[x/-] 2. Impact Analysis:` *[Narrative confirming 3.4.1 performed, including dependency search outcomes. Mark `[x]` if done.]*
        - `[x/-] 3. Hypothesis Verification:` *[Narrative confirming **all** hypotheses stated in 3.4.1.b were explicitly verified using the mandatory reporting format (Verification Method/Execution/Outcome) **immediately** after being stated. List key hypotheses verified or state 'N/A'. Mark `[x]` only if all stated hypotheses were verified.]*
        - `[x/-] 4. Logic Preservation Plan:` *[Narrative confirming 3.4.1.e performed if applicable (logic documented, preservation plan detailed). Mark `[x]` if done, `[-]` if N/A.]*
        - `[x/-] 5. Architectural/Root Cause Blocks Handled:` *[Narrative confirming 3.5/3.6 checks performed and guidance sought/received if necessary. Mark `[x]` if checks done and blocks resolved/approved, `[-]` if N/A.]*
        - `[x] 6. Confirmation:` Pre-computation verification summary complete. *(This point is always marked `[x]` when the summary is generated).*
        ```

### 4. Post-computation Standards Check (Verification Phase)

*   **Trigger:** After generating a specific code edit proposal (`edit_file` content) but **before** calling the `edit_file` tool. Also triggered **after** successful `edit_file`/`reapply` calls to verify completion.
*   **Action:**

    **CRITICAL WARNING:** VERIFY ALL DIFF LINES, NOT JUST INTENDED CHANGES
    The model applying edits (`edit_file`) frequently introduces unintended lines. **Verification **MUST** cover *every single line* present in the applied diff.** Failure to meticulously verify the *entire* applied diff is a primary cause of regressions and process violations.

    **Guideline:** Atomic Edit-Verify Cycle
    Strive to complete the sequence [Plan -> Pre-Verify -> Edit Tool Call -> Post-Verify & Summarize] for a single file/action within one response cycle or immediate consecutive responses.

    **CRITICAL REMINDER (NON-NEGOTIABLE PRE-EDIT CHECKS):**
    *   **Purpose:** The following Pre-Edit Verification steps (4.A) are **MUST** be performed **before** *every* `edit_file` tool call (4.B). Their purpose is to meticulously scrutinize the *proposed code edit diff* generated by the AI against the verified plan (Step 3) *before* it is applied.
    *   **Risk of Skipping:** The AI model generating the diff and the separate model applying it (`edit_file`) can make mistakes or introduce unintended changes. Skipping or rushing the pre-edit diff verification (Step 4.A) has **repeatedly resulted in the application of incorrect, incomplete, or damaging code**, necessitating rollbacks and significant rework.
    *   **Mandate:** **DO NOT SKIP OR RUSH THESE STEPS.** Treat the proposed diff with skepticism until fully verified.

    ### 4.A: Pre-Edit Verification (Mandatory Before 4.B)

    1.  **`4.A.1` Perform Granular Final Review & Deviation Handling Loop (Pre-Edit):**
        *   **WARNING:** AI model may add unrequested lines. **Treat all lines in the proposed diff, especially unplanned ones, with extreme skepticism until rigorously verified using tools and facts.**

        *   `4.A.1.a - Summarize Pre-Edit Context:` **Before generating the `code_edit` diff**, briefly summarize the relevant existing code structure (e.g., surrounding functions, class definition, imports/dependencies) based *directly* on the current file content obtained via recent tool calls (`read_file`). This ensures the immediate context is loaded before proposing changes.

        *   `4.A.1.b - Review Intended Changes:` Review planned lines in `code_edit` diff. Verify alignment with plan and standards.

        *   `4.A.1.c - Identify Deviations:` Compare *entire* proposed `code_edit` diff line-by-line against Step 3 plan. **Explicitly list all lines** not explicitly planned (**"Deviations"**).
            *   If Deviations found, state "Deviations identified, entering Deviation Handling Loop." Proceed to 4.A.1.d.
            *   If no Deviations exist, state "No deviations found." Proceed to 4.A.1.e.

        *   `4.A.1.d - Deviation Handling Loop (Fact-Check Each Deviation):` **CRITICAL:** Any line added, deleted, or modified by the AI in the `code_edit` diff that was *not* part of the explicit Step 3 plan **IS** a Deviation. Furthermore, **any change reliant on an assumption identified in Step 3.4.1.b **MUST** be explicitly verified here.** Treat all Deviations and assumptions with extreme skepticism. **DO NOT ACCEPT** Deviations or proceed based on assumptions due to perceived plausibility (e.g., 'looks reasonable'); **MUST** perform tool-based factual verification for **EVERY** Deviation and **EVERY** key assumption before proceeding. **For each Deviation, explicitly state the check being performed, the verification method/tool, and the outcome in your response.**
            *   *Dependencies/Imports:* **CRITICAL:** For *any* dependency reference statement (e.g., import, include, require) present in the proposed `code_edit` diff (whether added, modified, or included as unchanged context), perform the following sequence, **explicitly reporting the outcome of each check for EVERY reference**:
                1.  **Cross-Reference Prior Knowledge:** Check if the correct module path for the referenced symbol(s) was previously verified (e.g., during Step 1 or 3 file reads). If so, **use that verified path**. If not, or if unsure, proceed to Path Validation.
                2.  **Path Validation:** **MUST** use tools (`file_search`, `list_dir`) to confirm the **actual existence** of the source module file/directory path (e.g., `path/to/module.ext`, `path/to/package/index.ext`) within the project. **Do not assume path validity.** If the path does not exist, the reference is invalid and **MUST** be removed or corrected in the `code_edit` proposal. **Explicitly state the path validation method and outcome for EACH path checked.**
                3.  **Symbol Validation:** Only if the path is confirmed valid, **MUST** then use tools (`read_file` on target module/package definition, `grep_search`) to confirm the existence AND necessity of the referenced symbol(s) within that module/package context. **Do not assume symbol existence or necessity.** **Explicitly state the symbol validation method and outcome for EACH symbol checked.**
            *   ***Modifying Dependencies:*** When modifying an existing dependency reference line (adding or removing symbols), **MUST** re-verify the necessity of *all* symbols remaining on that line within the current file scope using tools (`grep_search`, `read_file`). Remove unused symbols. **Explicitly state the necessity check outcome.**
            *   *Variables/Logic/Defaults/Deletions:* Confirm requirement based *only* on verified state/requirements. Justify unexpected deletions.
            *   ***Context Line Discrepancies:*** **MUST** check if context lines in `code_edit` diff are shown incorrectly (+/-). If found, **IS** a Deviation. **STOP.** Re-read file section to confirm state before correcting/proceeding.
            *   `**Confirmation of Prior Verification:** Before accepting any code dependent on a previously stated assumption (hypothesis), **explicitly confirm** that the verification outcome documented in Step 3.4.1.b was 'Confirmed'. If verification failed, was skipped, or was not documented, **STOP**. State the process violation. The dependent code **MUST** be corrected or removed before proceeding.`
            *   **Decision & Action:**
                *   If Deviation verified & necessary: State justification (including **explicitly reported verification method and outcome**) and keep it. (e.g., "Verified dependency reference 'x' via `file_search`. Outcome: Confirmed, path exists. Keeping Deviation.")
                *   **If any Deviation cannot be factually verified or necessity confirmed:** State reason and **REMOVE the Deviation / Correct the dependent code / Re-plan.** (e.g., "Removed unverified dependency reference 'foo.bar'." or "Corrected context line based on `read_file`.")
            *   **Repeat for ALL Deviations and required Assumption Verifications before exiting loop.**

        *   `4.A.1.e - Integration Sanity Check:` Perform a brief qualitative check: Does the proposed change (intended + verified Deviations - removed Deviations) logically fit within the module/function's purpose and structure based on the context summary (Step 4.A.1.a)? Does it appear to interact correctly with immediately adjacent code? **Explicitly state confirmation.**

        *   `4.A.1.f - Review Final Proposed Diff:` After loop (if triggered), review final `code_edit` diff (intended changes + verified Deviations - removed Deviations) for correctness, consistency, standards adherence. **CRITICAL:** Explicitly verify and report** that all type/function/class names used *within the newly added or modified lines* are either defined locally or explicitly referenced/imported in the target file; add necessary references/imports if missing. Ensure it addresses root cause (Step 3.4). **Specifically verify and report** that all function/method calls within the proposed diff exactly match the signatures confirmed during Step 3.4.1.b verification.

    2.  **`4.A.2` Generate Pre-Edit Confirmation Statement (Before `edit_file`):** Before calling `edit_file`, **MUST** provide a brief statement confirming pre-edit checks, **specifically mentioning the explicit verification of key assumptions and dependencies**, and confirming logic preservation if applicable.
        *   *Example (Refactoring): "Pre-edit review (Step 4.A.1) complete. No deviations found. Explicitly verified key assumption (API structure 'X') via `read_file` (Outcome: Confirmed). Logic Preservation (Step 3.4.1.e): Confirmed plan preserves documented original behavior. Proceeding with edit."*
        *   *Example (Deviation Handling): "Pre-edit review (Step 4.A.1) complete. Deviations identified and handled: removed unverified reference 'foo.bar' (explicit check outcome reported). Explicitly verified key assumption (ORM model 'Y') via `grep_search` (Outcome: Confirmed). Logic Preservation (Step 3.4.1.e): N/A (New feature). Proceeding with corrected edit."*
        *   *Example (No key assumptions/refactoring): "Pre-edit review (Step 4.A.1) complete. No deviations found. No key assumptions required verification. Logic Preservation (Step 3.4.1.e): N/A. Proceeding with edit."*

    ### 4.B: Apply Edit (After Successful 4.A)

    1.  **`4.B.1` Apply Edit Tool:** (e.g., Call `edit_file`)

    ### 4.C: Post-Edit Verification (Mandatory After 4.B)

    1.  **`4.C.1` Verify Edit Application (Perform Checks for Summary Point 1):**
        *After a successful `edit_file` or `reapply` call, explicitly check **and report the outcome of each** of the following:*

        `4.C.1.a - Post-Reapply Verification:` **CRITICAL:** If modification resulted from `reapply`:
            *   **CRITICAL REMINDER:** `reapply` uses a different model. This check is **MUST** after *every* successful `reapply`. **DO NOT SKIP.**
            i.  **Treat Diff as New:** Treat the *entire* diff applied by `reapply` with extreme skepticism, as potentially introducing unrelated or incorrect changes compared to the file state *before* the `reapply` call.
            ii. **Perform Full Re-Verification:** Re-perform the **full Pre-Edit Verification checks (Steps 4.A.1.a - 4.A.1.f), explicitly reporting each check and outcome,** on the *actual diff applied by `reapply`*, comparing it against the file state *before* the `reapply` call.
            iii.**Explicit Deviation Handling:** As part of this re-verification (specifically mirroring Step 4.A.1.c and 4.A.1.d), **explicitly identify and fact-check EVERY deviation** present in the `reapply` diff compared to the file's prior state. **DO NOT** assume deviations are related to the original intent or automatically correct. Remove or correct any unverified/unnecessary deviations found in the `reapply` diff itself.
            iv. **Structured Log:** **Immediately after `reapply` result, **MUST** generate structured log confirming these re-verification checks (Steps i-iii above) *including an explicit statement on the outcome of deviation handling (iii)* before any other step.** Use format similar to Step 4.A.2's Pre-Edit Confirmation Statement, noting it's a post-reapply check.

        `4.C.1.b - Post-`edit_file` Verification:` **CRITICAL:** For diffs from standard `edit_file` (not `reapply`), perform the following checks **and explicitly report the outcome of each**:
            *   **WARNING:** Treat Diff Output with Extreme Skepticism.** Diffs may misrepresent changes (esp. context lines). **NEVER assume \"diff artifacts.\"** Investigate discrepancies using `read_file`.
            i.  **Diff Match:** Compare the *entire* applied diff line-by-line against the final intended `code_edit` proposal (from 4.A.1.f). **Explicitly identify ALL discrepancies**, including subtle changes to dependency references, type hints, comments, or whitespace introduced by the apply model.
            ii. **Targeted Semantic Spot-Check:** Rigorously re-apply semantic validation to *key* additions/changes (e.g., complex logic, function calls). Confirm semantic correctness.
            iii.**Dependency Re-verification:** For ALL dependency reference statements (e.g., imports) present in the *applied* diff (intended, unintended, or context), **MUST** re-verification using tools (`file_search`, `list_dir`, `read_file`, `grep_search`) MUST be performed to factually confirm the module path exists AND the referenced symbol(s) exist within that module. Explicitly report the outcome of this dependency re-verification.** Reject justifications based on 'model consolidation' or apparent reasonableness. **Do not assume** any dependency reference line is correct without this explicit post-edit check.
            iv. **Context Line Check:** Re-verify context lines were not unexpectedly modified/misrepresented.
            v.  **(Enhanced - Logic Replacement Check):** If the edit involved replacing logic (per 3.4.1.h), perform a final validation comparing the *applied* new logic against the *documented behavior of the original code* (from 3.4.1.h). Explicitly confirm that all essential original behaviors/paths appear preserved or were correctly modified according to the final plan.
            vi. **Discrepancy Handling:** If any discrepancy (diff mismatch (i), failed semantic validation (ii), failed dependency re-verification (iii), context issue (iv), **failed logic preservation check** (v)) cannot be justified or is incorrect, **trigger self-correction (Step 4.C.3)**.

        `4.C.1.c - No Introduced Redundancy:** Check for duplicate logic, unnecessary checks, redundant mappings. Remove if found.

        `4.C.1.d - Optional Post-Refactor Smoke Test:** After successfully applying significant refactoring edits to a command or core component, consider performing a minimal runtime \"smoke test\" (e.g., run command with `--help`, basic invocation) to catch immediate integration errors (DI resolution, framework invocation) before proceeding to the next task. Failure should trigger self-correction (Step 4.C.3). State if test performed and outcome.

    2.  **`4.C.2` Check for Leftover Code & Dependencies (Perform Checks for Summary Point 2):**
        *After successful edits (esp. refactoring), scan relevant files **and explicitly report the outcome of each check**:*

        `4.C.2.a No Commented-Out Code/Comments:** Ensure old code/comments fully deleted. Remove AI process comments.
        `4.C.2.b Verify Downstream Consumers:**
            i.  **Reference Check / Dependency Re-verification:** **RE-VERIFICATION:** For changes involving **modified OR ADDED** interfaces, paths, symbols, **or core domain models**, **explicitly state that dependency re-verification is being performed, execute the codebase search (`grep_search`)** from planning (Step 3.4.1) **AFTER the change (move/rename/delete)**, and report the findings. Use comprehensive strategies to definitively verify all dependent call sites/references were updated or unaffected. **Do not rely solely on initial planning search results, especially after moves/renames.** If missed updates found, correct (Step 4.C.3).
            ii. **Verification of Negative Results:** **CRITICAL (AFTER `delete_file`):** Confirm removal/move by searching for dangling references to the old path/symbol. Minimum searches: **1. Absolute path fragment (e.g., `grep 'old_module_path/old_filename'`). 2. Relative references in relevant directories (e.g., `grep 'from ./old_filename'`).** **Meticulously review *all* search result lines.** Consider other context (e.g., config files). **Explicitly report the search strategies used and the outcome.** If lingering references are found, **this constitutes a flaw and **MUST** trigger self-correction (Step 4.C.3) to remove/update them.
            iii.**Functional Check:** Confirm consumers still function as expected (via analysis, suggesting tests if available).

    3.  **`4.C.3` Self-Correct if Necessary (Perform Check for Summary Point 3):**
        `4.C.3.a Trigger:` If reviews (Step 4.A.1, 4.C.1, 4.C.2) reveal violations, incorrect application, redundancy, or leftover cleanup.
        `4.C.3.b Action:`
            i.  **DO NOT** proceed with flawed edit, or state issue in Post-Action Summary if already applied.
            ii. **MUST** explicitly state flaw** based on standard/process (e.g., \"Workaround violated Step 3.5.\" or \"Old config not removed.\" or \"Adjacent line altered.\").
            iii.**Triggers for Self-Correction (STOP and Address):**
                a.  Analysis reveals that a hypothesis was stated in Step 3.4.1.e without the **mandatory, immediate, documented verification execution and outcome** directly following it. (State violation, perform missed verification, re-evaluate, correct course).
                b.  Required **Pre-Edit Verification (4.A.1 + 4.A.2)**, **Post-`reapply` Verification (4.C.1.a)**, **Post-`edit_file` Verification (4.C.1.b)**, or **Dependency Re-verification (4.C.2.b.i)** were missed. (State oversight, perform missed steps, re-evaluate, correct course).
                c.  The mandatory **Post-Action Verification Summary (Step 4.C.4)** for the immediately preceding `edit_file`/`reapply` was missed. (State oversight, perform missed post-edit checks (4.C.1, 4.C.2), generate summary, proceed).
                d.  Analysis reveals that a **mandatory halt and request for guidance under Step 3.6 was previously missed**. (State oversight, perform required 3.6 actions, await direction).
                e.  Analysis at *any point* reveals that *any* mandatory step, check, reporting requirement, or explicit verification outlined in this document was missed, skipped, or incompletely performed in the preceding actions. (State violation, perform missed steps, document outcome, re-evaluate, correct course).
            iv. **MUST** revise plan/next step for investigation, correction, or cleanup.
            v.  Explain correction/next step.
            vi. **Requesting Manual Edits:** If repeated attempts (e.g., >3 loops on the same correction for a file) with available tools fail to apply a necessary correction correctly, **STOP**. State the tool failure and the presumed incorrect state of the file based on the last tool output. **Then, perform a final check: re-read the relevant section of the target file (`read_file`) to verify its *actual* current state.** If the file state is confirmed to still be incorrect after the re-read, **then** **MUST** request manual user intervention by providing a clear, complete, copy-pasteable code block showing the *entire relevant section* of the file in its desired final state, including **sufficient surrounding unchanged lines (e.g., 5-10 lines before and after)** for context. If the re-read shows the edit *did* succeed despite tool reports, state this and proceed accordingly.

    4.  **`4.C.4` Generate Post-Action Verification Summary (After `edit_file`/`reapply` - Final Summary Before Completion):**
        `4.C.4.a Trigger:` Immediately following the *final* `edit_file` or `reapply` tool result for the task/request.
        `4.C.4.b Action:` **MUST** generate a **Post-Action Verification Summary**. This confirms all post-edit checks (Steps 4.C.1, 4.C.2, 4.C.3 logic) were **explicitly performed and documented** in the preceding analysis. Structure using the following format:
        ```markdown
        **Post-Action Verification Summary:**
        - `[x/-] 1. Edit Application Analysis:` *[Narrative confirming checks 4.C.1.a (if `reapply`), **4.C.1.b (diff match & explicit semantic spot-check, including explicit dependency validation)**, and **4.C.1.c (redundancy removal)** were performed and results documented. **CRITICAL:** Explicitly state EITHER:** 'The applied diff matches the final intended edit **exactly**.' **OR** 'The applied diff contains the following discrepancies compared to the final intended edit: [List discrepancies]. These were [justification: e.g., verified during pre-edit, confirmed necessary, identified for correction via Step 4.C.3].' Detail findings based on explicit checks. Mark `[x]` if checks passed and diff OK/justified, `[-]` if 4.C.1.a N/A, or explain issues.]*\n        - `[x/-] 2. Leftover Code & Dependency Analysis:` *[Narrative confirming check **4.C.2.a (artifact removal)** and **4.C.2.b (explicit dependency re-verification search)** were performed and results documented. Detail findings (e.g., \"Old dependency X removed\", \"Dependency search confirmed no missed updates\", \"Dependency search found missed reference in file Y - corrected\"). Mark `[x]` if checks done and no issues remain, `[-]` if N/A.]*\n        - `[x/-] 3. Correction Assessment:` *[Narrative confirming check 4.C.3 was performed. State if corrections were needed/made. Mark `[x]` if assessment done and no corrections needed, `[-]` if N/A.]*\n        - `[x] 4. Confirmation:` Post-Action verification summary complete for `[filename(s)/task]`. *(This point is always marked `[x]` when the summary is generated).*\n        ```\n\n### 5. Adherence Checkpoint (Final Step in Cycle): **CRITICAL:**\n

---

**Reference:** Always refer to `STANDARDS.md` for the definitive definitions of the standards themselves. This document only defines the *process* for applying them. Refer to the project's language/framework-specific `code_architecture_standard.md` for technical patterns and guidelines.
