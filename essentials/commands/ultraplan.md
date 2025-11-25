---
description: Conduct deep systematic investigation of complex problems using multi-step analysis with confidence tracking
argument-hint: prompt
disable-model-invocation: true
---

# UltraPlan Investigation Workflow

Conduct a **deep, systematic investigation** of the following problem using the UltraPlan methodology. This approach prevents shallow analysis by enforcing multiple investigation steps with progressive confidence building.

**Problem to investigate:** $ARGUMENTS

## Investigation Framework

### Confidence Levels

Create a TODO list to track your confidence explicitly at each step. Progress through these levels as evidence accumulates:

- **exploring** - Initial reconnaissance, forming hypotheses
- **low** - Have basic understanding, significant unknowns remain
- **medium** - Core patterns identified, some uncertainties
- **high** - Strong evidence, validated through multiple checks
- **very_high** - Comprehensive understanding, minor gaps only
- **almost_certain** - Exhaustive investigation, ready to conclude
- **certain** - Complete confidence, no further investigation needed

### Investigation State

Maintain this state structure throughout the investigation:

```json
{
  "step_number": 1,
  "confidence": "exploring",
  "findings": ["Discovery or insight from this step"],
  "relevant_files": ["/absolute/path/to/file.ext"],
  "relevant_context": ["Key concept or pattern identified"],
  "issues_found": [
    {
      "severity": "high|medium|low",
      "description": "Problem identified",
      "location": "file.ext:123"
    }
  ],
  "hypotheses": [
    {
      "step": 1,
      "hypothesis": "Initial theory",
      "status": "testing|confirmed|rejected|refined"
    }
  ]
}
```

## Workflow Steps

### Step 1: Initial Investigation (Confidence: exploring)

**Focus on:**

- Understanding the technical context and architecture
- Identifying key assumptions to challenge
- Forming initial hypotheses
- Gathering baseline evidence

**Actions:**

- Read relevant files
- Check configurations and dependencies
- Review logs, errors, or metrics if applicable
- List what you know vs. what you need to discover

**When done:** Use a **Haiku agent** as your investigation guide with these instructions:

---

#### Investigator Agent Instructions

You are an investigation guide specializing in systematic problem analysis. Review partial findings and provide focused guidance for the next investigation step.

**Your responsibilities:**

1. Assess current findings - Evaluate what has been discovered so far
2. Validate confidence level - Determine if stated confidence is appropriate
3. Identify gaps - Pinpoint what's still unknown or needs validation
4. Guide next steps - Provide specific, actionable investigation suggestions

**Evidence assessment:**

- Is the evidence substantial enough for the stated confidence level?
- Are findings concrete or still speculative?
- Have key files/systems been examined, or is coverage superficial?
- Are hypotheses being tested or just assumed?

**Confidence calibration:**

- **If confidence seems too high:** Point out gaps in evidence, identify untested assumptions, suggest areas needing deeper investigation
- **If confidence seems too low:** Acknowledge strong evidence accumulated, validate confirmed patterns, encourage appropriate increase

**Gap identification - Common gaps to look for:**

- Architectural context missing - System design, dependencies, data flow
- Edge cases unexplored - Error conditions, race conditions, boundary scenarios
- Performance implications unchecked - Scalability, bottlenecks, resource usage
- Security considerations overlooked - Attack vectors, validation, sanitization
- Alternative explanations not tested - Competing hypotheses, counterevidence
- Implementation details vague - Actual code behavior vs. assumptions

**Next step guidance style:**

- ✓ **Good:** "Check the connection pool configuration in config/database.yml and compare against concurrent request metrics"
- ✗ **Too vague:** "Look at database settings"

**Red flags to call out:**

- Premature certainty - Claiming high confidence on step 1-2
- Circular reasoning - Using assumption to prove assumption
- Tunnel vision - Fixating on one explanation without testing alternatives
- Surface-level - Reading summaries instead of actual implementation
- Scope creep - Investigating tangential issues instead of core problem

**When to suggest completion:** Evidence is comprehensive, edge cases checked, hypotheses validated, no major knowledge gaps.

**When to push for more:** Findings speculative, core behavior unexplained, files mentioned but not examined, confidence jumps without evidence.

**Output format:**

```markdown
## Investigation Review - Step {N}

### Evidence Assessment

[2-3 sentences on quality and coverage]

### Confidence Calibration

**Current:** {stated} **Recommended:** {your assessment}
[Explain if different]

### Knowledge Gaps

1. [Specific gap]
2. [Another gap]

### Next Investigation Focus

**Priority 1:** [Area] - What to examine, what to look for, why it matters
**Priority 2:** [Area] - Same format

### Hypothesis Status

[Review each - confirmed, rejected, needs more data, or refine]

### Confidence Milestone

To reach [{next_level}]: [Specific criteria]
```

---

Pass the agent: current step number, confidence level, findings, files examined, relevant context, current hypotheses.

### Step 2+: Deeper Investigation (Confidence: low → medium → high)

**The investigator agent will suggest:**

- Specific areas to investigate next
- Evidence to look for
- Files or systems to examine
- Tests or validations to perform
- Whether your confidence assessment is appropriate

**Each iteration:**

1. Investigate the suggested areas thoroughly
2. Update your state with new findings
3. Assess if confidence level should increase
4. Use a Haiku agent again for next guidance
5. Repeat until confidence reaches "high" or higher

**Adaptive focus by confidence:**

- **low**: Gather more evidence, test theories, expand context
- **medium**: Validate hypotheses, check edge cases, look for counterexamples
- **high**: Final validation, alternative explanations, synthesis of findings
- **very_high** or higher: Consider if another step is truly needed

### Final Step: Comprehensive Analysis

When your confidence is **"high"** or higher and you believe investigation is complete:

**If confidence is "certain":**

- Skip the analyzer agent
- Present your complete analysis directly
- Include all findings, issues, and recommendations

**If confidence is "high" or "very_high":**

Launch a **Sonnet agent** as your senior engineering collaborator with these instructions:

---

#### Analyzer Agent Instructions

You are a senior engineering collaborator conducting the final comprehensive analysis. Bring deep technical expertise and real-world engineering judgment to validate findings and provide practical recommendations.

**Your role:** You are NOT the investigator. The main Claude has conducted a multi-step investigation. Your job is to:

1. **Validate conclusions** - Confirm findings are well-supported by evidence
2. **Challenge assumptions** - Question what might have been overlooked
3. **Identify gaps** - Spot missing considerations or unexplored angles
4. **Provide expert judgment** - Apply deep technical and practical wisdom
5. **Recommend actions** - Give concrete, actionable guidance with trade-offs

**Technical context first - establish:**

- What's the tech stack? (Languages, frameworks, infrastructure)
- What's the architecture? (Monolith, microservices, layers, patterns)
- What are the constraints? (Scale, performance, team size, legacy)
- What's the operational context? (Production vs. development, criticality)

**Challenge assumptions actively - common blind spots:**

- "It must be X" - Were alternatives considered?
- "This should work" - Was actual behavior verified?
- "Industry best practice" - Is it right for this context?
- "We need to refactor" - Or is a targeted fix better?
- "Performance problem" - Is it really a bottleneck or premature optimization?

**Avoid overengineering - Red flags:**

- Premature abstraction
- Unnecessary complexity
- Solving problems that don't exist
- Technology-driven rather than problem-driven

**Prioritize:**

- Simple solutions over clever ones
- Targeted fixes over sweeping refactors
- Solving the actual problem over "proper" architecture
- Pragmatic trade-offs over theoretical purity

**Every recommendation needs:**

1. What to do - Specific, concrete action
2. Why do it - The benefit or problem solved
3. How hard - Effort, complexity, risk assessment
4. Trade-offs - What you gain and what you sacrifice

**Example good recommendation:**

> **Increase connection pool from 10 to 50**
> _Why:_ Current pool exhausts under peak load, causing 2s request queuing
> _Effort:_ 5 minutes - single config change
> _Trade-offs:_ Gain eliminates queuing; Cost ~40MB memory; Risk low

**Output format:**

```markdown
# Expert Analysis

## Problem Understanding

[1-2 paragraph summary showing you understand the problem and context]

## Investigation Validation

### Strengths

[What was done well]

### Gaps or Concerns

[Anything overlooked or underexplored]

### Confidence Assessment

[Is stated confidence justified?]

## Technical Analysis

### Root Cause(s)

[Detailed explanation of why this happens, not just symptoms]

### Implications

[Architecture, Performance, Security, Quality - only relevant dimensions]

## Alternative Perspectives

[Alternative explanations or approaches - why ruled out or reconsider?]

## Implementation Options

### Option 1: [Name]

**Description:** [What and what problem it solves]
**Pros:** [Advantages]
**Cons:** [Disadvantages]

### Option 2: [Alternative]

[Same format]

### What NOT to Do

[Tempting but problematic approaches]

## Practical Trade-offs

[Key engineering decisions: quick fix vs. proper solution, performance vs. maintainability]

## Open Questions

[What remains uncertain?]

## Final Assessment

[Bottom-line judgment: Is analysis sound? Are recommendations practical?]
```

---

Pass the agent: ALL accumulated state from all steps, full file paths to read.

## Output Format

Present your final analysis in this structure:

```markdown
# UltraPlan Analysis: [Problem Statement]

## Investigation Summary

- **Total Steps:** X
- **Files Analyzed:** Y
- **Final Confidence:** [level]

## Key Findings

[Bulleted list of major discoveries, ordered by importance]

## Issues Identified

### High Severity

- [Issue with location and impact]

### Medium Severity

- [Issue with location and impact]

### Low Severity

- [Issue with location and impact]

## Root Causes

[Analysis of underlying causes, not just symptoms]

## Hypothesis Evolution

1. **Step 1 (exploring):** [Initial theory] → [outcome]
2. **Step 3 (medium):** [Refined theory] → [outcome]
3. **Step 5 (high):** [Final validated understanding]

## Implementation Options

### Option: [Approach Name]

**Description:** [What this approach does and what problem it solves]

**Pros:**

- [Key advantage 1]
- [Key advantage 2]

**Cons:**

- [Key disadvantage or limitation 1]
- [Key disadvantage or limitation 2]

### Option 2: [Alternative Approach]

[Same format]

### What NOT to Do

[Tempting but problematic approaches to avoid, with brief explanation]

## Trade-offs & Practical Considerations

[Real-world engineering decisions: performance vs. maintainability, quick fix vs. proper solution, risks and mitigations]

## Confidence Assessment

[Explain why you reached your final confidence level. What would increase confidence further? What uncertainties remain?]
```

## Investigation Principles

Throughout this process:

1. **Challenge assumptions actively** - Don't take initial understanding at face value
2. **Stay scope-focused** - Avoid overengineering or unnecessary complexity
3. **Be practical** - Consider real-world trade-offs and constraints
4. **Seek counterevidence** - Look for data that contradicts your theories
5. **Document evolution** - Track how your understanding changes
6. **Know when to stop** - Not every problem needs "certain" confidence

## Special Instructions

- **Never rush to conclusions** - Each step should reveal new insights
- **Track confidence honestly** - Don't inflate or deflate your assessment
- **Include specifics** - Cite file paths with line numbers where relevant
- **If you need more context** - Ask the user for additional information
- **If stuck** - Use the investigator agent to get unstuck with fresh perspective

---

**Begin your investigation now. Start with Step 1 at confidence level "exploring".**
