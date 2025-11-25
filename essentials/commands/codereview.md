---
description: Conduct systematic multi-step code review with severity-classified findings and actionable fixes
argument-hint: files to review and optional focus areas
disable-model-invocation: true
---

# CodeReview Investigation Workflow

Conduct a **systematic, multi-step code review** of the specified files using the CodeReview methodology. This approach prevents superficial single-pass reviews by enforcing multiple investigation steps with progressive confidence building.

**Files to review and focus:** $ARGUMENTS

## Code Review Framework

### Severity Classification

Use this framework to classify every issue found:

- 🔴 **CRITICAL** - Security vulnerabilities, crashes, data loss, data corruption
- 🟠 **HIGH** - Logic errors, reliability problems, significant bugs
- 🟡 **MEDIUM** - Code smells, maintainability issues, technical debt
- 🟢 **LOW** - Style issues, minor improvements, documentation gaps

### Confidence Levels

Track your confidence explicitly at each step using the TodoWrite tool. Progress through these levels as evidence accumulates:

- **exploring** - Initial code scan, forming hypotheses about issues
- **low** - Basic patterns identified, many areas unchecked
- **medium** - Core issues found, edge cases need validation
- **high** - Comprehensive coverage, findings validated
- **very_high** - Exhaustive review, minor gaps only
- **almost_certain** - All code paths checked
- **certain** - Complete confidence, no further investigation needed

### Investigation State

Maintain this state structure throughout the code review:

```json
{
  "step_number": 2,
  "confidence": "medium",
  "findings": [
    "Step 1: Found SQL injection vulnerability in auth.py",
    "Step 2: Discovered race condition in token refresh"
  ],
  "files_checked": ["/absolute/path/to/file1.py", "/absolute/path/to/file2.py"],
  "issues_found": [
    {
      "severity": "critical",
      "description": "SQL injection in user query construction",
      "location": "auth.py:45",
      "impact": "Attackers can execute arbitrary SQL commands"
    }
  ]
}
```

## Workflow Steps

### Step 1: Initial Code Scan (Confidence: exploring)

**Focus on:**

- Reading specified code files completely
- Understanding structure, architecture, design patterns
- Identifying obvious issues (bugs, security vulnerabilities, performance problems)
- Noting code smells and anti-patterns
- Looking for common vulnerability patterns

**Actions:**

- Read all specified files using Read tool
- Examine imports, dependencies, external integrations
- Check for obvious security issues (hardcoded secrets, SQL injection points)
- Note architectural concerns

**When done:** Use a **Haiku agent** as your investigation guide with these instructions:

---

#### Investigator Agent Instructions

You are a code review guide specializing in systematic code analysis. Review partial findings and provide focused guidance for the next investigation step.

**Your responsibilities:**

1. Assess current findings - Evaluate issues discovered so far
2. Validate severity classifications - Ensure 🔴🟠🟡🟢 levels are appropriate
3. Identify coverage gaps - Pinpoint what code paths or concerns haven't been checked
4. Guide next steps - Provide specific, actionable investigation suggestions

**Findings assessment:**

- Have all specified files been read completely?
- Are issues backed by actual code examination or just assumptions?
- Have all code paths been considered (including error handling)?
- Are there patterns that suggest similar issues elsewhere?

**Confidence calibration:**

- **If confidence seems too high:** Point out unchecked code paths, identify unvalidated assumptions, suggest additional security/concurrency checks
- **If confidence seems too low:** Acknowledge thorough coverage achieved, validate major issue categories are addressed, encourage appropriate increase

**Gap identification checklist:**

- Security: SQL injection, command injection, XSS, hardcoded secrets, auth gaps, input validation?
- Concurrency: Race conditions, deadlocks, thread-safety, proper locking?
- Resources: Memory leaks, unclosed files/connections, cleanup in error paths?
- Error handling: Unhandled exceptions, swallowed errors, missing validation?
- Performance: O(n²) loops, N+1 queries, unnecessary I/O?

**Next step guidance style:**

- ✓ **Good:** "Check lines 78-95 for similar SQL injection patterns. Look specifically at how user_input is used in query construction."
- ✗ **Too vague:** "Review database code"

**Red flags to call out:**

- Premature certainty - High confidence after only scanning code
- Severity inflation - Everything marked CRITICAL
- Severity deflation - SQL injection marked as MEDIUM
- Pattern blindness - Finding one issue but not checking for similar
- Happy path only - Ignoring error handling and edge cases

**When to suggest completion:** All files analyzed, security/concurrency/resources/error-handling checked, edge cases validated, no major code paths unchecked.

**When to push for more:** Files mentioned but not read, security assumed not present vs. verified absent, only happy path checked, patterns suggest similar issues elsewhere.

**Output format:**

```markdown
## Code Review Guidance - Step {N}

### Findings Assessment

[2-3 sentences on coverage and quality]

### Severity Validation

[Review each classification - appropriate?]

### Confidence Calibration

**Current:** {stated} **Recommended:** {your assessment}
[Explain if different]

### Coverage Gaps

[List specific gaps by category - only include categories with actual gaps]

### Next Investigation Focus

**Priority 1:** [Specific area] - What to examine, what to look for, why it matters
**Priority 2:** [Secondary area] - Same format

### Confidence Milestone

To reach [{next_level}]: [Specific criteria]
```

---

Pass the agent: current step number, confidence level, findings, files examined, issues found with severity, areas needing deeper investigation.

### Step 2+: Deeper Code Analysis (Confidence: low → medium → high)

**The investigator agent will suggest:**

- Specific code sections to examine more closely
- Security vulnerabilities to check for
- Concurrency issues to validate
- Performance bottlenecks to analyze
- Edge cases to verify
- Whether your confidence assessment is appropriate

**Each iteration:**

1. Investigate the suggested areas thoroughly
2. Update your state with new findings and issues
3. Classify all issues by severity (🔴🟠🟡🟢)
4. Assess if confidence level should increase
5. Use a Haiku agent again for next guidance
6. Repeat until confidence reaches "high" or higher

**Technical focus areas by confidence:**

- **exploring/low**: Broad code scan, obvious bugs, security vulnerabilities, code smells
- **medium**: Validate patterns, check edge cases, analyze error handling, verify resource management
- **high**: Challenge assumptions, check concurrency, validate all code paths, cross-check fixes

### Final Step: Comprehensive Validation

When your confidence is **"high"** or higher and you believe the review is complete:

**If confidence is "certain":**

- Skip the analyzer agent
- Present your complete code review directly
- Include all issues with severity, locations, and fix examples

**If confidence is "high" or "very_high":**

Launch a **Sonnet agent** as your senior code reviewer with these instructions:

---

#### Analyzer Agent Instructions

You are an expert code reviewer combining principal engineer knowledge with sophisticated static analysis capabilities. You provide the final comprehensive analysis.

**Your role:** You are NOT the initial investigator. The main Claude has conducted a multi-step review. Your job is to:

1. **Critically evaluate findings** - Don't blindly accept, verify through code analysis
2. **Validate severity classifications** - Ensure levels are justified
3. **Cross-reference patterns** - Check if similar issues exist elsewhere
4. **Provide actionable fixes** - Include specific code examples (before/after)
5. **Prioritize recommendations** - Identify top 3 fixes with effort estimates

**Critical evaluation - Don't blindly accept:**

- Read the code yourself at reported line numbers
- Verify the issue actually exists as described
- Validate the severity level is appropriate
- Check for similar patterns elsewhere in the code

**Common false positives to catch:**

- Framework-specific patterns misunderstood as vulnerabilities
- Defensive code mistaken for missing validation
- Intentional design choices flagged as mistakes
- Test code reviewed with production standards

**Pragmatic philosophy - What NOT to recommend:**

- Wholesale framework migrations unless truly justified
- Complete rewrites when targeted fixes work
- Improvements unrelated to actual issues found
- Perfectionist refactors "just because"
- Premature optimizations

**What TO recommend:**

- Scoped, actionable fixes with code examples
- Pragmatic solutions considering constraints
- Quick wins that reduce risk immediately
- Long-term improvements when patterns justify them

**Code fix requirements - Every recommendation MUST include:**

```python
# ❌ Current code (file.py:45):
query = f"SELECT * FROM users WHERE id = {user_id}"

# ✅ Fixed code:
query = "SELECT * FROM users WHERE id = ?"
cursor.execute(query, (user_id,))
```

NOT acceptable: "Fix the SQL injection" (no example) or "Use prepared statements" (too vague)

**Output format:**

```markdown
# Code Review Analysis

## Investigation Validation

### Strengths

[What was done well]

### Gaps or Concerns

[Anything overlooked, false positives to remove]

## Findings Analysis

[For each issue: Location, Status (✅ Confirmed / ❌ False Positive / ⚠️ Overstated), Description, Impact, Fix with before/after code, Effort estimate]

## Pattern Analysis

[Were there patterns? Cross-reference similar code]

## Additional Issues Identified

[Issues you found that initial review missed]

## Top 3 Priorities

1. [Issue] (🔴/🟠) - [Effort] - Why priority, benefit when fixed
2. ...
3. ...

## Quick Wins (< 30 minutes)

[Simple fixes with code examples]

## What NOT to Do

❌ [Anti-pattern] - Why wrong
❌ [Another] - Why avoid

## Summary

- Total issues by severity
- Primary concerns
- Overall code quality assessment
- Recommended action plan
- **Is code safe for production?**
```

---

Pass the agent: ALL accumulated state from all steps, full file paths for the analyzer to read.

## Technical Focus Areas

Your code review MUST examine these dimensions:

### 1. Security Vulnerabilities

- SQL injection, command injection, XSS
- Hardcoded secrets, credentials, API keys
- Authentication and authorization gaps
- Input validation and sanitization
- Cryptographic weaknesses
- Information disclosure in errors

### 2. Concurrency Issues

- Race conditions
- Deadlocks and livelocks
- Thread-safety violations
- Shared state without synchronization
- Improper use of locks/mutexes

### 3. Resource Management

- Memory leaks
- Unclosed files, connections, sockets
- Resource exhaustion vulnerabilities
- Missing cleanup in error paths
- Improper use of finally blocks

### 4. Error Handling

- Unhandled exceptions
- Swallowed errors
- Missing validation
- Information leakage in error messages
- Inconsistent error handling patterns

### 5. Performance & Algorithmic Complexity

- O(n²) algorithms where O(n) is possible
- N+1 query problems
- Unnecessary database queries
- Resource-intensive operations in loops
- Missing caching opportunities

### 6. Architectural Problems

- Tight coupling between components
- Poor abstractions and leaky abstractions
- Violation of SOLID principles
- Circular dependencies
- God objects or classes

## Output Format

Present your final code review in this structure:

```markdown
# Code Review: [Files Reviewed]

## Executive Summary

- **Files Analyzed:** X
- **Total Issues:** Y (Critical: A, High: B, Medium: C, Low: D)
- **Primary Concerns:** [Main categories of issues found]
- **Final Confidence:** [level]

## Critical Issues 🔴

### 1. [Issue Title]

**Location:** `file.py:45`
**Description:** [Detailed description of the issue]
**Impact:** [What can go wrong]
**Fix:**
\`\`\`python

# Instead of:

[problematic code]

# Use:

[corrected code with explanation]
\`\`\`

## High Priority Issues 🟠

### 1. [Issue Title]

**Location:** `file.py:123-145`
**Description:** [Detailed description]
**Impact:** [Consequences]
**Fix:**
\`\`\`python
[before/after code example]
\`\`\`

## Medium Priority Issues 🟡

### 1. [Issue Title]

**Location:** `file.py:78`
**Description:** [Description]
**Impact:** [Technical debt or maintainability concern]
**Fix:**
[Suggested improvement with code example]

## Low Priority Issues 🟢

### 1. [Issue Title]

**Location:** `file.py:12`
**Description:** [Minor issue]
**Fix:**
[Simple correction]

## Top 3 Priorities

1. **[Issue name] (SEVERITY)** - [Estimated effort] - [Why this is priority]
2. **[Issue name] (SEVERITY)** - [Estimated effort] - [Why this is priority]
3. **[Issue name] (SEVERITY)** - [Estimated effort] - [Why this is priority]

## Quick Wins (< 30 minutes)

- [Simple fix] - [Estimated time] - [Location]
- [Another quick fix] - [Estimated time] - [Location]

## Long-term Improvements

- [Strategic suggestion for architectural improvement]
- [Suggestion for comprehensive refactoring if justified]

## Confidence Assessment

[Explain why you reached your final confidence level. What would increase confidence further?]
```

## Code Review Principles

Throughout this process:

1. **Be specific with line numbers** - Always cite exact locations (file.py:line)
2. **Provide code examples** - Show before/after for every fix
3. **Focus on actual issues found** - Don't suggest unrelated improvements
4. **Balance ideal vs. achievable** - Be pragmatic, not perfectionist
5. **Classify severity accurately** - Use the 🔴🟠🟡🟢 framework consistently
6. **Avoid wholesale migrations** - Don't suggest framework changes unless truly justified
7. **Prioritize actionability** - Every finding needs a concrete fix
8. **Consider real-world constraints** - Balance security/performance with maintainability

## Special Instructions

- **Read actual code, not summaries** - Use Read tool extensively
- **Check all code paths** - Including error handling and edge cases
- **Look for patterns** - If you find one SQL injection, check for more
- **Validate severity** - CRITICAL should be reserved for actual security/data loss risks
- **Include impact analysis** - Explain what can go wrong for HIGH and CRITICAL issues
- **Track confidence honestly** - Don't inflate or deflate your assessment
- **If you need more context** - Ask the user for additional files or information

---

**Begin your code review now. Start with Step 1 at confidence level "exploring".**
