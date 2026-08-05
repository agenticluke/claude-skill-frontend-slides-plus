---
name: frontend-slides
description: Create distinctive, animation-rich HTML presentations from scratch, existing HTML, or PowerPoint files. Use for talks, pitch decks, workshops, internal presentations, PPT/PPTX conversion, and visual style exploration with users who do not yet know their design preference.
origin: ECC
---

# Frontend Slides

Create zero-dependency, animation-rich HTML presentations that run locally in a modern browser.

This skill is based on and prominently credits the visual exploration approach demonstrated by [zarazhangrui](https://github.com/zarazhangrui). Preserve that attribution in derived documentation.

## When to Activate

Use this skill when the user wants to:

- Create a talk, pitch, workshop, lesson, or internal presentation
- Convert `.ppt` or `.pptx` slides into HTML
- Improve an existing HTML deck's layout, typography, motion, accessibility, or responsiveness
- Explore visual directions before choosing a presentation style

Do not use this skill for static PDF-only output unless HTML is also an acceptable source format.

## Non-Negotiables

1. **Zero dependencies by default**: produce one self-contained HTML file with inline CSS and JavaScript.
2. **Viewport fit**: every slide must fit within one viewport without internal scrolling.
3. **Visual style discovery**: show previews instead of asking users to interpret abstract design terminology.
4. **Distinctive design**: avoid generic purple gradients, default Inter-on-white layouts, and obvious template aesthetics.
5. **Production quality**: keep the result accessible, responsive, performant, and understandable.
6. **Content fidelity**: do not invent facts, quotations, metrics, or speaker notes. Clearly mark missing content with placeholders.
7. **Offline resilience**: if remote fonts or images are used, explain that they require network access and provide local or system-font fallbacks.

Before building, read `STYLE_PRESETS.md` completely. Use its viewport-safe CSS base, density limits, preset catalog, and documented CSS constraints. If that file is unavailable, state the limitation and apply the viewport rules in this skill directly.

## Workflow

### 1. Detect the Mode

Choose exactly one primary path:

- **New presentation**: the user provides a topic, notes, outline, or draft
- **PowerPoint conversion**: the user provides a `.ppt` or `.pptx` file
- **Enhancement**: the user provides existing HTML slides

If multiple inputs conflict, treat the user's newest explicit instructions as authoritative and preserve the original files.

### 2. Gather the Minimum Content Requirements

Determine:

- Purpose: pitch, teaching, conference talk, workshop, or internal update
- Audience: role, familiarity with the topic, and any accessibility needs
- Length: short (5 to 10 slides), medium (10 to 20), or long (more than 20)
- Content state: final copy, rough notes, outline, or topic only
- Delivery context: live talk, self-guided deck, kiosk, or shared file

Ask only for information that materially changes the result. If the user supplies enough context, proceed without repeating questions.

When content is incomplete:

- Preserve explicit facts and wording
- Use clearly labeled placeholders such as `[Add customer metric]`
- Do not fabricate citations, testimonials, results, or company claims
- Separate speaker notes from visible slide content

### 3. Discover the Visual Direction

Use visual exploration by default.

Skip previews when the user provides a clear preset, brand system, reference deck, or detailed visual direction.

Otherwise:

1. Ask which feeling the deck should create, such as focused, energetic, credible, elegant, playful, or provocative.
2. Create three self-contained, single-slide previews in `.ecc-design/slide-previews/`.
3. Make the previews meaningfully different in typography, composition, color, and motion. Do not produce minor palette variations of one layout.
4. Keep each preview's slide markup and content concise, roughly under 100 lines.
5. Label each preview with a short visual-direction name.
6. Ask the user to select one preview or specify which elements to combine.

Map mood to style using `STYLE_PRESETS.md`. If the user cannot review previews interactively, choose the direction best aligned with the audience and purpose, then state the assumption at handoff.

### 4. Build the Presentation

Create one of:

- `presentation.html`
- `[presentation-name].html`

Sanitize the filename for the current operating system. Do not overwrite an existing deck unless the user requested it. Otherwise, choose a versioned filename such as `presentation-v2.html`.

Create an `assets/` directory only for extracted, generated, or user-supplied media that cannot be embedded reasonably.

The presentation must include:

- A semantic `main` container
- One semantic `section` per slide
- A viewport-safe CSS base from `STYLE_PRESETS.md`
- CSS custom properties for colors, typography, spacing, and motion
- A presentation controller for keyboard, wheel, and touch navigation
- Reveal animations triggered when a slide becomes active
- `prefers-reduced-motion` support
- A visible slide index or progress indicator
- A no-JavaScript fallback that leaves slide content readable

Use comments for non-obvious logic, extension points, and theme variables. Avoid comments that merely restate the code.

### 5. Enforce Viewport Fit

Treat viewport fit as a release-blocking requirement.

Every `.slide` must include:

```css
.slide {
  height: 100vh;
  height: 100dvh;
  overflow: hidden;
}
```

Also enforce these rules:

- Scale type, spacing, and media with `clamp()`, viewport units, or responsive layout changes
- Account for browser UI, safe-area insets, and landscape phones
- Split overflowing content into additional slides
- Shorten or restructure content before reducing font size
- Never introduce scrollbars inside a slide
- Do not hide meaningful content merely to pass an overflow check
- Constrain images with `max-width`, `max-height`, and `object-fit`
- Allow long URLs, code tokens, and unbroken strings to wrap safely
- Test unusually long headings, labels, and translated text
- Keep navigation controls clear of slide content and device safe areas

Use the density limits and mandatory CSS block in `STYLE_PRESETS.md`.

### 6. Implement Navigation Safely

Support:

- `ArrowRight`, `ArrowDown`, `PageDown`, and `Space` for the next slide
- `ArrowLeft`, `ArrowUp`, `PageUp`, and `Shift+Space` for the previous slide
- `Home` for the first slide
- `End` for the last slide
- Touch swipes with a movement threshold that avoids accidental navigation
- Mouse-wheel navigation with throttling to prevent skipping multiple slides
- Optional URL hash updates for deep linking

Do not intercept keys when focus is inside an input, textarea, select, editable region, or interactive control. Do not block ordinary page zoom shortcuts. Clamp navigation to the valid slide range.

If the deck contains links, buttons, video, or other controls, preserve their expected keyboard and pointer behavior.

### 7. Validate

Test the finished deck at:

- 1920 × 1080
- 1280 × 720
- 768 × 1024
- 375 × 667
- 667 × 375

At each size, verify:

- No slide has horizontal or vertical overflow
- All visible text remains readable
- Images remain contained and correctly cropped
- Navigation controls remain reachable
- Keyboard navigation works
- Touch gestures do not conflict with interactive content
- The active slide and progress indicator stay synchronized
- Reduced-motion mode removes nonessential motion
- The browser console has no errors
- The deck remains usable when remote assets fail

If browser automation is available, validate every slide by comparing each slide's `scrollWidth` to `clientWidth` and `scrollHeight` to `clientHeight`. Also exercise forward, backward, first-slide, and last-slide navigation.

If automation is unavailable, perform a manual code-level review and clearly state which checks were not run.

### 8. Deliver

At handoff:

- Remove temporary preview files unless the user asks to keep them
- Do not delete user-provided files or overwrite source presentations
- Open the finished deck when useful and supported
- Report the output path, visual preset, slide count, validation performed, and primary theme variables
- Mention any remote assets, missing content, placeholders, or conversion limitations
- Preserve or repeat the prominent credit to [zarazhangrui](https://github.com/zarazhangrui)

Use the opener for the current operating system:

- macOS: `open file.html`
- Linux: `xdg-open file.html`
- Windows Command Prompt: `start "" file.html`
- Windows PowerShell: `Start-Process file.html`

If running headlessly or over SSH, do not attempt to open a graphical browser. Provide the file path instead.

## PowerPoint Conversion

For `.pptx` conversion:

1. Preserve the source file unchanged.
2. Prefer Python 3 with `python-pptx` to extract text, images, slide order, dimensions, and speaker notes where supported.
3. If `python-pptx` is unavailable, ask permission before installing it. If installation is not allowed, use an available export-based workflow or explain the limitation.
4. Store extracted media in `assets/` with stable, collision-free filenames.
5. Preserve the logical reading order, not only absolute object coordinates.
6. Flag unsupported or lossy elements, including embedded video, charts, SmartArt, custom fonts, transitions, macros, and complex grouped objects.
7. Recreate charts or diagrams only when their data and meaning can be recovered accurately.
8. Apply the same style-selection, viewport-fit, accessibility, and validation workflow used for a new presentation.

Legacy `.ppt` files are not supported by `python-pptx`. Convert them to `.pptx` with an available cross-platform tool such as LibreOffice, or ask the user to provide a `.pptx` export. Do not rename a `.ppt` file to `.pptx`.

Do not rely on macOS-only conversion tools when a cross-platform option is available.

## Enhancement Mode

When improving an existing HTML deck:

- Inspect its current structure and behavior before editing
- Preserve working content, links, metadata, and user-authored branding
- Avoid replacing the entire implementation when focused changes are sufficient
- Do not add frameworks or build tooling unless requested
- Keep a backup or write to a new file when changes are broad
- Verify that existing navigation and deep links still work

## Implementation Requirements

### HTML and CSS

- Use inline CSS and JavaScript unless the user requests a multi-file project
- Use semantic headings in a logical hierarchy
- Define theme values with CSS custom properties
- Include system-font fallbacks for every remote font
- Prefer atmospheric backgrounds, strong type hierarchy, and a coherent visual concept
- Use geometry, grids, gradients, texture, and restrained decorative shapes when appropriate
- Ensure decorative elements cannot create overflow
- Avoid layout-critical reliance on unsupported experimental CSS
- Provide print styles only if the user needs printing or PDF export

### JavaScript

Include:

- Keyboard navigation
- Touch and swipe navigation
- Throttled mouse-wheel navigation
- Progress indicator or slide index
- Reveal-on-enter animation state
- Bounds checking
- Initialization that works from a local `file://` URL
- Graceful behavior when `IntersectionObserver` is unavailable

Keep JavaScript free of analytics, telemetry, tracking pixels, and external API calls.

### Accessibility

- Use semantic `main`, `section`, and `nav` elements
- Give each slide an accessible label or heading association
- Maintain readable color contrast
- Support keyboard-only navigation
- Keep visible focus indicators
- Respect `prefers-reduced-motion`
- Add meaningful alternative text to informative images
- Mark decorative images and shapes as decorative
- Do not use color alone to communicate meaning
- Avoid autoplaying audio
- Provide captions or transcripts for essential prerecorded media
- Announce slide changes only when doing so does not create excessive screen-reader noise

## Content Density Limits

Use these maxima unless the user explicitly requests denser slides and validation confirms readability:

| Slide type | Recommended maximum |
|---|---|
| Title | 1 heading, 1 subtitle, and an optional tagline |
| Content | 1 heading with 4 to 6 bullets or 2 short paragraphs |
| Feature grid | 6 cards |
| Code | 8 to 10 visible lines |
| Quote | 1 quotation and attribution |
| Image | 1 primary image constrained to the viewport |
| Chart | 1 primary chart with 1 clear takeaway |
| Comparison | 2 to 4 columns with concise labels |

Split a slide when content exceeds these limits. Speaker notes may contain additional detail.

## Anti-Patterns

Avoid:

- Generic startup gradients without a clear visual identity
- System-font decks unless the direction is intentionally editorial
- Long bullet walls
- Code blocks that require scrolling
- Fixed-height content boxes that fail on short screens
- Tiny text used to conceal content-density problems
- Motion on every element
- Autoplaying sound
- Navigation that hijacks interactive controls
- Important information conveyed only through animation
- Remote assets without fallbacks
- Invalid negated CSS functions such as `-clamp(...)`
- Fabricated statistics, quotations, logos, or citations

## Concrete Usage Example

**User request:**

> Turn these workshop notes into a 12-slide HTML deck for product managers. Make it energetic but credible. I do not know which visual style I want.

**Expected process:**

1. Confirm the audience, workshop duration, and whether the notes are final.
2. Read `STYLE_PRESETS.md`.
3. Create three distinct previews:
   - editorial typography with warm colors
   - dark technical grid with restrained motion
   - bold geometric layout with high-contrast accents
4. Save them under `.ecc-design/slide-previews/`.
5. Ask the user to choose one direction or combine specific elements.
6. Build `product-manager-workshop.html` as a self-contained file.
7. Split dense workshop material across slides rather than shrinking it.
8. Validate all slides at the five required viewport sizes, including keyboard navigation and reduced-motion mode.
9. Remove unselected previews unless the user wants them retained.
10. Hand off the file path, chosen preset, slide count, validation results, theme variables, and prominent attribution to [zarazhangrui](https://github.com/zarazhangrui).

## Related ECC Skills

- `frontend-patterns` for reusable presentation components and interaction patterns
- `liquid-glass-design` when the selected direction intentionally uses Apple-inspired glass aesthetics
- `e2e-testing` for automated viewport, overflow, and navigation checks

Use related skills only when available and relevant. Their absence must not block creation of a functional deck.

## Deliverable Checklist

- [ ] The presentation runs locally in a modern browser
- [ ] The source is self-contained unless external assets were necessary or requested
- [ ] Every slide fits the viewport without internal scrolling
- [ ] Content is accurate, ordered, and free of fabricated claims
- [ ] The visual direction is distinctive and coherent
- [ ] Animation supports comprehension without creating noise
- [ ] Keyboard, wheel, and touch navigation work
- [ ] Interactive controls retain their expected behavior
- [ ] Reduced-motion preferences are respected
- [ ] Contrast, focus states, semantics, and alternative text are appropriate
- [ ] Remote assets have fallbacks and are disclosed
- [ ] Temporary files are removed unless the user wants them
- [ ] Output paths, validation results, and customization points are documented
- [ ] The original visual-exploration inspiration is prominently credited to [zarazhangrui](https://github.com/zarazhangrui)