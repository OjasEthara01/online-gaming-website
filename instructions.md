# ArcadeNexus: Online Gaming Platform (Developer Specification & Verification Guide)

## 1. Project Overview

### 1.1 Summary
ArcadeNexus is a simple online gaming website. The site runs in any web browser and contains three simple games: Tic-Tac-Toe, Snake, and Memory Match. These games open and play in popup windows (modals). The website tracks user scores and total games played. It saves these scores on the user's computer using the browser's localStorage. The website is built using standard HTML, CSS, and JavaScript.

### 1.2 Core Deliverables
The developer must deliver these files inside a folder named `ArcadeNexus/`:
1. `index.html`: The layout of the website using semantic HTML tags.
2. `styles.css`: The styling sheet containing the color variables, mobile layouts, and card hover effects.
3. `app.js`: Main JavaScript file to manage the layout, popup windows, mute settings, and statistics updates.
4. `games.js`: JavaScript file containing the logic for the three games.
5. `README.md`: Text documentation explaining how to open the site, check the games, and list file license sources.
6. `assets/`: Folder holding the brand images and audio files:
   * `assets/images/logo.svg`: Brand logo.
   * `assets/images/ttt_card.png`: Cover card image for Tic-Tac-Toe.
   * `assets/images/snake_card.png`: Cover card image for Snake.
   * `assets/images/memory_card.png`: Cover card image for Memory Match.
   * `assets/audio/click.mp3`: Click sound for buttons and grid cells.
   * `assets/audio/score.mp3`: Sound played when points are scored or cards are matched.
   * `assets/audio/gameover.mp3`: Sound played when a game ends.
   * `assets/audio/cyber_beats.mp3`: Loop background music track.

### 1.3 Target Audience
* **Audience:** Casual players looking for quick games in a web browser without creating accounts.
* **Goals:** Fast loading times, clear button hovers, sound feedback, and persistent scores that stay saved when reloading the page.

---

## 2. Layout Structure & UI Architecture

The page is a single-page layout with five parts:

### 2.1 Navigation Bar (Header)
A structural header fixed at the top of the page:
* **Brand Logo:** The name `ArcadeNexus` in bold Orbitron font with a neon teal shadow glow.
* **Anchor Links:** Menu buttons pointing to the Catalog grid and the Stats dashboard.
* **Global Sound Toggle:** An interactive speaker button to turn the background music and sound effects on or off.

### 2.2 Hero Banner
The introduction area at the top of the viewport:
* **Main Slogan:** Bold gaming text in Orbitron font.
* **CTA Button:** Highlighted button that scrolls the page down to the games catalog.
* **Welcome Message:** Dynamic text greeting return users (e.g., "Welcome back, Player! Ready to beat your Snake high score of 120?"). If no stats are saved, it shows "Welcome to the Nexus, Guest!".

### 2.3 Games Selection Catalog
A grid layout displaying three game cards. Cards feature:
* Dark card background surfaces matching Cyber Gray.
* Game cover PNG cards.
* List of current local scores (wins or high score).
* Card scale-up animation and cyan glow outlines on hover.
* "Play Now" action buttons that open the gameplay popup.

### 2.4 Statistics Dashboard
A section rendering saved localStorage data:
* Header reading "NEXUS PERSISTENT CORE" in Orbitron font.
* Columns displaying: Total Games Played, Tic-Tac-Toe win rate, Snake high score, and Memory Match best moves.
* **Reset Stats Button:** Triggers a confirmation check, deletes user statistics from localStorage, and updates all numbers on the page to 0 immediately.

### 2.5 Footer Section
The bottom section containing copyright details, author name, and brand closing line: *Play instantly, challenge your limits on ArcadeNexus.*

---

## 3. Brand Identity & Design Tokens

Developers must use only the tokens defined below.

### 3.1 Color Tokens Palette
* **Background Surface (Deep Void):** HEX #0B0C10 (Primary background).
* **Card Fills & Borders (Cyber Gray):** HEX #1F2833 (Card panels, borders, and outlines).
* **Neon Accent (Neon Teal):** HEX #66FCF1 (Active buttons, focus borders, glow text, active lines).
* **Secondary Brand Accent (Laser Cyan):** HEX #45A29E (Subheadings, passive text links, passive outlines).

### 3.2 Typography Tokens
* **Titles, Headers, Logos, and Score Counters:** Orbitron (Google Fonts).
* **Content Copy, Metrics, and Lists:** Inter (Google Fonts).

### 3.3 Visual Motion
* Card hover scale-up: `transform: scale(1.03)` with a smooth 0.3-second transition.
* Modal popups: Smooth opacity transition from hidden to visible over 0.25 seconds.
* Active buttons: Press down scale effect (`transform: scale(0.97)`) on click.

---

## 4. Gameplay Logic & Engines

### 4.1 Tic-Tac-Toe
* **Grid:** 3x3 layout.
* **Mode:** Single-player vs computer AI.
* **AI Logic:** The computer opponent plays automatically:
  1. *Win:* Plays in a slot to win if two in a row are marked.
  2. *Block:* Blocks player's double-in-a-row if player is about to win.
  3. *Fallback:* Plays in a random empty slot if no win or block is available.
* **Game Over:** Displays win lines, blocks further clicks, plays `gameover.mp3`, and alerts the player.

### 4.2 Snake
* **Board:** HTML5 canvas element size 400x400.
* **Controls:** Keyboard arrows or WASD. Opposite directions are ignored.
* **Speed:** Starts at 100ms refresh rate. Every time 5 food items are eaten, the speed increases by 5%.
* **Game Over:** Triggers when the head hits canvas boundaries or the snake's tail. Plays `gameover.mp3`.

### 4.3 Memory Match
* **Board:** 4x4 card layout (16 cards total, 8 matching pairs).
* **Shuffle:** Cards are randomized using a shuffling function on start.
* **Flip Logic:** Clicking flips card face up. The game ignores clicks if two cards are already flipped.
* **Matching:** Matching cards stay face up and play `score.mp3`. Mismatched cards flip back face down after 1 second.
* **Moves:** A turn counter tracks flip attempts and displays the score.

---

## 5. Persistence & localStorage Configuration

All scores must be stored under the localStorage key name `arcadenexus_stats`.

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

---

## 6. Technical Quality & Acceptance Criteria

### 6.1 DOM & Code Organization
* Separation of concerns: JavaScript controls UI events (`app.js`) and game engines (`games.js`) separately.
* Separation of HTML/JS: No inline handlers inside index.html (no `onclick`). Binds listeners dynamically.
* Framework limits: No external framework scripts (React, Vue, jQuery) can be imported.

### 6.2 Layout Breakdown
* **Mobile (<576px):** Single-column grid cards list, navbar items wrap.
* **Tablet (576px - 992px):** Two-column cards grid layout.
* **Desktop (>992px):** Three-column horizontal cards grid.

### 6.3 Sound Level & Routing
* Looped background music played at low volume.
* SFX played at standard volume.
* Navbar toggle button mutes all audio elements instantly.

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
