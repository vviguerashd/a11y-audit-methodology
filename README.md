# VHVD: WCAG Accessibility Audit Methodology

> A personal, three-level web accessibility audit methodology based on WCAG 2.1 AA, Section 508, and EN 301 549.
> Built and refined through hands-on audits of my own projects, not just coursework.

---

## Overview

This repo documents my personal process for auditing web accessibility. It covers how I define scope, the order in which I run manual and automated tests, and how I document and prioritize findings.

The methodology is structured in three audit levels depending on the depth required. Each level builds on the previous one. The audit template (`.xlsx`) included in this repo reflects the same structure.

---

## Audit Levels

| Level | Name | Tools | WCAG Coverage | Legal Alignment |
|-------|------|-------|---------------|-----------------|
| **L1** | Quick Scan | WAVE + Lighthouse + manual review | High-impact issues only (does not confirm conformance) | — |
| **L2** | Standard Audit | WAVE + axe DevTools + keyboard nav + Chrome DevTools | WCAG 2.1 AA | Section 508 (US) · EN 301 549 (EU) |
| **L3** | Full Audit | All of the above + screen reader (VoiceOver / NVDA) | WCAG 2.1 AA (full coverage) | Section 508 (US) · EN 301 549 (EU) |

> **On legal alignment:** EN 301 549 (v3.2.1) references WCAG 2.1 AA, while Section 508 references WCAG 2.0 AA (2017 refresh). Because WCAG 2.1 AA includes every WCAG 2.0 AA success criterion, auditing against WCAG 2.1 AA covers the WCAG-based web requirements of both standards. Both also contain requirements beyond WCAG (for example, for software, documents, and support documentation) that a web audit does not cover. This is not a legal certification. It means the audit findings are expressed in terms those frameworks recognize and can act on.

> **L2 vs L3:** The difference is depth of screen reader coverage. L2 tests most WCAG 2.1 AA criteria with automated tools and keyboard checks, but some criteria can only be confirmed with assistive technology. L3 adds manual screen reader testing (VoiceOver / NVDA), which is required to verify criteria that automated tools cannot fully assess, particularly around live regions, dynamic content, and complex ARIA patterns.

---

## My Process

### 1. Scope

Before touching any tool, I define what I'm evaluating and against what standard.

- **What:** A single page, a user journey, or a full site? I map out every page or flow in scope before starting.
- **Against what:** Always WCAG, but I clarify the target conformance level (A, AA, or AAA) with whoever is requesting the audit. If there's no brief, I default to WCAG 2.1 AA.
- **Constraints:** Are there known assistive technologies in use? Any platform or browser requirements? This affects how I weigh findings later.

Skipping this step leads to audit drift. You end up testing things that don't matter and missing things that do.

---

### 2. Test

#### 2a. First pass (unassisted visual review)

I start without any automated tool running. I want to form my own opinion of the page before an algorithm tells me where to look. This comes naturally to me from reviewing student code. I'm used to reading a page and spotting structural issues before running anything.

In practice, I have two things open: the page itself and **Chrome DevTools (Elements panel)**. The visual layer tells me what the user sees; the DOM tells me what's actually there. A heading that looks like an `h2` might be a styled `div`, and the inspector catches that immediately.

In this pass I look for:

- Images, icons, and non-text elements (meaningful or decorative?)
- Dropdown menus, modals, and interactive components
- Forms and error states (are errors written out or just styled in red?)
- Color contrast (obvious failures visible to the naked eye)
- Page sections and heading hierarchy (does the structure make sense in the DOM?)
- Any visible user flows (login, checkout, search) that need end-to-end testing

I also keep a **notebook and pen** nearby. Before anything goes into the spreadsheet, it goes on paper. It keeps me focused and lets me sketch relationships between findings without committing to a structure too early.

#### 2b. Manual checklist (quick yes/no questions)

Before the automated tools, I run through a short checklist of yes/no questions. This builds a baseline I can later compare against the automated results. A separate file in this repo contains the full question list mapped to their WCAG criteria. See [`manual-checklist.md`](./manual-checklist.md).

A few examples:

- Do images and icons have alt text?
- Are semantic elements used, or is it div soup?
- Are form inputs associated with visible labels?
- Are errors identified in text, not just by color or icon?
- Is there a logical heading structure (h1 → h2 → h3)?
- Can I identify the language of the page in the HTML?
- Are there any obvious keyboard traps on first look?

#### 2c. Automated tools

After I have my own baseline, I run the tools in this order:

1. **WAVE:** broad visual overlay, fast to scan, good for spotting missing alt text, contrast errors, and structural issues at a glance
2. **Lighthouse** (Accessibility audit): gives a score and flags issues with WCAG references, useful for quick prioritization
3. **axe DevTools:** most precise of the three, lowest false positive rate, and findings map directly to WCAG criteria

I run them in this order because WAVE is the fastest to read visually, Lighthouse gives context, and axe is where I dig into specifics. I compare their output against my manual notes. Disagreements are always worth investigating.

#### 2d. Keyboard navigation

Tab through the entire page without a mouse. I check:

- Every interactive element is reachable by keyboard
- Focus indicator is always visible
- Tab order follows a logical reading sequence
- No keyboard traps

#### 2e. Screen reader (L3 only)

VoiceOver on macOS or NVDA on Windows. I listen to how the page is announced, not just whether elements exist, but whether they make sense out of visual context.

---

### 3. Document

Every finding goes into the audit log with:

- **ID:** sequential (F-001, F-002...)
- **Page / Component:** where exactly
- **WCAG Criterion:** number and short name (e.g. 1.1.1 Non-text Content)
- **Level:** A, AA, or AAA
- **Severity:** Critical / Serious / Moderate / Minor
- **Tool that found it:** or "Manual" if I caught it in the first pass
- **Issue description:** what is broken and why it fails the criterion
- **Impact:** who is affected and how
- **Recommended fix:** a concrete suggestion, HTML or CSS first, JS only when necessary

#### Prioritization logic

Once documented, I order findings like this:

1. **Level A failures first.** These are the floor. Nothing else matters if Level A is broken.
2. **Fixes that only require HTML or CSS.** High impact, low implementation cost. These should be the first things a dev team tackles.
3. **Level AA failures,** after the structural foundation is solid.
4. **Fixes that require JavaScript.** Valid but higher effort, flagged clearly so the team can plan accordingly.

I'm transparent about this order in my reports. Remediating accessibility is not my role as an auditor, but I do provide enough context for a development team to understand what to tackle first and why.

---

## Audit Template

The `.xlsx` template in this repo reflects this process directly. It includes:

- **Audit Log:** one row per finding, with dropdowns for severity, POUR principle, WCAG level, and status
- **Audit Scope:** metadata sheet (site, date, standards, tools, pages in scope)
- **Summary Dashboard:** auto-calculated counts by severity and status
- **WCAG Quick Ref:** the 16 most common criteria in audits, with the audit question each criterion maps to

[Download the template](./accessibility_audit_template.xlsx)

---

## Tools

| Tool | What I use it for |
|------|-------------------|
| **Chrome DevTools (Elements and Accessibility panels)** | My starting point. I read the DOM alongside the visual layer from the first pass. |
| **WAVE** | Fast visual overlay, first automated pass, good for alt text and contrast |
| **axe DevTools** | Precise findings mapped to WCAG criteria, lowest false positive rate |
| **Lighthouse** | Quick score and prioritized issue list with WCAG references |
| **Keyboard only** | Tab order, focus visibility, keyboard traps. No tool replaces this. |
| **VoiceOver / NVDA** | Screen reader testing for L3 audits |

---

## Standards Reference

| Standard | Jurisdiction | Notes |
|----------|-------------|-------|
| [**WCAG 2.1 AA**](https://www.w3.org/TR/WCAG21/) | Global baseline | Foundation for all three audit levels |
| [**Section 508**](https://www.section508.gov/)| United States | Federal ICT; references WCAG 2.0 AA since 2017 refresh |
| [**EN 301 549**](https://www.etsi.org/deliver/etsi_en/301500_302000/301549/03.02.01_60/en_301549v030201p.pdf)| European Union | References WCAG 2.1 AA; harmonised standard for the public sector under the Web Accessibility Directive, and the main technical reference for the European Accessibility Act (applies from June 2025) |

---

## Disclaimer

This methodology reflects my current audit practice and evolves with each audit. It is not a certification framework, and completing an audit using this process does not guarantee legal compliance with any specific regulation. Findings are expressed in terms aligned with WCAG 2.1 AA, Section 508, and EN 301 549, but legal accessibility compliance should always be assessed in the context of applicable law.

---

*Last updated: April 2026 — Victor Vigueras*
