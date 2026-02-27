# Prioritized Backlog

## Progress

| Phase | Total | Done | In Progress | Not Started |
|-------|-------|------|-------------|-------------|
| Pilot | 21 | 0 | 0 | 21 |
| V1 | 10 | 0 | 0 | 10 |
| V2+ | 7 | 0 | 0 | 7 |

## Pilot (MVP)

| # | Priority | Item | PRD Section | Complexity | Dependencies | Status |
|---|----------|------|-------------|------------|--------------|--------|
| 36 | P0 | Design system definition (color palette, typography, spacing, component tokens) | 8 | M | Architecture | Not Started |
| 37 | P0 | Screen designs for all Pilot screens (gameplay, progress map, title, level complete, settings) | 8.3 | L | #36 | Not Started |
| 38 | P0 | Asset manifest and gem sprite generation specs | 8 | M | #36 | Not Started |
| 1 | P0 | Gem-splitting core mechanic (drag-to-split, animate, display equation) | 4.1 | L | Architecture, #37 | Not Started |
| 2 | P0 | Audio narration system (Hebrew TTS, speaker toggle, autoplay handling) | 4.9 | M | Architecture | Not Started |
| 3 | P0 | Hebrew language UI and RTL layout (all text, menus, navigation in Hebrew RTL; math notation stays LTR) | 1.1, 8.1 | M | Architecture, #36 | Not Started |
| 4 | P0 | i18n framework setup (externalize all UI strings and narration text into locale files; Hebrew as default locale) | 3.3 | M | Architecture | Not Started |
| 5 | P1 | World 1 — "Learning to Split" (5–6 levels, free exploration of splits) | 4.2 | M | #1 | Not Started |
| 6 | P1 | World 2 — "Making Tens" (8–10 levels, combine splits to make 10, Power Gem animation) | 4.3 | L | #1 | Not Started |
| 7 | P2 | World 3 — "Treasure Chest Challenge" (6–8 levels, multi-addend decomposition) | 4.4 | L | #1, #6 | Not Started |
| 8 | P2 | Progress map (cave-themed level map, completion tracking, localStorage persistence) | 4.7 | M | None | Not Started |
| 9 | P2 | Hint system — Tier 1 (ghost splitter after 2 failed attempts) | 4.8 | S | #1 | Not Started |
| 10 | P3 | Celebration & feedback animations (Power Gem fusion, treasure chest open, error bounce-back) | 4.1, 4.3 | M | #1 | Not Started |
| 11 | P3 | Hebrew narration scripts (write all narrator dialogue, feedback messages, and equation read-aloud text in Hebrew) | 4.9, 8.4 | M | #2 | Not Started |
| 12 | P3 | Hebrew font selection (sans-serif font with full niqqud/vowel diacritics support for early readers) | 8.1 | S | None | Not Started |
| 13 | P3 | Title screen + settings overlay (play button, audio toggles) | 8.3 | S | None | Not Started |
| 14 | P3 | Level complete screen (equation summary, replay/next/map buttons) | 8.3 | S | #1 | Not Started |
| 15 | P3 | 10-frame gem layout for World 2 | 4.3 | S | #1 | Not Started |
| 16 | P4 | Basic anonymous analytics event logging (level_start, level_complete, split_action, hint_triggered) | 5.5 | M | None | Not Started |
| 17 | P4 | Cross-browser testing (Chrome on Chromebook + Safari on iPad) | 5.1, 9.1 | M | All Pilot items | Not Started |
| 18 | P4 | WCAG 2.1 AA accessibility pass (aria-labels, keyboard nav, color contrast) | 5.3 | M | All Pilot items | Not Started |

## V1

| # | Priority | Item | PRD Section | Complexity | Dependencies | Status |
|---|----------|------|-------------|------------|--------------|--------|
| 19 | P1 | World 4 — "The Deep Mine" (place value decomposition, gem pouches, 8–10 levels) | 4.5 | L | #1, #6 | Not Started |
| 20 | P1 | World 5 — "The Cave Exit" (subtraction decomposition, 6–8 levels) | 4.6 | L | #1, #19 | Not Started |
| 21 | P1 | Teacher dashboard (class roster, per-student progress, hotspot levels) | 4.10 | L | #16 | Not Started |
| 22 | P2 | Student profiles (teacher-created, pseudonym-based, server-side progress sync) | 4.10 | M | #21 | Not Started |
| 23 | P2 | Adaptive difficulty (challenge variants after 3 clean completions; scaffolded variants after 2 failures) | 3.2 | M | All worlds | Not Started |
| 24 | P2 | Hint system — Tier 2 (full animated walkthrough after 4 failed attempts) | 4.8 | M | #9 | Not Started |
| 25 | P3 | CRA transition: pictorial dot arrays replacing gems in later World 4/5 levels | 4.5 | M | #19 | Not Started |
| 26 | P3 | CRA transition: abstract number notation with caret marks in final levels | 3.2 | M | #25 | Not Started |
| 27 | P2 | English language localization (UI strings, LTR layout, English narration scripts and TTS) | 3.2 | M | #4, #2 | Not Started |
| 28 | P3 | Explorer character customization (skin tone selection) | 8.1 | S | None | Not Started |

## V2 / Future

| # | Priority | Item | PRD Section | Complexity | Dependencies | Status |
|---|----------|------|-------------|------------|--------------|--------|
| 29 | -- | Additional strategies: compensation, adding up, constant difference | 3.3 | L | V1 complete | Not Started |
| 30 | -- | Number talk classroom mode (teacher poses problem, students submit decomposition approaches) | 3.3 | L | #21 | Not Started |
| 31 | -- | Parent dashboard | 3.3 | M | #21 | Not Started |
| 32 | -- | Offline mode (service worker) | 3.3 | M | All | Not Started |
| 33 | -- | Spanish language localization (UI strings, narration scripts, Spanish TTS) | 3.3 | M | #4, #2, #27 | Not Started |
| 34 | -- | Google Classroom roster sync | 7 | M | #21 | Not Started |
| 35 | -- | LTI 1.3 LMS integration | 7 | L | #21 | Not Started |
