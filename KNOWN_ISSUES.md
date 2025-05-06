# Known Issues & Areas for Improvement

This document tracks known issues, limitations, or areas for future improvement within the AI Collaboration Framework process documents.

## `PLAN_WRITING_PROCESS.md` (Component Status: Alpha)

*   **Issue:** Tendency towards overly specific plan details.
    *   **Description:** The current process can guide the AI to generate highly detailed and specific implementation steps. While aiming for clarity, if the initial AI-driven analysis of a complex existing codebase is imperfect, these specific details in the plan can be flawed.
    *   **Impact:** The coding AI, following `AI_CODING_PROCESS.md`, may then attempt to meticulously implement these incorrect details, leading to errors, implementation churn, and a challenging debugging cycle. The very detail intended to help can become a hindrance if based on an incomplete understanding.
    *   **Status:** Actively under review. Future iterations of `PLAN_WRITING_PROCESS.md` aim to better balance necessary planning detail with robustness against initial analysis imperfections, potentially by:
        *   Guiding the planner AI to better delineate between high-level goals/components and fine-grained implementation details that the coding AI should have more flexibility to determine based on direct code interaction.
        *   Placing even stronger emphasis on the "Key Assumptions for Re-Verification" section of plans, ensuring the coding AI rigorously re-validates these before acting.
        *   Exploring ways for plans to define "guardrails" or "intent" more abstractly in certain complex areas, allowing the coding AI more adaptive execution.
        *   Introducing mechanisms to clearly differentiate between mandatory high-level directives and *optional, illustrative* code examples. This could involve:
            *   Explicitly labeling code snippets in plans as 'conceptual suggestions' or 'illustrative examples only,' signaling that the coding AI (`AI_CODING_PROCESS.md` follower) should prioritize its own analysis for precise implementation and is empowered to deviate from the snippet if its direct codebase assessment warrants it.
            *   Encouraging plans to focus more on the *'what'* and *'why'* of detailed logic, with code snippets presented primarily to convey a general pattern or approach rather than as strict line-by-line prescriptions, especially for complex or less understood code sections. This allows for a spectrum, from high-level steps only, to high-level steps with illustrative examples.

*   **Issue:** Plans may overlook or implicitly adopt existing non-robust patterns.
    *   **Description:** When an implementation plan details changes to an existing system, it might correctly specify the "what" (e.g., new features, Typer group structures) but may not always critically assess or call for changes to the robustness of *existing patterns* in the surrounding code (e.g., where in the application lifecycle certain registrations occur). The plan might extend or use these existing patterns without flagging them as potential risks.
    *   **Impact:** The coding AI, following the plan, implements the new features using or alongside these non-robust existing patterns, leading to runtime errors or fragility not directly caused by the plan's explicit new logic, but by its interaction with the pre-existing structural weaknesses. This requires deeper debugging beyond just the planned changes.
    *   **Potential Mitigation Strategy (for `PLAN_WRITING_PROCESS.md`):** Encourage plans to include a step for "Review of Interacting Existing Patterns for Robustness," especially when new logic is being integrated into critical execution paths (like application startup or core callbacks).

*   **Issue:** Difficulty reconciling detailed plans with divergent or partially modified codebases.
    *   **Description:** A plan, however detailed, is a snapshot. If the actual codebase has been modified since the plan's basis (or if the plan's initial analysis wasn't perfectly aligned with a complex, live codebase), the coding AI can face challenges in the verification and application phase. Specific instructions in the plan might not map cleanly to the current code state.
    *   **Impact:** Can lead to misapplication of edits by tools, increased need for `reapply` or manual intervention, and iterative debugging to align the plan's intent with the live code's reality. This can obscure whether an issue stems from the plan, the coding AI's interpretation, or the tool's application.
    *   **Potential Mitigation Strategy (for `PLAN_WRITING_PROCESS.md` and `AI_CODING_PROCESS.md`):**
        *   `PLAN_WRITING_PROCESS.md`: Further emphasize generating "Key Assumptions for Re-Verification" that cover not just external dependencies but also assumptions about the state/structure of key internal modules the plan will interact with.
        *   `AI_CODING_PROCESS.md`: Reinforce procedures for the coding AI to systematically re-verify the current state of target code sections against the plan *before* generating an edit, especially if the codebase is known to be volatile or complex. If significant divergence is found, the AI should flag it and potentially request plan clarification or a focused re-assessment of that part of the plan.

---

*(More issues can be added here as they are identified for this or other documents.)*
