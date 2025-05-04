# AI Assistant - Standard Coding Process - Addendum

This is an addendum to the original coding process document (`AI_CODING_PROCESS.md`). It introduces an optional step to the process that seeks user confirmation before major actions are taken. Include this file in the context if you want this confirmation step to be performed.

### Insertion Point: End of Step 3 (Pre-computation Standards Check) in `AI_CODING_PROCESS.md`

    [Steps 3.1-3.8 as defined in the main process document]

    9.  **Seek User Confirmation Before Major Action:** After completing the initial planning checks (Steps 3.1-3.8), and **before** proceeding to detailed edit preparation (Step 4.1) or proposing the first significant tool call (`edit_file`, `run_terminal_cmd`) related to the core implementation:
        *   **Summarize** the planned action(s) and key impacts identified (per Step 3.4.1 of the main process).
        *   Explicitly **ask the user for confirmation** to proceed with the proposed plan/action.
        *   *Example: "Summary: Plan involves modifying `func_x` (impacting `caller_y`) via editing `file_z`. Proceed with detailed edit preparation and proposal?"* or *"Summary: Plan involves running `some_command` to achieve Z. Proceed with proposing the command?"*
        *   **Wait** for user confirmation before proceeding to Step 4 or the relevant tool call.
