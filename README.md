# FitMap — Workout Planner

A single-file, browser-based workout planning application. No installation, no account, no internet required after the initial load. Open the HTML file in any modern browser and it works. Everything — exercises, programs, your personal schedule, saved blends, session history — is stored locally in your browser.

**Core idea:** an anatomical body heatmap that lights up as you build your workout, so you can see at a glance which muscles you're training and which you're neglecting. Paired with a nutrition blender, a weekly schedule, and a cardio planning system with dynamic configuration.

---

## Current Version: v1.0

**File:** `index.html` — 353KB, completely self-contained.

---

## Table of Contents

- [User Guide](#user-guide)
  - [Exercise Library](#exercise-library)
  - [Anatomical Muscle Heatmap](#anatomical-muscle-heatmap)
  - [Workout Builder](#workout-builder)
  - [Cardio Activities](#cardio-activities)
  - [Analysis Tab](#analysis-tab)
  - [Warm-Up & Cool-Down](#warm-up--cool-down)
  - [Weekly Schedule](#weekly-schedule)
  - [Programs](#programs)
  - [The Blender](#the-blender)
  - [Profile](#profile)
  - [Data Persistence](#data-persistence)
- [Developer Documentation](#developer-documentation)
  - [Architecture Overview](#architecture-overview)
  - [Design System](#design-system)
  - [Data Layer](#data-layer)
  - [Constant Maps](#constant-maps)
  - [Function Reference](#function-reference)
  - [State Variables](#state-variables)
  - [Key Rendering Patterns](#key-rendering-patterns)
  - [Health Check Script](#health-check-script)
  - [Known Technical Debt](#known-technical-debt)
- [Feature Candidates for Next Patch](#feature-candidates-for-next-patch)

---

## User Guide

### Exercise Library

- **180 exercises total** — 150 strength exercises across all major categories plus 12 cardio activity templates
- **Categories:** Chest · Back · Shoulders · Biceps · Triceps · Legs · Core · Full Body · Cardio
- **Equipment filters:** Barbell · Dumbbell · Cable · Machine · Bodyweight · Band/Rope · Kettlebell
- **Difficulty filters:** Beginner · Intermediate · Advanced
- **Search:** Live filtering by exercise name or category
- Every strength exercise includes: 6-step instruction guide, coaching tips, primary and secondary muscles targeted, equipment required, difficulty rating, and a YouTube tutorial thumbnail where available
- Every cardio activity includes: step-by-step technique guide, coaching tips, and sport-specific advice

---

### Anatomical Muscle Heatmap

- Front and back body views with clickable muscle regions
- Real-time thermal colour ramp as exercises are added: cool blue (untrained) → amber (moderate) → deep red (heavily targeted)
- Clicking any muscle region opens a suggestion panel with exercises that target that muscle
- Cardio-only sessions replace the heatmap with a full-body green glow and a pulsing heart indicator
- Zoom (scroll wheel) and pan (click and drag) with percentage indicator
- Double-click to reset position

---

### Workout Builder

- Drag exercises from the library or click to add
- **Strength exercises:** Sets / Reps / Weight (kg) inputs per exercise
- **Cardio exercises:** Dynamic configuration panel specific to each activity type — pace selectors, incline sliders, duration, distance, intensity — all feeding into a live calorie estimate
- Re-order exercises, remove individual items, view instructions from within the builder
- Save current session as a named custom program
- Schedule sync bar: blue indicator when a session was loaded from schedule; save changes back to the schedule with one click

---

### Cardio Activities

12 activity templates, each exposing configuration options relevant to that sport:

| Activity | Configuration |
|---|---|
| **Walking** | Pace (slow/brisk/fast/race-walk), incline slider 0–15%, duration, optional distance |
| **Running** | Type (easy/tempo/intervals/sprints), duration, distance |
| **Cycling** | Type (easy spin/steady state/hill climb/HIIT), duration, distance |
| **Rowing** | Intensity, duration, optional distance in metres |
| **Jump Rope** | Type (basic/fast/double-unders/alternating feet), duration, rounds |
| **Swimming** | Stroke (freestyle/breaststroke/backstroke/butterfly), pace, duration, distance |
| **HIIT Circuit** | Work interval, rest interval, rounds, estimated duration |
| **Elliptical** | Intensity, ramp incline slider, duration |
| **Stair Climber** | Step pace, duration, optional floors |
| **Assault Bike** | Protocol (steady/HIIT/max sprints), duration, rounds |
| **Kettlebell Swings** | Style (Russian/American/single-arm), duration, rounds |
| **Custom Cardio** | Free-text activity name, intensity, duration |

Calorie estimates are calculated from MET values × bodyweight (from Profile) × duration. Incline and intensity adjustments modify the MET multiplier in real time.

---

### Analysis Tab

- **Strength sessions:** Push/pull balance warning, leg volume check, muscle volume bar chart, missing muscle group alerts
- **Cardio sessions:** Total estimated calories, total duration, number of activities, dominant heart rate zone, bodyweight-based calculation
- **Mixed sessions:** Cardio stats block + full strength analysis beneath
- **Suggest Blend button:** Analyses your workout and recommends up to 3 preset blends from the library with reasoning

---

### Warm-Up & Cool-Down

- Auto-generated based on muscles in your current workout
- **Warm-up:** Dynamic movements specific to the muscles you're about to train
- **Cool-down:** Static stretches targeting the muscles you just trained
- Each item shows movement name, instructions, duration, and muscle targeted

---

### Weekly Schedule

- Full-screen week grid view showing Mon–Sun
- Today's date is highlighted with a red border
- Auto-fill from any program (strength or cardio) in one click
- Manual assign: Rest / Use current workout / Build custom / Any program day
- Day detail view: load to builder, mark complete (✓), change assignment, remove
- Session history: log of completed workouts with expandable details (sets, volume, duration)
- Schedule ↔ Builder sync: loading a scheduled day into the builder shows a sync bar; save changes back to the schedule with one click

---

### Programs

- **9 strength programs:** Beginner Full Body · PPL · Arnold Split · Upper/Lower · 5×5 Strength · German Volume Training · PHUL · Starting Strength · Athlete Performance
- **3 cardio programs:** Fat Burn Foundation (Zone 2, 4 days/week) · HIIT Shred (3 days/week intervals) · Active Recovery Week (7 days light movement)
- Each program shows expandable day previews with exercise names, sets, and reps
- Clicking any exercise name in the preview opens the instruction modal
- Load any day directly into the workout builder
- Custom programs can be saved and managed from the Programs tab

---

### The Blender

- **175+ ingredients:** 150 smoothie/superfood items + 25 gym food essentials
- **12 preset recipes** covering recovery, muscle building, fat burning, immunity, and more
- **Library:** Search and filter by category (Fruit, Vegetable, Dairy, Protein, Nut & Seed, Superfood, Liquid, Grain, Spice, Sweetener, Food, Extra)
- **Interactive glass:** SVG blender visualization with live liquid fill, colour mixing from ingredient combination, floating emoji ingredient bubbles inside the liquid
- **Physics:** Tilt/slosh effect when dragging the glass — liquid skews in the opposite direction of movement and springs back with damping
- **Drag and drop:** Library ingredient cards and suggestion bubbles are draggable onto the glass. Custom emoji ghost follows cursor. Pour animation flies the ingredient into the glass on drop.
- **Quick add chips:** When blender is empty, shows 6 suggested starter ingredients
- **Suggestion bubbles:** When blend has items, floating emoji bubbles below the glass suggest compatible ingredients (scored by yummy value + category match). Hover shows tooltip with calories, protein, yummy score. Click or drag to add.
- **Nutrition panel:** Macro totals (protein/carbs/fat), all nutrients, per-serving toggle, comparison against profile goals, key vitamins and compounds, health benefits, compatibility warnings
- **Yummy panel:** Yummy score 0–10, blend archetype name, flavour profile radar chart with clickable labels that suggest matching ingredients
- **Recipes panel:** 12 preset recipes + your saved blends. Each shows ingredients, nutrition summary, load button.
- **Save blend:** Name your blend, save it to the Recipes panel. Custom blends persist in localStorage.
- **Custom ingredients:** Add your own ingredients with full nutrition data via the "+ Add Ingredient" button
- **Export:** Copy recipe as formatted text

---

### Profile

- Name, age, weight (kg), height (cm), goal (Build Muscle / Lose Fat / Maintain / Athletic Performance / General Fitness), activity level
- Macro targets: protein, carbs, fat in grams — used in Blender nutrition comparison bars and cardio calorie estimates
- Data persists in localStorage

---

### Data Persistence

All user data is stored in the browser's localStorage:

| Key | Contents |
|---|---|
| `fitmap_workout_v2` | Current workout builder session |
| `fitmap_profile_v1` | Profile settings and macro goals |
| `fitmap_schedule_v1` | Weekly schedule assignments |
| `fitmap_log_v1` | Session history log |
| `fitmap_custom_progs_v1` | User-saved custom programs |
| `fitmap_recipes_v2` | User-saved blender recipes |
| `fitmap_custom_ings_v1` | User-created custom ingredients |
| `fitmap_panels_v1` | Panel width preferences |

---

## Developer Documentation

### Architecture Overview

FitMap is a **single-file HTML/CSS/JS application** with no build step, no framework, no external dependencies beyond Google Fonts. The entire application ships as one 353KB `.html` file that runs in any modern browser.

**Why single-file?**
Originally chosen for simplicity and portability — anyone can download one file and use the app. The modular source structure in `fitmap-src/` exists for development convenience; `build.py` concatenates all sources into the single deliverable.

**Structure:**

```
index.html (deliverable)
├── <head>         Google Fonts links
├── <style>        All CSS (~58KB)
├── <body>         All HTML markup
└── <script>       All JS (~284KB)
    ├── Data arrays (exercises, programs, ingredients, recipes)
    └── Application logic (139 functions)

fitmap-src/ (development sources)
├── build.py
├── head.html
├── body.html
├── styles/main.css
├── data/
│   ├── exercises.js
│   ├── constants.js
│   ├── programs.js
│   ├── warmups.js
│   ├── cooldowns.js
│   ├── ingredients.js
│   ├── food-ingredients.js
│   └── preset-recipes.js
└── scripts/app.js
```

**Build command:**

```bash
cd fitmap-src
python3 build.py
# → writes index.html
```

---

### Design System

**Design language:** Precision Dark — warm charcoal base, not cold black. Single primary accent. Monospace for data readouts. Generous whitespace.

**CSS Custom Properties (tokens):**

```css
/* Canvas */
--bg:       #111110   /* main background */
--bg2:      #181817   /* panel backgrounds */
--bg3:      #222220   /* input backgrounds */
--bg4:      #2c2c2a   /* hover surfaces */
--bg5:      #363633   /* active surfaces */

/* Borders */
--line:     rgba(255,250,235,0.06)   /* default border */
--line2:    rgba(255,250,235,0.11)   /* hover border */
--line3:    rgba(255,250,235,0.18)   /* focus border */

/* Text */
--ink:      #f0ebe0   /* primary text */
--ink2:     #7a7670   /* secondary text */
--ink3:     #3e3c38   /* muted text */

/* Accents */
--verm:     #e8422c   /* vermilion — primary action */
--verm2:    #f26b54   /* vermilion light — hover */
--amber:    #d4952a   /* warnings */
--celadon:  #3db88a   /* success / health / cardio */
--cerulean: #3e8fd4   /* info / schedule */

/* Geometry */
--r:  6px   /* card border-radius */
--rs: 4px   /* input border-radius */
--t:  .16s cubic-bezier(.4,0,.2,1)   /* transition */

/* Layout */
--left-w:  310px   /* left panel width (JS-overridable) */
--right-w: 300px   /* right panel width (JS-overridable) */
```

**Typography:**
- UI: `DM Sans` — weights 300/400/500/600/700
- Data readouts: `DM Mono` — weights 300/400/500
- Minimum text size: 9px (filter labels, metadata)
- Body text: 13px
- Card names: 13px/500
- Modal titles: 18px/600

**Layout:**

```css
.app {
  display: grid;
  grid-template-columns: var(--left-w) 1fr var(--right-w);
  grid-template-rows: 48px 1fr;
  height: 100vh;
}
```

Three resizable panels (left/center/right) plus a 48px topbar spanning all columns.

---

### Data Layer

#### Exercise Schema (Strength)

```javascript
{
  id: Number,           // unique, 1–150
  name: String,
  cat: String,          // from CATS array
  eq: String,           // from EQUIPS array
  diff: String,         // 'beginner' | 'intermediate' | 'advanced'
  pri: [String],        // primary muscles — keys from ML object
  sec: [String],        // secondary muscles — keys from ML object
  yt: String,           // YouTube video ID (optional)
  steps: [String],      // 6-step instruction array
  tips: [String],       // coaching tips array
}
```

#### Exercise Schema (Cardio Activity)

```javascript
{
  id: Number,           // unique, 151–162
  name: String,
  emoji: String,        // displayed in library and builder
  cat: 'Cardio',
  eq: String,
  diff: String,
  type: 'cardio',       // critical flag — drives all cardio-specific rendering
  basemet: Number,      // base MET value for calorie calculation
  config: [FieldSchema],// dynamic config panel definition (see below)
  steps: [String],
  tips: [String],
}
```

#### Cardio Config Field Schema

```javascript
// Select field
{ key: String, label: String, type: 'select',
  options: [{val: String, label: String, met: Number}],
  default: String }

// Slider field
{ key: String, label: String, type: 'slider',
  min: Number, max: Number, step: Number, unit: String,
  default: Number,
  metFn: String }   // e.g. "incline => 1 + incline * 0.09"
                    // evaluated as new Function() for MET multiplier

// Number input
{ key: String, label: String, type: 'number',
  min: Number, max: Number, unit: String,
  default: Number, optional: Boolean }

// Free text
{ key: String, label: String, type: 'text', default: String }
```

#### Program Schema

```javascript
{
  id: String,           // e.g. 'p1' (strength) or 'cp1' (cardio)
  name: String,
  diff: String,
  desc: String,
  days: [{
    label: String,
    rest: Boolean,
    exercises: [{
      exId: Number,
      sets: Number,
      reps: Number,
      note: String,
      cardioConfig: Object   // present only for cardio exercises
    }]
  }]
}
```

#### Ingredient Schema (Blender)

```javascript
{
  id: Number,           // 1–150 smoothie, 201–225 food
  name: String,
  emoji: String,
  cat: String,
  cal: Number,          // per 100g
  protein: Number,
  carbs: Number,
  fat: Number,
  fiber: Number,
  sugar: Number,
  color: String,        // hex — used for liquid color blending
  sweet: Number,        // 0–10 — used for yummy score
  acid: Number,         // 0–10
  bitter: Number,       // 0–10
  creamy: Number,       // 0–10
  benefits: [String],
  vitamins: [String],
  isCustom: Boolean,    // true for user-created ingredients
}
```

#### Workout Item Schema (in-memory + localStorage)

```javascript
// Strength
{ exId: Number, sets: Number, reps: Number, weight: String }

// Cardio
{ exId: Number, cardioConfig: { [key]: value } }
```

---

### Constant Maps

```javascript
const ML = { pec_major: "Chest", front_delt: "Front Deltoid", ... }
// 20 muscle keys → display labels. Used by heatmap and modal.

const CATS = ["Chest","Back","Shoulders","Biceps","Triceps",
              "Legs","Core","Full Body","Cardio"]

const EQUIPS = ["barbell","dumbbell","cable","machine",
                "bodyweight","band","kettlebell"]

const EQUIP_LABEL = { barbell:"Barbell", dumbbell:"Dumbbell",
                      cable:"Cable", machine:"Machine",
                      bodyweight:"Bodyweight", band:"Band/Rope",
                      kettlebell:"Kettlebell" }

const DIFF_LABEL = { beginner:"Beginner", intermediate:"Intermediate",
                     advanced:"Advanced" }

const DAY_LABEL = { mon:"Monday", tue:"Tuesday", ... }
// Used by schedule week grid.
```

---

### Function Reference

139 functions total. Key functions by category:

#### Data Access

| Function | Description |
|---|---|
| `getAllExercises()` | Returns `[...EXERCISES, ...CARDIO_ACTIVITIES]` — always use this instead of `EXERCISES` directly |
| `getAllPrograms()` | Returns `[...PROGRAMS, ...CARDIO_PROGRAMS, ...customPrograms]` |
| `getAllIngredients()` | Returns `[...INGREDIENTS, ...FOOD_INGREDIENTS, ...customIngs]` |
| `getFiltered()` | Applies current search/category/equipment/difficulty filters to `getAllExercises()` |

#### Heatmap & Body Figure

| Function | Description |
|---|---|
| `buildSVG(view)` | Generates the full anatomical SVG string for `'front'` or `'back'` view. Each muscle region gets a `data-m` attribute matching ML keys and a `muscle-region` class. 210×560 viewBox. |
| `setView(v)` | Switches front/back, rebuilds SVG, re-applies heatmap |
| `getMI()` | Calculates muscle intensity map — counts exercises per muscle weighted by primary (1.0) vs secondary (0.5) |
| `heatColor(t)` | Converts intensity 0–1 to CSS colour on the thermal ramp. 0=transparent, 0.3=amber, 0.7=orange, 1.0=deep red |
| `updateHeatmap()` | Applies heatColor to every `.muscle-region` element. If `isCardioWorkout()`, applies celadon glow + heart overlay instead |
| `isCardioWorkout()` | True when all workout items are cardio type |
| `hasCardioMix()` | True when at least one cardio item exists in mixed workout |
| `applyTransform()` | Applies current `panX`, `panY`, `zoomLevel` to `#svg-wrap` |

#### Exercise Library

| Function | Description |
|---|---|
| `renderExList()` | Renders filtered exercise cards into `#ex-list`. Guards `ex.pri\|\|[]` and `ex.sec\|\|[]` for cardio safety. Shows ❤️ Cardio tag for cardio activities. |
| `filterEx(val)` | Sets `searchVal` and re-renders |
| `setCatFilter(cat, btn)` | Toggles category filter button active state |
| `toggleEquip(eq, btn)` | Adds/removes from `activeEquips` Set |
| `toggleDiff(d, btn)` | Adds/removes from `activeDiffs` Set |
| `openSP(mk)` | Opens the muscle suggestion panel for muscle key `mk`. Renders primary and secondary exercises that target that muscle. |
| `closeSP()` | Closes muscle suggestion panel |

#### Workout Builder

| Function | Description |
|---|---|
| `addEx(id)` | Adds exercise to `workout` array. Cardio: initialises `cardioConfig` from activity's config schema defaults. Strength: `{sets:3,reps:10,weight:''}`. |
| `removeEx(i)` | Splices workout at index `i` |
| `renderWorkout()` | Renders all workout items. Routes to `renderCardioItem()` for cardio, inline template for strength. |
| `renderCardioItem(w, i, ex)` | Generates dynamic config panel HTML for a cardio workout item. Reads `ex.config` schema to decide which input types to render. Calls `calcCardioCalories()` for the live estimate. |
| `setCardioConfig(workoutIdx, key, val)` | Updates `workout[workoutIdx].cardioConfig[key]`, saves, re-renders workout and analysis |
| `calcCardioCalories(ex, cfg, weight)` | MET × bodyweight × duration/60. Iterates `ex.config` fields: select options set base MET, slider fields apply metFn multiplier via `new Function()`. |
| `renderAnalysis()` | Separate paths for cardio-only, strength-only, and mixed sessions |
| `scheduleSave()` | Persists `workout` to `fitmap_workout_v2` localStorage |

#### Modal (Instruction)

| Function | Description |
|---|---|
| `openModal(exId)` | Opens the full-screen instruction overlay. Guards all array accesses (`pri\|\|[]`, `sec\|\|[]`, `steps\|\|[]`, `tips\|\|[]`). Cardio: shows "Full body cardio" muscle pill instead of muscle map. Strength: shows YouTube thumbnail if `ex.yt` exists. |
| `closeModal(e)` | Closes on button click or overlay click |

#### Programs

| Function | Description |
|---|---|
| `renderPrograms()` | Renders all programs (strength + cardio + custom) as expandable cards with day previews |
| `toggleProg(id)` | Expands/collapses a program card |
| `toggleProgDayPreview(progId, dayIdx, btn)` | Expands/collapses the exercise list preview for a specific day |
| `loadProgramDay(progId, dayIdx)` | Loads a program day into `workout`. Uses `getAllPrograms()` to find the program. Preserves `cardioConfig` from program data for cardio exercises. |
| `saveCustomProgram(name)` | Saves current `workout` as a named custom program to localStorage |
| `deleteCustomProgram(id)` | Removes custom program by ID |

#### Schedule

| Function | Description |
|---|---|
| `openSchedule()` | Opens full-screen schedule overlay |
| `closeSchedule()` | Closes schedule overlay |
| `renderWeekGrid()` | Renders the 7-day week grid. Reads `scheduleData`, marks today's date, shows workout names and exercise count previews. |
| `autoFillWeek()` | Takes selected program, distributes days across the week grid matching the program's `schedule` array |
| `openDayDetail(dayKey)` | Shows the detail view for a specific day with load/mark done/change/remove actions |
| `markDone(dayKey)` | Marks day complete, logs the session to `fitmap_log_v1` |
| `assignDay(dayKey, progId, dayIdx)` | Assigns a program day to a schedule slot |
| `assignRest(dayKey)` | Assigns a rest day |
| `assignCurrentWorkout(dayKey)` | Saves the current builder workout to a schedule slot |
| `loadDayToBuilder(dayKey)` | Loads a scheduled workout into the builder, shows sync bar |
| `syncBackToSchedule()` | Saves current builder state back to the schedule slot it was loaded from |
| `renderHistory()` | Renders session log with expandable detail cards |

#### Blender

| Function | Description |
|---|---|
| `openBlender()` / `closeBlender()` | Open/close blender overlay |
| `blAddIng(id)` | Adds ingredient to `blendItems`, updates glass |
| `blRemoveIng(id)` | Removes ingredient from blend |
| `blUpdateGlass()` | Central update function: toggles empty state/glass visibility, calculates liquid fill level and colour, updates SVG elements, renders floating bubbles, renders ingredient rows, renders suggestion bubbles |
| `blendColor()` | Averages RGB hex colours of all blend ingredients weighted by amount |
| `calcYummy(ing)` | Yummy score: `sweet×0.4 + creamy×0.25 + (10-acid)×0.2 + (10-bitter)×0.15`, capped at 10 |
| `blendYummy()` | Weighted average yummy across all blend items |
| `blRenderLib()` | Renders ingredient library cards with search/category filter |
| `blRenderRight()` | Routes to Nutrition/Yummy/Recipes panel based on active tab |
| `blRenderNutrition()` | Calculates and renders macro totals, nutrient bars, compatibility warnings, health benefits, vitamins |
| `blRenderYummy()` | Renders yummy score, archetype, flavour radar (SVG polygon), clickable flavour labels |
| `blRenderRecipes()` | Renders preset + saved recipes as expandable cards |
| `blRenderSuggestionBubbles()` | Scores unblended ingredients by yummy + category match, renders top 8 as floating animated bubbles with tooltip on hover |
| `blGlassInitPan()` | Attaches mousedown/mousemove/mouseup handlers to the glass. Tracks velocity for tilt physics. |
| `blApplyLiquidTilt()` | Applies `skewX(${tilt}deg)` to `#bl-liquid-fill` for liquid physics |
| `blSpringLiquidBack()` | setInterval that decays `blLiquidTilt` by 72% per 16ms frame until < 0.2 |
| `blStartCardDrag(e, ingId, emoji)` | Custom drag implementation (not HTML5 drag API). Detects drag vs click using 4px threshold. Shows emoji ghost, highlights drop zone, fires `blDropAdd()` on release over `#bl-center`. |
| `blDropAdd(ingId, dropX, dropY)` | Creates a DOM particle at drop coordinates, animates it toward the glass center, then calls `blAddIng()` after 480ms |
| `blConfirmSave()` | Saves current blend as a named recipe to localStorage |

#### Blend Recommendations

| Function | Description |
|---|---|
| `getBlendRecommendations()` | Analyses current workout — extracts primary muscles, volume, cardio content — and scores preset recipes for compatibility. Returns top 3 with reasoning strings. |
| `renderBlendRecs()` | Renders recommendation cards in the Analysis tab |
| `openBlendFromRec(recipeId)` | Loads a preset recipe into the blender and opens it |

#### Workout Templates (from heatmap)

| Function | Description |
|---|---|
| `buildTemplateForMuscle(mk)` | Builds a balanced workout template for a clicked muscle: primary exercises for that muscle + antagonist exercises for balance |
| `loadTemplate()` | Confirms and loads the template into the builder |
| `closeTmpl()` | Closes the template preview modal |

#### Initialisation

| Function | Description |
|---|---|
| `init()` | Entry point. Calls: `loadWorkout()`, `loadProfile()`, `loadCustomPrograms()`, `loadBlenderData()`, `loadScheduleData()`, `bindClicks()`, `renderExList()`, `renderWorkout()`, `renderAnalysis()`, `renderWarmup()`, `renderCooldown()`, `renderPrograms()`, `renderHistory()`, `buildSVG('front')`, `setView('front')`, `updateHeatmap()`, panel width init |

---

### State Variables

```javascript
// Core workout state
let workout = []              // array of workout items
let scheduleData = {}         // { 'mon': {...}, 'tue': {...}, ... }
let sessionLog = []           // array of completed session objects
let customPrograms = []       // user-saved programs

// Library filter state
let searchVal = ''
let activeFilter = null       // category string or null
let activeEquips = new Set()
let activeDiffs = new Set()

// Body figure state
let panX = 0, panY = 0
let zoomLevel = 1
let isPanning = false
let panStartX = 0, panStartY = 0
let currentView = 'front'

// Blender state
let blendItems = []           // [{ingId, amount, unit}]
let customIngs = []           // user-created ingredients
let savedRecipes = []
let blSearchVal = ''
let blCatFilter = null
let blGlassZoomLevel = 1
let blGlassPanX = 0, blGlassPanY = 0
let blGlassIsPanning = false
let blLiquidTilt = 0          // current tilt angle for physics
let blTiltSpring = null       // setInterval handle for spring-back

// Schedule state
let currentDetailDay = null
let assigningDay = null
let scheduleSourceDay = null  // day key that was loaded into builder
```

---

### Key Rendering Patterns

**Template literal HTML generation:**
All rendering functions build HTML strings using template literals and assign to `element.innerHTML`. No virtual DOM, no diffing — full re-render on every state change. This is intentional for simplicity; the dataset is small enough that full re-renders are imperceptible.

**Cardio type guard pattern — must always be used:**

```javascript
// WRONG — crashes on cardio exercises which have no pri/sec:
ex.pri.map(m => ...)
ex.sec.slice(0,1).map(m => ...)

// CORRECT:
(ex.pri||[]).map(m => ...)
(ex.sec||[]).slice(0,1).map(m => ...)
(ex.steps||[]).map(s => ...)
(ex.tips||[]).map(t => ...)
```

**Cardio routing pattern:**

```javascript
const isCardio = ex.type === 'cardio';
if (isCardio) return renderCardioItem(w, i, ex);
// else: render strength item
```

**MET calorie calculation:**

```javascript
function calcCardioCalories(ex, cfg, weight) {
  let met = ex.basemet;
  ex.config.forEach(field => {
    const val = cfg[field.key] ?? field.default;
    if (field.type === 'select') {
      const opt = field.options.find(o => o.val === String(val));
      if (opt?.met) met = opt.met;
    }
    if (field.type === 'slider' && field.metFn && val != null) {
      const fn = new Function('incline', 'return ' + field.metFn.split('=>')[1]);
      met *= fn(parseFloat(val));
    }
  });
  return met * weight * duration / 60;
}
```

---

### Health Check Script

Run this after any code change to verify file integrity:

```python
import re
from collections import Counter

c = open('index.html').read()
script_start = c.find('<script>') + len('<script>')
js = c[script_start:c.find('</script>')]

issues = []
opens, closes = js.count('{'), js.count('}')
if opens != closes: issues.append(f"BRACE IMBALANCE: {opens} vs {closes}")
fns = re.findall(r'\nfunction (\w+)\(', js)
dupes = {k:v for k,v in Counter(fns).items() if v>1}
if dupes: issues.append(f"DUPLICATE FUNCTIONS: {dupes}")
bad = re.findall(r'/\* [^*]+ \*function', js)
if bad: issues.append(f"MALFORMED COMMENT MARKERS: {bad}")
if 'ex.pri.map' in js: issues.append("UNSAFE ex.pri.map (no cardio guard)")
required = ['getAllExercises','renderExList','renderWorkout','renderCardioItem',
            'calcCardioCalories','updateHeatmap','buildSVG','openModal','init']
for fn in required:
    if f'function {fn}(' not in js: issues.append(f"MISSING FUNCTION: {fn}")
strength = len(re.findall(r'\{id:\d+,name:"[^"]+",cat:"[^"]+",eq:', c))
if strength != 150: issues.append(f"WRONG EXERCISE COUNT: {strength}")
if c.count('type:"cardio"') < 12: issues.append("MISSING CARDIO ACTIVITIES")
if len(re.findall(r"id:'p\d+'", c)) != 9: issues.append("WRONG PROGRAM COUNT")
if len(re.findall(r"id:'cp\d+'", c)) != 3: issues.append("WRONG CARDIO PROGRAMS")
print("ISSUES:" if issues else "ALL PASS")
for i in issues: print(f"  * {i}")
print(f"File: {len(c)//1024}KB | Functions: {len(fns)} | Dupes: {len(dupes)}")
```

---

### Known Technical Debt

- **`new Function()` for metFn evaluation** — the slider MET multiplier uses `new Function()` to evaluate the `metFn` string from the config schema. Works but could be replaced with a named function lookup table.
- **Full re-render on every state change** — simple and fast enough now, but will become noticeable if the workout grows to 30+ items or the ingredient list is extended significantly.
- **No mobile layout** — the 3-column CSS grid is fixed-width. Below ~1000px, the layout breaks. `@media` queries and a tab-based mobile layout have not been implemented.
- **Single script tag** — all JS is in one `<script>` block. The modular split in `fitmap-src/` helps development but the deliverable is still a monolith. No module system, no tree-shaking.
- **No error boundary** — if any rendering function throws, the affected panel goes blank silently. The health check script catches structural issues but runtime errors during render require browser DevTools to diagnose.

---

## Feature Candidates for Next Patch

### High Priority

**1. Progressive Overload Tracking**
Each exercise in the workout builder shows "Last session: 80kg × 3×10 ↑" pulled from `sessionLog`. Requires reading the most recent log entry containing that `exId` and displaying the previous weight/sets/reps inline above the inputs.

**2. "Today's Session" Bar**
A persistent strip at the top of the workout builder showing today's scheduled session name (if assigned), with a single "Load" button. Bridges the gap between the schedule and the builder without opening the full schedule modal.

**3. Visual Design Depth Pass**
Subtle box shadows on cards, slightly different background levels between nested surfaces, more pronounced hover states, interactive elements that feel physically pressable. No functionality changes — pure CSS.

### Medium Priority

**4. HIIT Builder**
A dedicated interval design tool accessible from the Analysis tab when a cardio session is loaded. User sets work interval, rest interval, number of rounds, and chooses a base exercise. Renders a visual timeline of work/rest blocks with estimated total duration and calorie burn. Preset protocols: Tabata (20/10), EMOM, 30/30, Pyramid.

**5. Muscle Recovery Indicator**
Reads `sessionLog` to determine which muscles were trained in the last 48 hours. Shows a recovery state overlay on the body heatmap — muscles still recovering shown in cool blue with a clock icon. Warns in the Analysis tab when the current session re-trains a muscle that hasn't had 48h recovery.

**6. Workout Streak Tracker**
Counts consecutive weeks with 3+ completed sessions from `sessionLog`. Shows a streak counter on the topbar. Simple to implement, strong habit-forming effect.

### Lower Priority

**7. Mobile Layout**
Responsive breakpoints below 1000px. The 3-column grid collapses into a tab-based layout: Library tab, Workout tab, Analysis tab. The body figure moves to a collapsed panel with expand-on-tap.

**8. Share Workout via URL**
Base64-encode the current `workout` array into a URL parameter. Anyone opening that URL gets the workout loaded automatically. Zero backend required.

**9. Print / Export Workout**
A `window.print()` triggered layout showing the workout as a clean, ink-friendly sheet — exercise name, sets, reps, weight, notes, warm-up, cool-down. CSS `@media print` stylesheet.

**10. Blend of the Day**
On opening the Blender, if it's empty, auto-suggest a featured blend based on the day of the week or the most recent workout.

---

*Built with Claude (Anthropic) — May 2026*
