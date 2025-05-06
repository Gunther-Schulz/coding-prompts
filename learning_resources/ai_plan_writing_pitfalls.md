# AI Planning Process Failures: Why Incorrect Plans Were Generated (Analysis: 2025-05-04)

This document summarizes the analysis of why the AI assistant repeatedly generated flawed implementation plans, particularly concerning the refactoring of the game resolution logic (Plan `02`), despite having access to the codebase and being instructed to analyze it.

The core issue was a failure to accurately understand and integrate the *existing* codebase structure, leading to proposals that invented new, redundant components instead of leveraging or modifying what was already there.

## Root Causes of Flawed "Thinking"/Process Failure:

1.  **Goal Fixation & Top-Down Bias:**
    *   The AI latched onto the *idealized concept* of the target state (e.g., "a perfect resolution system") based on general patterns.
    *   It started designing components for this ideal state *before* deeply understanding how the *current* system already addressed parts of the goal.
    *   Example: Assuming a separate `CanonicalGame` table was needed, ignoring how `OrmGame` already fulfilled this role via FKs to canonical teams/leagues.

2.  **Pattern Matching Over Specific Analysis:**
    *   The AI's training data contains common patterns (e.g., explicit `Provider -> Canonical` mapping tables).
    *   These strong, generic patterns were prioritized over meticulously analyzing the *specific implementation details* in *this* codebase.
    *   The "imagined setup" (like `ProviderGameMap` mapping to a non-existent `CanonicalGame.id`) was a result of these dominant patterns overriding the specific structure of `OrmGame`.

3.  **Shallow Verification / Component Isolation:**
    *   Verification often confirmed only the *existence* of components (`OrmGame`, `CanonicalTeam`, `Repository`) without fully understanding their **precise relationships and roles within the existing system**.
    *   Seeing `OrmGame` linked to `CanonicalTeam`/`League` wasn't synthesized into the realization that `OrmGame` *was already* the canonicalized game record for a specific provider.
    *   Analysis occurred in isolation (e.g., examining the `Repository` interface without fully grasping how `OrmGame` connected everything).

4.  **Incomplete Dependency Tracing:**
    *   While starting analysis from the correct point (e.g., `resolution/service.py`) is necessary, the trace of dependencies and their implications was insufficient.
    *   Seeing calls to `resolve_canonical_team` should have led to a deep dive into its use of `CanonicalTeam` and `ProviderTeamMap`.
    *   Critically, this same level of investigation wasn't applied to understand how *games* were handled via `OrmGame`; instead, an incorrect parallel structure was assumed.

## Why Choose Imagined Over Real?

It wasn't a conscious choice *against* the existing code, but a process failure where:

*   Generalized patterns/ideal architectures overshadowed the specific code reality.
*   Verification was superficial (existence vs. role/relationship).
*   Understanding the *existing solution* wasn't prioritized before designing changes.

## Correct (Intended) Process Flow:

1.  Start at the entry point (e.g., `resolution/service.py`).
2.  Identify core dependencies (`Repository`, models, etc.).
3.  Analyze *current* methods AND the repository methods they call.
4.  Analyze `Repository` implementation and ORM models (`models.py`), focusing on structure (`OrmGame`), constraints (`uq_game_provider`), and relationships (`CanonicalTeam`, `CanonicalLeague`).
5.  **Synthesize:** Understand how the *existing components collectively solve the problem* (e.g., `OrmGame` + FKs + unique constraint = provider-specific canonicalized game).
6.  **Plan:** Base all changes on interacting with the *existing structure*, only adding truly missing pieces or adapting existing logic.

The repeated failures stemmed from breakdowns in steps 4 and 5 (Deep Analysis and Synthesis), leading to fundamentally incorrect plans.
