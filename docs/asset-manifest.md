# Asset Manifest

> **Version:** 1.0
> **Date:** 2026-03-14
> **Author:** UX-Design Agent
> **Status:** Draft — awaiting founder review

---

## Generation Guidelines

**Art style:** Warm, hand-painted children's book illustration. Slightly textured brushstrokes, not flat vector. Rich, saturated colors against a dark cave environment. Think "friendly adventure" — inviting, not scary. Characters and objects have soft edges, rounded forms, and gentle lighting.

**Mood:** Cozy exploration. Like reading a bedtime storybook about a treasure hunt. Warm golden light contrasts with cool purple-brown cave shadows.

**Constraints:**
- All assets must work on dark backgrounds (`#1C1226` cave)
- Character must be gender-neutral and culturally neutral (no specific ethnicity implied — stylized/cartoonish)
- No text in generated images (all text is rendered via code for i18n)
- Gems must be simple enough to be distinguishable at `32px` (smallest size)
- All raster assets delivered at 2x resolution for Retina/HiDPI displays

---

## External Assets (require image generation)

| # | Name | Description | Format | Dimensions | Art Style Notes | Generation Tool | Status |
|---|------|-------------|--------|------------|-----------------|-----------------|--------|
| 1 | Explorer Character — idle | Full-body explorer character standing, facing viewer. Wearing adventure hat, small backpack, holding a lantern. Friendly smile. Age ~8-10 appearance. Gender-neutral. | PNG (transparent bg) | 400×480 @2x | Hand-painted, soft lighting from lantern. Warm tones. No text on clothing. | Midjourney / Scenario | Not Generated |
| 2 | Explorer Character — celebrate | Same character, arms raised in excitement, big smile, lantern held high. Sparkle effects near lantern. | PNG (transparent bg) | 400×480 @2x | Same style as #1. Dynamic pose. | Midjourney / Scenario | Not Generated |
| 3 | Explorer Character — thinking | Same character, hand on chin, looking upward thoughtfully. Lantern at side. | PNG (transparent bg) | 400×480 @2x | Same style as #1. Contemplative pose. | Midjourney / Scenario | Not Generated |
| 4 | Explorer Face — avatar | Head/shoulders crop of explorer for narrator bubble avatar. | PNG (transparent bg) | 96×96 @2x | Same character as #1, friendly expression. | Crop from #1 or generate separately | Not Generated |
| 5 | Cave Entrance — title bg | Wide cave entrance view from outside looking in. Warm light spilling from inside cave. Stalactites, mossy rocks, gem crystals embedded in walls. | PNG | 1920×1080 @2x | Painterly, atmospheric. Darker at edges, warm light in center. No text. | Midjourney / Scenario | Not Generated |
| 6 | Cave Interior — map bg | Interior cave wall texture/scene for Progress Map background. Vertical orientation. Crystal formations, subtle gem glints, rock layers. | PNG (tileable vertically) | 960×1600 @2x | Darker than #5. Subtle detail — this is behind UI elements. Should not compete with level nodes. | Midjourney / Scenario | Not Generated |
| 7 | Cave Deep — gameplay bg | Deep cave background for Gameplay screen. Darker, more atmospheric. Subtle crystal glow in corners. | PNG | 1920×1080 @2x | Darkest of the three cave bgs. Very subtle so gems and UI elements pop. | Midjourney / Scenario | Not Generated |

---

## Code-Generated Assets

Assets built in code (CSS, SVG, or Canvas) rather than external image generation. This approach ensures pixel-perfect scaling, theme-ability, and zero additional download size.

| # | Name | Description | Implementation Notes |
|---|------|-------------|---------------------|
| C1 | Gem sprites (all colors) | Round gems with radial gradient, specular highlight, and shadow. 4 color variants (ruby, sapphire, emerald, amber) + gold Power Gem. | CSS `radial-gradient` + `box-shadow` on `<div>`. Border-radius 50%. See design-spec §2.1 for gradient values. |
| C2 | Splitter control | Vertical line with circular grab knobs at top/bottom. Glow effect. | CSS `<div>` with height: 100%, width: 4px, border-radius. Knobs are `::before`/`::after` pseudo-elements. |
| C3 | Treasure chest | Wooden chest with gold trim. Open/closed states. | CSS box with gradients (`#8B4513` wood, `#D4A574` planks, `#FFD700` trim). Lid is a separate element with `transform: rotateX()` for open animation. |
| C4 | 10-frame grid | 2×5 grid of placeholder circles showing the target layout for 10 gems. | CSS Grid, `grid-template: repeat(2, 1fr) / repeat(5, 1fr)`. Circles are `border: 1px dashed` with low opacity. |
| C5 | Progress path lines | Curved SVG paths connecting level nodes on the map. | Inline `<svg>` with `<path>` elements using quadratic bezier curves. Stroke: `--color-text-secondary`, 4px. |
| C6 | Stalactite decorations | Triangular rock formations for cave atmosphere on map/backgrounds. | CSS `clip-path: polygon()` or SVG triangles. Colors: `--color-surface` variants with slight gradient. |
| C7 | Sparkle particles | Small gold/white dots for celebrations and Power Gem effects. | CSS `@keyframes` animation on small `<span>` elements. Random position/delay via inline styles or CSS custom properties. |
| C8 | Lock icon | Padlock for locked level nodes. | Inline SVG. 28×28px. Fill: `--color-locked`. |
| C9 | Checkmark icon | Checkmark for completed level nodes. | Inline SVG. 28×28px. Fill: `--color-success`. |
| C10 | Speaker icon | Speaker/volume icon for audio toggle. On/off states. | Inline SVG. 24×24px. Fill: `--color-text-primary`. "Off" state has diagonal line through it. |
| C11 | Settings gear icon | Gear/cog icon for settings button. | Inline SVG. 24×24px. Fill: `--color-text-primary`. |
| C12 | Back arrow icon | Arrow pointing to inline-start for "back to map" button. | Inline SVG. 24×24px. Fill: `--color-text-primary`. Mirrors automatically via RTL CSS. |
| C13 | Play icon | Triangle play button icon for title screen CTA. | Inline SVG. 24×24px. Fill: `#FFFFFF` (on primary button). |
| C14 | Replay icon | Circular arrow for replay button. | Inline SVG. 24×24px. Fill: `--color-primary`. |
| C15 | Gold light glow | Radial gradient for treasure chest interior glow. | CSS `radial-gradient(circle, rgba(251,191,36,0.8), transparent)` on a positioned overlay div. |

---

## Audio Assets

Audio assets listed for completeness. Not generated by UX-Design — managed by CTO-Implementation using Howler.js audio sprites.

| # | Name | Description | Duration | Notes |
|---|------|-------------|----------|-------|
| A1 | chime.mp3 | Bright chime for correct actions | ~500ms | Pitched up, warm tone |
| A2 | sparkle.mp3 | Sparkle/twinkle for Power Gem creation | ~800ms | Multi-tone ascending |
| A3 | click.mp3 | Soft click for gem splits | ~200ms | Subtle, not harsh |
| A4 | buzz-soft.mp3 | Gentle buzz for invalid actions | ~300ms | Low pitch, not alarming |
| A5 | fanfare.mp3 | Short triumphant fanfare for level complete | ~1500ms | Orchestral feel, celebratory |
| A6 | bell.mp3 | Gentle bell for hint trigger | ~400ms | Calming, attention-getting |
| A7 | creak.mp3 | Wood creak for treasure chest opening | ~300ms | Subtle, atmospheric |

---

*Document version: 1.0*
*Last updated: 2026-03-14*
*Author: UX-Design Agent*
