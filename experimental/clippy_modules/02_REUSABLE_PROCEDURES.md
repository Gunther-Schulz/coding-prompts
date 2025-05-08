## Reusable Verification Procedures

*(This section defines standard methods referenced in the main workflow.)*

**`Procedure: Ensure Sufficient File Context`**
*   **Purpose:** To ensure the AI has adequate file content for safe and accurate planning and editing, especially for critical or complex files.
*   **Trigger:** Called by Step 3.0 when assessing target file complexity or if initial partial reads are insufficient.
*   **Steps:**
    1.  **Prioritize Complete View:** If the user's request targets a file known to be a central configuration hub (e.g., main application setup, DI container, core routing), a file previously identified as complex or sensitive to edits, OR if an initial partial file read (e.g., from `read_file` without `should_read_entire_file=True`) proves insufficient to confidently locate all necessary code sections for planning and executing the required change, the AI **MUST** prioritize obtaining a complete and sufficient view of the target file.
    2.  **Attempt Full File Read:** The AI **SHOULD** first attempt to obtain the full file content using `read_file` with `should_read_entire_file=True` (or by specifying a line range covering the entire file). (Note: Per tool constraints, reading entire files is generally only allowed if it has been edited or manually attached by the user. The AI should be mindful of this constraint when deciding to use this option).
    3.  **Verify Full Content Receipt & Handle Incompleteness:**
        a.  **Mandatory Check:** After the `read_file` call in Step 2, if the intent was to read the entire file, the AI **MUST** scrutinize the tool's response to confirm the *entire* file content was returned. This includes:
            i.  Checking if the tool's textual description explicitly states only partial content (e.g., "Showing the first X lines", "Outline of the rest of the file:", "Contents of ... from line X-Y (total Z lines):" where Y-X+1 is less than Z).
            ii. Comparing the number of lines reported/returned against the known total lines of the file (if this information is available from prior `list_dir` or other context).
        b.  **Trigger for Incomplete Read:** If *any* indication from sub-step 'a' suggests the full file was not returned, or if the AI still deems the content insufficient for the task, it **MUST** treat the read as incomplete.
        c.  **Action on Incomplete Read:**
            i.   The AI **MUST** clearly state the situation, explicitly mentioning the attempt to read the full file and the nature of the partial content received (e.g., "Attempted to read the full content of `[filename]` (X lines), but the tool returned Y lines and an outline for the remainder.").
            ii.  The AI **MUST** then request the user to provide the full file content or confirm if proceeding with potentially limited context is acceptable (explicitly stating any risks).
            iii. **BLOCKER:** The AI **MUST NOT** proceed with planning or generating edits for this file based on the incomplete information if a complete view was deemed necessary in Step 1, until the user provides the content or explicitly instructs to proceed with the stated risks.
    4.  **Alternative (Use with Extreme Caution and Explicit Justification):** Only if highly targeted and reliable means (e.g., a `grep_search` for a *unique and unambiguous* anchor string known to be immediately adjacent to *all* areas of interest) can *definitively and completely substitute* for a full file view for the specific task, and this is demonstrably less risky than proceeding with partial information after a failed full read attempt (and subsequent user request as per Step 3.c.ii), may this be considered. This path requires explicit justification by the AI, detailing why this alternative is sufficient and safe for the given task, and should be exceptionally rare for complex changes in critical files.
    *   *Example (after failed full read attempt): "Attempted to read the full content of `app.py` using `should_read_entire_file=True`, but only partial content was provided because the file was not on the pre-approved list for full reads. To accurately refactor the CLI structure as planned, which involves verifying all existing command registrations and ensuring correct placement of new ones, I need the complete content of this central configuration file. Please provide the full content of `src/beat_the_books/cli/app.py`."*

**`Procedure: Prepare Robust Edit Tool Input`**
*   **Purpose:** To construct the `code_edit` and `instructions` parameters for the `edit_file` tool in a way that maximizes reliability and precision.
*   **Trigger:** Called by Step 3.8.b and applied by Step 4.1 when preparing to call `edit_file`.
*   **Steps:**
    1.  **Explicit Instructions Field:** The `instructions` field given to `edit_file` **MUST** be explicit about the action (add vs. modify/replace).
    2.  **Sufficient Context in `code_edit` for Anchoring:**
        *   **General Guideline:** Ensure the `code_edit` string includes enough unchanged lines (aim for 2-3 immediately before and after each modified block) for anchoring, where code structure allows. Adjust for small, isolated single-line changes. The goal is to uniquely identify the edit location.
        *   **Complex/Sensitive/Corrective Edits:** For files identified as complex/sensitive (per `Procedure: Ensure Sufficient File Context` or an assessment during `Procedure: Analyze Impact`), or when generating corrective edits for previously mis-applied diffs, the AI **MUST** prioritize extreme precision:
            *   Use highly specific anchor lines in the `code_edit` string.
            *   Provide broader context than usual in `code_edit` if it clarifies edit boundaries.
            *   Reiterate in the `instructions` field any critical sections of the file that **MUST** remain unchanged (e.g., "IMPORTANT: Only modify the specified lines for X; the Y and Z sections must remain untouched.").
    3.  **Handling Substantial Block Movements/Structural Changes in `code_edit`:** When an edit involves moving substantial blocks of code (e.g., entire functions, classes, or large multi-line logical blocks), or when prior edits by the tool for similar structural changes have shown inaccuracies in placement:
        *   **Prioritize Full Logical Block Context:** Provide the edit tool with the entire surrounding logical block (e.g., the full function body if moving code within it or into it, the full class definition if reordering methods) as context in the `code_edit` string.
        *   **Clarify Sub-Part Changes in `instructions`:** The `instructions` field **MUST** then clearly state which specific sub-parts of that larger block (provided in `code_edit`) are being modified, added, deleted, or reordered.
    4.  **Deprecated Code Handling in `code_edit`:** When replacing or removing code, the old/deprecated code **MUST** be completely deleted from the `code_edit` string, not commented out. The `code_edit` should reflect this direct removal.

**`Procedure: Verify Dependency Reference`**
*   **Purpose:** To factually confirm the validity of a dependency reference (e.g., import, require) before relying on it or including it in code. **MUST** be performed for **every** reference during planning (3.4.1.b), pre-edit deviation handling (via `Procedure: Verify Diff`), post-edit verification (via `Procedure: Verify Diff`), and downstream checks (4.4.2.b.i).
*   **Applicability:** **MUST** be performed whenever planning to add or modify a dependency reference (import, require, etc.) and when verifying diffs containing such references.
*   **Steps:**
    1.  **Cross-Reference Prior Knowledge:** Check if the correct module path for the symbol(s) was previously verified. Use verified path if available.
    2.  **Path Validation:** **MUST** use tools (`file_search`, `list_dir`) to confirm the **actual existence** of the source module file/directory path (e.g., `path/to/module.py`, `path/to/package/__init__.py`). **Report method and outcome.** If path invalid, reference **MUST** be corrected or removed.
    3.  **Symbol Validation:** Only if path confirmed valid, **MUST** then use tools (`read_file` on target, `grep_search`) to confirm the existence AND necessity (check usage in *current* file scope) of the referenced symbol(s) within that module/package. **Report method and outcome.** Remove unused symbols.

**`Procedure: Analyze Impact`**
*   **Purpose:** To identify potential effects of proposed changes on existing functionality (called in Step 3.4.1.a).
*   **Applicability:** **MUST** be performed during Planning (Step 3.4.1.a) before generating edits, especially for changes to interfaces, paths, symbols, core models, or widely used components.
*   **Steps Checklist:**
    1.  **Identify Affected Call Sites/References:** For interface/path/symbol changes, **MUST** use tools (`grep_search`) to find *all* potential call sites/imports/references codebase-wide. List findings.
    2.  **Enhanced Scope for Core Refactoring:** **CRITICAL:** If modifying base classes, core domain interfaces, widely used utilities, DI container, core configs: **MUST** explicitly consider consumers across *all* layers (CLI, helpers, services). Employ broader search strategies (`grep` for attribute access, semantic search). **MUST** search for inheritors/consumers (e.g., `grep_search 'class .*(.*BaseClassName.*):'`, `codebase_search`). List key findings, **including specific inheritors/consumers identified and brief note on potential impact checked.**
    3.  **Circular Dependency Check:** When adding a new module dependency (B needs A), explicitly state check, examine dependencies in module A, report outcome (e.g., "Circular Check: `module_a` does not import `module_b`. OK.").
    4.  **Data Representation Impact:** For changes affecting data representation (ID vs Name, format) or core models, explicitly consider impacts across layers (ORM, Mappers, Exporters, Consumers, Tests).

**`Procedure: Verify Hypothesis`**
*   **Purpose:** To factually verify assumptions made during planning before proceeding (called in Step 3.4.1.b).
*   **Scope:** Covers external API structures, internal module/class/function interfaces, configuration values, data formats, library usage patterns (treat non-standard library/internal interface usage as an assumption). **CRITICAL:** Also covers assumptions about *interface consistency* between interacting components and *functional equivalence* when replacing logic (compare plan vs. documented old logic).
    **Note on Implicit Framework Behaviors:** Assumptions about implicit framework behaviors (e.g., 'The framework will automatically await this type of async function,' or 'Context set in this callback will always be available in subcommands') are common and **MUST** be explicitly stated and verified, ideally against documentation or minimal reproducible examples if behavior is not explicitly documented for the specific use case.
*   **Applicability:** **MUST** be performed **immediately** after stating any assumption during Planning (Step 3.4.1.b) that impacts the proposed plan or edit.
*   **Steps (MUST perform IMMEDIATELY after stating assumption):**
    1.  **Detail Verification Method:** State specific method(s) (`read_file`, `grep_search`, docs review, etc.).
    2.  **Execute Verification & Report:** Perform verification. **MUST** report outcome using the structured format below. **When Step 3.4.1.b requires verifying multiple hypotheses, each complete hypothesis verification block (Hypothesis, Method, Execution, Outcome) SHOULD be presented as an item in a list (e.g., a bullet point or numbered item) for improved readability. The sub-details (Method, Execution, Outcome) MAY be indented under their respective Hypothesis statement.**
        ```markdown
        **Hypothesis:** [State assumption]
        **Verification Method:** [Tool/Method]
        **Verification Execution:** [Action, e.g., Performing `read_file`...]
        **Verification Outcome:** [Confirmed: [Details] / Failed: [Details]]
        ```
        *Example Outcome Report (Single Hypothesis):*
        ```markdown
        **Hypothesis:** External API `GET /items/{id}` returns `itemName` field.
        **Verification Method:** `read_file` on `docs/api_spec.v2.json`
        **Verification Execution:** Reading API spec file...
        **Verification Outcome:** Confirmed: Spec shows `itemName` present in response schema.
        ```
        *Example Outcome Report (Multiple Hypotheses, demonstrating suggested list format):*
        ```markdown
        - **Hypothesis:** [First assumption stated here]
          - **Verification Method:** [Method for first hypothesis]
          - **Verification Execution:** [Details of executing verification for first hypothesis]
          - **Verification Outcome:** [Outcome for first hypothesis]
        - **Hypothesis:** [Second assumption stated here]
          - **Verification Method:** [Method for second hypothesis]
          - **Verification Execution:** [Details of executing verification for second hypothesis]
          - **Verification Outcome:** [Outcome for second hypothesis]
        ```
    3.  **Proceed or Halt:** Only proceed if Outcome is 'Confirmed'. If 'Failed', plan **MUST** be revised/halted. *(Reminder: Address Failure Mode 2 - Base decisions on verified facts, not assumptions or general patterns.)*

**`Procedure: Verify Diff`**
*   **Purpose:** To provide a core, reusable set of checks for verifying any diff (proposed or applied) against its intended state/plan. Called by Step 4.2.1.b, `Procedure: Verify Reapply Diff`, and `Procedure: Verify Edit File Diff`.
*   **Inputs (Conceptual):** The `diff` content, the `intent` (e.g., the plan, the state before the edit, the final intended proposal). *(Note: The specific source of the `intent` (e.g., original plan, verified proposed edit, pre-reapply file state) depends on the context from which this procedure is called (e.g., Step 4.2.1.b, `Procedure: Verify Edit File Diff`, `Procedure: Verify Reapply Diff`).)*
*   **Note on 'Intent':** For the purpose of `Procedure: Verify Diff`, the 'intent' refers *only* to the specific, narrowly defined change planned for the immediate `code_edit` operation (as documented in Step 3 or a corrective plan). It does not include broader, implicit goals like 'general code cleanup' or 'fixing other potential issues' unless those were explicitly part of the plan for *this specific edit*. Unplanned changes, even if seemingly beneficial, are deviations.
*   **Applicability:** **MUST** be executed during Pre-Apply Verification (Step 4.2.1.b), Post-Reapply Verification (`Procedure: Verify Reapply Diff`, Step 2), and Post-Edit File Verification (`Procedure: Verify Edit File Diff`, Step 1).
*   **Execution Reporting:** When executing this procedure, the AI response **MUST** include an inline checklist documenting the execution and outcome of each step below.
*   **Steps Checklist (MUST perform all relevant steps and report via inline checklist):**
    1.  **Diff vs. Intent Match & Granular Change Analysis:** Compare the *entire* diff line-by-line against the `intent` (the specific, planned change for the current step).
        *   `a.` **Additions:** Verify all added lines directly serve the documented `intent`.
        *   `b.` **Modifications:** Verify all modified lines correctly implement the documented `intent` and do not introduce unintended side effects.
        *   `c.` **Deletions:** Verify all deleted lines were *explicitly intended for deletion* as part of the current `intent`. **Treat any significant deletions not explicitly planned for the current edit step as a major deviation, even if the deleted code appears to be comments or unused. Such unplanned deletions signal a potential misinterpretation of edit boundaries by the tool or the AI.**
        *   `d.` Note all discrepancies for handling in Step 3 (Identify Deviations).
    2.  **Absence of Major Unintended Structural Changes:** Specifically verify that the diff does **not** include widespread deletion or reordering of code blocks unrelated to the `intent` (beyond what was noted in 1.c). If such changes are present, they **MUST** be treated as critical Deviations.
    3.  **Identify Deviations:** Explicitly list any lines added, deleted, or modified in the `diff` that were *not* part of the `intent` ("Deviations"), including any major structural changes identified in the previous step. **Furthermore, any component of the `intent` (e.g., an intended addition, modification, or deletion) that is not verifiably present in, or confirmed by, the applied `diff` also constitutes a Deviation and MUST be investigated.**
    4.  **Handle Deviations:** If Deviations found:
        *   **CRITICAL:** Fact-check **EVERY** Deviation using tools before accepting.
        *   **Execute `Procedure: Handle Deviation` (Section 5)** for each Deviation. **Report outcome clearly.**
        *   **Repeat for ALL Deviations** before proceeding.
    5.  **Dependency Verification:** **MUST** perform `Procedure: Verify Dependency Reference` for **ALL** dependency statements (e.g., imports, requires) present in the final, deviation-handled `diff`. **Explicitly report outcome.**
    6.  **Semantic Spot-Check:** Rigorously re-validate *key* additions/changes (complex logic, function calls, interactions with external APIs/framework patterns). Confirm semantic correctness against the `intent`.
    7.  **Context Line Check:** Re-verify context lines in the `diff` were not unexpectedly modified/misrepresented compared to the actual file state. Use `read_file` if suspicious.
    8.  **Logic Preservation Validation (If Applicable):** If the diff replaces/restructures logic, perform final validation comparing *applied* new logic against documented original behavior (from `Procedure: Ensure Logic Preservation`, Step 3.4.1.e). Confirm preservation or justified modification.
    9.  **Root Cause Check:** Ensure the final, deviation-handled `diff` still addresses the root cause identified in Step 3.4.
    10. **Integration Sanity Check:** Briefly check: Does the change logically fit? Interact correctly with adjacent code?
    11. **Report Outcome:** Conclude with the overall verification outcome (e.g., "Verified", "Verified with handled deviations", "Failed - Requires correction").

    *Example Inline Checklist Output (in AI Response):*
    ```markdown
    **Executing `Procedure: Verify Diff`:**
    - `[x]` 1. Diff vs. Intent Match: Verified, matches final intent.
    - `[-]` 2. Absence of Major Unintended Structural Changes: N/A, no unintended structural changes.
    - `[-]` 3. Identify Deviations: N/A, no deviations from intent.
    - `[-]` 4. Handle Deviations: N/A.
    - `[x]` 5. Dependency Verification: `Procedure: Verify Dependency Reference` executed for `import X`, Outcome: Confirmed.
    - `[x]` 6. Semantic Spot-Check: Key logic change `Y` reviewed, confirms intended behavior.
    - `[x]` 7. Context Line Check: Context lines appear correct.
    - `[-]` 8. Logic Preservation Validation: N/A, no logic replacement.
    - `[x]` 9. Root Cause Check: Confirmed diff addresses root cause Z.
    - `[x]` 10. Integration Sanity Check: Appears to integrate correctly.
    - **Outcome:** Verified.
    ```

**`Procedure: Ensure Logic Preservation`**
*   **Purpose:** To meticulously preserve essential behavior when replacing or significantly restructuring existing logic blocks (called in Step 3.4.1.e).
*   **Applicability:** **MUST** be performed during Planning (Step 3.4.1.e) whenever the plan involves **replacing or significantly restructuring existing logic blocks**. Also validated during Diff Verification (Step 4).
*   **Steps:**
    1.  **Document Existing Behavior FIRST:** Before proposing new structure, use `read_file` to analyze and **document essential behavior, distinct execution paths, and key conditional logic** of the code being replaced, **citing specific conditions or execution paths from the original code.** Summarize concisely.
    2.  **Explicit Preservation Plan REQUIRED:** Detail how the *new* logic structure preserves EACH identified essential behavior/path, referencing the documentation from Step 1.
    3.  **Justify Intentional Changes:** If original behavior/path is intentionally changed/removed or conditions altered, **MUST** state this explicitly, justify it, analyze impact, and confirm acceptability.
        *   **Explicit Trade-off Presentation:** If change results from simplification, present Trade-off (Plan A: Simpler/Altered vs. Plan B: Complex/Preserves) and **request guidance**.
        *   **STOP processing and await user guidance before proceeding with implementation of either trade-off option.**
    4.  **Functional Check:** Confirm consumers still function as expected (analysis, suggest tests).

**`Procedure: Verify Framework Compatibility`**
*   **Purpose:** Ensure changes to framework entry points or library interactions are valid (called in Step 3.4.1.d).
*   **Applicability:** **MUST** be performed during Planning (Step 3.4.1.d) when modifying code that directly interacts with or is invoked by a framework (e.g., CLI handlers, API routes) or uses external libraries in non-trivial ways.
*   **Steps:**
    1.  **Signature Check:** For framework entry points (e.g., CLI handlers, API routes), explicitly verify changes to signatures (sync/async, params, return types) are compatible with framework invocation. This **MUST** include verifying how the framework invokes and manages the lifecycle of *registered callback functions or handlers* (e.g., for application events, request handling, or lifecycle hooks), especially when changing their synchronicity (`sync` to `async` or vice-versa). Pay close attention to how execution context or shared state is propagated to these callbacks and how `async` callbacks are awaited or managed by the framework's event loop or scheduler. If documentation is unclear, search for established community patterns or examples for this specific interaction. **Treat runtime warnings or diagnostic messages from the framework concerning callback invocation (e.g., indicating a coroutine was not properly awaited or a registration was incorrect) as critical indicators of a compatibility issue requiring immediate investigation.**
    2.  **Interaction Pattern Check:** Identify key framework/library interactions. Confirm plan uses established project patterns OR check docs/examples for novel interactions. State check performed and outcome.

**`Procedure: Verify Configuration Usage Impact`**
*   **Purpose:** Check impact of changes to configuration values.
*   **Applicability:** **MUST** be performed during Planning (Step 3.4.1.f) when proposing changes to configuration values or structure.
*   **Steps:** Use `grep_search` to identify potential usage locations codebase-wide. State findings and confirm compatibility or necessary updates in plan.

**`Procedure: Verify Reapply Diff`** (Called by Step 4.4.1.a)
*   **Purpose:** To meticulously verify the diff applied by the `reapply` tool.
*   **Trigger:** Called by Step 4.4.1.a immediately after a `reapply` tool call completes.
*   **Steps:**
    1.  **Treat Diff as New:** Approach with extreme skepticism.
    2.  **Perform Core Diff Verification:** **MUST** execute `Procedure: Verify Diff` (Section 4) on the *actual diff applied by `reapply`*. The 'intent' for this verification is the file state *before* the `reapply` call. **Emphasize extra scrutiny due to `reapply` context.**
    3.  **Structured Log:** **Immediately after `reapply` result,** **MUST** generate structured log confirming the execution and **overall outcome** of `Procedure: Verify Diff` (Step 2), explicitly mentioning the handling of any deviations. Use format similar to Step 4.2.2, noting it's post-reapply.

**`Procedure: Verify Edit File Diff`** (Called by Step 4.4.1.b)
*   **Purpose:** To meticulously verify the diff applied by the standard `edit_file` tool.
*   **Trigger:** Called by Step 4.4.1.b immediately after a standard `edit_file` tool call completes.
*   **Steps:**
    1.  **Perform Core Diff Verification:** **MUST** execute `Procedure: Verify Diff` (Section 4) on the *actual diff applied by `edit_file`*. The 'intent' for this verification is the *final intended proposal from Step 4.2 (specifically, the verified proposed `code_edit` diff after `Procedure: Verify Diff` execution in Step 4.2.1.b)* (incorporating any handled deviations from the pre-apply check).
    2.  **Discrepancy Handling:** If the overall outcome of `Procedure: Verify Diff` (Step 1) is not 'Verified' (or 'Verified with handled deviations') and cannot be justified/corrected, **trigger self-correction (Step 4.4.3)**.
