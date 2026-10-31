# Homepage Redesign Plan — Personal Maker’s Desk

## Objective

Rebuild the homepage around the flow and visual rhythm of the referenced Figma portfolio while using Purnet’s own content, project assets, company history, personal story, and original illustrations.

The page should feel compact, friendly, playful, and highly authored:

1. Floating navigation
2. Interactive personal maker’s desk
3. Friendly introduction
4. Self-scrolling company marquee
5. One featured Auto QA case study with an honest “Impact & scale” panel
6. A 2×2 grid of four additional projects
7. About section with Skills and Tools folded into it
8. Contact panel
9. Footer with a decorative reprise of the personal objects

The implementation remains a dependency-free static homepage.

## Confirmed Decisions

- **Reference:** Figma file `0vUtDi42iLA6FpEm0G7K0S`, node `4:3`
- **Use of reference:** Follow its page rhythm and visual language closely, but do not copy its portfolio content or decorative artwork
- **Hero concept:** Original interactive “Playful maker’s desk”
- **Hero assets:** Custom illustrated cutouts, not stock photography
- **Portrait:** About section only
- **Featured project:** Auto QA
- **Featured evidence treatment:** Mixed “Impact & scale” panel
- **Project grid:** Employee Experience, WiseWay, Calora, and SpendSmart
- **Company marquee order:** Prime Focus Technologies, Horus Music, Dentsu Creative, Kapture CX
- **Skills and Tools:** Fold into About while preserving current deep links
- **Scope:** Homepage and the new assets it needs; existing case-study, Build Lab, and contact pages remain unchanged

## Repository and Reference Constraints

- The homepage is a self-contained static document in `index.html`.
- No React, Tailwind, package dependency, or build framework should be introduced.
- Existing linked pages deep-link to these homepage IDs; preserve them:
  - `#work`
  - `#about`
  - `#principles`
  - `#tools`
- Preserve working destinations for:
  - `work/auto-qa.html`
  - `work/employee-experience.html`
  - `work/wiseway.html`
  - `build-lab/calora.html`
  - `build-lab/spendsmart.html`
  - `build-lab.html`
  - `contact.html`
  - `assets/Purnet_resume.pdf`
  - Current Kapture CX, Behance, LinkedIn, and GitHub links
- Keep current canonical, Open Graph, Twitter, and description metadata unless the theme-color value needs to change.
- Use the reference as a layout/design target, not as a content source. Do not ship its hero/footer collage images.
- The downloaded `.figma/design-context/remix-4-3` directory is inspection-only and must not be included in the final commit.

## Source Figma Assets

### Exact company logos

Use the supplied Figma nodes and their exact SVG assets:

1. **Prime Focus Technologies**
   - File: `2OXvMQoGyQwuQHhU1CAQo7`
   - Node: `170:134`
   - Archive: `f98f34f5-5d37-49e7-ba39-ea1dc8ca2399.zip`
   - Asset: `143b0.svg`

2. **Horus Music**
   - Node: `170:141`
   - Archive: `caa76b07-3e08-4cbe-9828-4be8fb9c0747.zip`
   - Asset: `036fc.svg`

3. **Dentsu Creative**
   - Node: `170:182`
   - Archive: `c14069df-c9fd-4f61-ba41-1f8e9bd63bfe.zip`
   - Asset: `6b263.svg`

4. **Kapture CX**
   - Node: `170:199`
   - Archive: `db6a8509-385f-4cca-a433-d2ba762e6e88.zip`
   - Asset: `f2cf5.svg`

During implementation:

- Download each archive with `curl -L`.
- Extract the exact SVG.
- Save stable local copies under `assets/companies/`.
- Do not retain temporary Figma URLs.
- Confirm every SVG is non-empty.
- Preserve each SVG’s root width and height; size it through a wrapper rather than broad rules that overwrite the root geometry.

### Imported reference fonts

Follow the Figma font manifest exactly before implementing typography:

- Plus Jakarta Sans — Bold
- Plus Jakarta Sans — Medium
- Plus Jakarta Sans — SemiBold
- Roboto Mono — Regular
- Roboto Serif — Medium
- Roboto Serif — Regular
- Roboto Serif — SemiBold
- Georgia — Regular

Implementation procedure:

1. Create one JSON manifest containing every exact family/style row.
2. Run one `figma fonts resolve --input <manifest> --out-dir public/fonts`.
3. Correlate results by exact family/style.
4. Copy any resolved file-backed payloads needed by this static site into `assets/fonts/`.
5. Declare one `@font-face` for every resolved `cssFamily`, preserving resolver-provided style, weight, and format.
6. Reference the stable `assets/fonts/` copy from `index.html`; do not leave build-time or temporary paths.
7. If a row is blocked, use the documented fallback under its own family name and report it rather than inventing a face.
8. Do not invoke the separate fonts-wiring skill for these manifest rows.

## Visual Design System

### Overall composition

Follow the reference’s alternation between a narrow narrative column and wider portfolio sections:

- Intro, company strip, featured-project narrative, and About copy: approximately 720–768px content width
- Project grid, Contact panel, and wider media moments: approximately 1152px maximum width
- Full page background: soft gray
- Cards: off-white or white
- Use thin gray dividers to create rhythm between sections
- Use compact vertical spacing in the intro and featured sections, then larger breathing room around the grid and contact panel

### Color

Use the reference-derived palette:

- Page gray: `#F2F2F2`
- Surface: `#FAFAFA`
- White card: `#FFFFFF`
- Primary ink: `#222222`
- Secondary ink: `#555555`
- Quiet text: `#999999`
- Border: `#D6D6D6`
- Hero coral: `#ED7454`
- Deep accent: `#C0305E`
- Navigation glass: `rgba(17,17,17,.80)`

Use coral for the hero and key emphasis. Use the deep accent for project links, section kickers, and contact actions. Avoid carrying forward the current cobalt/acid palette on the homepage.

### Typography

- **Plus Jakarta Sans:** navigation, introduction, featured-project narrative, buttons, and About
- **Roboto Mono:** project names, major grid headings, and selected display lines
- **Roboto Serif:** project card descriptions, evidence labels, metadata, and contact supporting copy
- **Georgia:** fallback only where the imported reference uses it and the exact face is available

Preserve exact resolved faces rather than substituting generic weights.

### Surfaces

- Navigation: 54px dark translucent floating bar with 8px radius and blur
- Featured media: 8px radius
- Small evidence cards: 8px radius
- Project cards and contact panel: 24px radius
- Use low, diffuse shadows only on project cards
- Use pills for metadata tags and primary contact action

## Page Structure

### 1. Floating navigation

Build a centered floating navigation bar similar to the reference.

- Brand/avatar mark at left
- Links:
  - Work
  - Highlight
  - About
  - Experience
  - Contact
- Resume link at right or within the menu
- Active section state via `IntersectionObserver`
- On narrow screens, collapse into an accessible menu button
- Support:
  - `aria-expanded`
  - link-click close
  - outside-click close
  - `Escape` close
  - keyboard-visible focus
- Preserve the existing skip link before navigation

### 2. Interactive personal maker’s desk (`#top`)

Create an original illustrated desk/pegboard at the top of the page, inspired by the reference’s tactile collage but based entirely on Purnet’s personal story.

#### Personal objects

Create custom cutout illustrations for:

1. **Music production controller**
   - Represents years producing music with MIDI controllers
   - Connects to audio technology, complex systems, and multi-channel signal flow

2. **Camera**
   - Represents travel, café photography/video, Mass Communication, and visual storytelling

3. **Game controller**
   - Represents downtime and an interest in interactive UI physics and tactile interfaces

4. **The Paradox of Choice**
   - Render as an illustrated book cover with the title as text
   - Represents “Subtractive Taste,” simplifying architecture, and reducing decision fatigue

5. **Cup of tea/coffee**
   - Represents energy for rapid iteration and hands-on vibe-coding sessions

6. **Bengaluru marker**
   - Represent as a map pin, transit-sign fragment, or illustrated city label
   - Connects Purnet to India’s startup and technology ecosystem

7. **Sticky notes**
   - Include short, non-claim phrases such as “reduce the choice,” “test the edge,” and “ship the system”

8. **Terminal/code snippet**
   - Use a compact code window derived from the current Build Lab vocabulary

Before creating these assets, invoke the `make/icon-illustration` skill and follow it for a coherent illustration family.

#### Illustration language

- Flat cutout forms with limited detail
- Coral, charcoal, cream, and muted green
- Thin dark outlines
- Subtle paper texture and drop shadows
- Small rotations and imperfect placement
- Avoid emoji, generic stock icons, or photorealistic imagery
- Keep all meaningful words as real HTML text where possible; do not convert labels or the book title to paths

#### Interaction model

- Visitors can drag each object within the desk boundary using pointer or touch input.
- Clicking/tapping an object brings it to the front.
- A “Shuffle desk” control animates objects to a new safe arrangement.
- A “Reset” control restores the authored default arrangement.
- Dragging uses pointer capture and clamps objects inside the stage.
- No object state needs to persist across reloads.
- Keyboard support:
  - Each object is focusable
  - Arrow keys move the focused object in small increments
  - `Shift` + arrow uses a larger increment
  - Focused objects expose a short accessible label
- Add a nonvisual list describing the personal meaning of each object for screen readers.
- Under reduced motion:
  - Remove idle movement and settling animation
  - Keep direct drag/keyboard positioning
  - Make Shuffle change positions without animation
- On small screens:
  - Use fewer overlapping positions
  - Reduce drag bounds and object count only if necessary for readability
  - Keep every personal object represented, with minor objects grouped into one layer if needed

### 3. Friendly introduction

Place the introduction immediately below the maker’s desk in the narrow narrative column.

Preserve the current core homepage copy:

- “Design systems that hold up.”
- “I design with engineering constraints and edge cases in mind — so ideas actually ship.”

Actions:

- “View my work” → `#work`
- “View resume” → `assets/Purnet_resume.pdf`

Use the reference’s direct, friendly scale rather than the current oversized full-viewport hero.

### 4. Company marquee (`#experience`)

Add a self-scrolling strip labeled “Companies and teams I’ve worked with.”

Order:

1. Prime Focus Technologies
2. Horus Music
3. Dentsu Creative
4. Kapture CX

Behavior:

- Duplicate the visual track for a seamless CSS loop
- Mark the duplicate as `aria-hidden="true"`
- Preserve one semantic list of company names for assistive technology
- Pause on hover and keyboard focus
- Use grayscale/monochrome logos with consistent optical height, not equal box width
- Add soft edge masks
- Under reduced motion, disable animation and render a centered wrapping row
- Do not link logos unless a verified company destination is already available; Kapture can link to its existing public URL, while the others remain noninteractive until destinations are explicitly supplied

### 5. Featured Auto QA project (`#work`, `#highlight`)

Make Auto QA the dominant case study.

#### Header and media

- Section kicker: “Featured Project”
- Use `assets/Mockups/comp-qa.jpg` as the dominant feature media
- Add one supporting UI screen from `assets/QA-opt/`
- Use a coral media field inspired by the reference
- Preserve the current project statement:
  - “Designed a system for agent QA and self-improvement, aligned with real workflow constraints.”
- Use a concise overview derived from the existing Auto QA case study:
  - AI-powered evaluation for customer-support conversations at scale
  - Continuous, structured, actionable quality feedback
  - Human-readable scoring and override controls

#### “Impact & scale” panel

Use four cards in a responsive 2×2 panel:

1. **100%**
   - Label: “Workflow adoption”
   - Classification: verified outcome supplied by the user

2. **2**
   - Label: “Core personas designed in parallel”
   - Supporting text: Evaluators and agents
   - Classification: repository-supported scope

3. **6**
   - Label: “Core product capabilities”
   - Classification: repository-supported scope

4. **100s**
   - Label: “Conversations supported in batch review”
   - Classification: repository-supported scale; phrase as capability, not measured adoption

Do not present all four as business outcomes. The section title and small labels must make the evidence type honest.

Actions:

- “Read case study” → `work/auto-qa.html`
- “Visit website” → existing Kapture Auto QA URL

### 6. 2×2 project grid

Use a wide 1152px section matching the reference’s card rhythm.

Each card includes:

- Large media area
- Category / project type
- Role or scope label where supported
- Project name
- Existing project description
- Up to three factual tags
- One evidence line where supported
- Clear destination link

#### Card 1 — Employee Experience

- Media: `assets/Mockups/comp-ex.jpg`
- Preserve its existing homepage statement
- Evidence: `~25% boost in team efficiency`
- Mark as a user-supplied verified outcome
- Supporting repository evidence may mention 4 personas, 4 surfaces, 60+ components, and 8 layout patterns
- Link to `work/employee-experience.html`

#### Card 2 — WiseWay

- Media: `assets/Mockups/comp-ww.jpg` or the strongest existing WiseWay thumbnail
- Preserve its existing homepage statement
- Do not invent a success outcome
- Show factual project specifications:
  - Academic project
  - Maximum 3-step booking flow
  - 56px minimum touch targets
  - WCAG AA contrast
- Link to `work/wiseway.html`

#### Card 3 — Calora

- Preserve the existing Build Lab description verbatim
- Tags: AI, E-commerce, Discovery
- Use a noninteractive lazy preview of the existing live app inside the media slot on desktop:
  - Source: existing Calora deployment
  - `pointer-events: none`
  - `tabindex="-1"`
  - `aria-hidden="true"`
- Keep a branded coral/cream fallback beneath the preview so the card remains designed if the frame is unavailable
- On narrow screens or reduced-data layouts, show the static fallback instead of the live frame
- Link the card to `build-lab/calora.html`

#### Card 4 — SpendSmart

- Preserve the existing Build Lab description verbatim
- Tags: Personalization, Claude API, FinTech
- Use the same lazy noninteractive preview/fallback strategy as Calora
- Link the card to `build-lab/spendsmart.html`

### 7. About (`#about`)

Follow the reference’s compact personal profile composition.

- Section title: preserve “Hi, I’m Purnet”
- Reuse `assets/profilepic.png`
- Keep portrait only here, never in the hero
- Preserve the current three biography paragraphs verbatim
- Include a dark profile panel with:
  - Design Engineer / Product Designer positioning
  - Bengaluru, India
  - Contact CTA
  - Resume CTA
- Do not add education, email, or availability claims not currently provided

### 8. Folded Skills and Tools (`#principles`, `#tools`)

Integrate both current sections inside About instead of keeping them as full-page standalone sections.

#### How I work (`#principles`)

- Preserve all six existing principle names and descriptions
- Render as a compact accessible accordion or two-column disclosure list
- Use native `<details>` / `<summary>` where practical
- Keep one item open by default on desktop and all closed on mobile
- Ensure all copy remains available without JavaScript

#### Toolkit (`#tools`)

- Preserve the six current tools and descriptions:
  - Figma
  - Claude
  - Framer
  - ChatGPT
  - GitHub
  - Vercel
- Render as a compact icon strip plus an accessible list of descriptions
- The visual icon strip may self-scroll slowly only if it remains readable; otherwise use a static row
- Pause motion on hover/focus and disable it under reduced motion

### 9. Contact panel (`#contact`)

Use the reference’s centered, wide contact panel:

- Off-white/gray card with 24px radius
- Preserve:
  - “Have a project in mind?”
  - “Taking on a small number of product-design engagements for 2026 — systems work, zero-to-one, or the messy middle.”
- Primary CTA → `contact.html`
- Secondary links:
  - LinkedIn
  - Behance
  - Resume
- Do not expose or invent a raw email address; the existing contact route remains the primary contact mechanism

### 10. Footer

- Reuse the custom hero illustrations in a new, noninteractive closing arrangement
- Keep this footer collage decorative and `aria-hidden`
- Preserve:
  - Purnet Moirangthem brand
  - Copyright
  - Navigation links
  - Case-study links
  - Existing social destinations
- Keep current placeholder legal links as placeholders unless they are intentionally removed; do not fabricate policy routes

## Copy and Claims Policy

- Preserve existing homepage/project narrative copy where it is reused.
- New personal-object captions may summarize only the details supplied by the user.
- Treat:
  - `100% workflow adoption` as a verified user-supplied Auto QA outcome
  - `~25% boost in team efficiency` as a verified user-supplied Employee Experience outcome
- Do not create a quantitative WiseWay success claim.
- Clearly distinguish:
  - verified outcomes
  - project scope
  - design specifications
  - system capability
- Do not convert repository-supported design facts into implied business impact.

## Responsive Behavior

### Desktop, 1200px+

- Centered floating navigation
- 768px intro/feature narrative
- 1152px project grid and contact panel
- Full interactive maker’s desk
- 2×2 evidence and project grids
- About portrait/profile split layout

### Tablet, 768–1199px

- Keep the navigation centered but reduce link spacing
- Scale the maker’s desk proportionally
- Preserve a 2×2 project grid where card copy still fits; otherwise use one column
- Feature evidence remains 2×2
- About shifts to a simpler two-column composition

### Mobile, below 768px

- Navigation becomes an accessible menu
- Maker’s desk becomes a contained, shorter stage with touch dragging
- Introduction remains left aligned
- Company marquee becomes a static or slowly scrolling row based on motion preference
- Featured evidence becomes one or two columns
- Project grid becomes one column
- Disable live project iframes and show static fallbacks
- About portrait, profile panel, principles, and toolkit stack
- Contact panel uses full available width
- Prevent horizontal overflow from draggable objects and marquees

## Interaction and Accessibility

- Preserve semantic `header`, `nav`, `main`, `section`, `article`, and `footer` landmarks.
- Keep one `h1` and a logical heading hierarchy.
- Use native links and buttons.
- Restore/retain the standard cursor.
- All controls must have visible focus.
- All touch targets must be at least 44×44px.
- Use meaningful alt text for project media and the portrait.
- Mark duplicated/decorative marquee and collage layers as hidden from assistive technology.
- Do not rely on hover for project information.
- Respect `prefers-reduced-motion`.
- Avoid scroll hijacking.
- Use `requestAnimationFrame` only for active direct manipulation or bounded effects.
- Suspend inactive animation when the document is hidden.

## Implementation Steps

1. **Prepare imported design resources**
   - Download and extract the four company logo archives.
   - Resolve the exact Figma font manifest.
   - Install stable logo and font files under `assets/`.
   - Confirm no temporary Figma URL or archive path remains.

2. **Create the personal illustration set**
   - Invoke `make/icon-illustration`.
   - Produce the eight coherent hero cutouts under `assets/hero/`.
   - Keep textual labels as HTML where possible.
   - Verify non-empty files and intended dimensions.

3. **Rebuild homepage design tokens**
   - Replace the current bone/cobalt/acid system with the reference-derived gray/coral system.
   - Add exact `@font-face` declarations.
   - Establish narrow and wide content containers, card radii, shadows, typography, and responsive breakpoints.

4. **Rebuild the page structure**
   - Floating navigation
   - Maker’s desk
   - Introduction
   - Company marquee
   - Featured Auto QA
   - 2×2 project grid
   - About with integrated Skills and Tools
   - Contact
   - Footer
   - Preserve required IDs and destinations.

5. **Implement maker’s-desk interaction**
   - Pointer/touch dragging
   - Boundary clamping
   - Front-layer selection
   - Shuffle and Reset
   - Keyboard movement
   - Reduced-motion behavior
   - Nonvisual descriptions

6. **Implement the marquee and page interactions**
   - Seamless company track
   - Pause/focus and reduced-motion states
   - Active navigation
   - Mobile menu
   - Section reveals only where they improve comprehension
   - Lazy project previews with static fallbacks

7. **Performance pass**
   - Eager-load only above-the-fold hero assets
   - Lazy-load project media, portrait, and iframes
   - Use explicit dimensions/aspect ratios to prevent layout shift
   - Do not run continuous animation when offscreen
   - Keep the duplicate marquee track presentational

8. **Cleanup**
   - Remove superseded current homepage styles and scripts
   - Remove temporary downloaded archives
   - Remove `.figma/design-context/remix-4-3` before the final commit
   - Do not modify linked pages

## Files Expected to Change

- `index.html`
- New files under `assets/companies/`
- New files under `assets/hero/`
- New resolved font files under `assets/fonts/`, if the resolver returns file-backed faces

Do not modify `.figma/make/**` because the deployment scripts already include the entire `assets/` directory.

## Verification Strategy

### Asset fidelity

- Verify every company SVG and hero illustration exists and is non-empty.
- Verify every visible asset has the intended slot, callsite, and rendered aspect ratio.
- Confirm no temporary Figma asset URL appears in source.
- Confirm no inspection-only reference artwork ships in the app.

### Content and claims

- Audit retained introduction, project, About, Skills, Tools, Contact, and footer copy.
- Confirm the two user-supplied metrics appear exactly with their qualifiers.
- Confirm WiseWay has no invented outcome.
- Confirm the Auto QA panel distinguishes outcome from scope/capability.

### Link and anchor audit

- Confirm every local asset and internal destination exists.
- Confirm `#work`, `#about`, `#principles`, `#tools`, `#experience`, `#highlight`, and `#contact` resolve.
- Confirm linked case-study pages can still return to the required homepage anchors.
- Confirm resume download and social links.

### Functional checks

- Mouse drag, touch drag, keyboard movement, Shuffle, Reset, and object z-order
- Object boundary clamping after viewport resize
- Mobile menu click, outside click, link click, and `Escape`
- Company marquee loop, pause, focus, reduced-motion static state, and hidden duplicate
- Project iframe fallback behavior and non-focusability
- Skills disclosures without JavaScript
- Active section navigation

### Responsive checks

Review at minimum:

- 1440×900
- 1024×768
- 768×1024
- 390×844
- 320px-wide narrow mobile

Check:

- no horizontal overflow
- draggable items stay inside the stage
- no overlapping navigation and hero controls
- project cards do not clip long copy
- company logos retain optical balance
- featured evidence labels remain honest and readable
- About anchors land correctly beneath the fixed navigation

### Runtime checks

- Run JavaScript syntax validation on the inline script.
- Parse the HTML for duplicate IDs and missing local references.
- Inspect the browser console when a browser runtime is available.
- Confirm the supervised preview serves the homepage, logo SVGs, illustrations, fonts, project media, and resume.
- Run `figma make verify-bootstrap` and require terminal success.
- Do not run deploy/deploy-preview verification unless lifecycle scripts are changed.

## Acceptance Criteria

- The page follows the reference’s compact visual rhythm and section flow without copying its portfolio content or artwork.
- The maker’s desk feels personal to Purnet and works with mouse, touch, and keyboard.
- The company marquee uses the four exact supplied logos in the confirmed order.
- Auto QA is unmistakably the featured project and presents one verified outcome plus honest scale evidence.
- Employee Experience displays the supplied `~25%` team-efficiency result.
- WiseWay contains no fabricated metric.
- The remaining work appears as a polished responsive 2×2 grid on desktop and one column on mobile.
- Skills and Tools remain accessible inside About and preserve existing deep links.
- Contact and footer complete the requested flow.
- The page is responsive, keyboard usable, reduced-motion safe, and free of horizontal overflow.
- No temporary Figma URLs, archives, or inspection-only files ship.
