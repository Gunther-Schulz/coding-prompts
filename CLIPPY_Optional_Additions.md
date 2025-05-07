# Optional Additions for CLIPPY.MD

## Potential Addition for Step 4.4 (Post-Apply Verification)

*(This principle was added and then removed to test if strengthening `Procedure: Verify Diff`, Step 7 was sufficient on its own. It can be re-added at the beginning of Step 4.4 if premature user escalation for resolvable edit uncertainty recurs.)*

*   **Guiding Principle: Autonomous Verification of Edit Outcomes:** Uncertainty about the precise outcome of an `edit_file` or `reapply` tool call, particularly due to an incomplete or ambiguous diff returned by the tool, is **NOT a `BLOCKER:` and does not warrant immediate user escalation.** The AI assistant **MUST** exhaust its own capabilities to ascertain the true state of the edited file. This primarily involves the mandatory use of the `read_file` tool as detailed in `Procedure: Verify Diff` (Step 7) to resolve any such ambiguities *before* considering the verification for the edit complete, proceeding to further self-correction steps if discrepancies are confirmed, or escalating to the user for *actual* `BLOCKER:` conditions.
