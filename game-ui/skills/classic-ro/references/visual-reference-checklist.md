# Visual Reference Checklist

Use this checklist when collecting or evaluating reference screenshots for a classic RO-inspired interface.

## Capture these reference types
1. Main game client with multiple UI windows visible.
2. Character status/attributes window.
3. Inventory and equipment window.
4. Skill window and shortcut bar.
5. Item tooltip or context menu.
6. Chat, quest, or party panel.
7. Window title bar, buttons, borders, and selected states at close range.
8. A monster or pet sprite sheet (idle, move, hurt/death frames) when designing any character or mascot.

Local only (copyrighted, git-ignored, so a plugin install does not have it): `assets/reference-images/poring-sprite-sheet.png` (classic RO monster sheet). The sprite grammar in SKILL.md already records what it shows. When present, use it only to observe outline colour, shading bands, face size and the squash-and-stretch / splatter frames; do not trace or copy it.

Observed (screenshot not stored): classic **equipment window**: white body; thin gray-blue frame with white and pale-blue inner lines; slightly rounded top corners; short light-blue gradient title strip with a small icon and dark title text; circular ⊖ ⊗ controls at the top-right; tabs under the title; equipment slots as pale blue-gray rounded placeholders flanking the character; a light-blue label strip ("人物能力屬性狀態欄") separating the stats section; stats in compact two-column label/value rows. This is the reference for the "Cool variant" in SKILL.md.

## What to observe
- Outer silhouette and proportions of each window.
- Title strip height, placement of close/minimize controls, and internal padding.
- Border thickness, double lines, highlights, shadows, and surface texture.
- Typical icon size, pixel scale, outline, shading, and palette.
- Density and alignment of labels, values, and controls.
- How selected, disabled, active, and hover states are shown.
- Relationship between interface colors and the in-game world.
- How several windows coexist without losing readability.

## How to use references
- Treat screenshots as visual evidence, not as assets to copy blindly.
- Record recurring patterns in a small component specification.
- Use references from the same game era/version when consistency matters.
- Do not infer details from unrelated modern RO remakes, fan sites, or mobile games.
- If exact version or era is uncertain, describe the result as “classic RO-inspired,” not an exact reproduction.

## Review method
After rendering the implementation:
1. Compare side by side with the closest reference.
2. Mark the three biggest differences.
3. Fix silhouette and component structure before color micro-adjustments.
4. Fix icon style and material treatment before adding ornaments.
5. Re-check at the target viewport and at a narrow viewport.
6. For sprites, compare every animation frame before and after, not only the rest pose (see "Seam audit" in SKILL.md).
