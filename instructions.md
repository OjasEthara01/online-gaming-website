# ArcadeNexus: Online Gaming Platform (Enhanced Developer Specification & Verification Guide)

## 1. Project Overview

### 1.1 Executive Summary
ArcadeNexus is a next-generation retro-inspired online gaming platform designed to run in any modern web browser. The platform serves as an interactive, single-page casual gaming portal containing a responsive navigation system, landing page hero, games directory catalog, and a central user statistics dashboard. The portal hosts three fully client-side games (Tic-Tac-Toe, Snake, and Memory Match) running inside overlay modals. Player metrics must persist offline across browser sessions utilizing the HTML5 Web Storage API (`localStorage`), eliminating the requirement for complex backend databases or remote APIs.

### 1.2 Core Deliverables
The final submission package must compile to the following files organized inside the `ArcadeNexus/` root directory:
1. `index.html`: The structural backbone of the platform, built using modern Semantic HTML5 tags.
2. `styles.css`: Visual styling framework utilizing CSS variables, responsive design rules, glassmorphic overlays, and CSS transitions.
3. `app.js`: Main platform controller managing DOM loading, modal overlays, global audio routing, and dashboard statistics calculations.
4. `games.js`: Modular game loop logic and event-handling code for the three playable client-side mini-games.
5. `README.md`: System documentation detailing quickstart guidelines, browser compatibility matrix, and asset license disclosures.
6. `assets/`: Folder holding static graphics and audio assets structured as:
   * `assets/images/logo.svg`: Brand vector logo.
   * `assets/images/ttt_card.png`: Tic-Tac-Toe selection card image.
   * `assets/images/snake_card.png`: Snake selection card image.
   * `assets/images/memory_card.png`: Memory card selection card image.
   * `assets/audio/click.mp3`: Standard UI button interaction SFX.
   * `assets/audio/score.mp3`: Sound triggered when score points are achieved.
   * `assets/audio/gameover.mp3`: Audio trigger for match/game conclusion.
   * `assets/audio/cyber_beats.mp3`: Ambient electronic looped background track.

### 1.3 Target Audience and Persona
* **Audience Profile:** Casual players, aged 15-45, seeking quick-launch browser games requiring no signup.
* **Persona Goal:** Immediate load times (under 1 second), seamless transitioning between games, tactile user feedback (sound effects, visual glow transitions), and persistent high score tracking to incentivize replay value.

---

## 2. Layout Structure & UI Architecture

The interface must be designed as a unified single-page layout segmented into five sections:

### 2.1 Navigation Bar (Header)
A structural `<header>` fixed at the top of the viewport. It contains:
* **Branding Area:** Text logo `ArcadeNexus` set in bold **Orbitron** with a subtle glowing neon teal text shadow.
* **Anchor Links:** Smooth scrolling anchors pointing to `#games-catalog` and `#stats-dashboard`.
* **Global Sound Toggle:** An interactive icon button indicating audio status (Speaker Active / Speaker Muted) controlling all playbacks.

### 2.2 Hero Banner Section
An engaging visual introduction viewport containing:
* **Main Slogan:** "Level Up Your Leisure" or similar gamer hook text in uppercase Orbitron typography.
* **Sub-headline:** Quick summary of features in Inter typography.
* **CTA Button:** Pulsing accent-colored button scrolling users directly to the Games Grid catalog.
* **Welcome Banner:** Dynamically rendered string greeting return users (e.g., "Welcome back, Player One! Ready to beat your Snake high score of 120?"). If no local stats are detected, default to "Welcome to the Nexus, Guest!"

### 2.3 Games Selection Catalog
A grid layout displaying three card modules (Tic-Tac-Toe, Snake, Memory Match). Cards must feature:
* Transparent backdrop surface colors (`rgba` alpha variables) to achieve a glassmorphic aesthetic.
* Specific cover thumbnail PNGs.
* A list of key game stats (e.g., wins count, high score).
* Hover state scaling up the container by `1.03` with a glowing border transition.
* "Play Now" action buttons triggering overlay modals.

### 2.4 Statistics Dashboard Section
A structured section rendering aggregated localStorage data. Must present:
* A header reading "NEXUS PERSISTENT CORE" in Orbitron.
* Tabular metrics columns: Total Games Played, Tic-Tac-Toe win percentage, Snake maximum score, and Memory Match minimum moves.
* **Reset Stats Button:** Triggers verification alert, wipes the statistics key from memory, and refreshes all metrics on screen to 0 immediately.

### 2.5 Footer Section
A footer containing logo branding, system copyright text, and links to the assets licensing documents.

---

## 3. Brand Identity & Design Token Guidelines

Developers must use *only* the tokens defined below. Custom style extensions, external font injections, or styling offsets will result in verification failure.

### 3.1 Color Tokens Palette
* **Background Surface (Deep Void):** HEX `#0B0C10` (Primary deep dark background).
* **Card Fills & Borders (Cyber Gray):** HEX `#1F2833` (Card backdrops, outline strokes, input borders).
* **Neon Accent (Neon Teal):** HEX `#66FCF1` (Active buttons, focus outlines, text glowing shadows, active game states).
* **Secondary Brand Accent (Laser Cyan):** HEX `#45A29E` (Subheadings, passive links, border highlights, muted buttons).

### 3.2 Typography Tokens
* **Titles, Logos, Buttons & Counter Headers:** Orbitron (Google Fonts, sans-serif, bold/black weights 700/900).
* **Content Copy, Metrics Data, Description & Settings:** Inter (Google Fonts, sans-serif, weights 300/400/600).

### 3.3 Visual Motion Standards
* Card hover transition: `transform 0.3s cubic-bezier(0.25, 0.8, 0.25, 1), box-shadow 0.3s ease`.
* Modal transitions: Smooth opacity fade-in (`0` to `1` over `0.25s`) with scaling overlay bounds.
* All input buttons must feature a scale-down effect (`transform: scale(0.97)`) on click/active states.

---

## 4. Gameplay Engine Implementations

### 4.1 Tic-Tac-Toe Logic Specifications
* **Grid:** 3x3 interactive squares initialized as empty values.
* **Mode:** Single-player VS local Computer AI.
* **AI Engine:** The AI must run checking routines on every turn:
  1. *Winning Move:* Check if AI can win immediately.
  2. *Blocking Move:* Check if Player is one move away from winning and place a block.
  3. *Random Fallback:* If no block/win exists, select a random remaining empty tile.
* **State Detection:** Compute grid combinations on turn ends. If a match is found, draw a colored neon line across the grid victory line, lock controls, play `gameover.mp3`, increment stats, and display the win banner. If no moves remain and no line matches, trigger tie game over sequence.

### 4.2 Snake Engine Specifications
* **Drawing Canvas:** 2D canvas context set to exactly 400x400 display dimensions.
* **Controls:** Binds WASD or Arrow Keys. Ignores opposite direction inputs (e.g. going left cannot instantly switch to right).
* **Speed Scaling:** Initial update loop runs at `100ms`. Every time the snake consumes 5 food objects, the update loop interval must decrease by 5% (increasing update speed), up to a max velocity limit of `40ms`.
* **Collisions:** If the snake coordinate matches boundary edges or intersects with its own coordinate set, terminate loop, display "GAME OVER" in Orbitron text on screen, and play `gameover.mp3`.

### 4.3 Memory Match Engine Specifications
* **Grid Layout:** 4x4 matrix housing 8 distinct pairs (16 cards total).
* **Shuffle Algorithm:** Must dynamically randomize card placements on initialization using Fisher-Yates shuffle algorithms.
* **Flipping Action:** Click triggers 3D CSS rotate transform. The board must reject clicks if two cards are already active or if clicking a card already matched.
* **Matching Rules:** If matching, lock cards face up and play `score.mp3`. If mismatching, allow a 1.0-second delay for user visualization, then flip them back face down.
* **Moves counter:** Tracks each pair checking event and increments the score panel.

---

## 5. Persistence & localStorage Configuration

All scores and configurations must be saved inside a single JSON string key named `arcadenexus_stats`. 

### 5.1 JSON Storage Schema
```json
{
  "total_games_played": 15,
  "games": {
    "tictactoe": {
      "played": 5,
      "wins": 3,
      "losses": 1,
      "ties": 1,
      "high_score": 3
    },
    "snake": {
      "played": 6,
      "wins": 0,
      "losses": 6,
      "ties": 0,
      "high_score": 142
    },
    "memory": {
      "played": 4,
      "wins": 2,
      "losses": 2,
      "ties": 0,
      "high_score": 12
    }
  }
}
```

### 5.2 Read/Write Synchronization Logic
* **On Load:** Initialize default JSON structure if the `arcadenexus_stats` key does not exist. Read and bind data fields to populate the UI Stats Dashboard and Welcome Hero banner.
* **On Game Completion:** Game engine increments relevant fields, recalculates ratios, updates `localStorage`, and updates dashboard DOM elements *without* requiring full page reloads.

---

## 6. Technical Quality & Acceptance Criteria

### 6.1 DOM & Code Organization
* Strictly modular Javascript: Divide UI state management (`app.js`) from game engine states (`games.js`).
* Avoid polluting global scopes: encapsulate routines in modules or classes.
* Separation of Concerns: No inline event listeners inside `index.html` (e.g. do not write `<button onclick="play()">`). All listeners must be attached via `addEventListener` in JS code.

### 6.2 Responsive Boundaries CSS
Developers must optimize visual rendering across three distinct breakpoint media queries:
* **Mobile Viewport (<576px):** Navbar links fold to dropdown or stack vertically. Grid layout maps cards as a single-column block. Game modals scale to 95% width.
* **Tablet Viewport (576px - 992px):** Grid maps cards in a two-column layout. Navbar lists inline links with reduced margins.
* **Desktop Viewport (>992px):** Grid maps cards as a three-column horizontal list.

### 6.3 Sound Level & Audio Routing
* Background tracks must play at `-20 LUFS` to ensure they sit comfortably under sound effects.
* SFX (`click.mp3`, `score.mp3`, `gameover.mp3`) must play at `-14 LUFS`.
* Toggling global sound setting controls active Web Audio API routing nodes, muting all outputs instantly when set.

---

## 7. Quality Assurance Checklist

Verify execution against these checklist items before submission:
1. Double-clicking `index.html` locally loads the entire page and navbar with zero console errors.
2. Clicking navbar links smooth-scrolls to target blocks.
3. Resizing viewport to 360px width causes no layout clipping or horizontal scrolls.
4. Tic-Tac-Toe AI prevents immediate wins and completes blocking moves correctly.
5. Canvas snake accelerates on eating food and terminates on tail collisions.
6. Memory cards display flip animations and do not register clicks on already matching pairs.
7. Statistics dashboard updating is persistent and updates on live completions without manual reload.
8. Mute toggle terminates looped music tracks and silences SFX immediately.
9. Wiping stats clears local storage and updates numbers on screen instantly.
