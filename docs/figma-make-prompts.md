# Figma Make Prompts

> **Purpose:** Flattened, self-contained screen descriptions for Figma Make. Each prompt contains all concrete values (no tokens or references) so Make can produce an accurate mockup in a single pass.
>
> **Usage:** Copy one prompt at a time into Figma Make. Each prompt produces one frame.
>
> **Base context (include before each prompt or set as a project-level instruction):**
> Hebrew-first educational math game for 6–7 year olds. Cave/treasure adventure theme. RTL layout (right-to-left). Dark backgrounds with bright gem-colored accents. Friendly, warm, hand-painted children's book illustration style.

---

## Prompt 1: Title Screen

```
Design a mobile/tablet game title screen at 1024×768 pixels.

BACKGROUND: Full-bleed dark cave entrance scene. Use a radial gradient from #2D1F3D at center to #1C1226 at edges. Add subtle stalactite silhouettes along the top edge in #231730 — triangular shapes hanging down 40–80px, spaced irregularly. A faint warm glow (radial gradient, #F59E0B at 8% opacity) emanates from the center-bottom third of the screen, suggesting light from inside the cave.

LOGO (centered, upper third of screen):
- Primary text: "אוצר העשר" in Heebo Bold 48px, color #FBBF24 (gold), text-shadow: 0 2px 8px rgba(0,0,0,0.5). Right-to-left text. Centered horizontally.
- Subtitle below: "Treasure of Tens" in Heebo Regular 16px, color #D4B896, 8px below the Hebrew title. Centered.
- The logo group is positioned at Y=140 from the top.

CHARACTER (centered, middle of screen):
- A placeholder rectangle 200×240px, centered horizontally, top edge at Y=260.
- Inside, show a friendly child explorer character (age ~8-10, gender-neutral) wearing an adventure hat and holding a glowing lantern. Warm lighting from the lantern illuminates the character's face. Hand-painted children's book illustration style. The character stands at a cave entrance, looking at the viewer with a welcoming smile.
- The character area has no background — it sits directly on the cave gradient.

PLAY BUTTON (centered, below character):
- Rounded rectangle button: width 200px, height 56px, corner radius 8px.
- Background: #2563EB (bright blue).
- Shadow: 0 4px 12px rgba(0,0,0,0.4).
- Inside: A play triangle icon (12×14px, white #FFFFFF) on the right side (RTL — so visually on the right), then 8px gap, then text "!שחק" in Heebo SemiBold 24px, color #FFFFFF. Content centered in button.
- Button is centered horizontally, top edge at Y=540.

SPEAKER TOGGLE (bottom-right corner — RTL, so bottom-right is the "start" corner):
- A 44×44px circular touch target, positioned 48px from the right edge and 48px from the bottom edge.
- Inside: a speaker/volume icon, 24×24px, color #FEF3C7 (cream), centered in the circle.
- No visible background on the circle (transparent), just the icon.

Overall feel: Warm, inviting, magical. A child's first impression of a treasure-hunting adventure. The dark cave contrasts with the warm golden logo and the bright blue play button.
```

---

## Prompt 2: Progress Map

```
Design a game progress map screen at 1024×768 pixels. This is a cave-themed level selection map for a children's math game. RTL layout (Hebrew, right-to-left).

BACKGROUND: Solid #1C1226 (very dark purple-brown). Subtle vertical gradient overlay: slightly lighter (#231730) at the top, slightly darker (#150D1C) at the bottom, suggesting depth into a cave. Faint crystal/gem shapes in corners — small diamond shapes (8–12px) in #2563EB at 10% opacity and #DC2626 at 10% opacity, scattered decoratively.

TOP BAR (full width, height 56px, background #2D1F3D):
- Right side (RTL start): Speaker icon (24×24px, #FEF3C7) with 16px padding from right edge. Next to it (16px gap): gear/settings icon (24×24px, #FEF3C7).
- Left side (RTL end): Title text "פיצול אבני חן :1 עולם" (World 1: Splitting Gems) in Heebo SemiBold 20px, color #FEF3C7, 16px from left edge, vertically centered.
- Bottom border: 1px solid #3D2E52.

MAP PATH (main content area, below top bar):
The path winds vertically through the screen with 6 level nodes connected by curved lines.

Path line: 4px wide, color #6B7280 (gray), with smooth bezier curves connecting nodes.

The path starts from the right side (RTL) and winds like this:
- Row 1 (Y=140): Node 1 at X=780, Node 2 at X=540
- Row 2 (Y=280): Node 3 at X=300, Node 4 at X=540
- Row 3 (Y=420): Node 5 at X=780, Node 6 at X=540

Each path segment curves gently between nodes (use S-curves).

NODE DESIGNS (each node is 56×56px circle):

Nodes 1–2 (COMPLETED):
- Circle: 56×56px, background #10B981 at 20% opacity, border 3px solid #10B981.
- Inside: An open treasure chest icon (28×28px). The chest is brown (#8B4513) with gold trim (#FFD700), lid open.
- Small green checkmark badge (16×16px, bg #10B981, white ✓) overlapping the top-left of the circle.

Node 3 (CURRENT — next playable):
- Circle: 56×56px, background #2D1F3D, border 3px solid #FBBF24 (gold).
- Gold glow: box-shadow 0 0 20px rgba(251,191,36,0.6) — visible as a soft golden halo around the node.
- Inside: A closed treasure chest icon (28×28px), brown with gold lock, no checkmark.
- A small explorer character (40×48px) stands next to this node (to the right of it, 8px gap), indicating "you are here."

Nodes 4–6 (LOCKED):
- Circle: 56×56px, background #6B7280 at 30% opacity, border 3px solid #6B7280 at 50% opacity.
- Inside: A padlock icon (28×28px), color #6B7280.
- Overall opacity on the entire node: 0.5.

CAVE DECORATIONS (purely atmospheric, behind the path):
- 3–4 stalactite shapes hanging from the top bar area: CSS triangles in #231730, heights 30–60px, widths 20–40px, spaced unevenly.
- 2–3 small crystal cluster shapes near the edges: rotated squares (diamonds) 12–16px, in faint gem colors (#DC2626, #2563EB, #059669) at 15% opacity.

Overall feel: A winding underground cave path. Completed areas feel warm and accomplished (green). The current level glows invitingly. Locked levels are mysterious and waiting. The explorer character marks where the player is.
```

---

## Prompt 3: Gameplay Screen (World 2 — Making Tens)

```
Design a gameplay screen at 1024×768 pixels for a children's math game. The player is splitting groups of gems to make a group of exactly 10. RTL layout (Hebrew). This is the core interaction screen.

BACKGROUND: Solid #1C1226 (dark cave).

TOP BAR (full width, height 56px, background #2D1F3D):
- Right side (RTL start): Speaker icon (24×24px, #FEF3C7), 16px from right edge. Then 16px gap. Gear icon (24×24px, #FEF3C7). Then 16px gap. Back arrow icon (24×24px, #FEF3C7) — arrow points RIGHT (because RTL; this goes back to map).
- Left side (RTL end): Level label "2-3 שלב" (Level 2-3) in Heebo SemiBold 18px, color #FEF3C7, 16px from left edge.
- Bottom border: 1px solid #3D2E52.

GEM WORKSPACE (centered, main area from Y=72 to Y=540):
All gem groups are centered horizontally within a max-width of 600px area.

GEM CLUSTER 1 — "7 gems" (top of workspace, Y=100):
- Container: 320×100px, background #2D1F3D, border-radius 12px, padding 16px, centered horizontally.
- Inside: 7 circular gems in a line, each 40×40px with 2px gaps between them.
- First 4 gems are RUBY RED: each is a circle with radial-gradient(circle at 35% 35%, #F87171, #DC2626 60%, #991B1B). Border: 2px solid #991B1B. Shadow: 0 2px 4px rgba(0,0,0,0.3), inset 0 1px 2px rgba(255,255,255,0.2).
- A SPLITTER LINE between gem 4 and gem 5: a vertical line, 4px wide, #2563EB, height 72px (extends 16px above and below the gems). At the top and bottom of the line, a small circle (20×20px, #2563EB, shadow 0 4px 12px rgba(0,0,0,0.4)) acts as a grab handle.
- Last 3 gems are SAPPHIRE BLUE: radial-gradient(circle at 35% 35%, #60A5FA, #2563EB 60%, #1D4ED8). Border: 2px solid #1D4ED8. Same shadow as ruby gems.
- Below the ruby group: number "4" in Rubik Bold 32px, color #DC2626, centered under the 4 red gems.
- Below the sapphire group: number "3" in Rubik Bold 32px, color #2563EB, centered under the 3 blue gems.
- The split gap between the two groups is 24px wide.

GEM CLUSTER 2 — "5 gems" (below cluster 1, Y=230):
- Container: 240×100px, same styling as cluster 1, centered horizontally.
- 5 ruby red gems in a line (same gem style as above), no splitter.
- Below: number "5" in Rubik Bold 32px, color #DC2626, centered.

COMBINE ZONE (below clusters, Y=360):
- Container: 220×110px, centered horizontally.
- Background: #2D1F3D. Border: 2px dashed #6B7280. Border-radius: 12px. Padding: 12px.
- Inside: a 2×5 grid (2 rows, 5 columns) of faint placeholder circles. Each circle is 32×32px, border 1px dashed #6B7280 at 30% opacity, no fill. Grid gap: 4px.
- Below the grid, centered: text "10" in Rubik Bold 24px, color #6B7280 at 50% opacity.

NARRATOR BUBBLE (bottom-right area, Y=560):
- Bubble: max-width 380px, min-height 72px, background #FFFBEB (warm cream), border 2px solid #F59E0B, border-radius 12px, padding 16px. Positioned at X=580 (right-aligned for RTL).
- A triangular speech tail (12×8px, same cream color with amber border) points from the bottom-right of the bubble toward a character position.
- Inside text: "!בואו נפצל כדי ליצור קבוצה של 10" in Heebo Regular 24px, color #1C1226. RTL text alignment (right-aligned).
- To the right of the bubble (8px gap): a circular character avatar, 48×48px, showing the explorer character's face (same character from title screen, friendly expression). Border: 2px solid #F59E0B, border-radius 50%.

BOTTOM BAR (full width, height 64px, background #2D1F3D, anchored to bottom):
- Top border: 1px solid #3D2E52.
- Left side (RTL end): Equation placeholder "? = 5 + 7" in Rubik Medium 24px. The "7" is color #DC2626, "+" is color #D4B896, "5" is color #DC2626, "=" is color #D4B896, "?" is color #6B7280. Direction is LTR (dir="ltr"). Vertically centered, 16px from left edge.
- Right side (RTL start): Confirm button — rounded rectangle 120×44px, background #2563EB, border-radius 8px. Text "אשר" (Confirm) in Heebo SemiBold 20px, color #FFFFFF, centered. Shadow: 0 1px 3px rgba(0,0,0,0.3). Positioned 16px from right edge, vertically centered.

Overall feel: Focused gameplay. Dark cave background makes the colorful gems and blue splitter pop. The workspace is clean and uncluttered. The narrator provides guidance in a warm, friendly bubble. The equation bar at the bottom tracks the mathematical thinking.
```

---

## Prompt 4: Level Complete Screen

```
Design a level completion celebration screen at 1024×768 pixels for a children's math game. RTL layout (Hebrew). This screen appears after a student successfully completes a level.

BACKGROUND: Solid #1C1226 (dark cave). Overlay with a subtle radial gradient of gold — radial-gradient(circle at 50% 40%, rgba(251,191,36,0.12), transparent 70%) — creating a warm celebratory glow in the center.

GOLD PARTICLES (scattered across the screen):
- ~20 small circles (4–8px each) in gold tones (#FBBF24, #FDE68A, #F59E0B) scattered across the upper two-thirds of the screen. They should look like floating sparkles/confetti. Vary sizes and opacity (0.4–1.0). Some slightly overlapping the content. Place them at random positions but avoiding the center content area.

TREASURE CHEST (centered, Y=120):
- A treasure chest illustration, 120×100px, centered horizontally.
- The chest is OPEN: wooden box with brown (#8B4513) base, lighter brown (#D4A574) plank lines, gold (#FFD700) trim and hinges. The lid is rotated open (tilted back ~110 degrees). A warm golden glow (radial gradient #FBBF24 at 40% opacity) emanates from inside the chest. Small gem shapes (8px circles in #DC2626, #2563EB, #059669) peek out from inside.
- Below the chest: a soft gold glow shadow — box-shadow 0 0 40px rgba(251,191,36,0.4).

CELEBRATION HEADING (centered, Y=250):
- Text: "!מצאת את האוצר" (You found the treasure!) in Heebo Bold 36px, color #FBBF24 (gold). Text-shadow: 0 2px 8px rgba(0,0,0,0.5). Centered horizontally. RTL text.

EQUATION SUMMARY (centered, Y=320):
- A horizontal equation in LTR direction (dir="ltr"), centered:
- "7 + 5 → 10 + 2 = 12"
- Each number uses its gem color: "7" in #DC2626, "5" in #DC2626, "10" in #F59E0B (gold, representing the Power Gem), "2" in #DC2626, "12" in #FEF3C7 (cream, the final answer — slightly larger).
- Operators (+, →, =) in #D4B896 (secondary text color).
- Font: Rubik Bold 32px for numbers, Rubik Medium 24px for operators. The final "12" is Rubik Bold 48px.
- The entire equation sits on a subtle surface: background #2D1F3D, border-radius 8px, padding 12px 24px.

POWER GEM DISPLAY (centered, Y=400):
- A large gold gem circle, 56×56px, centered horizontally.
- Background: radial-gradient(circle at 35% 35%, #FDE68A, #F59E0B 60%, #B45309). Border: 2px solid #B45309.
- Inside: "10" in Rubik Bold 28px, color #1C1226, centered.
- Glow: box-shadow 0 0 20px rgba(251,191,36,0.6).
- Next to it (16px gap, to the right): "2" in Rubik Bold 32px, color #DC2626, with two small red gem circles (20×20px) beside it.

NARRATOR BUBBLE (centered, Y=480):
- Bubble: max-width 360px, background #FFFBEB, border 2px solid #F59E0B, border-radius 12px, padding 12px 16px. Centered horizontally.
- Text: "שבע ועוד חמש שווה שתים עשרה!" in Heebo Regular 20px, color #1C1226. RTL.
- Character avatar (48×48px circle) to the right of the bubble (RTL), same explorer face as other screens.

ACTION BUTTONS (centered, Y=600, arranged horizontally with 16px gaps):
Three buttons in a row, centered horizontally:

Button 1 (leftmost — "Next Level", PRIMARY):
- 140×48px, background #10B981 (green/success), border-radius 8px, shadow 0 4px 12px rgba(0,0,0,0.4).
- Inside: play triangle icon (12×14px, white) on the right (RTL), 8px gap, text "הבא" (Next) in Heebo SemiBold 20px, white #FFFFFF.

Button 2 (center — "Back to Map", SECONDARY):
- 140×48px, background transparent, border 2px solid #2563EB, border-radius 8px.
- Inside: map icon (20×20px, #2563EB) on the right (RTL), 8px gap, text "מפה" (Map) in Heebo SemiBold 20px, color #2563EB.

Button 3 (rightmost — "Replay", SECONDARY):
- 140×48px, background transparent, border 2px solid #2563EB, border-radius 8px.
- Inside: circular replay arrow icon (20×20px, #2563EB) on the right (RTL), 8px gap, text "שוב" (Again) in Heebo SemiBold 20px, color #2563EB.

Overall feel: Joyful celebration! Gold everywhere — particles, glow, the power gem. The dark background makes the gold pop. The child should feel proud and accomplished. The equation summary reinforces the math they just did. The "Next" button is prominently green to encourage continuation.
```

---

## Prompt 5: Settings Overlay

```
Design a settings overlay/modal for a children's math game at 1024×768 pixels. The overlay appears on top of any game screen. RTL layout (Hebrew).

BACKDROP:
- The entire 1024×768 canvas is covered by a semi-transparent overlay: background rgba(0,0,0,0.6). This darkens whatever screen is behind it.

SETTINGS CARD (centered on screen):
- Rectangle: 320×220px, centered both horizontally (X=352) and vertically (Y=274).
- Background: #3D2E52 (elevated surface color).
- Border-radius: 16px.
- Shadow: 0 8px 24px rgba(0,0,0,0.5).

CLOSE BUTTON (top-left corner of card — RTL end):
- A 44×44px circular touch target, positioned 8px from the top-left corner of the card.
- Inside: an X icon, 20×20px, color #FEF3C7 (cream), centered.
- No visible background (transparent).

TITLE (top of card, below close button area):
- Text: "הגדרות" (Settings) in Heebo SemiBold 28px, color #FEF3C7. Right-aligned (RTL), 24px from the right edge of the card, Y=24 from the top of the card.

TOGGLE ROW 1 — Narration (Y=90 inside card):
- Full width of card minus 24px padding on each side (272px wide).
- Right side (RTL start): Label "קריינות" (Narration) in Heebo Regular 20px, color #FEF3C7.
- Left side (RTL end): Toggle switch.
  - Toggle track: 48×28px, border-radius 9999px (fully rounded/pill shape), background #10B981 (green, indicating ON state).
  - Toggle thumb: 24×24px circle, #FFFFFF, positioned at the LEFT end of the track (ON position in LTR/visual terms), shadow 0 1px 3px rgba(0,0,0,0.3).
  - 2px padding inside the track around the thumb.
- The label and toggle are vertically aligned to center.

TOGGLE ROW 2 — Sound Effects (Y=150 inside card):
- Same layout as Row 1.
- Right side: Label "אפקטי קול" (Sound Effects) in Heebo Regular 20px, color #FEF3C7.
- Left side: Toggle switch, same dimensions. This one shows the OFF state:
  - Track background: #6B7280 (gray).
  - Thumb positioned at the RIGHT end of the track (OFF position).

DIVIDER between rows:
- A 1px line, color #2D1F3D (darker than card bg), spanning the width of the card minus 24px padding on each side. Positioned at Y=120 inside the card.

Overall feel: Simple, clean, minimal. Just two toggles. The dark purple card floats above the dimmed game screen. Large touch targets for small fingers. Clear Hebrew labels. The green ON toggle is immediately recognizable, the gray OFF toggle is clearly different.
```

---

## Usage Notes for Figma Make

1. **Frame size:** All prompts use 1024×768 (landscape tablet). Adjust to 768×1024 for portrait iPad if needed.

2. **Font substitution:** If Figma doesn't have Heebo/Rubik loaded, use these substitutes:
   - Heebo → "Assistant" or "Noto Sans Hebrew" (both support Hebrew + niqqud)
   - Rubik → "Inter" or system sans-serif for numerals

3. **RTL rendering:** Figma Make may not perfectly handle RTL text flow. After generation, manually set text layers to RTL alignment and verify Hebrew text reads correctly right-to-left.

4. **Gem rendering:** The gradient specs for gems are CSS syntax. In Figma, recreate as:
   - Fill: Radial gradient, focal point at 35% from top-left
   - Color stops: light variant at 0%, base color at 60%, dark variant at 100%

5. **Iteration order:** Generate in this order: Title Screen → Gameplay → Level Complete → Progress Map → Settings. The Gameplay screen establishes the core visual language that other screens reference.

---

*Document version: 1.0*
*Last updated: 2026-03-14*
*Author: UX-Design Agent*
