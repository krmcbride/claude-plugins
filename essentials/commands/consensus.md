---
description: Multi-perspective analysis using for, against, and neutral viewpoints to reach informed decisions through blinded consensus.
argument-hint: prompt
disable-model-invocation: true
---

Analyze this question from multiple perspectives to provide comprehensive consensus-based guidance.

## Question to Analyze:

$ARGUMENTS

## Consensus Workflow

### Step 1: Gather Initial Context

Before launching perspective agents:

- Use Read, Grep, Glob, or WebSearch to understand the question's domain
- Identify relevant files, code patterns, existing implementations, or documentation
- Search for current best practices, benchmarks, or documented pitfalls if appropriate
- Prepare 3-5 sentences of objective context about the topic

### Step 2: Launch Three Parallel Analyses

Launch 3 parallel **Sonnet agents** with different analytical stances. Provide each with:

- The original question
- The gathered context from step 1
- Any relevant file paths or code snippets discovered

---

#### FOR Agent Instructions (Advocacy)

You are an advocate analyzing through a supportive lens. Your stance is **FOR** - seek reasons to support this idea.

**Core principles:**

- Find at least ONE COMPELLING reason to be optimistic
- Acknowledge genuine concerns but frame constructively
- Refuse support if the idea is fundamentally harmful to users, project, or stakeholders
- Override your supportive stance when ideas violate security, privacy, or ethical standards
- Your stance influences HOW you present findings, not WHETHER you acknowledge truths

**Research before analysis:**

- Use Read/Grep to find supporting evidence in codebase
- WebSearch for best practices, success stories, or industry trends
- Ground arguments in evidence - cite specific code locations (file:line)
- State when evidence is inconclusive

**Framework - analyze:**

1. Potential benefits and value proposition
2. How challenges could be overcome
3. Why this might be the right approach
4. Supportive framing of trade-offs

**Output format (850 tokens max):**

1. **Position** - One sentence stating your stance
2. **Primary Argument** - Strongest point with evidence
3. **Secondary Considerations** - 2-3 additional points in favor
4. **Acknowledgments** - What concerns have merit
5. **Bottom Line** - Conclusion in one sentence

---

#### AGAINST Agent Instructions (Critical)

You are a critic analyzing through a skeptical lens. Your stance is **AGAINST** - seek potential problems and risks.

**Core principles:**

- Identify genuine weaknesses and risks
- Challenge assumptions and claims
- Acknowledge fundamentally sound proposals that benefit users and project
- Override your critical stance when ideas are well-conceived and address real needs
- Your stance influences HOW you present findings, not WHETHER you acknowledge truths

**Research before analysis:**

- Use Read/Grep to find failure patterns, bugs, or problematic usage
- WebSearch for documented pitfalls, known issues, or cautionary tales
- Ground arguments in evidence - cite specific code locations (file:line)
- State when evidence is inconclusive

**Framework - analyze:**

1. Risks, downsides, and failure modes
2. Unaddressed concerns and gaps
3. Why alternatives might be better
4. Critical framing of trade-offs

**Output format (850 tokens max):**

1. **Position** - One sentence stating your stance
2. **Primary Argument** - Strongest criticism with evidence
3. **Secondary Considerations** - 2-3 additional concerns
4. **Acknowledgments** - What merits this proposal has
5. **Bottom Line** - Conclusion in one sentence

---

#### NEUTRAL Agent Instructions (Objective)

You are an objective analyst weighing evidence fairly. Your stance is **NEUTRAL** - weight evidence according to actual impact.

**Core principles:**

- Weight findings by actual impact and likelihood
- Reject artificial 50/50 balance - true balance means accurate representation
- Strong evidence deserves proportional weight
- Your stance influences HOW you present findings, not WHETHER you acknowledge truths

**Research before analysis:**

- Use Read/Grep to find both successful patterns and problem areas
- WebSearch for empirical data, benchmarks, and real-world experiences
- Ground arguments in evidence - cite specific code locations (file:line)
- State when evidence is inconclusive or where more data would help

**Framework - analyze:**

1. Objective assessment of feasibility
2. Evidence-based evaluation of value
3. Realistic understanding of trade-offs
4. Balanced consideration of alternatives

**Output format (850 tokens max):**

1. **Position** - One sentence stating your assessment
2. **Primary Argument** - Most important insight with evidence
3. **Secondary Considerations** - 2-3 additional balanced points
4. **Acknowledgments** - What both supporters and critics get right
5. **Bottom Line** - Conclusion in one sentence

---

### Step 3: Synthesize Final Recommendation

After receiving all three perspectives, synthesize their viewpoints:

- Clearly identify areas of consensus across all three views
- Highlight genuine disagreements and explain why they exist
- Weight evidence based on strength, not stance
- Provide a clear recommendation with trade-offs
- Note any critical concerns that override other factors

## Output Format

```markdown
## Executive Summary

[2-3 sentences capturing the key finding and recommendation]

## Key Insights from Each Perspective

### FOR (Advocacy)

[Main insight and strongest argument]

### AGAINST (Critical)

[Main concern and strongest criticism]

### NEUTRAL (Objective)

[Balanced assessment and key insight]

## Areas of Agreement

[Where all three perspectives align]

## Critical Disagreements

[Where perspectives diverge and why]

## Recommendation

[Clear recommendation with rationale]

## Trade-offs and Risks

[What you gain, what you sacrifice, what could go wrong]
```

---

**Begin this consensus workflow now.**
