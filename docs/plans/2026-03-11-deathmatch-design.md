# Benioff vs McDermott: Death Match

A neon cyberpunk top-down arena shooter featuring tech CEO battles.

## Overview

- **Genre:** Top-down arena shooter
- **Visual Style:** Neon cyberpunk (dark backgrounds, glowing effects)
- **Modes:** Single player (vs AI) + Local multiplayer (2 players, 1 keyboard)
- **Tech:** HTML5 Canvas + JavaScript (single HTML file)

## Characters

### Shared Weapons
1. **Neon Blaster** - rapid-fire laser, medium damage
2. **Plasma Grenade** - throwable, area damage
3. **Dash** - quick burst, brief invincibility

### Benioff (Salesforce)
- **Cloud Bolt** - lightning strike, tracks enemy
- **Ohana Shield** - temporary damage absorption
- Stats: Speed 4/5, Health 3/5, Attack 3/5

### McDermott (ServiceNow)
- **Workflow Blast** - cone wave, pushes back
- **Automation Drone** - auto-attacking drone for 5s
- Stats: Speed 3/5, Health 4/5, Attack 4/5

## Arenas

1. **Cloud City** - Floating platforms, purple/pink neon, Salesforce tower
2. **Data Center** - Server rack cover, blue/cyan neon, data streams
3. **Boardroom** - Conference table obstacles, gold/white neon, skyline

## Controls

**Player 1:**
- Move: WASD
- Weapons: 1/2/3
- Specials: Q/E

**Player 2:**
- Move: Arrow keys
- Weapons: 7/8/9 (numpad)
- Specials: ,/. (comma/period)

## Game Flow

1. Main Menu → Character select → Arena select → Match
2. Best of 3 rounds, 60 seconds per round
3. Win: Deplete health OR most health at timeout

## Implementation Split

| Component | Owner |
|-----------|-------|
| Game engine (physics, collision, AI) | Claude |
| Rendering system | Claude |
| UI/Visual design (menus, effects) | Gemini |
| Character sprites/assets | Gemini |
| Sound effects | TBD |

## File Structure

```
/
├── index.html          # Main game file
├── CLAUDE.md           # Project instructions
├── README.md           # User documentation
├── docs/
│   └── plans/
│       └── 2026-03-11-deathmatch-design.md
└── assets/             # (optional) sprites, sounds
```

## Scope Boundaries (Not Included)

- Online multiplayer
- Unlockables/progression
- Mobile support
- Saved settings
