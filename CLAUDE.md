# CLAUDE.md - AI Assistant Guide for Digital Deterrence Simulator

## Project Overview

This is a **Digital Deterrence Simulator** - an interactive, browser-based educational tool that explores the trade-offs between "software" and "steel" in national security resource allocation. Players act as a national security team for a small, globally connected state with limited resources.

**Purpose**: Teaching tool for strategic decision-making, designed to surface trade-offs, substitution limits, and second-order effects in defense and intelligence investments.

**Target audience**: Students, instructors, and professionals in national security, defense policy, and strategic studies.

## Codebase Structure

```
digital-deterrence-sim/
├── index.html      # Single-file web application (HTML + CSS + JavaScript)
├── README.md       # Project description (minimal)
└── CLAUDE.md       # This file - AI assistant guidance
```

### Architecture

This is a **single-page application** with no build process, dependencies, or backend:

- **HTML**: Semantic markup with sections for intro, simulation, and summary
- **CSS**: Embedded in `<style>` tag using CSS custom properties (variables)
- **JavaScript**: Embedded in `<script>` tag, vanilla JS with no frameworks

## Key Concepts

### Game Mechanics

1. **Turns**: 8 scenario-based turns representing escalating security challenges
2. **Budget**: 100 points per turn allocated across 4 categories
3. **Allocations**:
   - `AI-ISR`: Sensors, fusion, automation, counter-UAS analytics
   - `HUMINT`: Human intelligence, local networks, field presence
   - `Alliances`: Sharing, basing, joint warning, political cover
   - `Conventional`: Patrols, air defense readiness, manpower

4. **KPIs** (Key Performance Indicators tracked across turns):
   - `det`: Deterrence posture (0-100)
   - `warn`: Warning quality (0-100)
   - `res`: Resilience (0-100)
   - `esc`: Escalation risk (0-100, inverted - lower is better)
   - `rep`: Reputation/legitimacy (0-100)

### Stressors per Turn

Each turn has stress factors affecting outcomes:
- `threat`: Direct security threat level
- `cyber`: Cyber attack intensity
- `ambiguity`: Intelligence clarity
- `political`: Political/public pressure

## Code Organization (index.html)

| Section | Lines | Description |
|---------|-------|-------------|
| CSS Styles | 7-61 | Custom properties, responsive grid, UI components |
| HTML Markup | 63-242 | Three main sections: intro, sim, summary |
| Scenario Data | 247-296 | `TURNS` array with 8 scenarios |
| State Management | 299-307 | Game state variables (`idx`, `locked`, `kpi`, `history`) |
| DOM References | 309-356 | Element selectors |
| Helper Functions | 358-374 | `clamp()`, `sumAlloc()`, `colorFor()` |
| Render Functions | 376-429 | KPI bars, sliders, turn loading |
| Core Evaluation | 431-516 | `evaluateTurn()` - the simulation engine |
| Game Flow | 518-637 | Turn progression, commit, end game |
| Event Wiring | 639-661 | Button and slider event listeners |

## Development Guidelines

### Running the Application

Simply open `index.html` in any modern browser. No server required.

```bash
# Option 1: Direct file open
open index.html

# Option 2: Simple HTTP server (for development)
python3 -m http.server 8000
# Then visit http://localhost:8000
```

### Making Changes

1. **Styling**: Modify CSS custom properties in `:root` for theme changes
2. **Scenarios**: Edit the `TURNS` array to add/modify scenarios
3. **Game Balance**: Adjust coefficients in `evaluateTurn()` function
4. **UI Elements**: HTML structure in the `<main>` section

### Code Conventions

- **No external dependencies**: Keep the application self-contained
- **Vanilla JavaScript**: No frameworks; use standard DOM APIs
- **CSS Variables**: Use `var(--name)` for consistent theming
- **Semantic HTML**: Use appropriate elements (`section`, `button`, etc.)
- **Comments**: Section headers use `// ---------- Name ----------` format

### Testing

Manual testing only - no automated test suite. Key test scenarios:

1. Complete a full 8-turn game
2. Test edge cases (all budget to one category)
3. Verify CSV export generates valid data
4. Test responsive layout at different viewport sizes
5. Verify auto-balance and reset functions

## Important Functions

### `evaluateTurn(turn, alloc)`
The core simulation engine (lines 432-516). Calculates KPI changes based on:
- Allocation weights (normalized to 0-1)
- Turn-specific stress factors
- Strategic trade-off formulas
- Teaching moment penalties/bonuses

### `renderKPI()`
Updates the visual KPI bars with traffic-light coloring (green/yellow/red).

### `commit()`
Locks in allocation, evaluates turn, records history, shows outcomes.

### `toCSV()` / `downloadCSV()`
Exports game history to CSV for analysis.

## Design Decisions

1. **Single file**: Enables easy sharing and offline use for educational settings
2. **No framework**: Reduces complexity and ensures long-term browser compatibility
3. **Deterministic evaluation**: Same inputs produce same outputs (no randomness)
4. **Teaching moments**: Specific turns (6, 7, 8) have special outcome text to reinforce lessons

## Potential Enhancements

When asked to extend this project, consider:

- Keeping the single-file architecture if possible
- Maintaining the educational focus
- Preserving the existing game balance before adding features
- Testing thoroughly after changing `evaluateTurn()` coefficients
- Adding new scenarios to `TURNS` array for expanded gameplay

## Common Tasks

### Adding a New Scenario

```javascript
// Add to TURNS array
{
  id: 9,
  tag: "New Scenario Name",
  text: "Description of the situation facing the player...",
  stress: { threat: 25, cyber: 20, ambiguity: 30, political: 25 }
}
```

### Modifying KPI Starting Values

```javascript
// In State section (around line 303)
let kpi = {
  det: 50, warn: 50, res: 50, esc: 30, rep: 60
};
```

### Changing Color Theme

```css
/* Modify :root variables */
:root {
  --bg: #f6f7fb;      /* Background */
  --accent: #2563eb;  /* Primary action color */
  --good: #10b981;    /* Positive indicator */
  --warn: #f59e0b;    /* Warning indicator */
  --bad: #dc2626;     /* Negative indicator */
}
```

## Git Workflow

- `main` branch contains the stable version
- Feature branches should be prefixed with `claude/` for AI-assisted development
- Commit messages should describe the user-facing change

## Files to Never Commit

This project has no sensitive files, but general best practices:
- No `.env` files
- No credentials or API keys
- No large binary assets
