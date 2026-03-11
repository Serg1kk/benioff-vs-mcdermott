# Implementation Plan: Benioff vs McDermott Death Match

## Overview

This plan separates work between **Claude** (game engine/logic) and **Gemini** (UI/visual design).

---

## Phase 1: Core Engine (Claude)

### 1.1 Game Loop & State Machine
**File:** `index.html` (script section)

```javascript
// Structure needed:
const GameState = {
  MENU: 'menu',
  CHARACTER_SELECT: 'character_select',
  ARENA_SELECT: 'arena_select',
  PLAYING: 'playing',
  PAUSED: 'paused',
  ROUND_END: 'round_end',
  GAME_OVER: 'game_over'
};

// Main loop at 60fps
function gameLoop(timestamp) {
  // Update → Render → Request next frame
}
```

**Tasks:**
- [ ] Create game state enum and transitions
- [ ] Implement 60fps game loop with fixed timestep
- [ ] Add state change handlers

### 1.2 Input Handler
**File:** `index.html` (script section)

```javascript
// Key mappings needed:
const CONTROLS = {
  P1: { up: 'KeyW', down: 'KeyS', left: 'KeyA', right: 'KeyD',
        weapon1: 'Digit1', weapon2: 'Digit2', weapon3: 'Digit3',
        special1: 'KeyQ', special2: 'KeyE' },
  P2: { up: 'ArrowUp', down: 'ArrowDown', left: 'ArrowLeft', right: 'ArrowRight',
        weapon1: 'Numpad7', weapon2: 'Numpad8', weapon3: 'Numpad9',
        special1: 'Comma', special2: 'Period' }
};
```

**Tasks:**
- [ ] Keyboard event listeners (keydown/keyup)
- [ ] Input state tracking (pressed, justPressed, justReleased)
- [ ] Prevent default browser behavior for game keys

### 1.3 Physics System
**File:** `index.html` (script section)

**Tasks:**
- [ ] Vector2 class (x, y, add, subtract, normalize, magnitude)
- [ ] AABB collision detection
- [ ] Movement with friction
- [ ] Boundary collision (arena walls)

---

## Phase 2: Game Objects (Claude)

### 2.1 Base Entity Class
```javascript
class Entity {
  constructor(x, y, width, height)
  update(dt)
  render(ctx)
  collidesWith(other)
}
```

**Tasks:**
- [ ] Create Entity base class
- [ ] Position, velocity, acceleration
- [ ] Active/destroyed state

### 2.2 Player Class
```javascript
class Player extends Entity {
  constructor(character, playerNum)
  // character: 'benioff' | 'mcdermott'
  // Stats based on character
  // Weapon cooldowns
  // Special ability cooldowns
}
```

**Tasks:**
- [ ] Character stats (speed, health, attack)
- [ ] Movement with input
- [ ] Health system with damage
- [ ] Invincibility frames during dash
- [ ] Facing direction tracking

### 2.3 Projectiles
```javascript
class Projectile extends Entity {
  constructor(x, y, direction, owner, type)
  // type: 'blaster' | 'cloud_bolt' | 'workflow_blast'
}
```

**Tasks:**
- [ ] Neon Blaster (fast, straight, medium damage)
- [ ] Plasma Grenade (arc trajectory, explodes on impact/timer)
- [ ] Cloud Bolt (tracks enemy briefly)
- [ ] Workflow Blast (cone shape, pushback)

### 2.4 Special Abilities
```javascript
class Shield extends Entity { /* Benioff's Ohana Shield */ }
class Drone extends Entity { /* McDermott's Automation Drone */ }
```

**Tasks:**
- [ ] Ohana Shield (temporary invincibility)
- [ ] Automation Drone (AI-controlled, attacks nearest enemy)

---

## Phase 3: AI System (Claude)

### 3.1 AI Controller
```javascript
class AIController {
  constructor(difficulty) // 'easy' | 'normal' | 'hard'
  update(player, enemy, arena)
  decideAction()
}
```

**Tasks:**
- [ ] Easy: Random movement, slow reactions, basic attacks only
- [ ] Normal: Track player, use cover, occasional specials
- [ ] Hard: Predict movement, combo attacks, optimal ability usage
- [ ] Reaction time variance by difficulty

### 3.2 AI Behaviors
**Tasks:**
- [ ] Pathfinding around obstacles
- [ ] Distance management (approach/retreat)
- [ ] Ability timing optimization

---

## Phase 4: UI/Menus (Gemini)

### 4.1 Main Menu
**Location:** HTML structure + CSS styling

**Requirements:**
- Game title with neon glow effect
- Buttons: "Single Player", "Multiplayer", "Controls"
- Animated background (subtle particles/grid)
- Hover effects on buttons (color shift, scale)

**HTML Structure needed:**
```html
<div id="main-menu" class="screen">
  <h1 class="title">BENIOFF vs McDERMOTT</h1>
  <p class="subtitle">DEATH MATCH</p>
  <div class="menu-buttons">
    <button class="neon-btn" data-action="single">Single Player</button>
    <button class="neon-btn" data-action="multi">Multiplayer</button>
    <button class="neon-btn" data-action="controls">Controls</button>
  </div>
</div>
```

### 4.2 Character Select Screen
**Requirements:**
- Two character cards side-by-side
- Benioff card: Blue/purple theme, cloud icon
- McDermott card: Orange/red theme, circuit icon
- Stats display (speed/health/attack bars)
- Special ability descriptions
- Selected state with glowing border
- "Ready" button

### 4.3 Arena Select Screen
**Requirements:**
- Three arena thumbnails in a row
- Each with distinct color scheme:
  - Cloud City: Purple/pink gradients
  - Data Center: Blue/cyan tech look
  - Boardroom: Gold/white corporate
- Arena name and brief description
- Selected state indicator

### 4.4 HUD (In-Game)
**Requirements:**
- Health bars (top corners)
  - Gradient fill (green → yellow → red based on health)
  - Character name and portrait icon
  - Pulsing animation when low
- Round timer (top center)
  - Countdown format (1:00, 0:59...)
  - Flashes red in last 10 seconds
- Special ability cooldowns (below health bars)
  - Icon + circular cooldown indicator
  - Ready state glow
- Round score (P1 wins / P2 wins)

### 4.5 CSS Theme
**Requirements:**
```css
:root {
  --bg-dark: #0a0a1a;
  --bg-medium: #1a1a2e;
  --neon-cyan: #00ffff;
  --neon-magenta: #ff00ff;
  --neon-gold: #ffd700;
  --neon-blue: #4a90d9;
  --neon-orange: #ff6b35;
  --text-primary: #ffffff;
  --text-secondary: #8888aa;
}

/* Neon glow utility */
.neon-glow {
  text-shadow: 0 0 10px currentColor, 0 0 20px currentColor;
  box-shadow: 0 0 10px currentColor, 0 0 20px currentColor;
}
```

---

## Phase 5: Visual Effects (Gemini)

### 5.1 Character Rendering
**Requirements:**
- Neon outline style (not filled sprites)
- Benioff: Blue glow, subtle cloud particles trailing
- McDermott: Orange glow, data stream particles trailing
- Animation states: idle, moving, attacking, hit, victory, defeat
- Size: approximately 40x60 pixels

### 5.2 Projectile Effects
**Requirements:**
- Neon Blaster: Cyan bullet with trail
- Plasma Grenade: Pulsing sphere, explosion particles
- Cloud Bolt: Lightning effect, branching
- Workflow Blast: Horizontal wave distortion

### 5.3 Arena Backgrounds
**Requirements:**
- Canvas layer behind gameplay
- Subtle animated elements (not distracting)
- Grid/particle effects
- Distinguishable cover objects with neon edges

### 5.4 Screen Effects
**Requirements:**
- Screen shake on heavy hits (intensity by damage)
- Flash on hit (white overlay, quick fade)
- Slow-motion on KO (0.5x for 0.5 seconds)
- Victory screen overlay with winner announcement

---

## Phase 6: Integration

### 6.1 Wire Everything Together
**Tasks:**
- [ ] Connect menu state changes to game initialization
- [ ] Pass character/arena selections to game
- [ ] Wire AI controller to player 2 in single player
- [ ] Connect HUD updates to game state
- [ ] Handle round/match progression

### 6.2 Game Flow
```
MENU → CHARACTER_SELECT → ARENA_SELECT → PLAYING → ROUND_END → [PLAYING or GAME_OVER]
                                    ↑_______________|
```

---

## Phase 7: Polish

### 7.1 Sound Effects (Optional)
**Tasks:**
- [ ] Blaster shot
- [ ] Grenade explosion
- [ ] Special ability activation
- [ ] Hit/damage
- [ ] Round start/end
- [ ] Victory fanfare

### 7.2 Final Testing
**Tasks:**
- [ ] Test all game modes
- [ ] Verify AI difficulty levels
- [ ] Check all controls work
- [ ] Test on Chrome, Firefox, Safari
- [ ] Balance testing (character fairness)

---

## File Sections in index.html

```html
<!DOCTYPE html>
<html>
<head>
  <title>Benioff vs McDermott</title>
  <style>
    /* GEMINI: All CSS styling here */
  </style>
</head>
<body>
  <!-- GEMINI: Menu HTML structures -->
  <div id="game-container">
    <canvas id="game-canvas"></canvas>
  </div>

  <script>
    // CLAUDE: Game engine code

    // === CONFIG ===
    // === UTILITIES (Vector2, etc) ===
    // === ENTITY CLASSES ===
    // === PLAYER CLASS ===
    // === PROJECTILE CLASSES ===
    // === AI CONTROLLER ===
    // === GAME STATE MANAGER ===
    // === INPUT HANDLER ===
    // === RENDERER ===
    // === GAME LOOP ===
    // === INITIALIZATION ===
  </script>
</body>
</html>
```

---

## Handoff Notes for Gemini

1. **Canvas size:** 960x640 pixels (16:10 aspect)
2. **Game objects rendered by engine:** Players, projectiles, cover objects
3. **UI elements you style:** Menus, HUD, overlays
4. **Color palette:** See CSS theme section above
5. **Test in browser:** Open index.html directly, no server needed
6. **Don't modify:** JavaScript sections marked with `// CLAUDE:`

---

## Progress Tracking

| Phase | Owner | Status |
|-------|-------|--------|
| 1. Core Engine | Claude | Not Started |
| 2. Game Objects | Claude | Not Started |
| 3. AI System | Claude | Not Started |
| 4. UI/Menus | Gemini | Not Started |
| 5. Visual Effects | Gemini | Not Started |
| 6. Integration | Both | Not Started |
| 7. Polish | Both | Not Started |
