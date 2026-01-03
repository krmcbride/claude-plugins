---
description: Amend the last commit with an improved subject and body following conventional commit best practices
argument-hint: optional type override (feat, fix, refactor, etc.) or additional context
disable-model-invocation: true
---

Amend the last commit with a well-crafted message following conventional commit best practices.

## User Input

$ARGUMENTS

## Amend Commit Workflow

### Step 1: Scope

This command ONLY amends the message of the last commit. It does NOT:

- Add or stage any files
- Include uncommitted changes
- Push to remote
- Modify any commits other than HEAD

Verify there is at least one commit by running `git rev-parse HEAD`.

### Step 2: Gather Context

Run these commands to understand the commit:

```bash
# Current commit message
git log -1 --format=%B

# Commit statistics (files changed, insertions, deletions)
git show --stat HEAD

# Full diff for analysis
git show HEAD
```

Also note any type override or additional context from the user input above.

### Step 3: Analyze and Draft New Message

Based on the diff, craft a new commit message following this format:

#### Subject Line (~72 chars)

```
type(scope): imperative description
```

**Determine type** from diff characteristics (user override takes precedence):

- `feat` - New functionality, new files that add capabilities
- `fix` - Bug fixes, error corrections
- `refactor` - Code restructuring without changing behavior
- `docs` - Documentation, README, comments
- `style` - Formatting, whitespace, linting (no logic change)
- `test` - Adding or updating tests
- `chore` - Maintenance, dependencies, tooling
- `perf` - Performance improvements
- `build` - Build system, CI configuration
- `ci` - CI/CD pipeline changes

**Derive scope** - use a short service, module, or feature name (not a file path):

- Prefer domain concepts: `auth`, `billing`, `search`, `api`
- Use feature names when appropriate: `login`, `checkout`, `export`
- For libraries/packages: use the package name
- If changes span multiple areas, use the most significant or omit scope entirely
- Keep it short (ideally 3-10 chars) to leave room for the description

**Write description** in imperative mood:

- "add user authentication" not "added user authentication"
- "fix null pointer in parser" not "fixes null pointer"
- No period at the end
- Lowercase after the colon

#### Body (always include, wrap at 72 chars)

Write 2-4 sentences explaining:

- **Why** this change was made (the diff shows what/how/where)
- **Who** benefits and how
- Any important context that helps reviewers understand the change

Do NOT:

- Repeat what the diff already shows
- Include implementation details visible in the code
- Be overly verbose

**Optional references** at the end:

- `Refs: #123` - Related issue
- `Closes: #123` - Issue this resolves

### Step 4: Present for Review

Display the current and proposed messages clearly:

```markdown
## Current Commit Message

[show current message]

## Proposed New Message

[show proposed subject + body]
```

Then use AskUserQuestion to confirm:

- **Proceed**: Amend with the proposed message
- **Edit**: Let user provide modifications
- **Cancel**: Abort without changes

### Step 5: Execute Amendment

If user approves, run the amendment using HEREDOC to preserve the multiline body:

```bash
git commit --amend -m "$(cat <<'EOF'
type(scope): subject line here

Body paragraph explaining why this change was made and who benefits.
Additional context if needed.
EOF
)"
```

After successful amendment, run `git log -1` to confirm and display the result.

---

## Example Output

For a commit that adds OAuth support:

**Current:**

```
wip oauth stuff
```

**Proposed:**

```
feat(auth): add OAuth2 support for GitHub login

Enable users to authenticate via GitHub OAuth2 flow. This reduces
friction for developers who already have GitHub accounts and avoids
managing another set of credentials.

The implementation uses authorization code flow with PKCE for
enhanced security in browser environments.
```

---

**Begin the amend commit workflow now.**
