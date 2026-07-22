# Case Study: Facturas - Multi-Tenant Invoice Management System

## Problem

Build a multi-tenant invoice management system with:
- Gmail-based invoice extraction (PDF/XML)
- Confidence scoring for extracted data
- Duplicate detection
- Full-text search
- Multi-business support
- API for external access

## Context

**Tech Stack:** Next.js 16, TypeScript, SQLite, Tailwind CSS 4
**Timeline:** [Your timeline]
**Challenge:** Complex multi-tenant architecture with extraction pipeline

## AI Workflow Used

### 1. Explore Phase

**What I researched:**
- Multi-tenant SQLite patterns
- PDF/XML parsing libraries
- Gmail API integration
- Confidence scoring approaches

**Key discoveries:**
- better-sqlite3 for performance
- fast-xml-parser for Spanish XML invoices
- pdf-parse for PDF extraction
- FTS5 for full-text search

**Time spent:** [X hours]

### 2. Plan Phase

**How I broke down the work:**
1. Database schema design (multi-tenant)
2. Authentication system (bcrypt + cookies)
3. Extraction pipeline (Gmail → parse → store)
4. API endpoints (CRUD + search + export)
5. Frontend (server components + client interactivity)

**Key decisions:**
- SQLite over PostgreSQL for simplicity
- Server components by default
- Raw SQL over ORM for control

**Time spent:** [X hours]

### 3. Code Phase

**AI-assisted implementation:**
- Used AI to generate initial schema
- AI helped with extraction logic
- AI assisted with API route patterns
- AI helped debug multi-tenant isolation

**Key prompts used:**
```yaml
# Schema generation
- Input: "Design multi-tenant SQLite schema for invoices"
- Output: Complete schema with indexes
- Improvement: Added tenant isolation checks

# Extraction pipeline
- Input: "Parse Spanish XML invoice format"
- Output: Parser with confidence scoring
- Improvement: Added duplicate detection
```

**Time spent:** [X hours]

### 4. Verify Phase

**Quality gates:**
- ESLint: Zero warnings
- TypeScript: No errors
- Tests: All passing
- Manual: Multi-tenant isolation verified

**Bugs found and fixed:**
1. Tenant DB caching issue
2. FTS5 fallback not working
3. Confidence score calculation error

**Time spent:** [X hours]

### 5. Document Phase

**What I documented:**
- API specification (OpenAPI)
- Data model with ER diagram
- Backend standards
- Frontend standards
- Development guide

**Documentation approach:**
- Started with wrong docs (from different project)
- Realized mismatch with actual codebase
- Rewrote all docs based on real implementation
- Added verification gates and workflows

**Time spent:** [X hours]

## Key Prompts That Worked

### For Multi-Tenant Architecture
```
Design a multi-tenant SQLite architecture where:
1. Each business has isolated database
2. Main database stores shared entities
3. Tenant is resolved from cookie
4. No cross-tenant queries possible

Include: schema, connection management, isolation patterns
```

### For Extraction Pipeline
```
Build invoice extraction pipeline:
1. Connect to Gmail API
2. Find PDF/XML attachments
3. Parse Spanish invoice formats
4. Score confidence (0.0-1.0)
5. Detect duplicates
6. Store in tenant database

Handle: malformed data, missing fields, encoding issues
```

### For Documentation
```
Rewrite documentation for this codebase:
1. Read actual source code first
2. Document what EXISTS, not what was planned
3. Include verification steps
4. Add troubleshooting sections

Focus on accuracy over completeness
```

## Results

**Time saved:** [X% faster than traditional approach]
**Quality:** [Bugs prevented, issues caught early]
**Skills gained:** [List key skills]

## Mistakes That Taught Me

### Mistake 1: Wrong Documentation Source
**What happened:** Started fixing docs from different project
**Why:** Didn't verify docs matched actual codebase
**Lesson:** Always read source code before documenting
**How I avoid now:** "Explore first" rule in workflow

### Mistake 2: Skipping Verification
**What happened:** Code worked locally but failed in production
**Why:** Didn't run full verification suite
**Lesson:** Verification gates prevent bigger slowdowns later
**How I avoid now:** Mandatory lint/typecheck/test after each change

### Mistake 3: Over-Engineering
**What happened:** Built complex system for simple problem
**Why:** Didn't understand requirements fully
**Lesson:** Start simple, iterate based on feedback
**How I avoid now:** "Minimum viable solution" principle

## What I'd Do Differently

1. **Start with docs earlier** - Specs help clarify thinking
2. **More test-driven** - Tests catch issues earlier
3. **Better error handling** - Production errors are harder to debug
4. **Simpler first iteration** - Build complexity only when needed

## Key Insights

> "Documentation is only useful if it matches reality. Wrong docs are worse than no docs."

> "Speed without quality is waste. Verification gates slow you down but prevent bigger slowdowns later."

> "The value is in the process documentation, not just the final code."

## Skills Demonstrated

- [ ] Multi-tenant architecture design
- [ ] AI-assisted development workflow
- [ ] Documentation-driven development
- [ ] Prompt engineering for complex systems
- [ ] Quality verification processes

---

*This case study documents my learning process, not just the final product.*
