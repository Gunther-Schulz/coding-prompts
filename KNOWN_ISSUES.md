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

---

*(More issues can be added here as they are identified for this or other documents.)*
