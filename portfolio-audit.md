# Portfolio audit: Udit Singh (`index.html`)

**Date:** 19 September 2026
**File audited:** `index.html` (about 37 KB, single file, Tailwind Play CDN + vanilla JS)

## How this audit was done

- Line-by-line code review of the HTML, CSS and JavaScript.
- Colour contrast ratios were calculated from the exact hex values used on the page (WCAG 2.x formula).
- Automated static checks: duplicate IDs, broken in-page anchors, unbalanced tags, heading structure.
- **Not done:** the page was not opened in a browser, and no Lighthouse, screen-reader or real-device testing was run. Treat the findings below as a code-level audit and re-test in a browser before publishing.

## Summary

| Area | Result | Notes |
|---|---|---|
| Structure and validity | Pass | No duplicate IDs, no broken anchors, tags balanced, one `h1` with a clean `h2` to `h4` outline |
| Responsive layout | Pass (by code review) | Mobile-first grid, hamburger menu, code window scrolls inside its own box |
| Keyboard access | Pass | Skip link, visible focus ring, menu closes on Esc, form fields have real labels |
| Reduced motion | Pass | Hero animation, typing effect and transitions are disabled when the user asks for it |
| Colour contrast | **Needs fixes** | Four combinations fall below WCAG AA (details below) |
| SEO and sharing | **Needs work** | Title and description exist; no favicon, Open Graph tags or canonical URL |
| Contact form | **Needs work** | Works only if the visitor has an email app set up; message is cleared on submit |
| Production readiness | **Needs work** | Tailwind Play CDN is meant for prototyping, and 5 links are still placeholders |

## Findings

### High priority (fix before sharing the link)

**H1. Button text on hover fails contrast (2.98:1)**
White text on `indigo-400` (used on hover for the hero button, form button and mobile CTA) is 2.98:1. AA needs 4.5:1 for normal text.
The resting state (`indigo-500`) is 4.47:1, which is just under the line.
*Fix:* use `bg-indigo-600` at rest (about 6.3:1 with white) and `hover:bg-indigo-500` on hover.

**H2. Small grey text is too faint (3.64:1 and 3.96:1)**
`text-slate-500` is used for the labels in the About facts panel, the social link sub-captions and the footer. Measured contrast is 3.64:1 on the panel colour and 3.96:1 on the page background.
*Fix:* change those to `text-slate-400` (6.76:1 on panel, 7.34:1 on background).

**H3. Contact form loses the message and depends on a mail client**
The form builds a `mailto:` link and then calls `form.reset()` immediately. If the visitor has no email app configured (common on shared or university computers), nothing opens and the typed message is gone.
*Fix (quick):* remove `form.reset()` and only clear the form after the visitor confirms.
*Fix (better):* post to a form service such as Formspree, Web3Forms or Netlify Forms, and show a real "Message sent" state.

**H4. Placeholder content is still live**
- 5 links use `href="#"` (project Source code / Live links). They scroll to the top instead of going anywhere.
- The email is `your.email@example.com` in two places, plus `CONTACT_EMAIL` in the script.
- GitHub and LinkedIn point to the site homepages, not your profiles.
- The three projects and the skill list are sample entries.
*Fix:* replace them, or remove any card or link you cannot fill in yet. An empty link looks worse than no link.

### Medium priority

**M1. Placeholder text in the form is very low contrast (2.48:1)**
`placeholder-slate-600` on the dark input background is hard to read. Placeholders are not a substitute for labels, and you do have labels, so this is not a blocker.
*Fix:* use `placeholder-slate-500` at minimum.

**M2. Form errors are not linked to their fields for screen readers**
Each field gets `aria-invalid` and a visible message, but the message is not connected with `aria-describedby`, so a screen reader may not read the error when the field is focused.
*Fix:* give each error `<p>` an `id` (for example `name-error`) and set `aria-describedby="name-error"` on the input when the error is shown.

**M3. Tailwind Play CDN is not meant for production**
It compiles styles in the visitor's browser, which adds a delay and a flash risk on slow connections, and Tailwind advises against it for live sites.
*Fix:* when you host the site, build a static CSS file with the Tailwind CLI and replace the `<script src="https://cdn.tailwindcss.com">` tag and inline config with a single `<link>` to the generated stylesheet.

**M4. Missing metadata for sharing and search**
There is no favicon, no Open Graph / Twitter tags (so link previews on WhatsApp, LinkedIn and Slack will look bare) and no canonical URL.
*Fix:* add a favicon, `og:title`, `og:description`, `og:image` (1200×630) and `og:url` once you know the final address.

**M5. Roll number is shown three times**
`25BCON0764` appears in the hero code window, the About panel and the education timeline. Your request asked for it, so it is included, but a student ID on a public page can be used to look you up or impersonate you in university systems.
*Suggestion:* keep it in one place (the timeline) or remove it from the public version entirely. Your call.

### Low priority

**L1. Typed heading and screen readers.** The rotating phrase is inside `aria-live="off"`, so it will not spam announcements, but a screen reader will read whichever phrase happens to be showing. Acceptable; if you want it fully stable, wrap the typed part in `aria-hidden="true"` and add a visually hidden sentence such as "I build web interfaces and useful software".

**L2. Google Fonts are loaded from a third party.** Fonts are set to `display=swap`, so text stays visible while loading. If privacy or speed matters, self-host the three font families.

**L3. Desktop nav link sizes.** The nav links are `text-sm` with a small hit area. They meet minimum size on desktop where pointers are precise; the mobile menu uses larger targets. No action needed unless you add a touch-screen laptop breakpoint.

**L4. No `<noscript>` note.** The page renders fully without JavaScript (hero shows the first typed phrase, all sections are visible), but the form and mobile menu need JS. A one-line `<noscript>` message near the form would help.

**L5. No sitemap or robots.txt.** Only relevant once the site has its own domain.

## What passed

- **Contrast (good):** body text 12.68:1, secondary text 7.34:1, cyan accent 12.99:1, indigo accent 9.45:1, error text 9.17:1, success text 12.35:1.
- **Semantics:** landmarks (`header`, `nav`, `main`, `footer`), one `h1`, labelled form controls, `aria-expanded` on the menu button, `role="status"` on the form message.
- **Motion:** the only automatic motion is the hero entrance and the typing effect; everything else responds to hover or click, and all of it respects `prefers-reduced-motion`.
- **Safety:** external links use `rel="noopener noreferrer"`; no user input is inserted into the page as HTML.
- **Integrity:** every in-page link (`#about`, `#skills`, `#projects`, `#contact`, `#home`, `#main`) has a matching target.

## Contrast measurements

| Foreground on background | Ratio | AA (4.5:1 for normal text) |
|---|---|---|
| Body text `slate-300` on page | 12.68 | Pass |
| Secondary text `slate-400` on page | 7.34 | Pass |
| Secondary text `slate-400` on card | 6.76 | Pass |
| Cyan `cyan-300` on page | 12.99 | Pass |
| Indigo `indigo-300` on page | 9.45 | Pass |
| Chip text `indigo-200` on card | 11.62 | Pass |
| Error `rose-300` on card | 9.17 | Pass |
| Success `emerald-300` on page | 12.35 | Pass |
| White on `indigo-500` button | 4.47 | **Fail (marginal)** |
| White on `indigo-400` button hover | 2.98 | **Fail** |
| Labels `slate-500` on card | 3.64 | **Fail** |
| Footer text `slate-500` on page | 3.96 | **Fail** |
| Placeholder `slate-600` on input | 2.48 | **Fail** (placeholder only) |

## Pre-launch checklist

- [ ] Swap button colours to `indigo-600` / hover `indigo-500` (H1)
- [ ] Change `text-slate-500` to `text-slate-400` (H2)
- [ ] Decide on a real form backend, or stop clearing the form after `mailto:` (H3)
- [ ] Replace placeholder email, social links, skills and projects (H4)
- [ ] Decide whether the roll number should stay public (M5)
- [ ] Add favicon and Open Graph tags (M4)
- [ ] Build Tailwind to a static CSS file for hosting (M3)
- [ ] Open the page on a phone and a laptop, tab through it with the keyboard, and run Lighthouse in Chrome DevTools
