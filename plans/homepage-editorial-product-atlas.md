# Homepage Redesign Plan — Editorial Product Atlas

## Objective

Completely redesign the homepage as a high-end editorial product portfolio that persuades design, product, and engineering hiring teams first. Preserve the existing narrative and project copy verbatim, but allow navigation labels, section numbering, metadata labels, and CTA labels to be adjusted where needed for the new information architecture.

The work is limited to the homepage. Existing case-study pages, Build Lab, contact page, 404 page, assets, and deployment setup remain unchanged.

## Confirmed Product Decisions

- **Direction:** Editorial Product Atlas
- **Primary audience:** Hiring teams
- **Scope:** Homepage only
- **Copy rule:** Preserve all narrative/project copy verbatim; navigation, labels, numbering, metadata, and CTA text may be revised
- **Motion:** Expressive, with accessible reduced-motion behavior
- **New assets:** None required; reuse the current project screenshots, WiseWay imagery, profile portrait, tool marks, and resume
- **Implementation model:** Continue using the current self-contained static `index.html`; add no framework or runtime dependency

## Repository Facts and Constraints

- The homepage’s markup, CSS, and JavaScript all live in `index.html`.
- The page currently has no external JavaScript runtime dependency.
- Existing project and site pages deep-link to these homepage IDs, which must remain unchanged:
  - `#work`
  - `#about`
  - `#principles`
  - `#tools`
- Existing linked destinations must continue to work:
  - `build-lab.html`
  - `work/employee-experience.html`
  - `work/auto-qa.html`
  - `work/wiseway.html`
  - `contact.html`
  - `assets/Purnet_resume.pdf`
  - Current external Kapture and Behance URLs
- Existing metadata, canonical URL, Open Graph tags, and Twitter tags should remain unless a visual theme-color update is needed.
- Do not modify `.figma/make/**`, linked pages, or asset files for this redesign.

## Information Architecture and Page Flow

### 1. Persistent editorial navigation

Replace the floating pill navigation with a thin, full-width editorial masthead.

- Left: brand/name linked to `#top`
- Center or right: Work, About, Skills, Tools anchors
- Far right: contact CTA
- Add a subtle section-progress marker or active-section state driven by `IntersectionObserver`
- Preserve the current mobile menu’s accessible button semantics:
  - `aria-expanded`
  - close on link selection
  - close on outside click
  - close on `Escape`
- Keep navigation readable over every section without relying on cursor color inversion

### 2. Asymmetric identity hero (`#top`)

Create a 12-column opening composition rather than a centered headline block.

- Place “Design Engineer - Portfolio '26” as a compact editorial kicker.
- Preserve the hero statement exactly:
  - “Design systems that hold up.”
  - “I design with engineering constraints and edge cases in mind — so ideas actually ship.”
- Use a large left-weighted headline with deliberately uneven line lengths.
- Place the supporting statement and primary actions in a narrow right-side column.
- Use cropped layers of existing project imagery as an “atlas preview” across the lower hero edge:
  - Employee Experience dashboard
  - Auto QA agent/conversation UI
  - WiseWay mobile screen
- Keep Build Lab and About destinations available, but CTA labels may be normalized for the new hierarchy.
- Remove the current cycling typewriter words; the preserved headline remains static and gains expressive entrance choreography instead.
- Add a visible cue that the project atlas continues below without adding a new product claim.

### 3. Indexed project atlas (`#work`)

Rebuild the four repeated cards into a desktop editorial atlas with a sticky project index and four distinct project chapters.

#### Desktop structure

- Left rail:
  - Sticky project index listing Build Lab, Employee Experience, Auto QA, and WiseWay
  - Active project updates as chapters enter the viewport
  - Category metadata and section position
- Right canvas:
  - Four vertically sequenced project chapters
  - Each chapter uses a different image composition while sharing consistent text hierarchy and controls
- Do not use scroll hijacking or mandatory scroll snapping.

#### Project chapter treatments

1. **Build Lab**
   - Treat as the experimental opener, but not as more important than shipped case studies.
   - Preserve its title, description, and “Testing ideas…” statement verbatim.
   - Replace the current simulated terminal-heavy card with a cleaner editorial “live experiment” composition.
   - A small terminal/progress motif may remain, but it should support rather than dominate the project.

2. **Employee Experience**
   - Use a dominant desktop screenshot with two partially layered supporting screens.
   - Preserve the project statement verbatim.
   - Keep both case-study and external-site destinations.

3. **Auto QA**
   - Use a side-by-side or offset contact-sheet treatment of agent, conversation, and dashboard screens.
   - Preserve the project statement verbatim.
   - Keep both case-study and external-site destinations.

4. **WiseWay**
   - Use a tall mobile composition that breaks the desktop grid.
   - Preserve the project statement verbatim.
   - Keep the case-study and Behance destinations.
   - Preserve the existing “Built for senior eyes,” “NGO-backed assistance,” “Medicine reminders,” and “Ads made for seniors” concepts if retained as visible annotations; they may be repositioned.

#### Mobile structure

- Collapse the sticky index into a compact numbered header or horizontal project selector.
- Render each project as a normal document-flow article.
- Use one primary image plus restrained supporting imagery to avoid oversized pages and horizontal overflow.
- Keep every CTA at least 44px in effective touch height.

### 4. Profile interlude (`#about`)

Move the About section directly after the work atlas as a full-width visual reset.

- Use a bold cobalt field or dark ink field to distinguish it from project content.
- Pair the full-height profile portrait with an editorial text column.
- Preserve all three biography paragraphs verbatim.
- Preserve “Hi, I'm Purnet 👋” and “Based in Bangalore, India.”
- Keep contact and resume actions.
- Use the portrait as a deliberate crop with responsive `object-position`, not as a generic card.
- Avoid sticky portrait behavior on small screens.

### 5. Numbered principles manifesto (`#principles`)

Replace the six equal cards with a typographic manifesto.

- Preserve the section heading, supporting sentence, all six principle titles, and all descriptions verbatim.
- Render principles as six large indexed rows or a staggered two-column editorial list.
- Pair each item with its existing category:
  - Foundations
  - Exploration
  - Craft
  - Partnership
  - Business
  - Cadence
- Use large numerals and typographic hierarchy instead of icon-led cards.
- On desktop, reveal rows in a controlled sequence; on mobile, use a simple stacked list.

### 6. Tools ledger (`#tools`)

Condense the current six dark cards into a capability ledger near the end of the narrative.

- Preserve the section heading and supporting sentence verbatim.
- Preserve all six tool names and descriptions verbatim.
- Use a structured table/ledger composition with tool mark, index, name, and description.
- Add expressive hover/focus expansion on capable devices, but keep all descriptions visible or programmatically reachable without hover.
- Avoid an auto-scrolling marquee that makes content difficult to read.

### 7. Engagement footer (`#contact`)

Turn the footer into a strong closing editorial panel.

- Preserve:
  - “Have a project in mind?”
  - The complete 2026 engagement sentence
  - Existing social destinations
  - Existing site and case-study destinations
  - Copyright
- Navigation and legal-link labels may be retained or simplified, but do not fabricate real policy destinations; placeholder `#` links must remain placeholders unless intentionally removed from the homepage.
- Make the contact route the clear final action.

## Visual Design System

### Typography

Replace the current Fraunces/JetBrains Mono/Caveat combination on the homepage with:

- **Display/editorial:** Instrument Serif
- **UI/body:** DM Sans
- **Metadata/indexes:** IBM Plex Mono

Load the public Google fonts through one CSS2 request. Use strong fallbacks and avoid using more than the necessary weights.

Suggested scale:

- Hero display: `clamp(4.5rem, 10vw, 10.5rem)`
- Section display: `clamp(3rem, 6vw, 6.5rem)`
- Project headline: `clamp(2rem, 4vw, 4.5rem)`
- Body: `clamp(1rem, 1.2vw, 1.2rem)`
- Metadata: 0.7–0.8rem with controlled tracking

### Color

Adopt a new restrained editorial palette:

- Canvas / bone: `#F2EFE7`
- Paper: `#FBFAF6`
- Ink: `#11110F`
- Muted ink: `#68665F`
- Cobalt accent: `#2457FF`
- Acid accent: `#D9FF57`
- Hairline: `rgba(17, 17, 15, 0.16)`

Use cobalt for section fields, active states, and selected project markers. Use acid sparingly for status, focus, and a small number of high-value accents. Do not carry over the existing rainbow gradient as a primary motif.

### Grid and surfaces

- Desktop: 12-column grid, maximum width approximately 1500px, 24–32px gutters
- Tablet: 8-column grid
- Mobile: 4-column grid with 18–20px side padding
- Prefer square or lightly rounded surfaces; remove the current collection of rounded floating cards
- Use thin rules, oversized indexes, cropped media, and whitespace as the main design devices
- Keep project screenshots legible and avoid decorative perspective distortion that obscures product UI

## Motion and Interaction

Implement expressive motion with native CSS and JavaScript only.

### Retain or add

- Hero sequence:
  - masthead/kicker reveal
  - line-by-line headline mask reveal
  - staggered supporting copy/actions
  - project-preview image layers entering with slight depth offsets
- Work atlas:
  - active project index updates via `IntersectionObserver`
  - image reveal masks as chapters enter
  - bounded image translation/parallax on desktop using a single `requestAnimationFrame` loop
  - hover/focus media emphasis without changing layout
- Principles:
  - sequential row reveal
- Tools:
  - focus/hover expansion or accent sweep
- Navigation:
  - active-section indicator
  - controlled masthead background transition after leaving the hero

### Remove

- Pixel-cube cursor trail
- Hidden system cursor
- Cursor dark/light inversion logic
- Cycling typewriter headline
- Existing Build Lab progress animation if it no longer matches the new project treatment
- Generic reveal classes that are superseded by section-specific motion

### Motion safeguards

- Do not hijack scrolling.
- Disable parallax, stagger delays, transforms, and looping animation under `prefers-reduced-motion: reduce`.
- Ensure reduced-motion content is immediately visible.
- Run parallax only when the relevant section is near the viewport.
- Use transform/opacity animation only; avoid layout-thrashing properties.
- Disable pointer-specific effects for coarse pointers and narrow screens.

## Accessibility and Semantics

- Preserve semantic landmarks: `nav`, `header`, `main`, `section`, `article`, `footer`.
- Add a `<main>` wrapper if absent.
- Maintain a single `h1` and logical heading order.
- Restore the native cursor rather than forcing `cursor: none`.
- Preserve visible focus states with sufficient contrast on every background.
- Keep meaningful image alt text and mark purely decorative layers appropriately.
- Ensure all image-heavy project chapters still expose project name, description, and actions in document order.
- Keep mobile menu keyboard-operable and support `Escape`.
- Ensure no text or interaction depends solely on hover.
- Maintain at least WCAG AA contrast for body text and controls.
- Preserve the existing reduced-motion media query, expanded for all new behavior.

## Responsive Behavior

### Desktop, 1200px and above

- Full 12-column composition
- Sticky project index
- Layered project imagery
- Expressive but bounded parallax
- Wide editorial type scale

### Tablet, approximately 768–1199px

- Reduce to an 8-column grid
- Keep the project index visible where space allows, but reduce its width
- Simplify layered imagery and parallax distance
- Convert hero previews from free layers to a controlled image strip

### Mobile, below approximately 768px

- Four-column document flow
- No sticky project rail
- No parallax
- Project chapters become stacked articles
- Headline remains large but avoids forced desktop line breaks
- About portrait and copy stack naturally
- Principles and tools become single-column lists
- Mobile navigation opens in a full-width panel below the masthead
- Prevent horizontal overflow from images, labels, and large display type

## Implementation Steps

1. **Inventory and copy lock**
   - Extract the existing narrative/project copy and destination URLs into a temporary implementation checklist.
   - Mark which labels/CTAs may change under the confirmed copy rule.
   - Confirm all cross-page anchor IDs that must survive.

2. **Replace homepage design tokens and global styles**
   - Update the Google Fonts request.
   - Replace color, type, spacing, radius, and grid variables.
   - Restore standard cursor behavior.
   - Establish focus, selection, base media, container, and responsive rules.

3. **Rebuild semantic markup**
   - Create the masthead, main wrapper, hero, project atlas, profile interlude, principles manifesto, tools ledger, and footer in the decided order.
   - Keep required IDs and linked destinations.
   - Preserve narrative/project copy verbatim.
   - Reuse only existing assets.

4. **Implement desktop editorial layouts**
   - Build the 12-column hero.
   - Build the sticky project index and four distinct chapters.
   - Build the full-width about interlude.
   - Build principles rows, tools ledger, and closing footer.

5. **Implement responsive layouts**
   - Add tablet simplifications.
   - Replace sticky/layered compositions with mobile document flow.
   - Test long headlines, CTA wrapping, image cropping, and menu behavior.

6. **Replace interaction code**
   - Remove the cursor trail, cursor inversion, typewriter, and superseded card animation code.
   - Implement mobile navigation behavior with `Escape` support.
   - Implement active navigation/project tracking with `IntersectionObserver`.
   - Implement entrance choreography and bounded desktop parallax.
   - Gate all motion with reduced-motion and pointer/viewport capability checks.

7. **Polish and performance pass**
   - Add explicit image dimensions or `aspect-ratio` containers to limit layout shift.
   - Use `loading="lazy"` for below-the-fold imagery while keeping hero-critical imagery eager.
   - Ensure duplicated decorative image layers do not create redundant screen-reader output.
   - Verify no unused old selectors or JavaScript remain.

## Verification Strategy

### Content and link audit

- Compare every original narrative/project string against the redesigned page.
- Confirm all internal and external destinations remain correct.
- Confirm `#work`, `#about`, `#principles`, and `#tools` still resolve from linked pages.
- Confirm resume download and contact route still work.

### Functional checks

- Desktop and mobile navigation
- Active-section state
- Sticky project index and active-project state
- All project CTAs
- Mobile menu open, close, outside-click, link-click, and `Escape`
- Scroll-to-top behavior if retained
- Reduced-motion behavior
- Keyboard-only traversal and visible focus

### Visual checks

Review at minimum:

- 1440×900 desktop
- 1024×768 tablet
- 390×844 mobile
- 320px-wide narrow mobile

Check:

- no horizontal overflow
- no clipped display text
- sensible image crops
- readable project screenshots
- stable sticky transitions
- footer and mobile menu layering
- contrast across bone, cobalt, ink, and acid surfaces

### Runtime checks

- Inspect the browser console for errors.
- Confirm the supervised preview returns the homepage and all referenced local assets successfully.
- Run `figma make verify-bootstrap` because this is a broad homepage rewrite and confirm terminal success.
- Do not run deploy or deploy-preview verification unless deployment scripts are changed.

## Acceptance Criteria

- The homepage reads as a new editorial portfolio rather than a restyle of the current card stack.
- Hiring teams encounter the strongest project evidence before biography, skills, or tools.
- All original narrative and project copy remains verbatim.
- Required homepage IDs and all existing case-study/contact/resume destinations continue to work.
- The desktop work section has a functioning indexed atlas treatment; mobile has a clear non-sticky equivalent.
- Expressive motion works without scroll hijacking and becomes static under reduced-motion preferences.
- The page is keyboard usable, retains visible focus, and does not rely on hover alone.
- The redesign is responsive at the specified breakpoints with no horizontal overflow.
- Only `index.html` is modified.
