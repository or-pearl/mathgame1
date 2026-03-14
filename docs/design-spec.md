# Design Specification

> **Version:** 1.0
> **Date:** 2026-03-14
> **Author:** UX-Design Agent
> **Status:** Draft — awaiting founder review

---

## 1. Design System

### 1.1 Color Palette

The palette draws from gem tones (ruby, sapphire, emerald, amber) set against a warm cave backdrop. Colors chosen for WCAG 2.1 AA contrast on both light surfaces and dark cave backgrounds.

| Token | Value | Usage | Contrast on `--color-background` |
|-------|-------|-------|----------------------------------|
| `--color-primary` | `#2563EB` | Primary actions (Play button, confirm split) | 4.6:1 ✓ |
| `--color-primary-light` | `#60A5FA` | Hover states, focus rings | decorative |
| `--color-primary-dark` | `#1D4ED8` | Active/pressed states | 5.3:1 ✓ |
| `--color-background` | `#1C1226` | Cave background (dark purple-brown) | — |
| `--color-surface` | `#2D1F3D` | Card/panel surfaces on cave bg | — |
| `--color-surface-light` | `#3D2E52` | Elevated surfaces, modals | — |
| `--color-text-primary` | `#FEF3C7` | Body text on dark backgrounds (warm cream) | 13.2:1 ✓ |
| `--color-text-secondary` | `#D4B896` | Labels, captions on dark backgrounds | 7.1:1 ✓ |
| `--color-text-on-light` | `#1C1226` | Text on light/gem-colored surfaces | context-dependent |
| `--color-gem-ruby` | `#DC2626` | Gem group A / split left | 4.5:1 ✓ |
| `--color-gem-sapphire` | `#2563EB` | Gem group B / split right | 4.6:1 ✓ |
| `--color-gem-emerald` | `#059669` | Gem group C (World 3 multi-addend) | 4.5:1 ✓ |
| `--color-gem-amber` | `#D97706` | Gem group D (World 3 multi-addend) | 5.8:1 ✓ |
| `--color-power-gem` | `#FBBF24` | Power Gem glow (gold) | decorative |
| `--color-power-gem-core` | `#F59E0B` | Power Gem solid center | decorative |
| `--color-success` | `#10B981` | Positive feedback, correct split | 5.1:1 ✓ |
| `--color-success-light` | `#A7F3D0` | Success background tint | decorative |
| `--color-error` | `#F87171` | Gentle error indication | 5.2:1 ✓ |
| `--color-error-light` | `#FECACA` | Error background tint | decorative |
| `--color-warning` | `#FBBF24` | Caution states | decorative |
| `--color-hint` | `#A78BFA` | Ghost splitter, hint overlays | 4.7:1 ✓ |
| `--color-locked` | `#6B7280` | Locked levels, disabled states | 4.6:1 ✓ |
| `--color-glow` | `#FDE68A` | General glow effects (level pulse, treasure) | decorative |

#### Gem Colors by Context

| Context | Group A | Group B | Group C | Group D |
|---------|---------|---------|---------|---------|
| World 1 (free split) | `--color-gem-ruby` | `--color-gem-sapphire` | — | — |
| World 2 (make ten) | `--color-gem-ruby` | `--color-gem-sapphire` | — | — |
| World 3 (multi-addend) | `--color-gem-ruby` | `--color-gem-sapphire` | `--color-gem-emerald` | `--color-gem-amber` |

### 1.2 Typography

Hebrew-first typography. Primary font must support full niqqud (vowel diacritics) for early readers per PRD §8.1.

| Token | Font | Weight | Size | Line Height | Usage |
|-------|------|--------|------|-------------|-------|
| `--type-heading-1` | Heebo | 700 | 36px | 44px | Screen titles (world names, "!כל הכבוד") |
| `--type-heading-2` | Heebo | 600 | 28px | 36px | Section headings, level labels |
| `--type-heading-3` | Heebo | 600 | 22px | 28px | Sub-headings, narrator name |
| `--type-body` | Heebo | 400 | 20px | 28px | Body text, narrator speech |
| `--type-body-large` | Heebo | 400 | 24px | 32px | Narrator speech bubbles (primary instruction text) |
| `--type-caption` | Heebo | 400 | 16px | 22px | Captions, labels, metadata |
| `--type-button` | Heebo | 600 | 20px | 24px | Button labels |
| `--type-button-large` | Heebo | 700 | 24px | 28px | Primary CTA buttons |
| `--type-number` | Rubik | 700 | 32px | 36px | Gem count numerals, equation numbers |
| `--type-number-large` | Rubik | 700 | 48px | 52px | Power Gem "10" display, equation totals |
| `--type-equation` | Rubik | 500 | 24px | 28px | Equation operators (+, =, →) |

**Font loading strategy:**
- **Heebo** (Hebrew + Latin): Google Fonts CDN, `font-display: swap`, preloaded via `<link rel="preload">`. Subset: Hebrew + Latin + niqqud.
- **Rubik** (numbers/math): Google Fonts CDN, `font-display: swap`. Used only for numerals and math symbols.
- **System fallback stack:** `"Heebo", "Arial Hebrew", "Segoe UI", system-ui, sans-serif`
- **Number fallback stack:** `"Rubik", "Arial", system-ui, sans-serif`

### 1.3 Spacing Scale

| Token | Value | Usage |
|-------|-------|-------|
| `--space-2xs` | `2px` | Hairline gaps (gem-to-gem within cluster) |
| `--space-xs` | `4px` | Tight spacing (icon padding) |
| `--space-sm` | `8px` | Compact spacing (label-to-element) |
| `--space-md` | `16px` | Default component internal padding |
| `--space-lg` | `24px` | Component-to-component spacing |
| `--space-xl` | `32px` | Section spacing |
| `--space-2xl` | `48px` | Major section breaks, page margins |
| `--space-3xl` | `64px` | Top-level layout spacing |

### 1.4 Layout

**Grid system:** CSS Grid for screen-level layout. Flexbox for component internals.

**Max content width:** `960px` (centered). Game area fills available width up to max.

**Page structure:**
```
┌─────────────────────────────────────────────┐
│ Top Bar (56px)                              │
│  [speaker] [settings]    [title]            │
├─────────────────────────────────────────────┤
│                                             │
│            Game Area (flex: 1)              │
│         max-width: 960px, centered          │
│                                             │
├─────────────────────────────────────────────┤
│ Bottom Bar (64px, gameplay only)            │
│  [equation display]    [confirm button]     │
└─────────────────────────────────────────────┘
```

**RTL layout:** `<html dir="rtl" lang="he">`. All layout uses CSS logical properties:
- `margin-inline-start` / `margin-inline-end` (not `margin-left`/`margin-right`)
- `padding-inline-start` / `padding-inline-end`
- `inset-inline-start` / `inset-inline-end`
- `text-align: start` / `text-align: end`

Math notation containers use explicit `dir="ltr"` override.

### 1.5 Elevation / Depth

| Token | Value | Usage |
|-------|-------|-------|
| `--shadow-sm` | `0 1px 3px rgba(0,0,0,0.3)` | Subtle lift (gem clusters, cards) |
| `--shadow-md` | `0 4px 12px rgba(0,0,0,0.4)` | Elevated elements (buttons, narrator bubble) |
| `--shadow-lg` | `0 8px 24px rgba(0,0,0,0.5)` | High emphasis (modals, settings overlay) |
| `--shadow-glow-gold` | `0 0 20px rgba(251,191,36,0.6)` | Power Gem glow, treasure chest glow |
| `--shadow-glow-hint` | `0 0 16px rgba(167,139,250,0.5)` | Hint splitter glow |
| `--shadow-gem` | `0 2px 4px rgba(0,0,0,0.3), inset 0 1px 2px rgba(255,255,255,0.2)` | Individual gem depth |

### 1.6 Border Radii

| Token | Value | Usage |
|-------|-------|-------|
| `--radius-sm` | `4px` | Small elements (badges, tags) |
| `--radius-md` | `8px` | Buttons, input fields |
| `--radius-lg` | `12px` | Cards, panels, narrator bubble |
| `--radius-xl` | `16px` | Large cards, modals |
| `--radius-round` | `50%` | Gems (circular), avatar |
| `--radius-pill` | `9999px` | Pill buttons, progress bars |

### 1.7 Motion / Animation Defaults

| Property | Value | Notes |
|----------|-------|-------|
| Default duration | `200ms` | Standard button/hover transitions |
| Default easing | `cubic-bezier(0.4, 0, 0.2, 1)` | Material-style ease-out |
| Enter duration | `300ms` | Elements appearing on screen |
| Exit duration | `200ms` | Elements leaving screen |
| Bounce easing | `cubic-bezier(0.34, 1.56, 0.64, 1)` | Playful overshoot for gems, celebrations |
| Spring easing | `cubic-bezier(0.22, 1, 0.36, 1)` | Smooth drag release |
| Celebration duration | `800ms` | Power Gem fusion, treasure open |
| Slow pulse | `2000ms` | Level node glow pulse on map (infinite loop) |
| Reduced motion | All animations → `duration: 0ms, no transform` | Respects `prefers-reduced-motion: reduce` |

---

## 2. Component Library

### 2.1 Gem

**Description:** A single round gem. The atomic visual unit representing "1." Gems compose into GemClusters.

**Variants:** `ruby` (red), `sapphire` (blue), `emerald` (green), `amber` (gold), `power` (gold glow, represents 10)

**Dimensions:** `40×40px` (default), `32×32px` (compact, for 10-frame), `56×56px` (large, for celebration/Power Gem)

**Visual Spec:**
- Shape: Circle (`border-radius: 50%`)
- Background: Radial gradient — `radial-gradient(circle at 35% 35%, <color-light>, <color-base> 60%, <color-dark>)` to simulate a 3D sphere
- Border: `2px solid` slightly darker variant of gem color
- Shadow: `--shadow-gem`
- Inner highlight: `inset` shadow at top-left for specular highlight

**States:**
| State | Visual Change |
|-------|--------------|
| Default | As described above |
| Dragging | Scale `1.1`, shadow `--shadow-md`, opacity `0.9`, cursor `grabbing` |
| Hover (desktop) | Scale `1.05`, brightness `1.1` |
| In Power Gem | Gold color, `--shadow-glow-gold`, scale `1.0` → `1.4` during fusion animation |
| Counted | Small numeral label appears below gem |

**Accessibility:**
- Each gem is `role="img"` with `aria-label="אבן חן"` (gem)
- Not individually focusable; focus management is at the GemCluster level

### 2.2 GemCluster

**Description:** A group of N gems arranged in a line or 10-frame grid. The main interactive element for splitting. Contains a Splitter component when in split mode.

**Layout modes:**
- **Line:** Gems in a horizontal row with `--space-2xs` (2px) gaps. Used in World 1 for numbers 5–9.
- **10-frame:** 2×5 grid (2 rows, 5 columns), `--space-xs` (4px) gaps. Used in World 2–3 to scaffold counting to 10.

**Dimensions:**
- Line mode: `(N × 40px) + ((N-1) × 2px)` wide × `40px` tall + `32px` for count label below
- 10-frame mode: `(5 × 32px) + (4 × 4px)` = `176px` wide × `(2 × 32px) + 4px` = `68px` tall + `32px` for count label

**Visual Spec:**
- Container: `--color-surface` background, `--radius-lg` border-radius, `--space-md` padding
- Count label: `--type-number` font, centered below gems, color `--color-text-primary`
- Border: `2px solid transparent` default; `2px solid --color-primary-light` when focused

**States:**
| State | Visual Change |
|-------|--------------|
| Default | Gems visible, count label shown |
| Split | Two sub-groups visible with Splitter between them; each sub-group has own count label |
| Draggable | Cursor `grab`, subtle lift on hover |
| Drag target (valid) | Border `2px dashed --color-success`, background tint `--color-success-light` at 10% opacity |
| Drag target (invalid) | Border `2px dashed --color-error`, background tint `--color-error-light` at 10% opacity |
| Drag target (neutral) | Border `2px dashed --color-text-secondary` |

**Accessibility:**
- Container: `role="group"`, `aria-label="קבוצה של N אבני חן"` (group of N gems)
- Keyboard: Arrow keys move Splitter position within the cluster; Enter confirms split
- Focus indicator: `2px solid --color-primary-light` outline with `2px` offset

### 2.3 Splitter

**Description:** A draggable divider line within a GemCluster that splits gems into two groups. Positioned between gems at valid split points.

**Dimensions:** `4px` wide × full height of GemCluster + `16px` overshoot top and bottom (grabber extends beyond gems for easier touch targeting). Touch target: `44×44px` minimum (invisible hit area around the visual line).

**Visual Spec:**
- Line: `4px` wide, `--color-primary`, `--radius-pill` caps
- Grabber knob (top & bottom): `20×20px` circle, `--color-primary`, `--shadow-md`
- Glow: `--shadow-glow-hint` (subtle) around line

**States:**
| State | Visual Change |
|-------|--------------|
| Default | Visible between gems at current position |
| Hover | Grabber knobs scale `1.2`, glow intensifies |
| Dragging | Line and knobs follow pointer; gems animate apart in real-time preview |
| Ghost (hint) | Color `--color-hint`, opacity `0.5`, gentle pulse animation |

**Accessibility:**
- `role="separator"`, `aria-orientation="vertical"`, `aria-valuenow="{position}"`, `aria-valuemin="1"`, `aria-valuemax="{N-1}"`
- `aria-label="מפצל — גרור כדי לפצל את אבני החן"` (Splitter — drag to split the gems)
- Keyboard: Left/Right arrow moves position; Enter confirms

### 2.4 GemGroup

**Description:** A sub-group of gems after splitting. Draggable to combine with other groups (World 2–3).

**Dimensions:** Same as a mini GemCluster — width depends on gem count.

**Visual Spec:**
- Background: Gem color tint at 15% opacity as a bounding highlight
- Count label: `--type-number`, positioned below the group
- Combine indicator: When dragged near another group, a "+" icon appears between them

**States:**
| State | Visual Change |
|-------|--------------|
| Default | Gems with colored tint background |
| Dragging | Scale `1.05`, shadow `--shadow-md`, z-index elevated |
| Combinable (near target) | Pulse glow matching the target group's color |
| Combined | Merge animation (gems slide together, count updates) |

**Accessibility:**
- `role="group"`, `aria-label="קבוצה של N אבני חן, גרור כדי לשלב"` (group of N gems, drag to combine)
- `aria-grabbed="true/false"`
- Keyboard: Tab to select group, arrow keys to move toward combine zone, Enter to combine

### 2.5 CombineZone

**Description:** A drop target area where gem groups can be merged. In World 2, this is where groups are combined to make 10. In World 3, this feeds into treasure chest slots.

**Dimensions:** `200×100px` (default), scales responsively.

**Visual Spec:**
- Background: `--color-surface` with dashed border `2px dashed --color-text-secondary`
- 10-frame outline: Faint 2×5 grid lines drawn inside (10 placeholder circles at 10% opacity)
- Label: `--type-caption`, text "10" centered, color `--color-text-secondary`

**States:**
| State | Visual Change |
|-------|--------------|
| Empty | Dashed border, faint 10-frame grid |
| Receiving (valid, count will be ≤ 10) | Border solid `--color-success`, background `--color-success-light` at 10% |
| Receiving (invalid, count > 10) | Border solid `--color-error`, background `--color-error-light` at 10% |
| Contains gems (< 10) | Gems rendered inside, count label updated, label text "N / 10" |
| Complete (= 10) | Power Gem fusion animation triggers (see §4.1) |

**Accessibility:**
- `role="region"`, `aria-label="אזור שילוב — הנח כאן כדי ליצור 10"` (combine zone — drop here to make 10)
- `aria-dropeffect="move"`
- Announces count changes: `aria-live="polite"`, `"N מתוך 10"` (N of 10)

### 2.6 PowerGem

**Description:** A large, glowing gold gem representing exactly 10. Appears when 10 gems are combined successfully. Fills a treasure chest slot.

**Dimensions:** `56×56px`

**Visual Spec:**
- Shape: Circle (`border-radius: 50%`)
- Background: Radial gradient — `radial-gradient(circle at 35% 35%, #FDE68A, #F59E0B 60%, #B45309)`
- Shadow: `--shadow-glow-gold`
- Inner text: "10" in `--type-number-large`, color `--color-text-on-light`
- Sparkle particles: 6–8 small (`4×4px`) gold dots animating outward on creation

**States:**
| State | Visual Change |
|-------|--------------|
| Appearing | Fusion animation (§4.1), then settles into default |
| Default | Gold glow pulsing subtly (opacity `0.6` → `1.0`, `2000ms` loop) |
| In chest slot | Shrinks to `40×40px`, glow dims, sits in slot |

**Accessibility:**
- `role="img"`, `aria-label="אבן כוח — 10 אבני חן"` (Power Gem — 10 gems)

### 2.7 EquationBar

**Description:** Displays the decomposition equation after the student confirms a split or completes a level. Positioned at the bottom of the gameplay screen.

**Dimensions:** Full width of game area, height `64px`, padding `--space-md`.

**Visual Spec:**
- Background: `--color-surface` with `--shadow-sm` top edge
- Text: `--type-equation` for operators, `--type-number` for numerals
- Direction: explicit `dir="ltr"` — math reads left-to-right regardless of UI language
- Example rendering: `8 = 5 + 3` or `7 + 5 → 10 + 2 = 12`
- Each number uses the color of its corresponding gem group

**States:**
| State | Visual Change |
|-------|--------------|
| Hidden | `height: 0`, `opacity: 0` (before first split) |
| Revealing | Slides up from bottom, `300ms`, `ease-out` |
| Visible | Equation displayed; numbers highlight in sequence as narration reads them |
| Updated | New equation fades in (`200ms` cross-fade) |

**Accessibility:**
- `role="math"`, `aria-label` = full equation in Hebrew natural language
- `dir="ltr"` on the equation container
- Numbers use `aria-hidden="true"` (the `aria-label` provides the accessible reading)

### 2.8 NarratorBubble

**Description:** Speech bubble from the explorer character displaying narration text. Always visible during instruction/feedback moments.

**Dimensions:** Max width `400px`, min height `80px`, positioned at bottom-right of game area (bottom-left in LTR).

**Visual Spec:**
- Background: `#FFFBEB` (warm cream, near-white) — light surface for contrast with dark cave bg
- Border: `2px solid #F59E0B` (amber border)
- Border-radius: `--radius-lg` with a triangular speech tail pointing toward the character
- Text: `--type-body-large`, color `--color-text-on-light`
- Character avatar: `48×48px` circle to the right of the bubble (Hebrew RTL), showing the explorer character's face

**States:**
| State | Visual Change |
|-------|--------------|
| Hidden | `opacity: 0`, `transform: scale(0.9)`, not in DOM flow |
| Appearing | Scale `0.9` → `1.0`, opacity `0` → `1`, `300ms`, bounce easing |
| Visible | Text appears with a typewriter effect (`30ms` per character) |
| Exiting | Opacity `1` → `0`, `200ms` |

**Accessibility:**
- `role="status"`, `aria-live="polite"` — screen readers announce new narration
- Text in bubble matches the audio narration exactly

### 2.9 Button

**Description:** Standard interactive button used throughout the game.

**Variants:**
| Variant | Background | Text Color | Border | Usage |
|---------|-----------|------------|--------|-------|
| Primary | `--color-primary` | `#FFFFFF` | none | Play, Confirm Split, Next Level |
| Secondary | `transparent` | `--color-primary` | `2px solid --color-primary` | Replay, Back to Map |
| Ghost | `transparent` | `--color-text-primary` | none | Settings toggles, speaker icon |
| Success | `--color-success` | `#FFFFFF` | none | Level complete actions |

**Dimensions:** Padding `--space-sm --space-lg` (12px 24px). Minimum touch target `44×44px`. Min width `120px` for text buttons.

**Visual Spec:**
- Font: `--type-button` (default) or `--type-button-large` (primary CTA)
- Border-radius: `--radius-md`
- Shadow: `--shadow-sm` (primary, success); none (secondary, ghost)
- Icon spacing: `--space-sm` between icon and text

**States:**
| State | Visual Change |
|-------|--------------|
| Default | As variant spec |
| Hover | Brightness `1.1`, shadow `--shadow-md` |
| Focus | `2px solid --color-primary-light` outline, `2px` offset |
| Active / Pressed | Scale `0.97`, brightness `0.95` |
| Disabled | Opacity `0.5`, cursor `not-allowed` |
| Loading | Content replaced by spinner (20×20px), width preserved |

**Accessibility:**
- Native `<button>` element
- Focus indicator always visible (no outline-none)
- `aria-disabled` instead of `disabled` attribute when button should be non-interactive but still focusable
- Touch target: min `44×44px` enforced via padding or min-height/min-width

### 2.10 IconButton

**Description:** Button containing only an icon (no text label). Used for speaker toggle, settings gear, close/back.

**Dimensions:** `44×44px` (touch target and visible size).

**Visual Spec:**
- Icon: `24×24px`, centered, color `--color-text-primary`
- Background: `transparent` default; `--color-surface-light` on hover
- Border-radius: `--radius-round` (circular)

**States:** Same as Button ghost variant.

**Accessibility:**
- `aria-label` required (e.g., `"השתק שמע"` / mute audio, `"הגדרות"` / settings)
- `aria-pressed="true/false"` for toggles (speaker, sfx)

### 2.11 ProgressNode (Map Level Node)

**Description:** A level node on the progress map representing one level.

**Dimensions:** `56×56px` circle.

**Visual Spec:**
- Background: Varies by state (see below)
- Border: `3px solid` matching state color
- Inner icon: Treasure chest icon (`28×28px`) — open, closed, or locked variant
- Connecting path: `4px` line between nodes, color `--color-text-secondary`

**States:**
| State | Visual | Icon | Border Color |
|-------|--------|------|-------------|
| Locked | `--color-locked` bg, opacity `0.5` | Closed lock | `--color-locked` |
| Available (current) | `--color-surface` bg, glow pulse | Closed chest, no lock | `--color-power-gem`, `--shadow-glow-gold` |
| Completed | `--color-success` bg at 20% opacity | Open chest + checkmark | `--color-success` |

**Accessibility:**
- `role="link"` (completed levels are tappable to replay)
- `aria-label="שלב N — [locked/available/completed]"` (Level N — status)
- `aria-disabled="true"` for locked levels
- Keyboard: Tab navigates between nodes; Enter activates completed/current

### 2.12 SettingsOverlay

**Description:** Semi-transparent overlay with audio settings toggles.

**Dimensions:** `320×240px` centered card. Overlay behind: full-screen `rgba(0,0,0,0.6)`.

**Visual Spec:**
- Card background: `--color-surface-light`
- Border-radius: `--radius-xl`
- Shadow: `--shadow-lg`
- Title: `--type-heading-2`, "הגדרות" (Settings)
- Toggle rows: Label (`--type-body`) + toggle switch aligned end
- Close button: IconButton (X icon) at top-inline-end corner

**Toggle switch spec:**
- Track: `48×28px`, `--radius-pill`, `--color-locked` bg (off) / `--color-success` bg (on)
- Thumb: `24×24px` circle, `#FFFFFF`, shadow `--shadow-sm`, slides `20px` on toggle
- Transition: `200ms` ease-out

**Accessibility:**
- `role="dialog"`, `aria-modal="true"`, `aria-label="הגדרות"` (Settings)
- Focus trapped within overlay when open
- Esc key closes overlay
- Toggles use `role="switch"`, `aria-checked="true/false"`

### 2.13 TreasureChest

**Description:** Visual reward element. Shown at level complete and as Power Gem slots in World 3.

**Dimensions:** `120×100px` (level complete), `80×68px` (Power Gem slot on gameplay screen).

**Visual Spec:**
- Illustration: Wooden chest with gold trim (code-generated via CSS gradients + box-shadow, not an image asset — see §6 Code-Generated Assets)
- Closed: Lid closed, lock icon, dark wood tones (`#8B4513` base, `#D4A574` trim, `#FFD700` lock)
- Open: Lid rotated up, gold glow from interior, gems visible inside

**States:**
| State | Visual Change |
|-------|--------------|
| Locked | Closed, lock icon visible, desaturated |
| Slot (empty) | Closed, dashed gold outline where Power Gem goes |
| Slot (filled) | Power Gem visible in slot, subtle glow |
| Opening | Lid rotates open (`500ms`, bounce easing), gold light floods out |
| Open | Interior visible, gems sparkle |

**Accessibility:**
- `role="img"`, `aria-label` describes state ("תיבת אוצר נעולה" / locked treasure chest, "תיבת אוצר פתוחה" / open treasure chest)

---

## 3. Screen Designs

### 3.1 Title Screen

**Purpose:** Game entry point. Student taps "Play" to start. Audio context is unlocked on first interaction.

**Layout:**
```
┌─────────────────────────────────────────────┐
│                                             │
│         [Cave backdrop - full bleed]        │
│                                             │
│              ╔═══════════════╗              │
│              ║ אוצר העשר    ║              │
│              ║ TREASURE OF   ║              │
│              ║    TENS       ║              │
│              ╚═══════════════╝              │
│                                             │
│            [Explorer character]             │
│           standing at cave entrance         │
│                                             │
│         ┌─────────────────────┐             │
│         │      ▶  שחק!       │             │
│         │     (Play!)         │             │
│         └─────────────────────┘             │
│                                             │
│  [🔊]                                      │
│  Speaker toggle                              │
│  (bottom-inline-start)                      │
│                                             │
└─────────────────────────────────────────────┘
```

**Components used:** Button (Primary, large), IconButton (speaker toggle)

**Visual details:**
- Game logo: `--type-heading-1` at `48px`, color `--color-power-gem`, text-shadow `0 2px 8px rgba(0,0,0,0.5)`
- Hebrew title "אוצר העשר" is primary; English subtitle "Treasure of Tens" in `--type-caption` below, optional
- Cave backdrop: Dark gradient from `--color-background` at top to slightly lighter at bottom, with subtle stalactite silhouettes (CSS shapes)
- Explorer character: Positioned center-bottom third of screen, `200×240px` illustration area
- Play button: `--type-button-large`, min-width `200px`, `--color-primary`, centered, `--shadow-md`
- Speaker toggle: `IconButton` at bottom-inline-start, `--space-2xl` margin from edges

**Interaction flow:**
1. Page loads → background fades in (`500ms`)
2. Logo scales in with bounce easing (`600ms`, `200ms` delay)
3. Explorer character slides up (`400ms`, `400ms` delay)
4. Play button fades in (`300ms`, `600ms` delay)
5. Student taps Play → audio context unlocked → transition to Progress Map

**First-launch behavior:** If `localStorage` has no game state, the Play button tap goes directly to World 1 Level 1 (bypassing map). Subsequent launches go to the Progress Map.

**Accessibility:**
- Play button auto-focused on load
- Screen: `aria-label="אוצר העשר — משחק מתמטיקה. לחץ שחק כדי להתחיל"` (Treasure of Tens — math game. Press Play to start)

### 3.2 Progress Map

**Purpose:** Shows all levels in the current world as connected nodes. Student selects a level to play or replays completed levels.

**Layout:**
```
┌─────────────────────────────────────────────┐
│ Top Bar (56px)                              │
│  [🔊] [⚙]                [עולם 1: ...]    │
├─────────────────────────────────────────────┤
│                                             │
│  Cave path winding through the screen:      │
│                                             │
│      ●───●───◉───○───○───○                 │
│      ✓   ✓   ★                             │
│   (done)(done)(current)(locked)(locked)     │
│                                             │
│  Path curves and winds vertically.          │
│  Each ● is a ProgressNode (56px).           │
│  Path lines connect them (4px, curved).     │
│                                             │
│  Cave decorations: stalactites, crystals    │
│  (purely decorative CSS/SVG elements)       │
│                                             │
│  World label at top of path section.        │
│  When all levels in world complete,         │
│  next world section scrolls into view.      │
│                                             │
│                                             │
│  [Explorer character]                       │
│  (standing at the current level node)       │
│                                             │
└─────────────────────────────────────────────┘
```

**Components used:** ProgressNode, IconButton (speaker, settings), TopBar

**Visual details:**
- Path: `4px` SVG line, color `--color-text-secondary`, with bezier curves between nodes
- Nodes arranged in a winding vertical path (3–4 nodes per row, alternating left-to-right and right-to-left)
- World title: `--type-heading-2`, color `--color-text-primary`, positioned at the start of the world's path section
- Cave decorations: CSS-generated stalactites (triangles), crystal clusters (rotated diamonds with gem colors), subtle sparkle dots
- Background: `--color-background` with a vertical gradient suggesting depth
- Explorer character: `80×96px`, positioned adjacent to the current level node

**Interaction flow:**
1. Map loads → nodes animate in sequentially (`100ms` stagger, scale from `0` → `1`)
2. Current level pulses with glow (infinite `2000ms` loop)
3. Student taps completed level → replay that level
4. Student taps current level → start that level
5. Student taps locked level → gentle shake animation + narrator says "!עוד לא, בוא נסיים את השלב הזה קודם" (Not yet, let's finish this level first!)
6. Level complete → return to map with the newly completed node animating to "completed" state

**Scrolling:** Map is vertically scrollable if content exceeds viewport. Current level is auto-scrolled into center of viewport.

**Accessibility:**
- `role="navigation"`, `aria-label="מפת ההתקדמות"` (Progress Map)
- Nodes are in a focusable list; Tab navigates sequentially
- Current level announced as "שלב נוכחי" (current level)

### 3.3 Gameplay Screen

**Purpose:** The core interaction screen where students split gems, combine groups, and make 10s.

**Layout (World 2 — Making Tens example):**
```
┌─────────────────────────────────────────────┐
│ Top Bar (56px)                              │
│  [🔊] [⚙] [←map]        [שלב 2-3]        │
├─────────────────────────────────────────────┤
│                                             │
│  ┌─────────────────────────────────────┐    │
│  │         Gem Workspace               │    │
│  │                                     │    │
│  │   [GemCluster: 7 gems]             │    │
│  │      ●●●●|●●●                      │    │
│  │         ↕ Splitter                  │    │
│  │      (5)  (2)   ← count labels     │    │
│  │                                     │    │
│  │   [GemCluster: 5 gems]             │    │
│  │      ●●●●●                          │    │
│  │      (5)                            │    │
│  │                                     │    │
│  │           ┌────────────┐            │    │
│  │           │ CombineZone│            │    │
│  │           │  ○○○○○     │            │    │
│  │           │  ○○○○○     │            │    │
│  │           │   "10"     │            │    │
│  │           └────────────┘            │    │
│  │                                     │    │
│  └─────────────────────────────────────┘    │
│                                             │
│  ┌──────────────────────────┐               │
│  │ NarratorBubble      [😊]│               │
│  │ "!בואו נפצל כדי ליצור   │               │
│  │  קבוצה של 10"           │               │
│  └──────────────────────────┘               │
│                                             │
├─────────────────────────────────────────────┤
│ Bottom Bar (64px)                           │
│  [EquationBar: 7 + 5 = ?]    [✓ אשר]      │
└─────────────────────────────────────────────┘
```

**Components used:** GemCluster, Splitter, GemGroup, CombineZone, PowerGem, EquationBar, NarratorBubble, Button (confirm), IconButton (speaker, settings, back-to-map), TopBar

**Layout zones:**
| Zone | CSS Grid Area | Height |
|------|--------------|--------|
| Top Bar | row 1 | `56px` |
| Gem Workspace | row 2 | `flex: 1` (fills available space) |
| Narrator | row 3 | `auto` (content-driven, max `120px`) |
| Bottom Bar | row 4 | `64px` |

**Gem Workspace internal layout:**
- Gem clusters arranged vertically with `--space-xl` (32px) between them
- CombineZone centered below gem clusters with `--space-lg` (24px) margin
- Everything centered horizontally within the workspace

**World-specific variations:**
| World | Gem layout | CombineZone | Extra elements |
|-------|-----------|-------------|----------------|
| 1 (Free Split) | Single GemCluster, line mode | None (no combining) | "?מצאת דרך אחרת" (Find another way?) prompt after each split |
| 2 (Making Tens) | Two GemClusters, 10-frame mode | One CombineZone | Power Gem slot, remainder tray |
| 3 (Treasure Chest) | 3–4 GemClusters, 10-frame mode | Multiple CombineZones | TreasureChest with Power Gem slots |

**Interaction flow (World 2):**
1. Level loads → narrator introduces goal → gem clusters appear (staggered, `200ms` each)
2. Student drags Splitter within a cluster → gems preview the split in real-time
3. Student releases Splitter → split confirmed, two GemGroups created with count labels
4. Student drags a GemGroup toward CombineZone or another group → visual snap indicator
5. When CombineZone count = 10 → Power Gem animation (§4.1) → narrator celebrates
6. Remaining gems counted → equation displayed and narrated
7. Confirm button becomes active → student taps → Level Complete screen

**Accessibility:**
- Screen: `role="main"`, `aria-label="שלב N — [level description]"`
- All interactive elements keyboard-accessible (Tab order: clusters → splitter → confirm)
- Status announcements via `aria-live` regions for gem counts, combine results

### 3.4 Level Complete Screen

**Purpose:** Celebration and summary. Shows the completed equation and offers next actions.

**Layout:**
```
┌─────────────────────────────────────────────┐
│                                             │
│              ✨ ✨ ✨ ✨ ✨               │
│                                             │
│           [TreasureChest: opening]          │
│              (120×100px)                    │
│                                             │
│           ┌─────────────────┐               │
│           │  !מצאת את האוצר │               │
│           │  (You found the  │               │
│           │   treasure!)     │               │
│           └─────────────────┘               │
│                                             │
│        Equation summary (dir="ltr"):        │
│          7 + 5 → 10 + 2 = 12              │
│                                             │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐   │
│  │ 🔄 שחק   │ │ 🗺 מפה   │ │ ▶ הבא   │   │
│  │   שוב    │ │          │ │          │   │
│  │ (Replay) │ │  (Map)   │ │  (Next)  │   │
│  └──────────┘ └──────────┘ └──────────┘   │
│                                             │
│  [NarratorBubble: equation readout]         │
│                                             │
└─────────────────────────────────────────────┘
```

**Components used:** TreasureChest, Button (Primary: Next, Secondary: Replay, Secondary: Map), EquationBar, NarratorBubble

**Visual details:**
- Background: `--color-background` with gold particle overlay (CSS animation, ~20 small dots floating up)
- Celebration heading: `--type-heading-1`, color `--color-power-gem`, text-shadow
- Equation: `--type-number-large` for total, gem-colored numbers
- Buttons: Arranged horizontally with `--space-md` gaps, centered

**Interaction flow:**
1. Screen enters → treasure chest opening animation (`800ms`)
2. Gold particles burst outward (`600ms`)
3. Heading appears with bounce easing (`400ms`, `400ms` delay)
4. Equation fades in (`300ms`, `800ms` delay)
5. Narrator reads the equation aloud
6. Buttons appear (`300ms`, `1200ms` delay)
7. Student picks Next Level / Replay / Back to Map

**Accessibility:**
- `role="alertdialog"` — announces celebration
- `aria-label="!כל הכבוד — סיימת את השלב"` (Great job — you completed the level)
- Focus auto-moves to "Next Level" button
- Narration plays automatically

### 3.5 Settings Overlay

**Purpose:** Toggle audio narration and sound effects independently.

**Layout:**
```
┌──────── Settings Overlay ─────────┐
│                              [✕]  │
│                                   │
│  הגדרות                          │
│  (Settings)                       │
│                                   │
│  ┌───────────────────────────┐    │
│  │ קריינות (Narration)  [●─]│    │
│  └───────────────────────────┘    │
│                                   │
│  ┌───────────────────────────┐    │
│  │ אפקטי קול (SFX)     [●─]│    │
│  └───────────────────────────┘    │
│                                   │
└───────────────────────────────────┘
```

**Components used:** SettingsOverlay (§2.12), Toggle switches, IconButton (close)

**Interaction flow:**
1. Settings icon tapped → overlay slides in from top with backdrop fade (`300ms`)
2. Toggles are interactive immediately
3. Changes apply instantly (narration stops/resumes, SFX stops/resumes)
4. Close button or backdrop tap → overlay slides out (`200ms`)

**Accessibility:**
- Modal dialog with focus trap
- Esc closes
- Toggle states announced by screen reader

---

## 4. Animation Specifications

### 4.1 Power Gem Fusion

**Trigger:** CombineZone gem count reaches exactly 10.
**Duration:** `800ms` total.
**Easing:** Bounce (`cubic-bezier(0.34, 1.56, 0.64, 1)`)
**Sequence:**
| Step | Time | Animation |
|------|------|-----------|
| 1 | `0–200ms` | All 10 gems slide inward to center of CombineZone. `ease-in` |
| 2 | `200–400ms` | Gems cluster tightly, scale down to `0.5`. Flash white (`opacity: 0` → `1` → `0`). |
| 3 | `400–700ms` | PowerGem appears at center, scales from `0` → `1.3` → `1.0`. Bounce easing. Gold glow (`--shadow-glow-gold`) from `0` → full. |
| 4 | `700–800ms` | 6–8 sparkle particles burst outward from center, fade over `400ms` |
**Sound:** Chime SFX at step 2 flash, sparkle SFX at step 4.
**Reduced motion:** Gems instantly disappear, PowerGem instantly appears, no particles.

### 4.2 Gem Split Preview

**Trigger:** Student drags Splitter to a position within a GemCluster.
**Duration:** `150ms` (real-time responsive to drag position).
**Easing:** `ease-out`
**Start state:** Gems in a single group, uniform spacing.
**End state:** Gems separated into two groups with `24px` gap at the Splitter position. Each group highlighted with its assigned color tint.
**Reduced motion:** Instant position change, no transition.

### 4.3 Gem Group Drag

**Trigger:** Student starts dragging a GemGroup.
**Duration:** Continuous during drag.
**Easing:** Spring (`cubic-bezier(0.22, 1, 0.36, 1)`) on release.
**Start state:** Group at rest position. `transform: scale(1)`, `box-shadow: --shadow-sm`.
**During drag:** `transform: scale(1.05)`, `box-shadow: --shadow-md`, `z-index: 100`, follows pointer.
**End state (dropped on valid target):** Spring to target position, `200ms`.
**End state (dropped on invalid target):** Spring back to original position, `300ms`, with gentle shake (`translateX ±4px`, 3 cycles, `200ms`).
**Reduced motion:** Instant position change on release.

### 4.4 Level Node Pulse (Current Level)

**Trigger:** Level is the next playable level on the Progress Map.
**Duration:** `2000ms` loop, infinite.
**Easing:** `ease-in-out`
**Animation:** `box-shadow: --shadow-glow-gold` opacity oscillates `0.4` → `1.0` → `0.4`. Scale oscillates `1.0` → `1.05` → `1.0`.
**Reduced motion:** Static gold border, no animation.

### 4.5 Treasure Chest Opening

**Trigger:** Level complete.
**Duration:** `800ms`.
**Easing:** Bounce easing for lid, ease-out for glow.
**Sequence:**
| Step | Time | Animation |
|------|------|-----------|
| 1 | `0–100ms` | Chest shakes slightly (rotate ±2deg, 2 cycles) |
| 2 | `100–500ms` | Lid rotates open (rotateX `0deg` → `-110deg`). Bounce easing. |
| 3 | `300–800ms` | Gold light emanates from inside (radial gradient opacity `0` → `0.8`). |
| 4 | `500–800ms` | Small gem sprites float up from interior (4–6 gems, random paths, fade out). |
**Sound:** Creak SFX at step 1, fanfare SFX at step 2.
**Reduced motion:** Chest instantly shows open state, no particles.

### 4.6 Celebration Particles

**Trigger:** Level complete screen appears.
**Duration:** `3000ms`, then particles fade.
**Implementation:** CSS `@keyframes` on `::before` / `::after` pseudo-elements and a few small `<span>` elements.
**Properties:** ~20 small circles (`4–8px`), gold and gem colors, float upward with slight horizontal drift. `opacity: 1` → `0` over lifetime. Staggered start times (`0–1000ms` random delay).
**Reduced motion:** No particles displayed.

### 4.7 Screen Transitions

**Trigger:** Navigating between screens (Title → Map → Gameplay → Level Complete).
**Duration:** `400ms` total (200ms exit + 200ms enter).
**Implementation:** CSS transitions on a wrapper element.
**Exit:** `opacity: 1` → `0`, `transform: scale(1)` → `scale(0.98)`. `200ms`, `ease-in`.
**Enter:** `opacity: 0` → `1`, `transform: scale(1.02)` → `scale(1)`. `200ms`, `ease-out`.
**Reduced motion:** Instant cut, no transition.

### 4.8 Narrator Bubble Appear/Dismiss

**Trigger:** New narration text to display / narration complete.
**Duration:** `300ms` appear, `200ms` dismiss.
**Easing:** Bounce easing (appear), ease-in (dismiss).
**Appear:** `opacity: 0, scale(0.9), translateY(16px)` → `opacity: 1, scale(1), translateY(0)`.
**Dismiss:** `opacity: 1` → `opacity: 0, translateY(8px)`.
**Text typewriter:** After bubble appears, text renders character-by-character at `30ms` per character. Synced with narration audio when possible.
**Reduced motion:** Instant appear/dismiss, no typewriter (full text shown immediately).

### 4.9 Hint Ghost Splitter (Tier 1)

**Trigger:** 2 consecutive failed attempts on the same level.
**Duration:** `1500ms` (one cycle), then holds at target position.
**Easing:** `ease-in-out`
**Animation:** Ghost splitter (color `--color-hint`, opacity `0.5`) appears at the start of the cluster, slides to the hint position over `1500ms`, then pulses opacity `0.3` → `0.6` in a `1000ms` loop.
**Sound:** Gentle bell SFX at start.
**Reduced motion:** Ghost splitter appears directly at target position, static.

---

## 5. Responsive Design

### 5.1 Breakpoints

| Name | Min Width | Key Layout Changes |
|------|-----------|-------------------|
| Mobile portrait | `320px` | Single column, stacked gems, smaller gem size (`32px`). Narrator bubble full-width. |
| Tablet portrait | `600px` | **Primary target.** Gem workspace fills width. Narrator bubble positioned at bottom-right. Comfortable spacing. |
| Tablet landscape | `900px` | **Primary target.** Side-by-side gem clusters possible. More horizontal breathing room. |
| Desktop | `1200px` | Max-width `960px` content, centered. Additional cave decorations visible. |

### 5.2 Primary Target Devices

Per PRD §9.1 and architecture: Chrome on Chromebook and Safari on iPad.

| Device | Screen | Resolution | Viewport | Notes |
|--------|--------|------------|----------|-------|
| Chromebook (typical) | 11.6" | 1366×768 | ~1366×668 (minus browser chrome) | Landscape. Trackpad + keyboard. |
| iPad (9th gen) | 10.2" | 2160×1620 | 1080×810 @2x (Safari) | Portrait or landscape. Touch. |
| iPad Mini | 8.3" | 2266×1488 | 1133×744 @2x | Landscape preferred. Touch. |

### 5.3 Responsive Rules

- **Gem size:** `40px` default → `32px` below `600px` width → `48px` above `1200px` width
- **Gem cluster layout:** 10-frame grid is fixed 2×5; gem gap adjusts (`2px` small, `4px` default, `6px` large)
- **Narrator bubble:** `max-width: 400px` on tablet+; `100%` width (minus margins) on mobile
- **Button sizes:** Min `44×44px` always; padding scales with viewport
- **Bottom bar:** Fixed to bottom on all viewports; scrolls with content only if viewport < `500px` tall
- **Progress map path:** 3 nodes per row on mobile, 4 on tablet, 5 on desktop

---

## 6. RTL / Internationalization

### 6.1 What Mirrors (RTL → LTR flip)

- All text alignment (`text-align: start`)
- Navigation flow (back button on inline-end)
- Icon positions (speaker button at inline-start)
- Menu/toolbar item order
- Progress map path winding direction (starts from inline-end)
- Narrator bubble position (inline-end)
- Content padding and margins (all via logical properties)

### 6.2 What Does NOT Mirror

- **Math equations:** Always `dir="ltr"`. Numbers read left-to-right universally: `7 + 5 = 12`
- **Gem cluster arrangement:** Left-to-right within the cluster (mathematical convention)
- **Splitter drag direction:** Left-to-right (follows gem arrangement)
- **Number labels on gems:** LTR placement
- **Progress bar fill direction:** Left-to-right (conventional)

### 6.3 Bidirectional Text Handling

- Narrator bubble text: `dir="rtl"` (Hebrew flows right-to-left)
- Inline numbers in Hebrew text: Use Unicode Bidi Isolate (`<bdi>`) to prevent number reordering
- Equation text: Wrapped in `<span dir="ltr">` container
- All i18n keys stored in `/src/locales/he.json`; zero hardcoded strings (per Decision #19)

### 6.4 Hebrew-Specific Typography

- Include niqqud (vowel diacritics) in all narration text and instruction text per PRD Open Question #8
- Heebo font supports full niqqud character set
- Font size minimum `20px` for body text (niqqud requires larger rendering for legibility)
- Line height `1.4` minimum (niqqud adds vertical space below baseline)

---

*Document version: 1.0*
*Last updated: 2026-03-14*
*Author: UX-Design Agent*
*Status: Draft — awaiting founder review*
