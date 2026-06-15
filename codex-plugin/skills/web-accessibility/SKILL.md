---
name: web-accessibility
description: Web accessibility router for HTML, JSX, CSS, ARIA, keyboard, forms, contrast, modals, live regions, headings, links, tables, mobile web, and WCAG review.
---

# Web Accessibility Router

Use this skill for web UI accessibility work in Codex.

## Workflow

1. Explicitly spawn `accessibility-lead` as a Codex custom subagent for every user-facing web accessibility task. Do not satisfy this step by reading the lead reference inline. The lead coordinates the same specialist team used by Claude Code.
2. Read `codex-plugin/references/specialists/accessibility-lead.md` and `codex-plugin/references/specialists/index.json` when available. In installed Codex plugin layouts, use `.agents/plugins/a11y-agents-codex/references/specialists/` or `~/.agents/plugins/a11y-agents-codex/references/specialists/`. Use the lead decision matrix and the index to select relevant specialist references and Codex subagents.
3. Identify the task domain: semantics, ARIA, keyboard, forms, contrast, overlays, live updates, headings, links, tables, mobile web, or full audit.
4. Check installed Accessibility Agents extensions before finalizing dispatch. Look for extension manifests under `.a11y-agents/extensions/`, `~/.a11y-agents/extensions/`, and this plugin's `extensions/` directory.
5. Dispatch matching Codex custom subagents by default for reviews, audits, new UI, changed UI, and PR accessibility checks. Do not make users manually name every specialist.
6. If Codex cannot spawn `accessibility-lead`, say so before continuing. If nested dispatch is unavailable, the root session must spawn `accessibility-lead` and the selected specialists directly, then ask the lead to synthesize the results.
7. The lead synthesizes specialist output: deduplicate, resolve conflicts, assign severity, map to WCAG/public standards or extension rules, and make a ship/no-ship call.
8. Label extension findings with the extension name.

## Default Subagent Dispatch

- Broad audit: `accessibility-lead`, `aria-specialist`, `keyboard-navigator`, `contrast-master`, `forms-specialist`, `modal-specialist`, `live-region-controller`, `alt-text-headings`, `tables-data-specialist`, `link-checker`
- New UI: `accessibility-lead`, `aria-specialist`, `keyboard-navigator`, `alt-text-headings`, plus domain specialists for forms, contrast, modals, live regions, tables, links, media, mobile, i18n, or cognitive accessibility as needed
- Changed UI: `accessibility-lead`, `keyboard-navigator`, plus any specialists matching the diff
- PR review: `pr-review` plus any web specialists matching the diff
- Small fix: `accessibility-lead` plus the single most relevant specialist, followed by the lead final checklist

Do not expose all specialists as top-level skills. Keep the router surface small and load deep instructions lazily.
