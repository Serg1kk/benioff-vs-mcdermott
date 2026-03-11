# Gemini Handoff: UI & Visual Design

## Your Task

You are building the UI and visual design for **Benioff vs McDermott: Death Match** - a neon cyberpunk top-down arena shooter.

The game engine is complete. Your job is to style the menus, HUD, and visual effects.

---

## Quick Start

1. Clone: `git clone https://github.com/Serg1kk/benioff-vs-mcdermott.git`
2. Open `index.html` in a browser - the game auto-starts in test mode
3. Edit only the `<style>` section and HTML menu structures
4. **DO NOT** modify JavaScript between `// CLAUDE:` markers

---

## What's Already Built

The game engine handles:
- Game state machine (menu → character select → arena select → playing → game over)
- Player movement, combat, special abilities
- AI opponent with 3 difficulty levels
- Projectile physics and collisions
- Basic HUD (health bars, timer, wins)

**Global access:** `window.game` exposes the game instance for menu integration.

---

## What You Need to Build

### 1. Main Menu Screen

Replace the current blank screen with a styled menu.

**HTML structure to add in `<body>` before `<div id="game-container">`:**

```html
<div id="main-menu" class="screen active">
  <h1 class="game-title">BENIOFF <span class="vs">vs</span> McDERMOTT</h1>
  <p class="game-subtitle">DEATH MATCH</p>

  <div class="menu-buttons">
    <button class="neon-btn" onclick="startSinglePlayer()">Single Player</button>
    <button class="neon-btn" onclick="startMultiplayer()">Multiplayer</button>
    <button class="neon-btn" onclick="showControls()">Controls</button>
  </div>

  <div id="difficulty-select" class="hidden">
    <p>Select Difficulty:</p>
    <button class="neon-btn small" onclick="startGame('easy')">Easy</button>
    <button class="neon-btn small" onclick="startGame('normal')">Normal</button>
    <button class="neon-btn small" onclick="startGame('hard')">Hard</button>
  </div>
</div>
```

**Required JS functions to add (put in a new `<script>` tag after the main script):**

```javascript
function startSinglePlayer() {
  // Show difficulty selector
  document.getElementById('difficulty-select').classList.remove('hidden');
}

function startMultiplayer() {
  window.game.startGame('benioff', 'mcdermott', true);
  hideAllScreens();
}

function startGame(difficulty) {
  window.game.startGame('benioff', 'mcdermott', false, difficulty);
  hideAllScreens();
}

function showControls() {
  // Show controls overlay
}

function hideAllScreens() {
  document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
}
```

**Style requirements:**
- Dark background (#0a0a1a)
- Neon glow on title (cyan/magenta gradient)
- Animated background particles or grid
- Buttons with hover effects (scale up, color shift)
- Subtle pulsing animation on title

---

### 2. Character Select Screen

**HTML structure:**

```html
<div id="character-select" class="screen">
  <h2>SELECT YOUR FIGHTER</h2>

  <div class="character-cards">
    <div class="character-card" data-character="benioff">
      <div class="card-portrait benioff"></div>
      <h3>BENIOFF</h3>
      <div class="stats">
        <div class="stat"><span>Speed</span><div class="stat-bar" style="--fill: 80%"></div></div>
        <div class="stat"><span>Health</span><div class="stat-bar" style="--fill: 60%"></div></div>
        <div class="stat"><span>Attack</span><div class="stat-bar" style="--fill: 60%"></div></div>
      </div>
      <div class="specials">
        <p><strong>Cloud Bolt</strong> - Tracking lightning strike</p>
        <p><strong>Ohana Shield</strong> - Temporary invincibility</p>
      </div>
    </div>

    <div class="character-card" data-character="mcdermott">
      <div class="card-portrait mcdermott"></div>
      <h3>McDERMOTT</h3>
      <div class="stats">
        <div class="stat"><span>Speed</span><div class="stat-bar" style="--fill: 60%"></div></div>
        <div class="stat"><span>Health</span><div class="stat-bar" style="--fill: 80%"></div></div>
        <div class="stat"><span>Attack</span><div class="stat-bar" style="--fill: 80%"></div></div>
      </div>
      <div class="specials">
        <p><strong>Workflow Blast</strong> - Cone pushback wave</p>
        <p><strong>Automation Drone</strong> - Auto-attacking drone</p>
      </div>
    </div>
  </div>

  <button class="neon-btn" onclick="confirmCharacter()">READY</button>
</div>
```

**Style requirements:**
- Cards side-by-side with gap
- Benioff: Blue/purple theme (#4a90d9, #aa88ff)
- McDermott: Orange/red theme (#ff6b35, #ff4444)
- Selected state: glowing border
- Stats bars with gradient fill
- Hover effect on cards

---

### 3. Arena Select Screen

**HTML structure:**

```html
<div id="arena-select" class="screen">
  <h2>SELECT ARENA</h2>

  <div class="arena-cards">
    <div class="arena-card selected" data-arena="cloud_city">
      <div class="arena-preview cloud-city"></div>
      <h3>Cloud City</h3>
      <p>Purple neon skies</p>
    </div>

    <div class="arena-card" data-arena="data_center">
      <div class="arena-preview data-center"></div>
      <h3>Data Center</h3>
      <p>Cyber server farm</p>
    </div>

    <div class="arena-card" data-arena="boardroom">
      <div class="arena-preview boardroom"></div>
      <h3>Boardroom</h3>
      <p>Corporate battlefield</p>
    </div>
  </div>

  <button class="neon-btn" onclick="startMatch()">FIGHT!</button>
</div>
```

**Style requirements:**
- Three cards in a row
- Each with distinct gradient background:
  - Cloud City: Purple/pink (#1a0a2e to #2e0a3e)
  - Data Center: Blue/cyan (#0a1a2e to #0a2e3e)
  - Boardroom: Gold/white (#2e2a1a to #3e3a2a)
- Selected state indicator

---

### 4. HUD Styling

The engine renders basic HUD elements. You can enhance with CSS overlays.

**Current HUD elements (rendered by engine):**
- Health bars (top corners)
- Round timer (top center)
- Win counters

**Add this HTML overlay for ability cooldowns:**

```html
<div id="hud-overlay" class="hidden">
  <div class="p1-abilities">
    <div class="ability" id="p1-special1">
      <div class="ability-icon">Q</div>
      <div class="cooldown-ring"></div>
    </div>
    <div class="ability" id="p1-special2">
      <div class="ability-icon">E</div>
      <div class="cooldown-ring"></div>
    </div>
  </div>

  <div class="p2-abilities">
    <!-- Same structure for P2 -->
  </div>
</div>
```

**Style requirements:**
- Circular cooldown indicators
- Ready state: glowing, full color
- Cooldown state: dark, animated fill revealing
- Position below health bars

---

### 5. CSS Theme

Add this to your `<style>` section:

```css
:root {
  --bg-dark: #0a0a1a;
  --bg-medium: #1a1a2e;
  --bg-light: #2a2a3e;
  --neon-cyan: #00ffff;
  --neon-magenta: #ff00ff;
  --neon-gold: #ffd700;
  --neon-blue: #4a90d9;
  --neon-orange: #ff6b35;
  --neon-green: #00ff88;
  --neon-red: #ff4444;
  --text-primary: #ffffff;
  --text-secondary: #8888aa;
  --font-main: 'Segoe UI', 'Roboto', sans-serif;
}

.screen {
  position: absolute;
  top: 0;
  left: 0;
  width: 960px;
  height: 640px;
  display: none;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  z-index: 100;
}

.screen.active {
  display: flex;
}

.neon-btn {
  /* Your styling here */
  /* Should have glow effect, hover state, etc */
}

.hidden {
  display: none !important;
}
```

---

### 6. Visual Effects (Optional Enhancements)

If you want to go beyond CSS, you can add canvas-based effects by hooking into the game:

```javascript
// Add particle effects to the game
window.game.addEffect({
  active: true,
  update: function(dt) { /* your logic */ },
  render: function(ctx) { /* your drawing */ }
});
```

**Ideas:**
- Trail effects on player movement
- Enhanced explosion particles
- Victory celebration effects
- Round start countdown animation

---

## File Structure

```
index.html
├── <style>              ← YOUR CSS HERE
├── <body>
│   ├── <div id="main-menu">        ← YOUR HTML
│   ├── <div id="character-select"> ← YOUR HTML
│   ├── <div id="arena-select">     ← YOUR HTML
│   ├── <div id="hud-overlay">      ← YOUR HTML
│   ├── <div id="game-container">
│   │   └── <canvas>                ← ENGINE RENDERS HERE
│   └── <script>                    ← YOUR MENU JS HERE
└── <script>            ← DO NOT MODIFY (engine code)
```

---

## Testing

1. Open `index.html` in browser
2. Game auto-starts in test mode (Benioff vs McDermott, normal AI)
3. Test your menus by calling functions from browser console:
   - `document.getElementById('main-menu').classList.add('active')`
   - `hideAllScreens()`

---

## Integration Notes

When your menus are ready, connect them by:

1. Remove the auto-start in the engine (find `game.startGame` at bottom of main script)
2. Make sure `hideAllScreens()` is called when game starts
3. Show game over screen that returns to menu

---

## Color Reference

| Element | Color |
|---------|-------|
| Background | #0a0a1a |
| Canvas border | #00ffff |
| Benioff primary | #4a90d9 |
| Benioff glow | #00aaff |
| McDermott primary | #ff6b35 |
| McDermott glow | #ff8844 |
| Health high | #00ff88 |
| Health mid | #ffff00 |
| Health low | #ff4444 |
| UI accent 1 | #00ffff |
| UI accent 2 | #ff00ff |
| UI accent 3 | #ffd700 |

---

## Deliverables

1. Styled main menu with animated background
2. Character select screen with stats display
3. Arena select screen with preview cards
4. Enhanced HUD with ability cooldowns
5. Game over/victory screen

Commit your changes to the `main` branch with descriptive messages.

---

**Questions?** Check `docs/plans/2026-03-11-implementation-plan.md` for full specifications.
