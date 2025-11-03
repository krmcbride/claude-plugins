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

**When done:** Launch the `ultraplan-investigator` agent with your findings:

```
I'm launching the ultraplan-investigator agent to review my initial findings and guide the next investigation step.
```

Pass the agent:

- Current step number
- Confidence level
- Findings from this step
- Files examined
- Relevant context identified
- Current hypotheses

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
4. Launch investigator agent again for next guidance
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

- Launch the `ultraplan-analyzer` agent for independent validation
- Pass ALL accumulated state (all findings from all steps)
- Include full file paths for the analyzer to read
- The analyzer acts as a senior engineering collaborator who will:
  - Validate your conclusions
  - Identify any gaps or overlooked aspects
  - Challenge assumptions
  - Provide practical recommendations with trade-offs

## Output Format

Present your final analysis in this structure:

```markdown
# UltraPlan Analysis: [Problem Statement]

## Investigation Summary

- **Total Steps:** X
- **Files Analyzed:** Y
- **Final Confidence:** [level]
- **Duration:** [time if relevant]

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

**Description:** [What this approach does and what problem it solves]

**Pros:**

- [Key advantage 1]
- [Key advantage 2]

**Cons:**

- [Key disadvantage or limitation 1]
- [Key disadvantage or limitation 2]

### Option 3 (if applicable): [Third Approach]

**Description:** [What this approach does and what problem it solves]

**Pros:**

- [Key advantage 1]
- [Key advantage 2]

**Cons:**

- [Key disadvantage or limitation 1]
- [Key disadvantage or limitation 2]

### What NOT to Do

[Tempting but problematic approaches to avoid, with brief explanation of why]

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
- **Use the agents** - They provide independent perspective and prevent blind spots
- **Track confidence honestly** - Don't inflate or deflate your assessment
- **Include specifics** - Cite file paths with line numbers where relevant
- **If you need more context** - Ask the user for additional information
- **If stuck** - Use the investigator agent to get unstuck with fresh perspective

---

**Begin your investigation now. Start with Step 1 at confidence level "exploring".**
