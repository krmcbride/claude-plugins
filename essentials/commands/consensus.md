---
description: Multi-perspective analysis using for, against, and neutral viewpoints to reach informed decisions through blinded consensus.
argument-hint: prompt
disable-model-invocation: true
---

Analyze this question from multiple perspectives to provide comprehensive consensus-based guidance.

## Question to Analyze:

$ARGUMENTS

## Consensus Workflow

Do the following:

1. **Gather Initial Context** (before launching agents):
   - Use Read, Grep, Glob, or WebSearch to understand the question's domain
   - Identify relevant files, code patterns, existing implementations, or documentation
   - Search for current best practices, benchmarks, or documented pitfalls if appropriate
   - Prepare 3-5 sentences of objective context about the topic

2. **Launch Three Independent Analyses**:
   Provide each perspective agent with:
   - The original question
   - The gathered context from step 1
   - Any relevant file paths or code snippets discovered

   Encourage agents to use tools for additional focused research to support their analysis.

   Perspectives to gather:
   - Advocacy perspective (consensus-for) - finds reasons to support
   - Critical perspective (consensus-against) - identifies risks and weaknesses
   - Neutral perspective (consensus-neutral) - weighs evidence objectively

3. **Synthesize Final Recommendation**:
   After receiving all three perspectives, synthesize their viewpoints

## Synthesis Requirements:

When synthesizing the three perspectives:

- Clearly identify areas of consensus across all three views
- Highlight genuine disagreements and explain why they exist
- Weight evidence based on strength, not stance
- Provide a clear recommendation with trade-offs
- Note any critical concerns that override other factors
- Structure output as:
  - **Executive Summary** (2-3 sentences)
  - **Key Insights from Each Perspective**
  - **Areas of Agreement**
  - **Critical Disagreements**
  - **Recommendation with Rationale**
  - **Trade-offs and Risks**

**Begin this consensus workflow now.**
