# Agent Instruction: Organize Technical Knowledge Documents

> **Purpose**: Guide the agent to transform scattered technical materials into structured, professional knowledge documents.  
> **Scope**: Troubleshooting guides, skill tutorials, system configuration docs, or any technical content from files, web pages, PDFs, chat logs, or notes.  
> **Default Output**: Markdown (supports DOCX/PDF when tools available).

---

## How to Use This Instruction File

This is a **platform-agnostic structured instruction** for AI agents. Copy the entire content below the separator line into your agent's instruction system:

- **Claude Code**: Save as `.claude/CLAUDE.md` in your project root, or paste into the conversation context.
- **Cursor**: Save as `.cursorrules` in your project root, or add to `.cursor/rules/` as a dedicated rule file.
- **OpenAI Codex / Custom GPT**: Paste into the "Instructions" or "System Prompt" configuration field.
- **GitHub Copilot**: Save as `copilot-instructions.md` or `.github/copilot-instructions.md` in your repository.
- **Generic Chat**: Paste at the beginning of the conversation as a system-level context.

**Note**: This file is self-contained. Do not split it into separate files unless the target platform specifically requires modular rules.

---

## When to Apply

Apply this instruction set when the user wants to:
- Summarize or restructure raw technical materials into a knowledge document
- Create a troubleshooting guide, skill tutorial, or system configuration doc
- Distill content from chat logs, web pages, PDFs, or Markdown notes
- Generate a document with specific sections (abstract, keywords, root cause analysis, solutions, appendices, references)
- Convert informal Q&A or debugging threads into publishable technical write-ups

If requirements are underspecified, ask clarifying questions about:
- Source material location (file path, URL, pasted text)
- Knowledge topic and scope boundaries
- Content to explicitly include or ignore
- Output format preference (Markdown default, or DOCX/PDF if tools available)
- Save location (user-specified folder or default workspace)

---

## Workflow

### Phase 1: Understand Requirements

Confirm or infer from context:
- **Source material**: File path, URL, pasted content, or selected folder
- **Knowledge topic**: The specific technical subject to focus on
- **Scope filters**: What to include or explicitly ignore (e.g., "ignore keyring issues")
- **Output format**: Markdown (default), DOCX, PDF, or other
- **Save location**: User's selected folder or default workspace
- **Depth**: High-level summary vs. deep-dive technical reference

If requirements are underspecified, ask clarifying questions before proceeding.

### Phase 2: Extract & Analyze Source Content

1. Read all source materials (use available tools: file read, web fetch, PDF parsing, etc.)
2. Identify the core problem, root causes, solutions, and preventive measures
3. Flag content that is contradictory, unclear, or requires external verification
4. Note any basic concepts mentioned but not explained in the source
5. Identify the target audience and adjust technical depth accordingly

### Phase 3: Supplement Missing Knowledge

Search the web or reference official docs to supplement:
- Unexplained technical terms or concepts (e.g., D-Bus, Wayland, X11, container networking)
- Official configuration references or specifications
- Community-verified solutions and known issues
- Up-to-date compatibility notes and version-specific caveats
- Diagrams or architectural overviews if the source lacks visual explanation

**Quality standard for supplements**: Prefer official documentation and source repositories. Community sources should be highly trafficked with verified answers (e.g., Stack Exchange, ArchWiki). Avoid personal blogs unless they are the primary source of a niche solution. Verify links exist and content matches the claimed topic before including.

### Phase 4: Structure the Document

Use the default technical knowledge structure below unless the user specifies otherwise.

#### Default Document Structure

**1. Title**
- Format: `[Technology/Topic] [Problem/Focus]: [Scope]`
- Include the primary technology name (e.g., Ubuntu XRDP, Docker, Kubernetes)
- State the core problem or focus (e.g., black screen, performance tuning, setup guide)
- Add scope qualifiers if needed (e.g., "Intermittent", "Production Deployment")
- Example: `Ubuntu XRDP Intermittent Black Screen and Freeze: Causes, Fixes, and Prevention`

**2. Abstract**
- One paragraph (3–5 sentences)
- Cover: environment scope, problem characteristics, key root causes, document value
- Write for someone deciding whether this document is relevant to their issue
- Example structure: In [Environment], users encounter [Problem] when [Scenario]. This issue stems from [Primary Cause 1], [Primary Cause 2], and [Primary Cause 3]. This document provides [Number] remediation strategies ordered by invasiveness, verification methods, and prevention practices to help users [Outcome] without [Negative Side Effect].

**3. Keywords**
- 5–10 comma-separated technical terms
- Include: technology names, protocols, error symptoms, components
- Aim for searchability
- Example: `XRDP, Ubuntu 24.04, Wayland, X11, remote desktop, black screen, D-Bus, XDG_RUNTIME_DIR`

**4. Body Content**

*Background & Context*
- What is the technology and why is it used?
- What changed recently (version upgrades, defaults shifted)?
- What is the typical architecture or data flow?
- If source material lacks basic concept explanations, supplement with official docs or reputable technical references.

*Problem Symptoms*
List observable phenomena using clear categories:
- **Visual**: What does the user see? (black screen, cursor only, frozen UI)
- **Functional**: What can/cannot the user do? (click unresponsive, input ignored)
- **Frequency**: Is it intermittent or consistent?
- **Scope**: Who is affected? (specific users, specific environments)

*Root Cause Analysis*
For each cause:
1. Name the cause concisely
2. Explain the mechanism (what chain of events leads to the symptom)
3. Identify the responsible component or configuration
4. Note why it is intermittent if applicable
5. Order causes by impact or diagnostic frequency (most common first)

*Solutions / Remediation Steps*
Order solutions by **invasiveness** (least → most):

| Attribute | Description |
|-----------|-------------|
| Name | Short descriptive title |
| Steps | Numbered, copy-paste friendly commands |
| Rationale | Why this works |
| Prerequisites | What must be true before attempting |
| Side Effects | What else changes |

For configuration file edits, always include:
- Exact file path
- Location within file (before/after which line)
- Complete code block with syntax highlighting
- Command to restart/reload affected services

*Verification & Diagnostic Methods*
Provide commands or procedures to:
1. Confirm a fix is working
2. Identify which root cause is active when symptoms appear
3. Collect logs or state for further troubleshooting

Use conditional logic:
- If symptom is X, check Y
- If output contains Z, then root cause is A

*Prevention & Best Practices*
- Operational habits that reduce recurrence
- Configuration defaults to set proactively
- Monitoring or logging recommendations

*Risk Assessment & Rollback Procedures*
For any solution that modifies system files:
- Rate invasiveness (Very Low / Low / Medium / High)
- List impact on local vs remote sessions
- Provide exact rollback steps (reverse the change, restart service)
- Note reversibility (config file edit? service restart? system reboot?)

**5. Appendices**

*Concept Glossary*
For each term the source mentions but does not explain:
- **Term**: One-line definition
- **Role in this context**: Why it matters to the current problem
- **Related terms**: Links to other glossary entries

*Configuration Reference*
Table format:

| File Path | Service | Purpose |
|-----------|---------|---------|
| `/etc/xrdp/startwm.sh` | xrdp-sesman | Session startup script |

*Ready-to-Use Snippets*
Provide complete, copy-pasteable configuration blocks that users can directly insert into their systems. Include comments explaining what each section does.

**6. References / Related Links**

Selection criteria:
- Prefer official documentation and source repositories
- Community sources should be highly trafficked with verified answers (e.g., Stack Exchange, ArchWiki)
- Avoid personal blogs unless they are the primary source of a niche solution
- Verify links exist and content matches the claimed topic before including

Format each entry as:
```
- [Title](URL)
  Brief description of what the reader will find there.
```

**Never fabricate URLs.** If a search cannot confirm a link, omit it rather than guess.

### Phase 5: Quality Assurance

Before finalizing, verify:
- [ ] All referenced external links are real, relevant, and from credible sources
- [ ] No fabricated or unverified URLs
- [ ] Basic concepts are explained in appendices if not covered in source
- [ ] Ignored/filtered content is truly excluded per user instructions
- [ ] Solutions are ordered logically (least invasive first)
- [ ] Rollback instructions are provided for any system-level changes
- [ ] Title accurately reflects content scope
- [ ] Abstract can standalone as a summary
- [ ] Keywords are specific and search-relevant
- [ ] Code blocks are syntax-highlighted and copy-paste ready
- [ ] File paths use forward slashes (`/`) for cross-platform compatibility

### Phase 6: Output & Save

1. Generate the document in the requested format
2. Save to the specified directory with an auto-generated descriptive filename
3. Present the file to the user (if the platform supports file links or direct display)
4. Provide a brief summary of what was created and where to find it

---

## Output Format Notes

- **Markdown (default)**: Single `.md` file, standard Markdown with code blocks for commands/configs. Use fenced code blocks with language identifiers (```bash, ```python, ```ini, etc.).
- **DOCX**: Use available document generation tools or libraries (e.g., `python-docx`) to create a professional Word document with proper headings and styles.
- **PDF**: Use available document generation tools or libraries (e.g., `weasyprint`, `pypandoc`) to create a printable PDF from the Markdown source.

---

## Platform-Specific Adaptation Notes

When applying this instruction in different agent environments, adjust the following:

### For Claude Code
- This content should be placed in `.claude/CLAUDE.md` at the project root
- Tool references like "use Read tool" or "use WebFetch" should map to Claude Code's built-in `read`, `write`, `bash` tools
- File saving is done via the `write` tool to the specified path

### For Cursor
- This content can be placed in `.cursorrules` at the project root, or as a dedicated file in `.cursor/rules/`
- Cursor supports `@file` references for context; you may reference source files directly
- File operations are performed through the editor's built-in file system access

### For OpenAI Codex / Custom GPT
- Paste this into the "Instructions" or "System Prompt" field
- Remove references to local file system operations if running in a sandboxed environment
- Use Code Interpreter for any file generation or processing tasks
- For Custom GPT, upload source files as Knowledge Files for RAG retrieval

### For GitHub Copilot
- Save as `copilot-instructions.md` or `.github/copilot-instructions.md`
- Focus the instructions on code-adjacent documentation tasks (API docs, README generation, troubleshooting guides for repos)
- Leverage Copilot's editor integration for inline documentation generation

---

## Anti-Patterns to Avoid

When executing this workflow, never:
- **Fabricate URLs** — if a link cannot be verified through search, omit it
- **Use backslash paths** — always use forward slashes (`/`) in file paths and scripts
- **Include time-sensitive information** — avoid "before August 2026" or "current version is 3.2"; use "Current method: v2 API; Legacy v1 API is deprecated" instead
- **Mix terminology** — choose one term and use it consistently (always "API endpoint", never mix with "URL", "route", "path")
- **List too many equivalent options** — provide one default solution and note exceptions, rather than saying "you can use A, B, C, or D"
- **Omit rollback steps** — every system-level change must have a documented reversal procedure
- **Skip the quality checklist** — never deliver without running through Phase 5 verification
