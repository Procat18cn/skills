---
name: organize-tech-knowledge-doc
description: >-
  Organize scattered technical materials into structured knowledge documents.
  Use when the user asks to summarize, distill, restructure, or organize
  technical content from files, web pages, PDFs, chat logs, or notes into a
  formatted document (default Markdown). Covers troubleshooting guides, skill
  tutorials, system configuration docs, and software usage references.
license: MIT
---

# Organize Technical Knowledge Document

## When to Use

Apply this skill when the user wants to:
- Summarize or restructure raw technical materials into a knowledge document
- Create a troubleshooting guide, skill tutorial, or system configuration doc
- Distill content from chat logs, web pages, PDFs, or Markdown notes
- Generate a document with specific sections (abstract, keywords, root cause analysis, solutions, appendices, references)

## Workflow

### Phase 1: Understand Requirements

Confirm or infer from context:
- **Source material**: File path, URL, pasted content, or selected folder
- **Knowledge topic**: The specific technical subject to focus on
- **Scope filters**: What to include or explicitly ignore (e.g., "ignore keyring issues")
- **Output format**: Markdown (default), DOCX, PDF, or other
- **Save location**: User's selected folder or default workspace

If requirements are underspecified, ask clarifying questions before proceeding.

### Phase 2: Extract & Analyze Source Content

1. Read all source materials (files, web pages, PDFs, etc.)
2. Identify the core problem, root causes, solutions, and preventive measures
3. Flag content that is contradictory, unclear, or requires external verification
4. Note any basic concepts mentioned but not explained in the source

### Phase 3: Supplement Missing Knowledge

Search the web or reference official docs to supplement:
- Unexplained technical terms or concepts (e.g., D-Bus, Wayland, X11)
- Official configuration references or specifications
- Community-verified solutions and known issues
- Up-to-date compatibility notes

### Phase 4: Structure the Document

Use the default technical knowledge structure below unless the user specifies otherwise.

For a detailed template with per-section guidance, see [references/template.md](references/template.md).

#### Default Structure

1. **Title** — Clear, descriptive headline including the technical topic
2. **Abstract** — One-paragraph summary of the problem scope, key findings, and document purpose
3. **Keywords** — 5–10 technical tags for searchability
4. **Body Content**
   - Background & context (what the technology is, why it matters)
   - Problem symptoms / phenomena
   - Root cause analysis (categorized, with technical depth)
   - Solutions / remediation steps (ordered by invasiveness or priority)
   - Verification & diagnostic methods
   - Prevention & best practices
   - Risk assessment & rollback procedures
5. **Appendices**
   - Basic concept glossary
   - Configuration file references
   - Ready-to-use code/script snippets
6. **References / Related Links** — Curated, reliability-checked external links

### Phase 5: Quality Assurance

Before finalizing, verify:
- [ ] All referenced external links are real, relevant, and from credible sources
- [ ] No fabricated or unverified URLs
- [ ] Basic concepts are explained in appendices if not covered in source
- [ ] Ignored/filtered content is truly excluded per user instructions
- [ ] Solutions are ordered logically (least invasive first)
- [ ] Rollback instructions are provided for any system-level changes

### Phase 6: Output & Save

1. Generate the document in the requested format
2. Save to the specified directory with an auto-generated descriptive filename
3. Present the output to the user

## Output Format Notes

- **Markdown (default)**: Single `.md` file, standard Markdown with fenced code blocks for commands/configs.
- **DOCX**: Use available document generation tools to create a Word document with proper headings and styles.
- **PDF**: Use available document generation tools to create a printable PDF from the Markdown source.
