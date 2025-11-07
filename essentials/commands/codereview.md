---
description: Conduct systematic multi-step code review with severity-classified findings and actionable fixes
argument-hint: files to review and optional focus areas
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
  "relevant_files": ["/absolute/path/to/file1.py"],
  "issues_found": [
    {
      "severity": "critical",
      "description": "SQL injection in user query construction",
      "location": "auth.py:45",
      "impact": "Attackers can execute arbitrary SQL commands"
    },
    {
      "severity": "high",
      "description": "Race condition in token refresh logic",
      "location": "auth.py:123-145",
      "impact": "Token invalidation and potential security bypass"
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

**When done:** Launch the `codereview-investigator` agent with your findings:

```
I'm launching the codereview-investigator agent to review my initial findings and guide the next investigation step.
```

Pass the agent:

- Current step number and confidence level
- Findings from this step
- Files examined
- Issues found with severity classifications
- Areas of concern that need deeper investigation

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
5. Launch investigator agent again for next guidance
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

- Launch the `codereview-analyzer` agent for independent validation
- Pass ALL accumulated state (all findings from all steps)
- Include full file paths for the analyzer to read
- The analyzer acts as a senior code reviewer who will:
  - Critically evaluate your findings (not blindly accept)
  - Cross-reference with independent code analysis
  - Validate severity classifications
  - Provide actionable recommendations with code examples
  - Identify top 3 priority fixes

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
- [Recommendation for additional testing or monitoring]

## Confidence Assessment

[Explain why you reached your final confidence level. What would increase confidence further? What areas might need additional review?]
```

## Code Review Principles

Throughout this process:

1. **Be specific with line numbers** - Always cite exact locations (file.py:line)
2. **Provide code examples** - Show before/after for every fix
3. **Focus on actual issues found** - Don't suggest unrelated improvements
4. **Balance ideal vs. achievable** - Be pragmatic, not perfectionist
5. **Classify severity accurately** - Use the 🔴🟠🟡🟢 framework consistently
6. **Avoid wholesale migrations** - Don't suggest framework changes or massive refactors unless truly justified
7. **Prioritize actionability** - Every finding needs a concrete fix
8. **Consider real-world constraints** - Balance security/performance with maintainability

## Special Instructions

- **Read actual code, not summaries** - Use Read tool extensively
- **Check all code paths** - Including error handling and edge cases
- **Look for patterns** - If you find one SQL injection, check for more
- **Validate severity** - CRITICAL should be reserved for actual security/data loss risks
- **Include impact analysis** - Explain what can go wrong for HIGH and CRITICAL issues
- **Use the agents** - They provide independent perspective and validation
- **Track confidence honestly** - Don't inflate or deflate your assessment
- **If you need more context** - Ask the user for additional files or information

---

**Begin your code review now. Start with Step 1 at confidence level "exploring".**
