# Product Requirements Document

## 1. Overview

### 1.1 Product Summary

**Treasure of Tens** is a browser-based math adventure game that teaches first-grade students (ages 6–7) the **decomposition strategy** for mental math — the ability to break numbers apart and recombine them to make "friendly numbers" (multiples of 10, doubles, near-doubles) that are easier to add or subtract. Players take on the role of a young treasure hunter exploring a gem-filled cave. Numbers are represented as **physical groups of gems** that students split apart by dragging a divider, then recombine into groups of 10 ("Power Gems") to unlock treasure chests, activate bridges, and progress through the adventure. The game progresses from concrete gem manipulation through pictorial representations to abstract number notation, following the research-backed Concrete-Representational-Abstract (CRA) framework.

**Target Market & Language:** The initial release targets the Israeli market in **Hebrew** (right-to-left UI). The first pilot will be conducted with a first-grade class in Israel. Upon successful validation, the game will be localized for **English** and **Spanish** speaking markets.

### 1.2 Problem Reference

**PDB:** `/docs/pdb.md`

First-grade students struggle to develop mental math fluency because traditional instruction moves too quickly from abstract symbols to procedural drilling, skipping the concrete and pictorial stages where genuine number sense is built. Decomposition — identified as "one of the most important strategies to teach" [Source: Four Essential Mental Math Strategies] — is particularly hard to learn from worksheets alone because it requires students to see numbers as flexible, breakable quantities rather than fixed symbols.

### 1.3 Success Metrics

| Metric | Target | Measurement Method | Source / Basis | Phase |
|--------|--------|--------------------|----------------|-------|
| Activity completion rate | ≥ 75% of started levels are completed (not abandoned) | Analytics: level_start vs. level_complete events | PDB Success Criteria #8 | Pilot |
| Decomposition accuracy | ≥ 70% of split decisions produce valid friendly-number combinations by end of World 2 | In-game scoring: track whether student-chosen splits lead to a 10 or doubles combination | Inferred from PDB Success Criteria #9 | Pilot |
| Error recovery rate | ≥ 60% of students who make an incorrect split succeed on the retry | Analytics: incorrect_split → next_attempt outcome | PDB Success Criteria #9 | Pilot |
| Voluntary replay | ≥ 40% of students replay at least one completed level within a session | Analytics: replay events | PDB Success Criteria #2 (engagement proxy) | Pilot |
| Session duration | Average 6–10 minutes per session | Analytics: session timestamps | PDB Constraint: 5–10 min attention span | Pilot |
| Conceptual explanation | ≥ 50% of pilot students can verbally explain a decomposition strategy when prompted by teacher | Teacher observation rubric (provided) | PDB Success Criteria #1 | V1 |
| Teacher adoption | ≥ 80% of pilot teachers use the tool ≥ 3×/week for 6 weeks | Usage analytics per teacher account | PDB Success Criteria #6 | V1 |

---

## 2. Reference Sources

| # | Source | Type | How It Affects This PRD |
|---|--------|------|------------------------|
| 1 | Four Essential Mental Math Strategies for Early Elementary Grades | Pedagogical / Research | Defines the decomposition strategy (friendly numbers and place value methods), provides worked examples, and establishes decomposition as "one of the first strategies I teach." Core game mechanic derives from this source. |
| 2 | Strategic Activities for Addition and Subtraction Within 20 | Pedagogical / Research | Recommends 4–5 days of focused practice per strategy before introducing alternatives; emphasizes hands-on exploration before abstract solving; provides game formats (bump games, spinners). Informs pacing and scaffolding design. |
| 3 | Five Common Mistakes in Elementary Math Instruction | Pedagogical / Research | Warns against skipping concrete phase, teaching too many strategies simultaneously, and prioritizing procedures over concepts. Informs what to avoid in game design. |
| 4 | First Grade Guide to Double-Digit Addition and Subtraction | Pedagogical / Research | Provides progression from single-digit to double-digit operations; emphasizes place value understanding and 10-frames. Informs World 4 level design. |
| 5 | Three Dynamic Math Review Activities for K-2 Students | Pedagogical / Research | Provides game-based review formats (races, partner activities). Informs engagement mechanics. |
| 6 | The Challenge of Teaching Math to First Graders | Pedagogical / Research | Documents real-world classroom challenges: diverse skill levels, limited attention, need for repetition with variety. Informs adaptive difficulty and session length. |

---

## 3. Scope & Phasing

### 3.1 Pilot Scope (MVP)

The Pilot is a **single-strategy, addition-only, single-digit game** that proves students can learn decomposition through interactive gem-splitting.

**IN:**
- Core gem-splitting mechanic (drag-to-split groups of gems)
- World 1: Splitting single-digit numbers (decompose 5, 6, 7, 8, 9 into two parts)
- World 2: Making Tens — combine splits from two numbers to form a group of 10
- World 3: Multi-addend friendly numbers (3–4 single-digit addends, find combinations that make 10)
- Visual gem representations (concrete) with transition to dot arrays (pictorial)
- Audio narration for all instructions (minimal reading required)
- Animated feedback: correct splits trigger gem-glow + treasure-chest unlock; incorrect splits trigger gentle "try again" with visual hint
- 15–20 levels across 3 worlds, each completable in ≤ 2 minutes
- Basic progress map showing completed levels
- Works on Chrome (Chromebook) and Safari (iPad) browsers
- **Hebrew language UI and narration** (right-to-left layout, Hebrew text, Hebrew TTS/audio)

**OUT (deferred to V1 or V2):**
- Subtraction decomposition
- Two-digit number decomposition (place value method)
- Teacher dashboard
- Student accounts / login
- Adaptive difficulty engine
- Parent-facing features
- Offline mode

### 3.2 V1 Scope

Completes the core decomposition experience, adds teacher visibility, and begins international expansion.

- World 4: Two-digit decomposition by place value (e.g., 47 = 40 + 7) using gem pouches (bundles of 10) and loose gems
- World 5: Subtraction decomposition (break apart the subtrahend to subtract in friendly steps)
- Teacher dashboard: class roster, per-student progress through worlds, common error patterns
- Student profiles (teacher-created, no student PII beyond first name / pseudonym)
- Adaptive difficulty: if a student completes 3 levels in a row with no errors, offer a "challenge" variant; if a student fails 2 levels in a row, offer a scaffolded "helper" variant
- Transition from pictorial (dot arrays) to abstract (number notation with caret decomposition marks) in later levels
- Hint system: after 2 failed attempts, show a ghost outline suggesting one valid split
- **English language localization** (UI, narration, and LTR layout) for English-speaking markets

### 3.3 V2 / Future Scope

- Additional strategies: compensation, adding up, constant difference (per source material)
- Number talk mode: multiplayer/classroom mode where teacher poses a problem and students submit their decomposition approach, then class reviews together
- Parent dashboard with home practice recommendations
- Offline mode (service worker caching)
- **Spanish language localization** (UI, narration, and LTR layout) for Spanish-speaking markets
- Sandbox / free-play mode for open-ended exploration
- Integration with Google Classroom for roster sync

### 3.4 Explicit Exclusions

- **Not a general math app.** This game teaches decomposition only. It does not cover counting, geometry, measurement, time, or money.
- **Not a drill tool.** The game does not present timed fact-recall exercises. Speed is never a success criterion for students.
- **Not a replacement for teacher instruction.** The game supplements number talks — it does not replace the teacher's role in naming and discussing strategies.
- **No competitive leaderboards.** No student-vs-student ranking. Progress is personal.
- **No advertising or in-app purchases.** The product is not monetized through ads.

---

## 4. Functional Requirements

### 4.1 Gem-Splitting Mechanic (Core Interaction)

**User Story:** As a student, I want to split a group of gems into two smaller groups by choosing where to divide them, so that I can see how numbers break apart into pieces.

**Description:** The central game mechanic. A number N is displayed as a visual cluster of N gems arranged in a line or 10-frame layout. The student drags a "splitter" (a glowing divider line) to a position within the cluster, dividing it into two groups. The two resulting groups animate apart, and their quantities are displayed. The student can reposition the splitter to try different splits before confirming.

**Source Basis:** Decomposition strategy from "Four Essential Mental Math Strategies" — "students understand how to break apart numbers flexibly." CRA framework from "Five Common Mistakes" — start with concrete manipulatives.

**Acceptance Criteria:**
- [ ] Gems are displayed as a visual cluster where each gem is individually distinguishable and countable
- [ ] Student can drag a splitter to any valid position within the cluster (N-1 possible split points for N gems)
- [ ] The two resulting group sizes are displayed as numerals next to each group after splitting [Source: CRA — pictorial + abstract labeling]
- [ ] Student can reposition the splitter freely before confirming (no penalty for exploration)
- [ ] A "confirm split" action (tap a button or drag a group to a target zone) locks the choice
- [ ] Splitting animation completes in ≤ 0.5 seconds
- [ ] Each gem group uses a distinct color to visually reinforce the two parts
- [ ] The original number and the equation (e.g., "8 = 5 + 3") are displayed after confirmation [Source: Four Essential Mental Math Strategies — "write it out... names the strategy and shows students what it looks like"]

**Edge Cases:**
- Student drags splitter to the far edge (resulting in N + 0 split): treat as valid but gently prompt "Can you split into two groups that both have gems?"
- Student attempts to split a group of 1: not applicable — minimum splittable group is 2

**Dependencies:** None (foundational mechanic).

**Phase:** Pilot

---

### 4.2 World 1 — "Learning to Split"

**User Story:** As a student, I want to practice breaking single numbers into two parts, so that I can understand that one number can be expressed as two smaller numbers added together.

**Description:** Introductory world with 5–6 levels. Each level presents a single number (5 through 9) as a gem cluster. The student's goal is simply to split it into two groups. All valid splits are accepted and celebrated. The purpose is to build fluency with the splitting mechanic and internalize that numbers are "breakable."

**Source Basis:** "Strategic Activities for Addition and Subtraction Within 20" — students need 4–5 days of focused practice with a single concept. "Five Common Mistakes" — allow extended exploration before introducing goals.

**Acceptance Criteria:**
- [ ] World 1 contains 5–6 levels, one for each number from 5 through 9 (and optionally 10)
- [ ] Any valid two-part split is accepted as correct (no single "right answer")
- [ ] After each split, the equation is read aloud by the narrator (e.g., "Eight is five plus three!") [Source: Four Essential Mental Math Strategies — math talk / verbalizing strategy]
- [ ] After completing a split, student is encouraged to try a different split of the same number ("Can you find another way?")
- [ ] Each level ends after the student has found at least 2 different valid splits
- [ ] A brief celebration animation plays upon level completion (gems sparkle, treasure chest glints)
- [ ] No time pressure; no score; no failure state

**Edge Cases:**
- Student produces the same split twice: gently prompt "You already found that one! Try moving the splitter somewhere else."

**Dependencies:** 4.1 Gem-Splitting Mechanic.

**Phase:** Pilot

---

### 4.3 World 2 — "Making Tens"

**User Story:** As a student, I want to split numbers and combine parts to make a group of exactly 10, so that I can learn to find "friendly numbers" that make addition easier.

**Description:** Core decomposition-for-addition world. Each level presents two numbers (e.g., 7 and 5). Both are shown as gem clusters. The student must split one or both numbers and drag parts together to form a group of exactly 10. When 10 gems merge, they transform into a glowing "Power Gem" with a satisfying animation, and the remaining gems form the ones. The level then shows the equation: e.g., "7 + 5 → 10 + 2 = 12."

**Source Basis:** "Four Essential Mental Math Strategies" — Decomposition Method A (Friendly Numbers): "to make a friendly number they might decompose this 5 into a 4 and a 1. So now they have 16 + 4 = 20." The game uses 10 as the primary friendly number target for single-digit work. Also: "Strategic Activities" — 10-frames as key scaffold.

**Acceptance Criteria:**
- [ ] Each level presents two single-digit numbers (sums between 11 and 18) as gem clusters
- [ ] Student can split either or both gem clusters using the splitting mechanic
- [ ] Student can drag a sub-group from one cluster onto another to combine them
- [ ] When exactly 10 gems are combined into one group, they fuse into a "Power Gem" with a distinct glow animation and sound effect
- [ ] The remaining gems (ones) are shown beside the Power Gem
- [ ] The completed equation is displayed and narrated: "[A] + [B] = 10 + [remainder] = [sum]" [Source: Four Essential Mental Math Strategies — "names the strategy and shows students what it looks like"]
- [ ] If student creates a group that is not 10, a gentle visual cue indicates "not quite 10 yet" (group does not glow; a faint "10" outline shows the target)
- [ ] World 2 contains 8–10 levels with increasing variety of number pairs
- [ ] Gem clusters are arranged in 10-frame layout (2×5 grid) to scaffold counting to 10 [Source: "Strategic Activities" — 10-frames as visual scaffold]

**Edge Cases:**
- Student combines gems into a group > 10: group flashes gently and narrator says "That's more than 10 — try splitting off some gems first."
- Student solves correctly without decomposing (e.g., counts directly): accept the answer but do not award the Power Gem bonus; narrator says "Great job! Can you also try making a group of 10?"

**Dependencies:** 4.1 Gem-Splitting Mechanic.

**Phase:** Pilot

---

### 4.4 World 3 — "Treasure Chest Challenge"

**User Story:** As a student, I want to solve multi-addend problems by finding friendly number combinations within 3–4 numbers, so that I can practice flexible decomposition with more complex problems.

**Description:** Each level presents 3–4 single-digit numbers as separate gem clusters (mirroring the source's example: 9 + 7 + 5 + 2). The student must find pairs or splits that combine to make 10s, then add the remaining gems. Each group of 10 produces a Power Gem that fills a slot on a treasure chest. When all Power Gem slots are filled and remaining gems are accounted for, the chest opens.

**Source Basis:** "Four Essential Mental Math Strategies" — Example 1: "9 + 7 + 5 + 2... decompose this 5 into a 4 and a 1. So now they have 16 + 4 = 20." Also Method B (doubles): "5 + 2 = 7. So in their head they're doing 9 + 7 + 7."

**Acceptance Criteria:**
- [ ] Each level presents 3 or 4 single-digit numbers (6–9 range) as gem clusters laid out horizontally [Source: Four Essential Mental Math Strategies — "notice how I wrote it horizontally; that's important... to discourage from the algorithm"]
- [ ] Student can split any cluster and drag sub-groups to combine with any other cluster or sub-group
- [ ] Multiple valid decomposition paths are accepted (e.g., for 9+7+5+2, both the "make 10 from 9+1" path and the "doubles 7+7" path are valid) [Source: Four Essential Mental Math Strategies — "both get us the same answer, both using decomposition, just different ways"]
- [ ] When a group of exactly 10 is formed, it becomes a Power Gem and fills a treasure chest slot
- [ ] After all gems are placed (into Power Gem slots or a "remaining gems" tray), the total is displayed and narrated
- [ ] The level shows the student's decomposition path as an equation sequence (e.g., "9 + 7 = 16, split 5 into 4 + 1, 16 + 4 = 20, 20 + 1 + 2 = 23")
- [ ] World 3 contains 6–8 levels, progressing from 3-addend to 4-addend problems
- [ ] Problems are selected so that at least one "make 10" combination exists

**Edge Cases:**
- Student combines all gems without making any 10s (just piles everything together): accept the correct total but prompt "You got the right answer! Can you try finding a group of 10 in there?"
- No clean pair makes 10 without splitting: ensure level design always includes at least one split-to-10 path

**Dependencies:** 4.1 Gem-Splitting Mechanic, 4.3 World 2.

**Phase:** Pilot

---

### 4.5 World 4 — "The Deep Mine" (Place Value Decomposition)

**User Story:** As a student, I want to break two-digit numbers into tens and ones using gem pouches, so that I can learn to decompose numbers by place value for bigger addition problems.

**Description:** Introduces two-digit numbers. Tens are represented as sealed "gem pouches" (each containing exactly 10 gems), and ones as loose gems. The student decomposes two-digit numbers by sorting them into pouches and loose gems (e.g., 47 = 4 pouches + 7 loose), then combines pouches with pouches and loose with loose to add.

**Source Basis:** "Four Essential Mental Math Strategies" — Decomposition Method B (Place Value): "47 is actually 40 + 7. And 19 is 10 + 9. I can take the 40 and the 10 to make 50. I know 7 + 9 = 16." Also: "First Grade Guide to Double-Digit Addition and Subtraction" — place value understanding with base-10 representations.

**Acceptance Criteria:**
- [ ] Two-digit numbers (11–50 range for V1) are represented as gem pouches (tens) + loose gems (ones)
- [ ] Student can open/seal pouches to verify each contains 10 gems (tap to peek inside)
- [ ] For an addition problem like 47 + 19, student separates both numbers into pouches + loose gems
- [ ] Student combines pouches together (tens + tens) and loose gems together (ones + ones)
- [ ] If the loose gem group reaches 10, it automatically bundles into a new pouch with a celebration animation
- [ ] The decomposition equation is displayed step-by-step: "47 + 19 → 40 + 7 + 10 + 9 → 50 + 16 → 50 + 10 + 6 → 66" [Source: Four Essential Mental Math Strategies — place value method notation]
- [ ] World 4 contains 8–10 levels, beginning with "teen + single-digit" and progressing to "two-digit + two-digit"
- [ ] Narration explains the strategy in kid-friendly language: "Let's split these into bags of ten and leftovers!"

**Edge Cases:**
- Student tries to add loose gems to a pouch that already has 10: pouch bounces back; narrator says "That pouch is full! Start a new one."
- Sum exceeds 50 (V1 range): ensure level design keeps sums ≤ 50 for V1; extend to 99 in V2.

**Dependencies:** 4.1 Gem-Splitting Mechanic, 4.3 World 2.

**Phase:** V1

---

### 4.6 World 5 — "The Cave Exit" (Subtraction Decomposition)

**User Story:** As a student, I want to decompose the number being subtracted into friendly parts so I can subtract step-by-step without regrouping, so that subtraction feels less scary and more manageable.

**Description:** Presents subtraction problems. The subtrahend is shown as a gem cluster that the student decomposes into parts that are easier to subtract sequentially. For example, 64 - 28: student splits 28 into 4 + 24, then subtracts 4 from 64 (= 60), then subtracts 24 from 60 (or further splits 24 into 20 + 4).

**Source Basis:** "Four Essential Mental Math Strategies" — Subtraction decomposition example: "break this 28 into 24 and 4. 64 - 4 = 60. Minus the 20 to make 40. Then 40 - 4 = 36."

**Acceptance Criteria:**
- [ ] Each level presents a subtraction problem with minuend (gem hoard) and subtrahend (gem cluster to remove)
- [ ] Student decomposes the subtrahend into 2–3 parts using the splitting mechanic
- [ ] Student removes each part sequentially from the minuend hoard, with the hoard count updating after each removal
- [ ] The step-by-step equation is displayed and narrated after completion [Source: Four Essential Mental Math Strategies — subtraction decomposition notation]
- [ ] Problems are designed so that decomposing to a "friendly" subtraction step (reaching a multiple of 10) is the intended path
- [ ] World 5 contains 6–8 levels, progressing from "two-digit minus single-digit" to "two-digit minus two-digit"
- [ ] Hint system (4.8) is especially active here, since subtraction decomposition is "not as commonly used" and students may need more scaffolding [Source: Four Essential Mental Math Strategies]

**Edge Cases:**
- Student attempts to remove more gems than exist in the hoard: gems bounce back; narrator says "Oops, there aren't enough gems for that. Try a smaller piece."

**Dependencies:** 4.1 Gem-Splitting Mechanic, 4.5 World 4.

**Phase:** V1

---

### 4.7 Progress Map

**User Story:** As a student, I want to see a map of the cave that shows which levels I've completed, so that I feel a sense of adventure and progress.

**Description:** A visual cave map connects all levels as nodes along a path. Completed levels show an open treasure chest icon. The current level pulses. Locked levels show a dim locked-chest icon. The map scrolls as the student progresses through worlds.

**Source Basis:** PDB-derived — engagement through narrative and visual progress rather than extrinsic rewards (points, badges).

**Acceptance Criteria:**
- [ ] Map displays all levels in the current world as connected nodes on a cave-themed illustrated path
- [ ] Completed levels are marked with an open treasure chest and a checkmark
- [ ] The current (next playable) level pulses with a glow animation
- [ ] Locked levels appear dimmed with a closed lock icon
- [ ] Student can tap any completed level to replay it
- [ ] World transitions are marked with a visual "deeper into the cave" animation
- [ ] Map state persists across browser sessions (localStorage in Pilot; server-side in V1)

**Edge Cases:**
- localStorage is cleared (e.g., school IT wipes browser data): student sees all levels reset; V1 server-side storage resolves this.

**Dependencies:** None.

**Phase:** Pilot

---

### 4.8 Hint System

**User Story:** As a student, I want to receive a helpful hint if I'm stuck, so that I can learn from my mistakes without giving up.

**Description:** After 2 consecutive failed attempts on the same level, the game displays a visual hint: a ghost outline showing one valid split position. The hint does not auto-solve the problem — the student must still perform the split. After 4 failed attempts, a more explicit hint shows the full decomposition path as a step-by-step animation the student can watch and then try.

**Source Basis:** PDB-derived (JTBD #2 — "immediate feedback that shows me why I'm right or wrong... learn from mistakes without shame"). Also: "Five Common Mistakes" — provide scaffolding rather than just marking wrong.

**Acceptance Criteria:**
- [ ] After 2 consecutive incorrect attempts, a Tier 1 hint appears: a translucent "ghost splitter" animates to a valid split position, then fades
- [ ] After 4 consecutive incorrect attempts, a Tier 2 hint appears: a full step-by-step animation showing one valid decomposition path, narrated aloud
- [ ] After the Tier 2 hint, the student retries the problem themselves (hint does not auto-complete)
- [ ] Hint usage is tracked in analytics but does not penalize the student's progress or celebration
- [ ] Hints are specific to the current problem (not generic tips)

**Edge Cases:**
- Student intentionally fails to trigger hints: no penalty; hints are a learning tool, not a cheat.

**Dependencies:** 4.1 Gem-Splitting Mechanic.

**Phase:** Pilot (Tier 1); V1 (Tier 2)

---

### 4.9 Audio Narration

**User Story:** As a student who is still learning to read, I want all instructions and feedback spoken aloud, so that I can play the game independently without needing an adult to read for me.

**Description:** All instructional text, feedback messages, equations, and hints are accompanied by recorded or TTS audio narration. A speaker icon allows replaying the current narration.

**Source Basis:** PDB Constraint — "Age-Appropriate UX: must accommodate emerging reading skills (minimal text, audio narration)." Also: "Four Essential Mental Math Strategies" — verbal explanation of strategy is core to number talks.

**Acceptance Criteria:**
- [ ] Every instructional message and feedback prompt has accompanying audio narration that plays automatically
- [ ] Equations are read aloud in natural language in Hebrew (e.g., "תשע ועוד שבע שווה שש עשרה" not "9 + 7 = 16"); English and Spanish narration added in V1/V2
- [ ] A speaker icon is visible on every screen; tapping it replays the most recent narration
- [ ] Narration voice is warm, encouraging, and age-appropriate; Pilot narration is in Hebrew
- [ ] Narration does not overlap with sound effects; sound effects duck in volume when narration plays
- [ ] Student can mute narration independently of sound effects via a settings toggle

**Edge Cases:**
- Browser blocks autoplay audio: display a one-time "tap to start" splash that enables audio context.

**Dependencies:** None.

**Phase:** Pilot

---

### 4.10 Teacher Dashboard

**User Story:** As a teacher, I want to see which levels each student has completed and where they're struggling, so that I can provide targeted support during number talks.

**Description:** A simple web dashboard where teachers can view per-student progress, common error patterns (e.g., "Student X consistently fails to find make-10 combinations with 8+N"), and class-level completion rates.

**Source Basis:** PDB JTBD #5 — "quickly identify which specific concepts each student struggles with." PDB Success Criteria #5 — "identify struggling students and specific skill gaps within 5 minutes."

**Acceptance Criteria:**
- [ ] Teacher can create a class and add students (first name or pseudonym only — no email, no PII)
- [ ] Dashboard shows a grid: students (rows) × worlds/levels (columns) with color-coded status (not started / in progress / completed / struggling)
- [ ] "Struggling" is defined as ≥ 3 failed attempts on a single level
- [ ] Teacher can click a student to see level-by-level detail: attempts, hints used, time spent
- [ ] Dashboard shows class-wide "hotspot" levels where ≥ 30% of students are struggling
- [ ] Dashboard loads and displays all data within 3 seconds
- [ ] Teacher accesses dashboard via a separate `/teacher` URL with a simple password-based login (no student-facing login)

**Edge Cases:**
- Teacher has multiple classes: support up to 5 classes per teacher account in V1.
- Student plays on a different device: without login (Pilot), progress is device-bound; with student profiles (V1), progress syncs.

**Dependencies:** Student profiles (V1), analytics pipeline.

**Phase:** V1

---

## 5. Non-Functional Requirements

### 5.1 Performance

- Page/level load time: ≤ 2 seconds on a Chromebook with standard school Wi-Fi (10 Mbps)
- Gem-splitting interaction: ≤ 100ms response to drag input (60fps animation target)
- Total application bundle size: ≤ 5 MB (compressed) for Pilot; ≤ 10 MB for V1
- Audio files: streamed or lazy-loaded; not bundled in initial payload

### 5.2 Security & Privacy

- **COPPA Compliance:** No collection of personal information from children. Pilot uses anonymous localStorage only. V1 student profiles use teacher-assigned pseudonyms — no real names, emails, or photos required. [Source: PDB Constraint — COPPA]
- **FERPA Compliance:** If deployed in schools, student progress data is stored per-class and accessible only to the class teacher. No data shared across schools or with third parties. [Source: PDB Constraint — FERPA]
- **No third-party tracking:** No analytics SDKs that transmit student data to external services. Pilot uses self-hosted, anonymous event logging only.
- **Teacher authentication:** Simple password-based access for Pilot; upgrade to school SSO integration in V2.

### 5.3 Accessibility

- **WCAG 2.1 AA compliance** for all interactive elements [Source: PDB Constraint]
- Color is never the sole indicator of state (all color cues are paired with shape, icon, or text)
- All gem clusters include aria-labels with count (e.g., "Group of 8 gems")
- Keyboard navigation: all interactions achievable via keyboard (Tab, Enter, Arrow keys) as alternative to touch/mouse
- Audio narration serves as built-in screen reader alternative for core content
- Minimum touch target size: 44×44 px

### 5.4 Scalability

- Pilot: support up to 500 concurrent students (single school pilot)
- V1: support up to 10,000 concurrent students (multi-school deployment)
- Static assets served via CDN
- Game state is client-side (Pilot) or lightweight server calls (V1) — no real-time server dependency during gameplay

### 5.5 Monitoring & Observability

- Client-side error logging (unhandled exceptions, failed asset loads) reported to a centralized log
- Analytics events: level_start, level_complete, split_action, hint_triggered, session_start, session_end
- Teacher dashboard uptime: 99.5% availability during school hours (7 AM – 5 PM local)

---

## 6. Data Requirements

### 6.1 Student Gameplay Data (Anonymous)

| Data Point | Purpose | Storage | PII? |
|-----------|---------|---------|------|
| Session ID (random UUID) | Group events within a session | Client (Pilot) / Server (V1) | No |
| Level ID + attempts + outcome | Track progress, identify struggling students | Client (Pilot) / Server (V1) | No |
| Split actions (what number, where split) | Analyze decomposition strategy patterns | Client (Pilot) / Server (V1) | No |
| Hint triggers (tier, level) | Measure difficulty calibration | Client (Pilot) / Server (V1) | No |
| Timestamps (session start/end, level start/end) | Session duration metrics | Client (Pilot) / Server (V1) | No |

### 6.2 Teacher Data (V1)

| Data Point | Purpose | Storage | PII? |
|-----------|---------|---------|------|
| Teacher email + password hash | Authentication | Server | Yes — teacher only, not student |
| Class name + student pseudonyms | Organize student progress | Server | No student PII |

### 6.3 Level / Content Data

- Level definitions (number sets, valid decomposition paths, hint configurations) stored as static JSON bundled with the application
- No dynamic content generation in Pilot; curated level library

---

## 7. Integration Requirements

### Pilot
- **None.** Standalone web application. No external integrations.

### V1
- **Google Classroom (V2 candidate):** Roster sync for teacher dashboard. Deferred — manual student entry is sufficient for V1 scale.

### Future
- **LMS Integration (V2):** LTI 1.3 compliance for embedding in school LMS platforms.
- **Clever SSO (V2):** Single sign-on for school districts that use Clever.

---

## 8. UX Requirements

### 8.1 Visual Style

- Bright, warm color palette (gem tones: ruby red, sapphire blue, emerald green, amber gold)
- Cave/treasure theme with illustrated backgrounds (not photorealistic)
- Characters: a friendly explorer character (gender-neutral, customizable skin tone in V1) who narrates and reacts
- Gems are round, chunky, and easy to distinguish at small sizes — no fine detail
- All text uses a clear, large sans-serif font (minimum 24px body, 32px headings); Hebrew font must support full niqqud (vowel diacritics) for early readers
- **RTL layout:** All UI elements, text flow, and navigation are right-to-left for the Hebrew Pilot. Gem cluster layouts and number lines maintain mathematical convention (left-to-right number lines) but all chrome, menus, and text are RTL

### 8.2 Core Interaction Flow

```
[Level Start]
  → Narrator introduces the goal ("Let's break these gems apart to make a group of 10!")
  → Gem clusters appear on screen
  → Student drags splitter to split a cluster
    → Gems animate apart; group sizes displayed
    → Student can re-split or combine groups
  → Student forms a group of 10 → Power Gem animation
  → Remaining gems counted
  → Equation displayed and narrated
  → Treasure chest opens / bridge activates
  → Celebration animation (2–3 seconds)
[Level Complete → return to map]
```

### 8.3 Screen Inventory (Pilot)

1. **Title Screen:** Game logo, "Play" button, speaker toggle, cave backdrop
2. **Progress Map:** Scrollable cave map with level nodes
3. **Gameplay Screen:** Gem clusters, splitter tool, combine zones, Power Gem slots, equation display area, hint button (after 2 fails), narrator character
4. **Level Complete Screen:** Celebration animation, equation summary, "Next Level" / "Replay" / "Back to Map" buttons
5. **Settings Overlay:** Audio narration on/off, sound effects on/off

### 8.4 Error & Feedback States

| State | Visual | Audio | Narration |
|-------|--------|-------|-----------|
| Correct split (makes 10) | Gems glow gold, fuse into Power Gem, sparkle particles | Chime + sparkle SFX | "!עשית עשר! כל הכבוד" (You made a ten! Great job!) |
| Valid split (doesn't make 10 yet) | Gems separate, counts shown | Soft click SFX | "?פיצול יפה! עכשיו אפשר לעשות קבוצה של 10" (Nice split! Now can you make a group of 10?) |
| Invalid action (group > 10) | Gems bounce back, gentle red flash | Soft buzz SFX | "זה יותר מ-10. נסה לפצל קצת אבני חן קודם" (That's more than 10. Try splitting off some gems first.) |
| Level complete | Treasure chest opens, gems fly in | Triumphant fanfare | "!מצאת את האוצר" (You found the treasure!) + [Equation readout] |
| Hint triggered (Tier 1) | Ghost splitter animates | Gentle bell SFX | "!הנה רמז — נסה לפצל כאן" (Here's a hint — try splitting here!) |
| Hint triggered (Tier 2) | Full animation walkthrough | Step-by-step narration | "...תראה איך אני עושה את זה" (Watch how I do it...) |

---

## 9. Constraints & Assumptions

### 9.1 Technical Constraints

- **Browser-based:** Must run in Chrome (Chromebook) and Safari (iPad) without installation. No native app for Pilot/V1. [Source: PDB — "Must work on tablets (iPads, Chromebooks) commonly used in elementary schools"]
- **No login for students (Pilot):** Progress stored in browser localStorage. Acceptable for single-device-per-student school setups.
- **No real-time multiplayer:** All gameplay is single-player, single-client.

### 9.2 Regulatory / Compliance Constraints

- **COPPA:** No personal information collected from children. Teacher-assigned pseudonyms only. Parental consent not required if no PII is collected. [Source: PDB Constraint]
- **FERPA:** Student progress data accessible only to assigned teacher. No cross-school data sharing. Data deletion available on request. [Source: PDB Constraint]
- **WCAG 2.1 AA:** All interactive elements accessible. [Source: PDB Constraint]

### 9.3 Assumptions

| # | Assumption | Support Level | Validation Plan |
|---|-----------|---------------|-----------------|
| 1 | Students in the Israeli pilot school have individual or shared access to a Chromebook, iPad, or other tablet during math time | Source-supported (PDB Assumption #1) | Confirm with pilot school |
| 2 | First graders can learn the drag-to-split gesture with minimal instruction (1-2 guided attempts) | Unsupported — needs validation | Usability test with 5 students pre-pilot |
| 3 | Teachers will use the game as a supplement to (not replacement for) number talks | Source-supported (Four Essential Mental Math Strategies — teacher-led discussion is critical) | Teacher onboarding guide |
| 4 | Making 10 is the most accessible entry point for decomposition (before place value or subtraction) | Source-supported (Four Essential Mental Math Strategies — "decomposition is one of the first strategies I teach"; examples start with make-10) | Pilot learning outcome data |
| 5 | 15–20 levels across 3 worlds provides sufficient practice for Pilot learning outcomes | Partially supported (Strategic Activities — "4–5 days of focused practice") | Pilot completion and mastery rate data |
| 6 | localStorage persistence is adequate for a 6-week pilot at a single school | Unsupported — risk of data loss from browser/cache clearing | Monitor data-loss reports during pilot; mitigate with V1 server storage |
| 7 | Israeli first-grade math curriculum covers decomposition and make-10 strategies, aligning with the game's pedagogical approach | Needs validation — Israeli curriculum may differ from US-based source material | Review Israeli Ministry of Education first-grade math standards before pilot |
| 8 | Hebrew TTS quality (browser SpeechSynthesis API or cloud TTS) is sufficient for clear, child-friendly narration | Partially supported — Hebrew TTS has improved but may require a cloud TTS service for natural-sounding child-directed speech | Test Hebrew TTS options during development; budget for cloud TTS if browser API is inadequate |
| 9 | Successful Hebrew pilot will validate the game concept for international expansion to English and Spanish markets | Unsupported — needs validation | Pilot learning outcome data; market research for EN/ES expansion |

---

## 10. Open Questions

| # | Question | Impact | Blocking? | Owner |
|---|----------|--------|-----------|-------|
| 1 | **Drag-to-split usability:** Can 6-year-olds reliably perform the splitter-drag gesture on both Chromebook trackpads and iPad touchscreens? | High — core mechanic depends on this | Yes (pre-pilot usability test) | Founder |
| 2 | **Level count sufficiency:** Are 15–20 Pilot levels enough for 6 weeks of use (3×/week, ~6-min sessions), or will students exhaust content? | Medium — affects engagement | No (can add levels iteratively) | PM |
| 3 | **Audio production:** Use recorded human narration (warmer but expensive to change) or TTS (cheaper, easier to iterate)? | Medium — affects UX quality and iteration speed | No (start with TTS, upgrade to human voice for V1 if needed) | Founder |
| 4 | **Doubles as friendly numbers:** Should the game teach doubles recognition (7+7=14) alongside make-10, or is that a distraction from the core make-10 skill in Pilot? | Medium — the source material covers both | No (defer to V1; keep Pilot focused on make-10) | PM |
| 5 | **Monetization model:** Free pilot; but what's the V1 model? School site license? Freemium? | High — affects long-term decisions | No (not blocking Pilot) | Founder |
| 6 | **Source gap — empirical efficacy data:** No controlled studies provided on digital decomposition games specifically. Pilot must generate its own efficacy evidence. | Medium — success metrics are best-guess targets | No (Pilot is the validation) | Founder |
| 7 | **Subtraction scaffolding level:** The source notes subtraction decomposition is "not as commonly used" — how much scaffolding is needed in World 5 beyond the standard hint system? | Medium — affects V1 design | No (V1 scope) | PM |
| 8 | **Hebrew niqqud (vowel diacritics):** Should in-game Hebrew text include niqqud to aid first-graders who are still learning to read, or is unvocalized text sufficient given that narration is audio-first? | Medium — affects readability for target age | No (default to including niqqud; can be removed later) | Founder |
| 9 | **Israeli curriculum alignment:** Do Israeli first-grade math standards emphasize decomposition and make-10 strategies in the same way as US curricula? Are there curriculum-specific adaptations needed? | High — affects pedagogical validity for target market | Yes (pre-pilot curriculum review) | Founder |
| 10 | **Localization architecture:** Should the codebase use an i18n framework from the start (anticipating EN/ES expansion), or build Hebrew-only first and refactor later? | Medium — affects development speed vs. future effort | No (recommend i18n from start to reduce rework) | Tech Lead |

---

*Document version: 1.1 — Hebrew-first language strategy*
*Last updated: 2026-02-25*
*Author: PM-Requirements Agent*
*Status: Draft — awaiting founder review*
