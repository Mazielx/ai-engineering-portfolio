# Effective Prompts - Before/After Examples

Real examples of prompt improvements from my learning.

---

## Example 1: Code Review

### ❌ Bad Prompt
```
Review this code
```

### ✅ Good Prompt
```
Review this TypeScript code for:
1. Security issues (especially SQL injection, auth bypass)
2. Multi-tenant isolation problems
3. Error handling gaps

Context: This handles invoice data for multiple businesses
Input: [paste code]

Output format: Checklist with severity (Critical/Warning/Info)
```

**Why it works:**
- Specific security concerns mentioned
- Context about multi-tenant system
- Clear output format
- actionable items

---

## Example 2: Bug Investigation

### ❌ Bad Prompt
```
Fix this bug
```

### ✅ Good Prompt
```
Investigate why invoice extraction fails silently.

Current behavior: No error shown, invoices not stored
Expected behavior: Error message displayed, partial data saved

Relevant files:
- src/lib/extraction/index.ts
- src/app/api/extract/route.ts

What I've tried:
- Checked Gmail API connection (works)
- Verified XML parsing (works standalone)

Please:
1. Identify root cause
2. Suggest fix with code
3. List files to change
```

**Why it works:**
- Clear symptoms described
- Expected vs actual behavior
- Files listed
- What was tried already
- Specific deliverables requested

---

## Example 3: Feature Implementation

### ❌ Bad Prompt
```
Add search functionality
```

### ✅ Good Prompt
```
Implement full-text search for invoices using SQLite FTS5.

Requirements:
- Search across: invoice number, emitter name, concepts
- Fallback to LIKE if FTS5 not available
- Highlight search terms in results
- Support partial matching (prefix search)

Constraints:
- Must work with existing multi-tenant schema
- No external dependencies (use built-in SQLite)
- Performance: <100ms for 10k records

Output: Production-ready code with tests
```

**Why it works:**
- Specific technology chosen (FTS5)
- Clear requirements list
- Constraints defined
- Performance target specified
- Output expectations clear

---

## Example 4: Documentation

### ❌ Bad Prompt
```
Write documentation
```

### ✅ Good Prompt
```
Document the extraction pipeline for new developers.

Include:
1. Overview (2-3 sentences)
2. Data flow diagram (text-based)
3. Key functions and their roles
4. Common failure modes
5. Debugging steps

Audience: Mid-level backend developer
Tone: Technical but accessible
Format: Markdown with code examples

Focus on: How it actually works, not how it was designed
```

**Why it works:**
- Clear scope defined
- Audience specified
- Format expectations set
- "Actually works" vs "designed" distinction

---

## Example 5: Learning Explanation

### ❌ Bad Prompt
```
Explain multi-tenancy
```

### ✅ Good Prompt
```
Explain multi-tenant SQLite architecture for a Next.js app.

My current level: I understand basic SQL and Node.js
My goal: Implement multi-tenant isolation for invoice system
My timeframe: Need to understand in 30 minutes

Please:
1. Start with the concept (2 sentences)
2. Show the pattern (code example)
3. Explain the tradeoffs
4. Common pitfalls to avoid

Avoid: Theory overload, focus on practical implementation
```

**Why it works:**
- Current knowledge level stated
- Specific goal defined
- Time constraint mentioned
- Clear structure requested
- Practical focus emphasized

---

## Example 6: Refactoring

### ❌ Bad Prompt
```
Make this code better
```

### ✅ Good Prompt
```
Refactor this extraction function for better error handling.

Current issues:
- Silent failures when XML is malformed
- No logging for debugging
- Confidence scores not calculated correctly

Current code:
[paste function]

Goals:
- Graceful handling of malformed data
- Detailed error logging
- Correct confidence calculation
- Maintain same external interface

Constraints:
- Don't change the function signature
- Keep backward compatibility
- Add tests for new error cases
```

**Why it works:**
- Specific problems identified
- Clear goals listed
- Constraints defined
- Backward compatibility mentioned
- Test requirements included

---

## Pattern: The 5W Framework

Every good prompt answers:

| Element | Question | Why it matters |
|---------|----------|----------------|
| **Who** | Role for AI | Sets context and expertise level |
| **What** | Clear task | Prevents vague responses |
| **Why** | Context/purpose | Helps AI understand importance |
| **When** | Constraints | Defines boundaries |
| **How** | Output format | Ensures usable results |

---

## Quick Fixes

| Problem | Solution |
|---------|----------|
| AI gives too much detail | Add "Be concise, max 3 sentences" |
| AI is too generic | Add specific context/constraints |
| AI misses the point | Rephrase with concrete example |
| AI output is wrong format | Show desired format explicitly |
| AI makes assumptions | State assumptions clearly |

---

*Update this file with your own discoveries.*
