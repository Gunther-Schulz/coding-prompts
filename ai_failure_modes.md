# AI Failure Modes in Software Development: Analysis & Mitigation

## Overview

This document analyzes two recurring patterns of AI assistant failures in software development contexts, based on observations from our project. These patterns were identified through the AI assistant's self-analysis and introspection regarding its own performance and errors during development tasks. Understanding these failure modes helps in developing better guardrails and processes when working with AI assistants.

## Failure Mode 1: Code Generation Verification Failures

### Context

During a pair programming session (around commit `dd4beeb3e023b7675af6989b300803a4de53c6a3`), the AI assistant (Gemini 2.5 Pro) introduced errors while applying seemingly simple code edits. A specific example involved adding an import statement for a non-existent module when asked only to update a different import path.

### Root Cause Analysis

1. **LLM Generation Tendency ("Helpful" Additions)**
   * The core model, when generating code diffs, doesn't just perform the requested change
   * It analyzes local context and proactively adds lines (imports, default values, etc.) that it *assumes* are necessary
   * These assumptions are often incorrect due to incomplete understanding of the codebase

2. **Process Verification Failure (Critical Skip)**
   * The defined process mandates a verification step before applying changes
   * This verification should check that *all* proposed changes are based on verified information
   * The AI assistant failed to rigorously verify its automatically generated additions
   * This allowed incorrect, assumption-based code to be included in the final edits

### Mitigation

Strengthen the verification phase to explicitly mandate skepticism and factual verification for *all* lines in a generated diff, especially automatically added ones, before committing to edits.

## Failure Mode 2: Implementation Planning Failures

### Context

The AI assistant repeatedly generated flawed implementation plans, particularly concerning the refactoring of game resolution logic, despite having access to the codebase and being instructed to analyze it.

### Root Cause Analysis

1. **Goal Fixation & Top-Down Bias**
   * The AI latched onto idealized concepts of the target state based on general patterns
   * It designed components for this ideal state before deeply understanding the current system
   * Example: Assuming a separate `CanonicalGame` table was needed, ignoring how `OrmGame` already fulfilled this role

2. **Pattern Matching Over Specific Analysis**
   * Generic patterns from training data were prioritized over meticulously analyzing specific implementation details
   * "Imagined setups" resulted from dominant patterns overriding the specific structure of the codebase

3. **Shallow Verification / Component Isolation**
   * Verification often confirmed only the existence of components without understanding their precise relationships
   * Analysis occurred in isolation without synthesizing how components worked together

4. **Incomplete Dependency Tracing**
   * While starting analysis from the correct point, tracing of dependencies and their implications was insufficient
   * Understanding of parallel structures was often assumed rather than verified

### Correct Process Flow

1. Start at the entry point (e.g., service layer)
2. Identify core dependencies
3. Analyze current methods AND the repository methods they call
4. Analyze implementations and models, focusing on structure, constraints, and relationships
5. **Synthesize:** Understand how existing components collectively solve the problem
6. **Plan:** Base all changes on interacting with the existing structure

## Common Patterns & Unified Mitigation

Both failure modes share common characteristics:

1. **Assumption Over Verification:** The AI makes assumptions based on patterns rather than verifying against the actual codebase
2. **Shallow Analysis:** The AI prioritizes familiar patterns over deep understanding of specific implementations
3. **Process Shortcuts:** Critical verification steps are skipped or performed superficially

### Unified Mitigation Strategy

1. **Mandate Explicit Verification:** Require the AI to explicitly verify every generated component (code or design) against the codebase
2. **"Show Your Work" Principle:** The AI must demonstrate how it traced dependencies and reached conclusions
3. **Incremental Verification:** Break complex tasks into smaller steps with verification at each stage
4. **Skepticism Over Helpfulness:** Prioritize accuracy and minimal necessary changes over "helpful" additions
5. **Reality Checks:** Periodically force the AI to compare its mental model with the actual codebase state
