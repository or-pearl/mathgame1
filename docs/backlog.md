# Prioritized Backlog

## Pilot (MVP)

| Priority | Item | PRD Section | Phase | Complexity | Source Dependency | Dependencies | Status |
|----------|------|-------------|-------|------------|-------------------|--------------|--------|
| P0 | Gem-splitting core mechanic (drag-to-split, animate, display equation) | 4.1 | Pilot | L | Four Essential Mental Math Strategies | None | Not Started |
| P0 | Audio narration system (TTS, speaker toggle, autoplay handling) | 4.9 | Pilot | M | PDB Constraint (emerging reading skills) | None | Not Started |
| P1 | World 1 — "Learning to Split" (5–6 levels, free exploration of splits) | 4.2 | Pilot | M | Strategic Activities (extended practice) | 4.1 | Not Started |
| P1 | World 2 — "Making Tens" (8–10 levels, combine splits to make 10, Power Gem animation) | 4.3 | Pilot | L | Four Essential Mental Math Strategies (friendly numbers) | 4.1 | Not Started |
| P2 | World 3 — "Treasure Chest Challenge" (6–8 levels, multi-addend decomposition) | 4.4 | Pilot | L | Four Essential Mental Math Strategies (multi-addend example) | 4.1, 4.3 | Not Started |
| P2 | Progress map (cave-themed level map, completion tracking, localStorage persistence) | 4.7 | Pilot | M | PDB (engagement) | None | Not Started |
| P2 | Hint system — Tier 1 (ghost splitter after 2 failed attempts) | 4.8 | Pilot | S | Five Common Mistakes (scaffolding) | 4.1 | Not Started |
| P3 | Celebration & feedback animations (Power Gem fusion, treasure chest open, error bounce-back) | 4.1, 4.3 | Pilot | M | None | 4.1 | Not Started |
| P3 | Title screen + settings overlay (play button, audio toggles) | 8.3 | Pilot | S | None | None | Not Started |
| P3 | Level complete screen (equation summary, replay/next/map buttons) | 8.3 | Pilot | S | None | 4.1 | Not Started |
| P3 | 10-frame gem layout for World 2 | 4.3 | Pilot | S | Strategic Activities (10-frame scaffold) | 4.1 | Not Started |
| P4 | Basic anonymous analytics event logging (level_start, level_complete, split_action, hint_triggered) | 5.5 | Pilot | M | None | None | Not Started |
| P4 | Cross-browser testing (Chrome on Chromebook + Safari on iPad) | 5.1, 9.1 | Pilot | M | PDB Constraint (device support) | All Pilot items | Not Started |
| P4 | WCAG 2.1 AA accessibility pass (aria-labels, keyboard nav, color contrast) | 5.3 | Pilot | M | PDB Constraint (WCAG) | All Pilot items | Not Started |

## V1

| Priority | Item | PRD Section | Phase | Complexity | Source Dependency | Dependencies | Status |
|----------|------|-------------|-------|------------|-------------------|--------------|--------|
| P1 | World 4 — "The Deep Mine" (place value decomposition, gem pouches, 8–10 levels) | 4.5 | V1 | L | Four Essential Mental Math Strategies (place value method) | 4.1, 4.3 | Not Started |
| P1 | World 5 — "The Cave Exit" (subtraction decomposition, 6–8 levels) | 4.6 | V1 | L | Four Essential Mental Math Strategies (subtraction decomposition) | 4.1, 4.5 | Not Started |
| P1 | Teacher dashboard (class roster, per-student progress, hotspot levels) | 4.10 | V1 | L | PDB JTBD #5 | Analytics pipeline | Not Started |
| P2 | Student profiles (teacher-created, pseudonym-based, server-side progress sync) | 4.10 | V1 | M | PDB Constraint (COPPA/FERPA) | Teacher dashboard | Not Started |
| P2 | Adaptive difficulty (challenge variants after 3 clean completions; scaffolded variants after 2 failures) | 3.2 | V1 | M | Challenge of Teaching Math (diverse skill levels) | All worlds | Not Started |
| P2 | Hint system — Tier 2 (full animated walkthrough after 4 failed attempts) | 4.8 | V1 | M | Five Common Mistakes (scaffolding) | Hint Tier 1 | Not Started |
| P3 | CRA transition: pictorial dot arrays replacing gems in later World 4/5 levels | 4.5 | V1 | M | Five Common Mistakes (CRA framework) | World 4 | Not Started |
| P3 | CRA transition: abstract number notation with caret marks in final levels | 3.2 | V1 | M | Four Essential Mental Math Strategies (caret notation) | CRA pictorial | Not Started |
| P3 | Explorer character customization (skin tone selection) | 8.1 | V1 | S | None | None | Not Started |

## V2 / Future

| Priority | Item | PRD Section | Phase | Complexity | Source Dependency | Dependencies | Status |
|----------|------|-------------|-------|------------|-------------------|--------------|--------|
| -- | Additional strategies: compensation, adding up, constant difference | 3.3 | V2 | L | Four Essential Mental Math Strategies (strategies 2–4) | V1 complete | Not Started |
| -- | Number talk classroom mode (teacher poses problem, students submit decomposition approaches) | 3.3 | V2 | L | Four Essential Mental Math Strategies (number talks) | Teacher dashboard | Not Started |
| -- | Parent dashboard | 3.3 | V2 | M | PDB Persona (parent) | Teacher dashboard | Not Started |
| -- | Offline mode (service worker) | 3.3 | V2 | M | PDB Constraint (connectivity) | All | Not Started |
| -- | Spanish language support | 3.3 | V2 | M | None | Audio narration system | Not Started |
| -- | Google Classroom roster sync | 7 | V2 | M | None | Teacher dashboard | Not Started |
| -- | LTI 1.3 LMS integration | 7 | V2 | L | None | Teacher dashboard | Not Started |
| -- | Sandbox / free-play mode | 3.3 | V2 | M | None | Core mechanic | Not Started |
