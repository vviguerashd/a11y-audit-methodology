# Manual Audit Checklist

> Quick yes/no questions to run before any automated tool.
> These target the most common issues found consistently across audits, a starting point, not a finish line.
> Organized by WCAG 2.1 principle (POUR) and criterion, in the order I naturally review them.

---

## Perceivable

### 1.1.1 — Non-text Content (Level A)
- [ ] Are images classified as decorative or informative? (Decorative images must have `alt=""`)
- [ ] Do informative images have alt text?
- [ ] Are icons used decoratively, or do they communicate something?
- [ ] If an image acts as a button or link, does the alt text describe where it goes or what it does?

### 1.3.1 — Info and Relationships (Level A)
- [ ] Is there div soup? (div or span elements used where semantic HTML should be)
- [ ] Are semantic HTML elements used? (nav, main, header, footer, section, article)
- [ ] Do headings reflect the visual structure of the content? (Text that looks like a heading should be marked up as one)
- [ ] Is there more than one `h1`, or are heading levels skipped? *(Best practice, not a 1.3.1 failure on its own)*
- [ ] Are lists wrapped in `ol` or `ul`?
- [ ] Do form labels provide enough information to understand what is being asked?
- [ ] Are error messages programmatically associated with their field? (e.g. `aria-describedby`)

### 1.4.1 — Use of Color (Level A)
- [ ] Is color the only means used to indicate an error or required field?
- [ ] Do links stand out from regular text by something other than color?
- [ ] Does any important information disappear when viewed in grayscale?

### 1.4.3 — Contrast Minimum (Level AA)
- [ ] Is there any low-contrast text visible at first glance?
- [ ] Is the low contrast related to text size? (Normal text: minimum 4.5:1 / Large text 18pt+: minimum 3:1)
- [ ] Is the low contrast related to bold text? (14pt bold: minimum 3:1)
- [ ] Is any text placed over a gradient? (Check the contrast value at the lowest point)

### 1.4.11 — Non-text Contrast (Level AA)
- [ ] Do buttons stand out visually from the background? (Minimum ratio: 3:1)
- [ ] Do inputs, checkboxes, and radio buttons have a visible border with sufficient contrast?
- [ ] Does the focus indicator have sufficient contrast against the background?

---

## Operable

### 2.1.1 — Keyboard (Level A)
- [ ] Can I reach all actionable elements with Tab? (links, buttons, inputs, selects, checkboxes, radio buttons)
- [ ] Is there any element where focus gets trapped and I cannot exit with the keyboard?
- [ ] Do dropdown menus open and close with the keyboard?

### 2.4.1 — Bypass Blocks (Level A)
- [ ] Is there a way to bypass repeated blocks of content? (skip link, landmarks, or headings)
- [ ] If there is a skip link, does it appear on focus and actually move focus to the main content?

### 2.4.3 — Focus Order (Level A)
- [ ] Does the Tab order follow a logical reading sequence? (left to right, top to bottom)
- [ ] Are there any unexpected jumps in focus order that could disorient the user?
- [ ] Is there any positive `tabindex` (1 or higher) forcing an artificial order?
- [ ] When a modal opens, does focus move into it? When it closes, does focus return to the element that opened it?

### 2.4.4 — Link Purpose (Level A)
- [ ] Does the link text describe its purpose on its own?
- [ ] Are there any links that say only "click here", "read more", or "see"?
- [ ] Does the link make it clear what will happen when activated?

### 2.4.7 — Focus Visible (Level AA)
- [ ] Is the focus indicator visible on all interactive elements?
- [ ] Is there any element where focus disappears or is barely visible?

---

## Understandable

### 3.1.1 — Language of Page (Level A)
- [ ] Does the `lang` attribute on `<html>` match the primary language of the page?
- [ ] If there are sections in another language, are they marked with `lang` on that element?

### 3.3.1 — Error Identification (Level A)
- [ ] Are form errors shown as text, not just indicated by color or icon?
- [ ] Does the error message identify which field failed?
- [ ] Can the error be read by a screen reader? (Is it in the DOM, not just visual?)

### 3.3.2 — Labels or Instructions (Level A)
- [ ] Do all form fields have a visible label associated with them?
- [ ] If a field has an expected format, is it indicated before the user tries to submit? (e.g. "format: MM/DD/YYYY")
- [ ] Are required fields clearly identified?

### 3.3.3 — Error Suggestion (Level AA)
- [ ] Does the error message explain how to fix the problem, not just that one exists?
- [ ] Does the error appear near the field that caused it, not just at the top of the form? *(Best practice)*

---

## Robust

### 4.1.2 — Name, Role, Value (Level A)
- [ ] Do icon-only buttons have an accessible name? (aria-label or visually hidden text)
- [ ] Do custom interactive elements have the correct role in the accessibility tree?
- [ ] Are component states communicated? (expanded/collapsed, selected, disabled)
- [ ] Are there elements in the accessibility tree marked as "unlabelled" or "generic" that should have a name?

### 4.1.3 — Status Messages (Level AA)
- [ ] If a global success or error message appears, is it announced without moving focus?
- [ ] Do dynamic messages use `aria-live` to be read by the screen reader?
- [ ] Does a spinner or loading indicator communicate its state to the screen reader?

---

## After this checklist, what is next?

These questions cover the most common and visible issues. After completing them:

1. **Automated tools:** WAVE, Lighthouse, axe DevTools. I compare their results against my notes. Disagreements are the most interesting ones.
2. **Keyboard navigation:** I go through the entire page without a mouse. No tool replaces this.
3. **Human judgment:** Is the alt text useful or just noise? Does this contrast technically pass but feel unreadable in context? Tools do not have judgment. I do.

---

*This checklist evolves with each audit. V.V.*
