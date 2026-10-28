---
name: classic-ro
description: Design and implement interfaces with the recognizable visual language of early classic Ragnarok Online PC MMORPGs. Use for RPG UI, game dashboards, inventory, character status, skill windows, quest panels, retro fantasy interfaces, or when a user requests an authentic classic RO-inspired look.
---

# Classic RO Game UI Designer

## Mission

Act as a senior MMORPG UI art director and frontend engineer. Build interfaces that immediately evoke the visual language of early classic Ragnarok Online (RO), not merely a modern website recolored brown.

The target is **classic PC MMORPG identity first, modern usability second, decoration third**. Preserve usability and accessibility, but do not default to contemporary SaaS patterns when they weaken the game-client feel.

Use RO as visual inspiration. Create original or properly licensed assets; do not claim that newly created artwork is official RO artwork or reproduce proprietary assets without authorization.

## Required workflow

### 1. Inspect before designing
- Inspect the current project, framework, styling conventions, routes, reusable components, and existing behavior.
- Read `references/visual-reference-checklist.md` before building a high-fidelity screen.
- Look for user-provided reference screenshots. If none exist and visual fidelity matters, ask for or locate legitimate reference images before asserting exact historical details.
- Distinguish actual visual evidence from assumptions. Do not invent details and label them authentic.

### 2. Establish the RO visual grammar
Before implementing a page, decide how the following will be expressed consistently:
- Classic desktop game windows and title strips.
- Framed panels, thin/double borders, restrained highlights and inner shadows.
- Compact but readable information density.
- Item slots, equipment placement, small skill icons, status bars, tooltips, tabs, and window controls.
- Warm fantasy surfaces with carefully controlled contrast.
- A coherent relationship between game-world artwork, sprites, and interface assets.

Use reference screenshots to guide proportions and specific details. The goal is a coherent interface from the same visual era, not a generic fantasy template.

### 3. Build the visual foundation first
Create or reuse shared tokens and components before expanding to many screens:
- Design tokens for color, borders, shadows, spacing, and typography.
- Window frame and title-bar component.
- Button component with default, hover, pressed, focus, selected, and disabled states.
- Inventory slot and item presentation.
- Status/progress bar.
- Tooltip/context menu.
- Tabs and compact labels.

Build one representative window first. Validate its appearance against references, then reuse the system.

### 4. Implement, render, compare, refine
- Implement the actual UI in the existing stack.
- Run the project or its available checks when possible.
- Capture or inspect the rendered page at the target viewport.
- Compare it directly with reference images.
- Identify the three largest mismatches in silhouette, icon style, border treatment, density, or hierarchy.
- Fix these mismatches before adding more decoration.
- Repeat until the screen feels like a coherent classic MMORPG client.

Do not report visual fidelity as verified if you have not inspected the rendered output.

Cheap ways to inspect without running the whole app:
- **Sprites stored as character grids:** a small Node script can `eval` the palette and frame arrays out of the source and write one SVG with every frame side by side, one `<rect>` per pixel at about 12×. On macOS, `qlmanage -t -s 2304 -o <dir> file.svg` turns it into a PNG you can view. For before/after, stack both rows in one SVG.
- **Windows and dialogs:** write a minimal HTML page in a scratch directory that imports the component and opens it. Serve the directory (`python3 -m http.server`), then screenshot it with headless Chrome: `"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" --headless=new --hide-scrollbars --window-size=480,360 --force-device-scale-factor=2 --virtual-time-budget=6000 --screenshot=out.png http://localhost:PORT/`. The virtual-time budget lets entrance animations finish. Capture every mode the component has (for example plain dialog and form mode), then stop the server.
- A user's screenshot of a tiny sprite may be too small to tell which part they mean. If you cannot locate the problem, say what you checked and ask which frame or area is wrong, rather than guessing at a redesign.

## Art direction

### Core mood
- Early-2000s PC MMORPG.
- Japanese fantasy anime charm.
- Friendly, whimsical adventure.
- Handcrafted game objects and collectible items.
- Rich details within a compact, organized interface.

### Color system
Use a warm, muted base; avoid applying every accent everywhere.

Suggested starting tokens:
- Parchment: `#E8DFC9`
- Light parchment: `#F4EDDC`
- Wood brown: `#80674E`
- Dark wood: `#514033`
- Border brown: `#A18A69`
- Muted forest green: `#78916D`
- Sky blue: `#779BBE`
- Antique gold: `#C4A46B`
- Text brown: `#42372E`
- Disabled gray: `#A69C8B`

Rules:
- Use parchment/warm neutrals for readable surfaces.
- Use brown for structure, frames, labels, and separators.
- Reserve gold for selected states, rewards, rare items, and key actions.
- Use green and blue for meaningful semantic states, not random decoration.
- Use red sparingly for danger, low health, or destructive actions.
- Show semantic states that repeat across many rows (up/down, gain/loss) as a small pale-tinted box with dark text in that hue, not as solid colour blocks; large solid fills drown the window.
- Maintain adequate text contrast. These colors are starting points, not a mandate to force every token into every screen.

#### Cool variant: equipment / status window (blue)
Classic RO also has a cool, light window family: the equipment, status and option windows. Use it when the user asks for blue, or a reference shows this family. Observed from a classic equipment-window screenshot:
- **Surface:** plain white body; sections split by thin pale blue-gray lines or a light blue label strip, not by cards.
- **Frame:** a thin gray-blue outer border, a 1px white inner line, then a pale blue inner line. The top corners are slightly rounded (about 4–5px) and the bottom is nearly square.
- **Title strip:** short (about 18–20px), a light blue vertical gradient with a 1px white highlight on top, and **dark** small title text with a small icon on the left. Do not use a dark bar with white text.
- **Window controls:** small (about 14px) **circular** buttons at the top-right (⊖ ⊗). Fill them with a vertical white to pale-blue gradient (`#FDFEFF → #D6E2F1`), a 1px white top highlight and a gray-blue ring. Draw the glyph as a pixel SVG in dark blue (`#34496B`); see "Tiny controls" below.
- **Buttons:** compact, slightly rounded (about 3px), white to pale-blue gradient, gray-blue border, white top-left and blue-gray bottom-right inner lines. Pressed swaps the lines. The primary action is a little bluer and has a 2px bottom border.
- **Inputs:** white field with a gray-blue border and a faint inset line.
- **Text:** dark blue-gray (`#27313F`) on white; muted hints in a mid blue-gray.

Starting tokens (also in `assets/design-tokens.css`, `[data-ro-theme="blue"]`): surface `#FFFFFF`, button `#E8EEF6`, line `#8EA4C3`, frame `#6880A6`, inner line `#D3DEEC`, title gradient `#EEF4FB → #C9D9EE → #B3C8E4`, title text `#1F2A3A`, focus `#3D6DB3`, text `#27313F`, muted `#66758A`, raised highlight `#FFFFFF` / shadow `#B8C7DA`.

Pick one family per interface and keep it consistent; do not mix parchment windows and blue windows in the same screen. When switching an existing UI between families, rename tokens to their role (surface, frame, line, focus) rather than leaving material names like "parchment" or "wood" pointing at blue values, and update any project docs that describe the old palette.

Windows in one product belong to one family. When a page built elsewhere (an embedded mini app, a ported screen) has to join a product that already uses this style, swap its tokens first, then restyle its containers, buttons, icons and lists by the host's rules; leave its layout alone.

### Materials and depth
Use restrained cues inspired by parchment, painted wood, leather, stone, and aged metal:
- Thin frames or carefully layered borders.
- Subtle inner shadows and highlights.
- Recessed item slots.
- Slightly raised or pressed buttons.
- Small corner details only where supported by the reference.

Avoid heavy grunge, dirty-paper overlays, exaggerated bevels, thick shadows, excessive gradients, glassmorphism, and decorative noise. The interface should feel crafted, not damaged.

### Typography
- Use highly readable text for body copy, labels, stats, and tooltips.
- Use decorative or pixel-style typography sparingly for headings or selected numeric accents.
- Keep labels compact and aligned.
- Do not imitate old low-resolution rendering by making modern text blurry or unreadable.

### Characters, monsters and mascots (sprite grammar)

Observed from a classic RO monster sprite sheet (`assets/reference-images/poring-sprite-sheet.png` where it exists locally; it is git-ignored and never shipped. Kept only as visual evidence; never copy or trace its pixels). Apply these to any creature, pet or mascot sprite so it reads as the same era:

- **Silhouette:** one soft, rounded mass (a jelly blob, a drop, a ball). Few protrusions; any appendage is a small rounded bump (antenna droplet, stubby limb), never spiky.
- **Outline:** a 1px outline in a **darker shade of the body's own hue** (dark pink on pink, dark orange on orange). No black or near-black outline.
- **Shading:** 3–4 smooth bands of the same hue (highlight, light, mid, shadow) that follow the volume: one large soft highlight blob at the upper-left, shadow pooling at the lower-right and along the ground contact. No dithering, no noise, no concentric edge-distance rings.
- **Palette:** low saturation, pastel-leaning, gentle contrast. The darkest colour on the sprite is its outline.
- **Face:** tiny. Eyes are two dark dots (about 2×2 px on a 36 px sprite) with at most a 1px highlight; the mouth is a 2–3 px mark or absent. No large anime eyes, no multi-pixel eye highlights, no blush pads unless the reference shows them.
- **Size:** roughly 32–48 px per frame, rendered at an integer scale with `image-rendering: pixelated`.
- **Idle animation:** squash-and-stretch. The body flattens wide, then stretches tall, with the bottom anchored to the ground; 4–8 frames at about 8–12 fps. Appendages lag a frame behind the body.
- **Move / enter:** small hops: crouch (squash), leap (stretch), land (squash), settle.
- **Hurt / exit:** the body flattens, then **splatters into 6–12 rounded droplets** of its own colours that fly outward and drop; the body does not fade or shrink to a point.
- **Expression comes from the whole body** (bounce, tilt, timing), not from redrawing the eyes.

### Sprite silhouette and shape continuity

When designing or editing pixel-art monsters, all attached anatomical or elemental features must integrate naturally into the main silhouette.

- Flames, horns, ears, tails and other attached features connect to the body through intentional pixel clusters: a wide root that is part of the body's own shape, not a part placed on top of it.
- Avoid visible gaps, detached-looking shapes, accidental single-pixel bridges and inconsistent outlines.
- Shading must cross the junction too: a dark band, a colour step or a change in outline colour exactly where the feature meets the body reads as a seam even when the outline is continuous. Let the body's gradient run into the feature (and the feature's highlights into the body).
- Preserve a consistent pixel grid, palette, outline thickness and shading style across body and features.
- Evaluate the sprite at its actual in-game display size, not only when enlarged. At 1× the feature and body must still read as one creature.
- When editing an existing sprite, preserve its identity (face, proportions, palette, canvas size, transparent background) and modify only the specified region unless a broader redesign is explicitly requested. Do not hide a seam with a glow or extra decoration.

#### Seam audit (run on every frame, not just the rest pose)
Users describe a seam as "the head and body colours don't connect", or as "one frame looks broken when it jumps". These were the real causes found on a flame mascot. Check for each of them:
1. **Outline colour switch at the junction.** A lighter outline on the feature and a darker one on the body, switched on the same row where they meet, draws a line across the creature. Ramp the outline over the height instead, with at least two in-between shades (for example `Q → q → o → O`, each step spanning about 3 rows). Never put the step on the junction row.
2. **Neck notch in the silhouette.** List each row's left and right edge. If a row juts out one pixel, the next rows step back in, and then it widens again, the body reads as a separate head on a neck. Around the junction the edges must widen or narrow monotonically. Fill the notch outward with the neighbouring interior colour, and turn a leftover outline pixel on the ledge into interior colour.
3. **Dim band at the junction.** If the bright core runs down the feature, dips by 1–2 steps for a row or two at the forehead or brow, and comes back on the face, it reads as a horizontal stripe. Keep the core continuous: the brow and eye rows carry the same core value as the rows above and below.
4. **A different fix per frame.** Animation frames are drawn separately, so a seam fixed in rest and squash frames often survives in rise or settle frames, where the body is narrower and the junction sits a row higher. During playback that one frame flickers. After any fix, render all frames side by side and compare the junction row in each.

When fixing these with a script, limit each rule to the junction band, from a few rows above the eyes or root down to the eye row. Run unrestricted, a "remove inner outline pixel" rule also eats the outline at flame tips and petal gaps, and the tips then look like spikes. Re-render after every rule and diff the frames against the originals. Hand-edit the few rows a rule cannot express cleanly, and assert each edited row keeps the canvas width.

### Character animation: full-body motion

Every character animation must treat the entire character as one animated sprite. Never animate only the head, flame, eyes or decorative elements while the body stays static.

**Full-body idle.** Animate the body and its attached features together:
- **Body:** subtle vertical bobbing, breathing or organic deformation. The torso or main body must visibly take part; head-only motion does not satisfy this.
- **Head / flame / ears:** secondary motion that follows the body with slight independent variation (lag by a frame).
- **Face:** preserve facial identity; optional subtle blinking.
- **Shadow:** stays grounded beneath the character, with small scale or opacity changes as the body rises or falls.

**Pixel-art animation rules.**
- Classic RO-inspired retro MMORPG sprite aesthetics.
- Deliberate pixel-frame changes, not motion carried entirely by smooth CSS transforms. CSS may add a hop or a settle on top; it must not be the only thing moving.
- Preserve the original silhouette, palette, pixel density and proportions.
- Subtle frame-by-frame changes, squash-and-stretch and secondary motion where appropriate.
- No excessive bouncing, exaggerated cartoon movement, motion blur or modern fluid easing.
- Must stay readable at the actual in-game sprite size.

**Frame requirements.** A looping idle has at least 4 intentional poses when the assets and implementation allow:
1. **Rest:** natural neutral pose.
2. **Compress:** body slightly lowers or compresses.
3. **Rise:** body expands or lifts, flame or appendages trailing slightly.
4. **Settle:** body returns toward the rest pose.
The poses must differ in the main body silhouette or position, not merely in a flame tip. Loop seamlessly with consistent timing and no visible snapping.

**Playing the frames.** Lay the frames out as one horizontal strip and slide it with a single stepped animation (`steps(n)` on the offset, or one timer that picks the frame). Do not give each frame its own show/hide animation: once their timings drift, there is a moment when every frame is hidden and the character blinks out.

**Before modifying an animation.**
- Inspect the existing sprite assets, frames and rendering code.
- Identify which parts move now and which are static.
- Determine how the character is rendered (single sprite, layers, CSS elements, canvas).
- Animate the actual body sprite or geometry, not only a head or flame layer; keep attached parts aligned with the body while giving them secondary motion.
- Never fake full-body animation by moving only a decorative child element.

**Acceptance.** Accept only when: the main body visibly moves; the head or flame has natural secondary motion; the character feels alive without particle effects; the base stays grounded and the shadow responds; the loop returns smoothly to its start; the pixel-art aesthetic is retained. Always preview the animation in motion at actual display size before calling it complete.

### Pixel-art integrity
- Prefer original or properly licensed pixel art for item icons, skill symbols, class emblems, and game indicators.
- Keep pixel scale, outlines, shading, palette, and perspective consistent.
- Do not mix unrelated emoji, corporate vector icons, photorealistic 3D icons, and pixel art in the same visual system.
- Do not apply a pixelation filter to unrelated artwork and call it authentic pixel art.
- If game assets are unavailable, use a coherent original placeholder set and clearly distinguish it from official artwork.

## Component rules

### Game windows
- Use clear window silhouettes, compact title strips, consistent frames, and deliberate control placement.
- Separate functional areas into windows/panels when it reinforces the classic game-client feel.
- Make panel title, content, and controls visually distinct.
- Use compact spacing, but keep text and hit targets usable.
- Avoid making every section a giant rounded card.

### Inventory and equipment
- Use a consistent grid of clearly bounded slots.
- Keep item art visually prominent and quantities readable.
- Support category grouping, selected states, rarity cues, and hover details when useful.
- Equipment layouts should communicate item placement clearly.
- Never rely on color alone to communicate rarity or status.

### Status and progress bars
- Keep HP, SP/mana, experience, cooldown, and quest progress styles consistent.
- Make labels and values readable.
- Use semantic colors consistently and avoid ornamental effects that reduce legibility.

### Buttons and controls
- Use tactile, compact controls with clear default, hover, pressed, focus, selected, and disabled states.
- A pressed button may look subtly recessed; hover may use a restrained highlight.
- Avoid pill-shaped controls everywhere.
- Make the primary action clear without making all actions equally prominent.
- Every visible control must perform its advertised action.

#### Tiny controls (window close / minimize, thumbnail remove, about 16px or smaller)
- **Draw the glyph as pixel art, not text.** A font `×`, `−` or `⊗` at 11–12px blurs and rarely sits at the true centre, because baselines and side bearings vary by font. Use a small inline SVG instead, with `viewBox` equal to its pixel size, `shape-rendering="crispEdges"` and `fill="currentColor"`, centred by grid. A 2px-wide diagonal in an 8×7 grid reads as a bold, symmetric ×: row *i* (0–6) fills x = *i*…*i*+1 and x = 6−*i*…7−*i*.
- **No radial gradients on the fill.** At this size a radial highlight smears into the glyph and the button reads as a blob. Use a vertical two-stop gradient plus a 1px inset highlight on top.
- **Pressed state without movement.** Darken the fill and swap the top highlight for a shadow line. A 1px `translateY` jump on a 14px button is very noticeable.
- **Check at 1× and 2×.** Screenshot at `--force-device-scale-factor=1` and `2`, crop the control and enlarge the crop. A control that looks fine at 4× can still be mush at 1×.

#### Menu and action icons (8×8 pixel glyphs)
Small action menus (a mascot's idle menu, a context menu, a toolbar) get one glyph per item, drawn as pixel art on an **8×8 grid** and rendered as inline SVG. This is the reference set used by the Fyro mascot menu (`packages/mascot`); reuse the format and the vocabulary so every menu in the system shares one icon language.

- **Author glyphs as 8 strings of 8 characters**, `#` = filled cell, `.` = empty. It reads like a sprite sheet in code, is trivial to edit, and leaves no room for anti-aliased vectors.
  ```js
  const ICONS = {
    chat:     ['.######.', '#......#', '#......#', '#......#', '.######.', '...#....', '..#.....', '........'], // speech bubble with a tail
    feature:  ['...##...', '...##...', '.######.', '.######.', '...##...', '...##...', '........', '........'], // plus = "add / want"
    bug:      ['..####..', '.#.##.#.', '.######.', '#.####.#', '.######.', '#.#..#.#', '........', '........'], // beetle: body, head split, three legs each side
    progress: ['..####..', '.#....#.', '#...#..#', '#...#..#', '#..##..#', '.#....#.', '..####..', '........'], // clock face with hands
    gear:     ['...##...', '.######.', '.##..##.', '##....##', '##....##', '.##..##.', '.######.', '...##...'], // settings; also the default when an item has no icon
  };
  ```
- **Render as SVG, one `<rect>` per filled cell**: `viewBox="0 0 8 8"`, `shape-rendering="crispEdges"`, `fill="currentColor"`, `aria-hidden="true"`. Colour comes from the button text colour (dark blue-grey, e.g. `#3c5378`, on the light gradient button), so hover/disabled states recolour the glyph for free. No emoji, no Lucide, no font glyphs beside these.
- **Drawing rules.** Silhouette first: the glyph must be recognisable as a solid shape, because an outline alone is 1px at this size and disappears. Use 2px strokes for the main mass and reserve 1px for details (bubble tail, bug legs, clock hands). Keep the glyph symmetric about the vertical axis unless the subject is inherently directional (the chat tail). Leave the bottom row empty so every glyph sits on the same baseline next to its label; keep at least one empty column on each side so neighbouring buttons don't crowd.
- **Placement.** Glyph left of a short label, 6px gap, inside a compact bevelled button (about 26px tall, 12px/600 label). The icon is a cue, not the control: never ship an icon-only menu item.
- **Size.** Display at an integer multiple of 8 (8 or 16px) for exact pixels. 12px (1.5×) is acceptable only when the target is a 2× screen, where it becomes 24 device pixels (3×); at 1× the cells alternate 1px/2px wide, so check it with the 1×/2× screenshot step above.
- **One default, never blank.** An item without an icon (for example a host-app "Settings" entry appended by the embedding system) gets the gear, so the column stays aligned.

### Lists and settings rows
- Each item first gets one line that says what it is: name plus a short identifier (a URL without the scheme and trailing slash; the full value in `title`). Add a per-row icon only when it tells the rows apart; if every row is the same kind, put the icon on the section title instead.
- Collapse an item's settings into "label + one-line summary + small button" rows (e.g. "Shared: example.com, 2 emails · Edit"); expand the inputs only when the user asks to edit, and keep the expanded state across re-renders. Let these rows span the full width under the item.
- Values copied verbatim (keys, tokens, URLs) go in a recessed monospace field; mask the middle of secrets (`fk_Ab3d…9xQz`) and offer Copy next to it.
- A destructive action is a small red text button at the far right of its item, apart from the ordinary actions.
- Adding is a small `+` pixel button in the list header, not a permanent form: it opens an inline input row under the header, shows as pressed while open (`aria-expanded`), and closes on a second press, on Esc (which closes only that row), or on success. Keep a single primary button per panel.
- In CSS grid rows use `minmax(0, 1fr)` for flexible columns, otherwise an input inside pushes the panel wider than its window.

### Tooltips and context menus
When relevant, include item name, icon, type/rarity, description, stats/effects, requirements, and available actions.
- Use a coherent warm-dark or parchment surface with a clear border.
- Position near the triggering item and prevent viewport overflow.
- Ensure keyboard and touch users can access essential information.

### Decoration
- Use small ornaments, corner details, rune motifs, or medieval accents only when they reinforce the composition.
- Do not fill empty space just because it is empty.
- Do not let decoration compete with content or make the interface feel like a fan-made skin.

## Responsive behavior

Desktop:
- Preserve the classic multi-window game-client composition where appropriate.
- Keep critical controls visible and prevent panels from obscuring essential information.
- When a mobile app moves into a desktop window: the bottom tab bar becomes tabs under the title strip, bottom sheets and drawers become small centred windows, and hit targets shrink from 44px to about 24–32px.
- Mobile-only affordances you hide (a sheet's drag handle, for example) may also be the dialog's focus landing spot (`autofocus`, `tabindex="-1"`). Hide those with zero size and absolute positioning, never `display: none` or `hidden`: otherwise the dialog focuses its first item on open and its clipped focus ring looks like a stray border on the first row.

Tablet:
- Reorganize panels into logical groups and reduce simultaneous open windows where needed.

Mobile:
- Do not shrink the desktop UI until it becomes unusable.
- Convert windows into tabs, drawers, stacked panels, or focused views.
- Preserve frames, palette, icon language, and tactile controls.
- Keep touch targets and text readable; do not let decoration consume the viewport.

## Engineering constraints

- Preserve existing business logic, routes, API contracts, and working interactions unless explicitly asked to change them.
- Follow the existing framework and styling conventions.
- Prefer reusable components and design tokens over duplicated styles.
- Avoid unnecessary dependencies or framework changes.
- Do not replace real data with mock data to simplify styling.
- Implement loading, empty, error, success, and disabled states where relevant.
- Keep semantic HTML, keyboard navigation, focus visibility, and adequate contrast.
- Do not claim tests or visual inspection passed unless actually performed.

## Anti-patterns: reject these before delivery

- A modern SaaS dashboard with brown backgrounds.
- Large rounded cards everywhere.
- Excessive whitespace and oversized headings that erase game-client density.
- Generic Lucide icons replacing all meaningful game icons.
- Random pixel decorations without a consistent pixel-art system.
- A mascot with black outlines, big highlighted anime eyes, blush pads and saturated high-contrast colours: that is modern kawaii pixel art, not a classic RO sprite (see "Characters, monsters and mascots").
- An "idle animation" where the body pixels never change and only a flame, ears or a CSS transform move (see "Character animation: full-body motion").
- Too many saturated accents.
- Thick, noisy, dirty textures.
- A different frame/button/icon style on every page.
- A static-looking control that does not work.
- Claiming “authentic RO” without comparing against references.

## Quality gate

Before finishing, check:

1. **Recognition:** Does the overall composition evoke a classic PC MMORPG rather than a modern dashboard?
2. **Silhouette:** Do windows, title bars, slots, and controls have a coherent classic-game structure?
3. **Assets:** Are item and skill visuals stylistically consistent?
4. **Materials:** Are borders, highlights, and surfaces restrained and coherent?
5. **Density:** Is information compact but easy to scan?
6. **Hierarchy:** Are game objects and important actions more prominent than decoration?
7. **Interaction:** Do hover, selection, pressed, disabled, focus, and tooltip states work? Open every dialog once: nothing should look focused until the user moves focus.
8. **Responsive:** Does the style survive tablet/mobile adaptation?
9. **Integrity:** Are assets original, authorized, or accurately described?
10. **Verification:** Was the actual rendered UI inspected, if the environment supports it?

Fix the three most important visual mismatches before calling the task complete.

## Final principle

Do not merely decorate a modern UI with retro colors. Build a coherent game interface whose window silhouettes, iconography, material language, information density, typography, and interactions all belong to the same classic MMORPG world.
