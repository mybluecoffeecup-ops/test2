# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A single-page marketing site for a football club ("Emerald Hawks FC"), implemented entirely in one static file: [index.html](index.html). There is no build step, no package manager, no framework, and no test suite — plain HTML/CSS/JS only.

## Running / previewing

There is nothing to install or build. Open the file directly in a browser:

```bash
open index.html
```

There is no linter or test runner configured in this repo. Verify changes by opening the file and manually exercising the page (see "Manual verification" below).

## Architecture

Everything lives in `index.html`, organized into three layers within that one file:

- **HTML** (`<body>`): semantic sections in document order — sticky `<header>`/nav, then `<main>` containing `#hero`, `#testimonials`, `#enquiry` (the enquiry form), and finally `<footer id="contact">`. Each major section is marked with an HTML comment (e.g. `<!-- Hero Section -->`) to make it easy to jump to.
- **CSS** (inline `<style>` in `<head>`): mobile-first, using CSS custom properties on `:root` for the green/yellow/accent color palette. Layout breakpoints are `@media (min-width: 640/768/1024px)`. Scroll-triggered reveals use a `.fade-in` / `.fade-in.visible` class pair driven by JS, not pure CSS.
- **JavaScript** (inline `<script>` at the end of `<body>`): four independent behaviors, in order:
  1. Mobile nav toggle (hamburger button ↔ `#main-nav.open`)
  2. Scroll fade-in via `IntersectionObserver` over all `.fade-in` elements (one-time reveal, unobserved after trigger)
  3. Footer copyright year (`#year`, set from `new Date()`)
  4. Enquiry form validation/submission (`#enquiry-form` submit handler) — validates name/email/phone client-side, writes per-field errors into `<span class="error-msg" id="{field}-error">`, and on success logs a plain `data` object to the console and shows a status message in the `aria-live` region. There is no backend: the `fetch(...)` call to persist submissions is stubbed out as a `// TODO` comment inside the submit handler — wire that up if a real API is added later.

Keep new code within this same three-layer structure (HTML in `<body>`, styles in the single `<style>` block, behavior in the single `<script>` block) rather than splitting into separate files, unless asked to change the project's build approach.

## Manual verification

When changing this file, check in a browser:
- Nav links smooth-scroll to their section; sticky header behaves correctly on scroll; hamburger menu opens/closes on narrow viewports.
- Layout at mobile (~375px) and desktop widths.
- Testimonial cards fade in on scroll.
- Enquiry form: submitting empty shows inline field errors (no `alert()`); submitting valid data shows the success message, resets the form, and logs the submitted data object to the devtools console.
- Footer year is correct.
- Full keyboard-only tab pass through nav, buttons, and form fields shows visible focus states.
