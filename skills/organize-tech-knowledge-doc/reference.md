# Reference: Technical Knowledge Document Template

This file provides detailed guidance for each section of the default technical knowledge document structure.

## Title

Format: `[Technology/Topic] [Problem/Focus]: [Scope]`

- Include the primary technology name (e.g., Ubuntu XRDP, Docker, Kubernetes)
- State the core problem or focus (e.g., black screen, performance tuning, setup guide)
- Add scope qualifiers if needed (e.g., "偶发性", "Production Deployment")

Example: `Ubuntu XRDP 偶发黑屏与卡死：原因、修复与预防`

## Abstract

- One paragraph (3–5 sentences)
- Cover: environment scope, problem characteristics, key root causes, document value
- Write for someone deciding whether this document is relevant to their issue

Example structure:
> In [Environment], users encounter [Problem] when [Scenario]. This issue stems from [Primary Cause 1], [Primary Cause 2], and [Primary Cause 3]. This document provides [Number] remediation strategies ordered by invasiveness, verification methods, and prevention practices to help users [Outcome] without [Negative Side Effect].

## Keywords

- 5–10 comma-separated technical terms
- Include: technology names, protocols, error symptoms, components
- Aim for searchability

Example: `XRDP, Ubuntu 24.04, Wayland, X11, remote desktop, black screen, D-Bus, XDG_RUNTIME_DIR`

## Body Content

### Background & Context

- What is the technology and why is it used?
- What changed recently (version upgrades, defaults shifted)?
- What is the typical architecture or data flow?

If source material lacks basic concept explanations, supplement with official docs or reputable technical references.

### Problem Symptoms

List observable phenomena using clear categories:

- **Visual**: What does the user see? (black screen, cursor only, frozen UI)
- **Functional**: What can/cannot the user do? (click unresponsive, input ignored)
- **Frequency**: Is it intermittent or consistent? (偶发性 vs 必现)
- **Scope**: Who is affected? (specific users, specific environments)

### Root Cause Analysis

For each cause:

1. Name the cause concisely
2. Explain the mechanism (what chain of events leads to the symptom)
3. Identify the responsible component or configuration
4. Note why it is intermittent if applicable

Order causes by impact or diagnostic frequency (most common first).

### Solutions

Order solutions by **invasiveness** (least → most):

| Attribute to Include | Description |
|---------------------|-------------|
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

### Verification & Diagnostics

Provide commands or procedures to:

1. Confirm a fix is working
2. Identify which root cause is active when symptoms appear
3. Collect logs or state for further troubleshooting

Use conditional logic:
- If symptom is X, check Y
- If output contains Z, then root cause is A

### Prevention & Best Practices

- Operational habits that reduce recurrence
- Configuration defaults to set proactively
- Monitoring or logging recommendations

### Risk Assessment & Rollback

For any solution that modifies system files:

- Rate invasiveness (极低 / 低 / 中 / 高)
- List impact on local vs remote sessions
- Provide exact rollback steps (reverse the change, restart service)
- Note reversibility (is it a config file edit? service restart? system reboot?)

## Appendices

### Concept Glossary

For each term the source mentions but does not explain:

- **Term**: One-line definition
- **Role in this context**: Why it matters to the current problem
- **Related terms**: Links to other glossary entries

### Configuration Reference

Table format:

| File Path | Service | Purpose |
|-----------|---------|---------|
| `/etc/xrdp/startwm.sh` | xrdp-sesman | Session startup script |

### Ready-to-Use Snippets

Provide complete, copy-pasteable configuration blocks that users can directly insert into their systems. Include comments explaining what each section does.

## References / Related Links

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

Never fabricate URLs. If a search cannot confirm a link, omit it rather than guess.
