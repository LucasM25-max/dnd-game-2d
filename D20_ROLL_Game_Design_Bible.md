# D20: THE ROLL
### A 2D Dungeons & Dragons Adventure RPG — Game Design Bible, v0.1

**Ruleset:** D&D 2024 (*Player's Handbook* 2024, *Dungeon Master's Guide* 2024) + *Monster Manual* 2025
**Platform:** **Browser-native.** No game engine. Pure code — TypeScript, open-source libraries only — living in a public GitHub repository and deploying as a static site to Vercel / Netlify / Cloudflare Pages. **The entire codebase is written by AI, in-conversation.** See Part III.
**First campaign:** *Phandelver and Below: The Shattered Obelisk*, Chapter 1 — "A Dangerous Journey" — packaged as **"The Tutorial"**
**Scope of this document:** Full vision, art direction, core mechanics, complete rules-to-game translation, and a beat-by-beat design of the goblin ambush. **Stops at the discovery of the goblin trail.** Nothing in the Cragmaw Hideout is designed here.
**Author note:** Every number marked ✅ was read out of the four attached source files during the writing of this document. Numbers marked ⚠️ come from knowledge of the ruleset and are **flagged for verification** — the attached *Monster Manual* and parts of the *Player's Handbook* are 5e.tools exports in which stat blocks, class tables and glossary entries are collapsed placeholder embeds, not plain text. See **Appendix A** for the full verification list.

---

## CONTENTS

- **PART I — THE PILLARS** — What this game is, and the six laws it will not break
- **PART II — ART DIRECTION: "EMBERLIGHT"** — The chosen style, and why it is the correct one
- **PART III — THE MACHINE** — A web-native, AI-buildable architecture: the stack, the simulation model, and what the browser does for us
- **PART IV — BASIC MECHANICS** — The five systems everything else is built from
- **PART V — CHARACTER CREATION** — Human Fighter, Soldier, from the campaign selector to the first swing
- **PART VI — VIDEO-GAMEIFYING THE RULES** — Every core rule, and what it becomes on screen
- **PART VII — ONE GAME, NOT THREE** — How exploration, social and combat are the same system
- **PART VIII — "THE TUTORIAL" CAMPAIGN** — The goblin ambush, beat by beat
- **PART IX — MOTION** — The complete animation and VFX production plan
- **PART X — SOUND**
- **PART XI — UI, HUD & ACCESSIBILITY**
- **PART XII — BUILDING THE NEXT CAMPAIGN** — Data architecture and extensibility
- **PART XIII — ROADMAP, RISKS & LEGAL**
- **APPENDIX A** — Source verification table
- **APPENDIX B** — The numbers of the ambush
- **APPENDIX C** — Data schema sketches

---
---

# PART I — THE PILLARS

## 1.1 The pitch

> **You are a soldier. You are on a road. A d20 is about to decide whether you live.**
>
> *D20: The Roll* is a hand-painted 2D action-RPG that plays a real game of Dungeons & Dragons 2024 in real time — where the die is not a hidden random number generator but the star of the show, where talking is a fight you win with the right weapon, and where you never once see a "Combat Mode." You walk, you look, you talk, you swing, and the ruleset underneath never blinks.

Most D&D video games do one of two things: they hide the rules to make a smooth action game, or they expose the rules to make a faithful spreadsheet. This game does neither. It makes **the ruleset itself the aesthetic.** Every beautiful thing on screen is a rule made visible: the flicker of a torch is a *Bright Light* radius, the ghosting of a goblin's outline is *half cover*, the sudden doubling of a die in the air is *Advantage*. If you learned nothing about D&D before you played, you would learn the whole game by watching it.

## 1.2 The Six Pillars

Every design decision in this document is judged against these six. If a feature contradicts a pillar, the feature dies.

### Pillar 1 — THE DIE IS THE DRAMA
The d20 is the protagonist of this game, not a subsystem. A roll is a **moment**: the world slows, the die leaves the UI and enters the world, it tumbles with real physics, it lands, and the number detonates into the fiction. Crits are gold. Misses whisper. Nat 1s crack the die in half. Players will lose fights on purpose just to see the roll. Nothing in the game is resolved offscreen: if a number decided something, the player *saw* the number.

### Pillar 2 — ONE BREATH, NOT THREE MODES
There is no exploration mode, no dialogue mode, no combat mode. There is **the world**, and the world has a tempo. Combat is simply the world at 6 seconds per round instead of 6 minutes per hour. Dialogue is simply combat where the battlefield is a person's mind and the weapon is Charisma. Exploration is simply combat where the enemy is a DC. The transition between them is a camera move and a music crossfade, never a screen change, never a load, never a menu.

### Pillar 3 — EVERY RULE IS A VERB
No rule lives in a tooltip. If D&D says you can shove a creature, the game gives you a button and an animation. If D&D says you can knock a goblin out instead of killing it, that is a *decision you make with your thumb*, not a checkbox. Weapon Masteries, Cover, Opportunity Attacks, Death Saves, Difficult Terrain, Prone, Help, Hide, Ready, Dodge — all of them are inputs with animations and consequences. **A rule that is not a verb is a rule the player will never discover.**

### Pillar 4 — THE WORLD IS A DM
The game is directed by a Dungeon Master simulation, not a scripted event list. It decides what the goblins do, whether the rain starts, whether the last goblin flees or fights, whether the NPC's attitude hardens. It follows the DMG's own advice: give the players clear objectives, provide multiple ways to progress, and let the adventure move even when they fail. **There is no way to soft-lock this game.** Every failure state is a new scene.

### Pillar 5 — CONSEQUENCE IS PERSISTENT
Everything writes to a single world-state ledger. Knock a goblin out and it tells you the truth; kill all four and you learn nothing. Spare a goblin and it may warn the hideout. Take the snare trap and you are *Restrained* and loud for the next minute. The game never resets the board between scenes, and it never lets the player forget what they did.

### Pillar 6 — MOTION IS MEANING
Nothing static. Fire breathes. Dust drifts. Torches flicker on the real light-radius boundary. Parchment unfurls. Ink writes itself. A goblin's ear twitches before it attacks. Every frame of animation is carrying information about the rules or the fiction — and when it isn't, it is carrying beauty. **The game should look like a painting that is secretly a machine.**

## 1.3 The three promises to the player

1. **You will always know why you won or lost.** Every outcome is auditable back to a die, a modifier, and a rule name.
2. **You will never be forced to fight.** Every encounter in the Tutorial has at least three non-combat resolutions, and the game rewards the clever one.
3. **You will be able to play real D&D at a real table after finishing the Tutorial** without anyone having to teach you the rules.

---
---

# PART II — ART DIRECTION: "EMBERLIGHT"

## 2.1 The choice, and the argument

I was asked to pick. Here is what I picked and why.

**Style: EMBERLIGHT — a hand-painted, side-lit, ¾-overhead 2D world rendered as a living illuminated manuscript, in which a single warm light source carves readable silhouettes out of a cool, painterly dusk.**

Not pixel art. Not silhouettes. Not photoreal 2.5D. **Emberlight.**

The reasoning is not aesthetic preference — it is that this is the only style in which the D&D ruleset is *legible at a glance*, which is the entire premise of the game.

### The five tests a style must pass

**Test 1: Can it render the light rules?**
D&D 2024's exploration layer is built on light. The PHB defines **Bright Light**, **Dim Light** (a *Lightly Obscured* area: Disadvantage on sight-based Wisdom (Perception) checks) and **Darkness** (a *Heavily Obscured* area: effectively the Blinded condition when trying to see into it). ✅ This is not flavour text — it is the game's core stealth and perception math.

Pixel art cannot render a light-radius gradient with enough fidelity for the player to *read* where the boundary falls; the player must know to the tile whether they are in Bright or Dim light, because that is the difference between seeing a trap and stepping in it. Photorealistic 2.5D renders light beautifully but destroys silhouette and readability. **Emberlight is built entirely out of one warm light carving shapes out of a cool field — the light *is* the art direction, so the rule and the picture are the same object.** The torch radius is a brushstroke. The player learns "outside the amber = Disadvantage" without ever reading the rule.

**Test 2: Can it hold a silhouette at 1080p and at 4K?**
Combat needs instant reads: is that goblin *Prone*? Is it behind *Three-Quarters Cover*? Is it *Hidden*? Emberlight's answer is a strict three-value shape language — **dark mass, warm rim light, single accent colour per faction** — so a state change is always a change in the *outline*, which is the fastest thing the human eye reads. Pixel art loses silhouette at scale; painterly realism loses it in clutter.

**Test 3: Can it be animated at production scale?**
This game's promise is "fully animated with everything." That is hundreds of animation states across humans, goblins, wolves, horses, oxen, wagons, fire, water, foliage, weather and UI. Emberlight's characters are **flat hand-painted shapes with internal line and a lit rim** — which means they can be rigged as 2D skeletal puppets with painted deformations rather than drawn frame-by-frame for every action. That is the only way "fully animated" survives contact with a budget. (Compare: true frame-by-frame pixel art at this fidelity would need roughly 8–14 hand-drawn frames per animation state; skeletal Emberlight needs 1 painted puppet and a rig. **See Part IX for the full math.**)

**Test 4: Does it photograph well?**
This game lives and dies on screenshots and trailer frames. A painterly, high-contrast, warm-cool split image is the single most screenshot-friendly art style in existence — it reads at thumbnail size, which pixel art does not and photoreal 2.5D does not.

**Test 5: Does it make D&D feel like D&D?**
D&D's visual heritage is the **book illustration**: the plate inside the cover, the spot illustration in the margin, the candle-lit table. Emberlight is literally that — the game world is lit like the inside of an illuminated manuscript, and the UI is made of the same paper. When you open your character sheet, you are looking at the same parchment the world is painted on. The game feels like *a book you fell into*.

### Rejected alternatives, and why

| Style | Why not |
|---|---|
| **High-res pixel art** | Beautiful, nostalgic, and wrong. Pixel grids fight the 5-foot grid — the player cannot tell whether a tile is in Dim Light or Bright Light when the tile boundary is itself a pixel-art decision. It also caps the animation ambition (frame-by-frame at scale) and dates the game on release. |
| **Silhouette / ink-wash 2.5D** | Superb readability and cheap to animate, but it collapses the *painterly richness* the brief demands. It cannot carry the "incredible looking" requirement across a whole campaign, and it cannot render the warm/cool light rules — it only has one value. Held in reserve as the **memory/flashback render mode** (see 2.9). |
| **Painterly realism (Diablo-lineage)** | Reads as generic action-RPG, loses silhouette in clutter, and is the most expensive style to animate by a wide margin. |
| **Flat vector / stylised flat** | Cheap and clean but reads as mobile-game. Cannot carry the "incredible" brief. |

## 2.2 The Emberlight look, concretely

### The palette: "Parchment, Ember, Verdigris, Ink"

Four colour families, no others. Every asset in the game is mixed from these.

| Family | Role | Values (approx) |
|---|---|---|
| **Parchment** | Base world tone, UI ground, fog, sky | `#E8D9B5` → `#B99F6E` → `#8A7448` |
| **Ember** | All *light*, all safety, all player-side energy, all crits | `#FFB347` → `#FF7A18` → `#C2410C` |
| **Verdigris** | Cold, shadow, threat, goblin-side energy, poison, magic | `#7FD1C0` → `#3E8E7E` → `#123B36` |
| **Ink** | Line, silhouette, UI text, death | `#1A1512` → `#2E2620` → `#4A3F35` |

**The rule that makes the game readable:** warm = known and safe. Cold = unknown and hostile. The player is always lit from the ember side; goblins are always rimmed in verdigris. A goblin standing in a torch's Bright Light is rimmed *amber* — and that is how the player learns, without reading anything, that **it can now see me too.**

Accent exceptions, used sparingly and always with meaning:
- **Gold leaf** (`#F5D06F`): critical hits, level-ups, magic items, Heroic Inspiration. Applied like actual gold leaf in a manuscript — flat, slightly raised, catching light.
- **Oxblood** (`#8E2A22`): damage numbers, Death Saves, the Redbrands.
- **Bone** (`#F2EDE3`): the die, UI highlights, text.

### The line: "Illuminated, not inky"

Characters and foreground props are drawn with a **variable-weight hand-drawn line** — thick on the shadow side, thin on the light side, breaking entirely where the rim light is brightest. Backgrounds have **no line at all**: they are pure painted mass. This single choice does enormous work: it makes the *interactive* layer (characters, props, objects) separate instantly from the *scenery* layer, at any zoom, in any lighting.

### The render stack (bottom to top)

```
7  WEATHER FX        rain sheets, drifting mist, embers, god rays, dust motes, insects
6  UI-LAYER FX        floating damage numerals, dice, roll banners, nameplates
5  FOREGROUND        blurred grass, hanging branches, rain-rippled puddle reflections, parallax 1.4x
4  ACTORS + PROPS     player, NPCs, monsters, wagon, horses, interactables  (parallax 1.0x)
3  GROUND             the painted tactical plane: road, dirt, rock, water   (parallax 1.0x)
2  MIDGROUND          embankments, thickets, trunks, ruins, treeline         (parallax 0.7x)
1  FAR                 hills, distant treeline, sky gradient, clouds          (parallax 0.35x)
0  SKY / VOID          the paper. Literally parchment where the world ends.
```

Layers 1–2 parallax at reduced rates; layer 5 parallaxes at 1.4x and is heavily blurred (f/1.4-equivalent). **The world's edges fade to parchment**, not to black — which is the single strongest signal of the art direction and costs nothing.

### The camera

¾-overhead at roughly **32° from horizontal**, orthographic, with a subtle hand-drawn barrel distortion at the frame edge (2–3%) that makes everything feel like it's painted on a curved page. Three zoom stops, all on one continuous spline (see Pillar 2 — no mode switches):

| Stop | Height | Name | Used for |
|---|---|---|---|
| **Z1** | Ground | **THE EYE** | Character level. Conversations, cinematics, resting, character creation. |
| **Z2** | 12 ft | **THE TABLE** | Default. Exploration and combat. The full 5-ft grid is implied by painted ground detail, not drawn. |
| **Z3** | 60 ft | **THE MAP** | Overland, marching order, tracking, region map. The world literally becomes a hand-drawn map as you zoom out — ink lines draw themselves over the painting. |

The zoom from Z2 to Z3 is the game's signature move: the painted forest *becomes* the illustrated map of it, brushstroke by brushstroke, in ~0.8 s.

## 2.3 Light as the primary art system

This is the technical heart of the style, so it gets its own section.

Every scene is lit by a **light field** computed on a coarse grid (2.5 ft resolution, i.e. half a D&D tile), producing per-tile values of `BRIGHT | DIM | DARK`. Light sources are painted *and* simulated:

- A **torch** = a hand-painted flame sprite with 3-frame flicker + a radial gradient in the light buffer. **Verified: a Torch burns for 1 hour, casting Bright Light in a 20-foot radius and Dim Light for an additional 20 feet** ✅ — and it doubles as a Simple Melee weapon dealing 1 Fire damage on a hit ✅, which is a verb the player will absolutely find.
- A **candle** = **Bright Light in a 5-foot radius, Dim Light for an additional 5 feet, for 1 hour** ✅. The smallest light source in the game, and the one that lights your journal at night.
- A **lantern (hooded / bullseye)** = a cone, not a sphere. Hood down = radius; hood up = a 5-ft cone. *(Exact radii ⚠️ — the lantern entries did not export as plain text; see Appendix A.3.)*
- A **campfire** = modelled on the torch scale (≈20 ft Bright / 40 ft Dim ⚠️ confirm), plus animated ember particles rising, plus a warm bounce light painted onto nearby ground.
- **Lighting anything is a Bonus Action with a Tinderbox; lighting any other fire takes 1 minute** ✅ — so striking a lantern mid-fight is a real tactical cost, and the game will make you feel the 1 minute.
- **Daylight** = a directional warm key from the sky layer + a cold ambient fill from the shadow side. Every object casts a soft, hand-authored shadow shape (not a real-time shadow map — hand-authored shadows *look* painted, which is the point).
- **Darkvision** (⚠️ 60 ft, verify) = a separate render pass: the world is redrawn in **monochrome verdigris-to-bone** with no warm light at all, and creatures emit a faint cold outline. This makes Darkvision *legible as a different way of seeing*, not just a brightness slider.

**The presentation rule:** the light-field boundary is **always visible** as a subtle painterly falloff. The player can see the exact tile where Bright becomes Dim. Then, when they hover the Perception prompt, the game draws a soft amber wash over the Bright tiles and a cool wash over the Dim tiles — *the rule and the picture are the same thing*, forever.

## 2.4 Character design language

### Humans (the player, and the folk of the North)
- **Shape:** grounded rectangles. Broad shoulders, planted feet, weight low. Soldiers are drawn *asymmetric* — one shoulder higher, one hip cocked — so they never look like mannequins.
- **Materials:** wool, boiled leather, worn mail, oiled steel. Everything has a **worn edge** — paint the chipping, never draw a clean line on metal.
- **Colour:** parchment-and-ink base with **one** ember accent per character (a scarf, a shield boss, a helmet plume, a cloak clasp). This is the "hero accent" and it is the thing the player's eye tracks in a melee.

### Goblins (the Cragmaw band)
- **Shape:** triangles and hooks. Oversized ears, jutting chins, spindly limbs, hunched silhouette. **The silhouette must be identifiable at 20% scale and in silhouette alone** — this is a hard art-test requirement.
- **Materials:** scavenged. Rust, raw hide, teeth, bone, mismatched leather. Every goblin wears something stolen from a victim — that's a storytelling detail the player can read.
- **Colour:** verdigris skin (`#4E8A7A` family) with an oxblood accent. **Cragmaws file their teeth to jagged points** ✅ (stated in the adventure text) — this must be visible in the portrait and in the attack animation's open-mouth frame.
- **Motion signature:** goblins never walk. They **skitter** — quick, twitchy, always slightly off-balance, with a bounce in the idle. Bugbears, when they arrive in later chapters, move with heavy weight and long recovery frames. Motion alone tells you what you're fighting.

### The horse and the oxen
The dead horses are the emotional centrepiece of the opening scene and are the most-painted assets in the Tutorial. They are not props. They get full anatomy studies, three poses of collapse, and a wind-blown mane pass. See 8.5.

## 2.5 Typography

| Use | Face | Notes |
|---|---|---|
| **Display / titles** | A hand-lettered blackletter-adjacent display face (custom, 2 weights) | Used only for campaign titles, chapter cards, level-up banners. Never for body text. |
| **Body / UI** | A warm humanist serif with strong small caps | Must be legible at 14 px. **OpenDyslexic toggle available.** |
| **Numbers / dice** | A slab-numeric with tabular figures | Damage numbers, DCs, totals. Tabular alignment matters because numbers stack in the roll ledger. |
| **Die faces** | Custom hand-cut numerals | The "20" is drawn with a gold-leaf inlay. The "1" has a hairline crack running through it — you only ever see it up close, on a nat 1. |

**Rule:** the die's numerals are the only text in the game that is *part of the world*. Everything else is UI.

## 2.6 The die itself

The d20 is the game's mascot, its logo, and its most-animated single asset.

- **Material:** carved bone with hand-cut inlaid numerals in oxblood, except the 20 which is gold leaf. Slight subsurface scattering so it glows faintly from within when in play.
- **Physics:** real rigid-body tumbling, with a scripted "settling" bias so it always comes to rest readable and always faces the camera on the last bounce. It does not cheat the result — the result is rolled first, then the die is *animated to match*.
- **States:** `IDLE` (resting in the die tray, slow breathing rotation), `THROWN` (arc + spin), `ROLLING` (tumble with motion blur), `SETTLING` (two bounces, slows), `REVEALED` (locks, glows, the numeral scales up 1.15× and holds 0.35 s), `CRIT` (gold leaf ignites across the faces, a shockwave ring, the numeral punches), `NAT1` (a hairline crack races across the die, the numeral flickers, a low cello drop).
- **Advantage/Disadvantage:** a second die appears — mirrored, spinning the *opposite* direction. One is lit gold-rimmed (Advantage), the other rust-rimmed (Disadvantage). Both land. **The losing die visibly crumbles to dust.** That single animation teaches the entire rule.
- **Heroic Inspiration:** a third die, small, gold, hovering above the pair, waiting.

## 2.7 Concept direction — key frames

Six frames that define the game. These are the pieces to commission first, because everything else is derived from them.

1. **"THE MUSTER"** — Neverwinter's gate at dawn. Parchment sky, cold blue shadow, a single warm banner. A soldier's back to camera, kit laid out on a barrel. Z1 camera. Establishes the UI-as-parchment idea.
2. **"THE ROAD"** — Z2, the Triboar Trail cutting diagonally through a painted forest. God rays through the canopy in hard ember shafts. The wagon small in frame. Establishes scale, parallax and the light system.
3. **"THE BEND"** — the ambush site. The two riderless horses in the middle of the road, saddlebags spilled, an empty leather map case in the dirt. Four *almost-invisible* goblin silhouettes in the thickets, readable only by their verdigris rim light. The most important frame in the game.
4. **"THE ROLL"** — time frozen mid-ambush. The die hanging in the air at the centre of the frame, the world motion-blurred around it, a goblin's scimitar a smear of verdigris. This is the key art.
5. **"THE INTERROGATION"** — Z1. A bound goblin, one hero accent light on its face, the player's silhouette in the foreground holding a lantern. Warm/cold split down the middle of the frame. Establishes that conversation is lit like combat.
6. **"THE SNARE"** — the player hanging upside down from a tree, the world rotated 180° on screen, the *Restrained* condition icon burning in the margin. Establishes that the UI and the world share one physics.

## 2.8 The "everything is animated" standard

A hard, checkable bar for every asset in the game. If an asset does not meet it, it is not finished.

**Tier A — Fully simulated (the player, key NPCs, monsters):**
Skeletal 2D rig, minimum 6 deformation bones per limb group, cloth and hair on secondary dynamics, facial rig with 12 blend shapes, minimum **3 idle variations** that cycle randomly with weight, blink on a 2.4–5.1 s random interval, breathing offset on the chest bone, and a unique **silhouette-changing tell** before every attack.

**Tier B — Cycle-animated (props, foliage, fire, water):**
Minimum 3-frame hand-authored loop (never a tween loop), with per-instance phase offset so nothing in a scene pulses in sync, plus a wind-reaction pass driven by a global wind vector.

**Tier C — Ambient (background layers, sky, weather):**
Continuous shader-driven motion: parallax, UV drift on clouds, animated noise on mist, particle systems for motes/embers/insects, and a slow global "breathing" exposure change (±2%) so no frame of the game is ever perfectly still.

**Tier D — UI:**
Every panel unfurls physically. Every number counts up. Every quest entry writes itself in ink, left to right, with a nib-shaped cursor. Every tab has a page-curl. **No element in the UI may ever appear instantly.**

## 2.9 Special render modes

| Mode | Look | Used for |
|---|---|---|
| **Emberlight** (default) | Full palette, warm/cool split | Everything |
| **Darkvision** | Monochrome verdigris-bone, cold creature outlines | When a character with Darkvision is the "eyes" of the party (later campaigns; in the Tutorial, shown in the Codex demo) |
| **The Sight** | World desaturates to parchment, only *hidden* things glow verdigris, only *traps* glow ember | Triggered on a successful Search / trap detection. A 0.6 s reveal wash. |
| **Inkwash** | Everything collapses to silhouette on parchment; only the die keeps colour | Flashbacks, Heroic Inspiration moments, the nat-20 kill frame |
| **The Map** | The world renders as a hand-drawn cartographer's map, ink drawing itself in | Z3 zoom, tracking, region map, journal |

---
---

# PART III — THE MACHINE

## 3.1 The non-negotiable architectural decision

**The rules engine and the game are two different programs that talk through an event log.**

Both live in one repository. Both are TypeScript. **Neither depends on a game engine.**

```
┌─────────────────────────────────────────────────────────────────────┐
│  @d20/client   — the game the player sees          (Vite app)       │
│  PixiJS renderer · custom shaders · skeletal rig · Web Audio        │
│  DOM/Lit panels · input · camera · particles · IndexedDB saves      │
│                    ▲  consumes GameEvents (typed, JSON)             │
│                    │  issues Intents (typed, JSON)                  │
├─────────────────────────────────────────────────────────────────────┤
│  @d20/rules  — "THE DM"                             (npm package)   │
│  PURE · DETERMINISTIC · HEADLESS · ZERO DEPENDENCIES                │
│  state · seeded dice · actions · initiative · conditions · cover    │
│  light field · AI director · world-state ledger · Zod validation    │
│                                                                     │
│  Runs identically in Node (tests), in the browser (game), and in a  │
│  Web Worker (Fate's Eye — 400 simulations off the main thread).     │
└─────────────────────────────────────────────────────────────────────┘
```

`@d20/rules` is a standalone npm package with **no imports at all** — no PixiJS, no DOM, no `window`. That is a hard rule enforced by lint. It means the entire D&D 2024 ruleset is testable in Node in milliseconds, publishable to npm on its own, and completely insulated from every rendering decision the client ever makes.

This is not an engineering nicety. **Four entire features of this game are impossible without it:**

1. **The Roll Ledger** — every outcome must be reproducible and auditable, which requires a pure event log.
2. **Fate's Eye** (see 3.5) — the ability to pause, simulate a proposed action forward without committing, and see the *range* of outcomes. Requires a forkable, side-effect-free simulation.
3. **The Codex that fills itself in** — the engine must be able to introspect its own rules to explain itself.
4. **Campaign packs as pure data** — Chapter 2, 3, 4… must ship as JSON, not code.

The engine is a **fixed-step deterministic simulation at 20 Hz** (50 ms ticks — fine enough for 5-ft movement resolution, coarse enough that the whole Tutorial simulates in milliseconds). Presentation renders at the display's refresh rate and interpolates. **Dice are seeded per action, not per tick**, so replays and re-simulations are exact.

## 3.2 Why real time works — the arithmetic that makes it honest

This is the most important idea in the document, so here it is in full.

D&D says: **a round is about 6 seconds**, and on your turn you can move up to your **Speed** (30 ft for a human ✅) and take one action. ✅

Therefore, in real time: **30 feet ÷ 6 seconds = 5 feet per second.**

So if the game runs at **1 foot per 0.2 seconds**, a Speed-30 character crosses exactly 30 feet in exactly 6 seconds — **one real-time round.** Real-time movement is not a simplification of D&D. **It is D&D, played at the correct speed.** Nobody has ever done this properly, and it is the whole reason real-time-with-pause is the right answer for a faithful D&D game.

### The "Flow Turn" model

The remaining problem is that D&D turns are *sequential* — you act, then the goblin acts — and real time is *simultaneous*. The resolution:

> **A creature's turn occupies the entire 6-second round. Movement is continuous and simultaneous. Actions are sequenced by initiative.**

- **Movement** happens in real time for everyone, always, at Speed. This is what makes the game feel alive and what makes positioning matter — you can see the goblin flanking you while you cross the road.
- **Actions** (Attack, Dash, Disengage, Dodge, Hide, Help, Ready, Search, Study, Utilize, Influence, Magic ✅ — the full 2024 action list) are **spent at your initiative slot** within the round. The initiative ribbon shows a marker travelling along the round; when it reaches your portrait, your queued action fires.
- **Bonus Actions** fire immediately after your action in the same slot.
- **Reactions** fire **instantly and out of order**, on their trigger, consuming your reaction until the start of your next turn. ✅ This is what makes Opportunity Attacks land with real impact: you watch a goblin start to run, the reaction icon snaps, and your blade goes through it mid-stride.
- **Free object interaction** (one per turn, during move or action ✅) is a hold-modifier on any action, not a separate input.

**What the player experiences:** a living battlefield where everyone is moving and dodging constantly, punctuated by the sharp, ordered *crack-crack-crack* of initiative slots resolving. Pause at any moment and the world stops with a parchment wash, the initiative ribbon expands into a full turn planner, and you can queue, re-queue, and preview.

**What the ruleset experiences:** a legal game of D&D 2024.

### Why this beats turn-based here

Turn-based games have a permanent tell: the world freezes between turns, so *nothing is ever actually moving*, and the "everything is animated" brief dies. Real-time-with-pause means the idle animations, the wind, the fire, the flinching, the goblin's ear twitch — all of it is always live. And it makes the single best moment in tabletop D&D, which is **the interruption**, finally work in a video game.

### Difficulty and speed
Global time scale **0.5× / 1× / 2×**, plus per-event **auto-pause triggers** (enemy spotted, ally downed, trap triggered, dialogue available, action queued ready). Speed does not change the rules — it changes only the presentation clock.

## 3.3 The event log and the Roll Ledger

Every engine action emits a structured event:

```json
{
  "tick": 1480, "round": 3, "actor": "pc_sergeant",
  "type": "attack.resolved",
  "roll": { "dice": "1d20", "results": [14], "advantage": null,
            "modifier": 5, "total": 19,
            "breakdown": [
              {"src":"d20","v":14},
              {"src":"ability.strength","v":3},
              {"src":"proficiency","v":2}
            ]},
  "target": "goblin_03",
  "vs": { "ac": 15, "cover": "none" },
  "outcome": "hit",
  "damage": { "dice":"1d8","results":[6],"modifier":3,"total":9,"type":"slashing" },
  "codex": ["Attack Roll","Ability Modifier","Proficiency Bonus","Armor Class"]
}
```

The **Roll Ledger** is this log rendered as a scroll of parchment down the right edge of the screen. Hover any line and the breakdown expands. **Click any line and the game rewinds the presentation to that moment** (the sim is already there). This is the single most powerful teaching tool in the game, and it is why Pillar 1 is achievable.

## 3.4 The World-State Ledger

A single append-only key/value store that every system reads and writes. Not a save file — a **history**.

```
flag: goblin_captured            = "goblin_02"
flag: goblins_killed             = 3
flag: goblin_fled                = false
flag: knew_cragmaws_know.spider  = true
flag: knew_cragmaws_know.sildar  = true
flag: knew_cragmaws_know.klarg   = true
flag: knew_cragmaws_know.psigoblins = true
flag: pc_hp_current              = 9
flag: pc_condition               = ["restrained"]  (temporary)
flag: wagon_position             = "roadside_clearing_A"
flag: horses_claimed             = true
flag: snare_triggered_by         = "pc_sergeant"
flag: barthen_told_of_ambush     = false
```

Three systems read this ledger and nothing else: **the AI director** (what the goblins do next), **the dialogue system** (what an NPC knows and how they feel), and **the journal** (what the player has learned). Because there is one ledger, the three can never disagree — which is the technical guarantee behind Pillar 5.

## 3.5 Fate's Eye — the game's signature mechanic

Because the engine is pure and deterministic, we can afford something no D&D game has ever had:

> **Hold `Tab` on any action to see what might happen before you commit.**

The engine forks the simulation, runs the proposed action **400 times**, and the UI draws the outcome distribution as a hand-inked fan of ghost futures behind the target: how many swing-arcs connect, where the goblin ends up if you Push it, which tiles you can reach this turn, what the odds are the last goblin flees. The ghosts are drawn in **Inkwash** mode so they read as possibility, not reality.

This is not a cheat and it is not a difficulty crutch — it is the **translation of what a good DM does out loud at a real table** ("if you shove him, he goes over the embankment"). It converts the ruleset from an obstacle into a readable tactical language, and it is the thing that will make people say "I've never seen a game do that."

**Cost:** it is free in the Tutorial and always available on the *Story* difficulty dial; it costs a **Heroic Inspiration** on *Veteran*; and it is unavailable on *Lethal*.

## 3.6 The stack — browser-native, open-source, AI-buildable

**Hard constraints, taken as given:** no game engine; everything is code; it lives in a public GitHub repo; it deploys as a static site to Vercel / Netlify / Cloudflare Pages; and **every line is written by AI, in conversation, with no human IDE in the loop.**

That last constraint is the one that actually decides the stack. It changes the criteria: the best library is not the most powerful one, it is the one with **the most public documentation, the most stable API, and the fewest ways to be subtly wrong.** An AI cannot open a GUI editor, cannot attach a native debugger, and cannot profile a device it doesn't have. So the stack below is chosen for **legibility and verifiability**, and every item is open-source and permissively licensed.

### The stack

| Layer | Choice | Why this and not the alternative |
|---|---|---|
| **Language** | **TypeScript 5, `strict: true`** | Types are the single highest-leverage thing for AI-authored code: the compiler catches a whole class of mistake before it ships. Non-negotiable. |
| **Build / dev** | **Vite 5** + **pnpm** workspaces monorepo | Instant HMR — the AI sees its own change in the live preview in seconds. Zero-config static output. One command to deploy. |
| **Repo shape** | `packages/rules` + `packages/client` + `packages/content` + `apps/web` | The rules package is independently testable and publishable. See 3.1. |
| **Rules engine** | **Hand-written TypeScript, zero dependencies** | Not Rust/WASM: WASM buys nothing here (the sim is tiny — see 3.2) and costs the AI its ability to read, debug and iterate on the most important code in the project. Pure TS runs identically in Node and the browser. |
| **Renderer** | **PixiJS v8** (WebGL2, WebGPU fallback) | The best-documented 2D WebGL renderer in existence: sprite batching, transforms, filters, masks, mature custom-shader API. Rejected: **Phaser** (its scene/physics opinions fight this design's custom simulation), **Three.js** (3D-first, wrong tool), **raw WebGL** (an AI would burn the whole budget on plumbing). |
| **Shaders** | **GLSL** via PixiJS `Filter` | The light field, god rays, Inkwash, Darkvision, The Sight and the parchment UI grain are all fragment shaders. Fully supported. |
| **Character animation** | **Custom code-driven skeletal rig, JSON-defined** (Part IX spec) — **spine-ts** as an optional drop-in later | **The most important stack decision in this document.** See 3.6.1. |
| **Panel UI** | **DOM + Lit** web components, styled to the parchment palette | See 3.6.2 — a deliberate revision of the original "no HTML overlays" position. |
| **In-world UI** | **PixiJS** — nameplates, damage numbers, the Eye, cover tags, the die | Must be parallaxed and lit by the world, so it lives in the canvas. |
| **State** | **Zustand** | Tiny, no boilerplate, trivially serialisable — which matters because saves and replays are just JSON. |
| **Audio** | **Web Audio API** + a hand-rolled layer mixer (~300 lines) + **Howler.js** for playback and fallback | Wwise and FMOD are native middleware and **do not exist in a browser.** The adaptive layer system in Part X is genuinely simple on `GainNode` crossfades. |
| **Input** | Hand-rolled keyboard/mouse + the browser **Gamepad API** | Full remap is required (§11.3); the Gamepad API gives controllers for free. |
| **Data** | **JSON + Zod** schemas | Campaign packs are data (§12). Zod validates at load and gives precise typed errors instead of mystery crashes. |
| **Persistence** | **IndexedDB** (via `idb-keyval`) + URL-serialisable state | Saves, permadeath slots, and **shareable replays** — see 3.7. |
| **Tests** | **Vitest** + **Playwright** | Vitest for the ~10,000 rules tests; Playwright for a headless smoke test that boots the game and asserts frame one renders. |
| **CI / deploy** | **GitHub Actions** → static build → **Vercel** (or Netlify / Cloudflare Pages) | Push to `main`, a preview URL appears. No server, no backend, no cost. |

**Explicitly rejected:** Unity, Unreal, Godot, Construct, GameMaker (engines). Babylon.js, Three.js (3D-first). Phaser (opinionated scene model conflicts with the custom sim). Any native audio middleware. Any binary asset format that cannot be inspected as text.

### 3.6.1 Why the animation system is hand-rolled — and why that is an upgrade

The original plan said "Spine 2D." **Spine's runtime is open source, but Spine's editor is a paid desktop application** — and an AI cannot open a desktop application. The same wall applies to DragonBones (free but a discontinued GUI), After Effects, Photoshop, Aseprite and Blender.

So the rig specified in Part IX — 24 body bones, 6 facial bones, 12 blend shapes, 8 deform meshes, 3 dynamic chains, 2 IK chains — is defined as a **JSON rig format animated entirely in code**:

```jsonc
// rigs/human_fighter.json — the whole rig is text the AI can read and write
{
  "bones": [
    { "id": "root",       "parent": null,     "pos": [0, 0] },
    { "id": "pelvis",     "parent": "root",   "pos": [0, -42] },
    { "id": "spine",      "parent": "pelvis", "pos": [0, -18] },
    { "id": "arm_upper_l","parent": "spine",  "pos": [-9, -14], "len": 16 }
  ],
  "slots":  [ { "id": "torso", "bone": "spine", "mesh": "torso_deform", "z": 0 } ],
  "chains": [ { "id": "cloak", "bones": ["cloak_1","cloak_2","cloak_3"],
                "solver": "spring", "stiffness": 0.18, "damping": 0.72 } ],
  "ik":     [ { "id": "foot_l", "target": "ground_l", "chain": ["thigh_l","shin_l"] } ]
}
```

And every animation is a **curve set in code**, not a timeline in a GUI:

```ts
// animations/mastery_push.ts — anticipation / impact / recovery, per Law 3 (§9.1)
export const masteryPush: Anim = {
  duration: 22, // frames @ 24fps
  tracks: {
    arm_upper_r:  rot(key(0, -38), key(8, 24), key(11, 61), key(22, 4)), // wind-up → drive → settle
    root:         pos(key(0, 0),   key(11, 9),  key(22, 0)),
    torso_deform: squash(key(11, 1.12), key(14, 0.94), key(22, 1.0)),
  },
  events: [{ at: 11, emit: 'impact', hitstop: 4, shake: 3, sfx: 'push_thud' }],
};
```

**Why this is better for this project, not merely possible:**

1. **An AI can author, read, diff and fix animations directly.** A rig and a curve set are text. A `.spine` project is not. The AI can generate 150 animation states by writing 150 curve sets — and can *revise frame 11 of one specific attack* from a single sentence of feedback. That is not true of any GUI pipeline.
2. **Retargeting becomes arithmetic.** §9.2's "one skeleton, retargeted across all humanoids" collapses to a scale-and-offset table: goblin bones are human bones at 0.72× with a shifted centre of mass. One line of code, not a manual re-rig.
3. **Procedural motion gets cheaper, not dearer.** Spring-solver cloth and hair, IK feet conforming to the painted ground, breathing on a sine, blink on a 2.4–5.1 s random interval — all cheaper in code than in a GUI, and all required by the Tier A standard in §2.8.
4. **Mesh deformation is available.** PixiJS v8 ships `SimpleMesh`; a ~20-line custom vertex shader gives weighted bone deformation, covering the 8 deform meshes.

**The honest cost:** hand-drawn *key art* still needs an image model or a human illustrator. The pipeline in §9.9 becomes: painted part sprites → JSON rig → code-authored curve sets → renderer. **No step requires a GUI application**, which is exactly the constraint.

### 3.6.2 A revision: the UI is hybrid, and it is better

The original plan said *"Do not use HTML overlays; the UI must be lit by the world."* In a browser that position is expensive and mostly wrong, so it is revised:

- **In-world UI** — nameplates, damage numbers, the contextual Eye, cover tags, and **the die itself** — renders **in the PixiJS canvas**, so it is parallaxed, lit, and affected by every render mode (Inkwash, The Sight, Darkvision). The die must be in-canvas: it is a physical object in the world.
- **Panel UI** — character sheet, journal, Codex, Roll Ledger, settings — is **DOM**. Because DOM gives us for free: real text selection, screen-reader access, browser zoom, `prefers-reduced-motion`, IME input for character names, native scroll, right-click, find-in-page, and **accessibility compliance that would take months to hand-build in a canvas.** §11.3's accessibility list is the reason, and it was always the priority.
- **The two are made to look like one object** by sharing the exact parchment palette, a canvas-grain texture applied as a CSS background, identical easing curves, and — critically — a **light-sampling filter**: each frame the renderer computes the world's average light colour and writes it to a CSS custom property, so every panel is genuinely tinted by the world it sits on. Panels are ember-lit in torchlight and cold in the dark.

**Net result:** panels gain real accessibility and real text handling, and the "lit by the world" requirement is met by a light-sampling filter rather than a full canvas re-implementation. Strict improvement, no design lost.

### 3.6.3 Performance budget — the browser is not a console

A hard budget, checked in CI, because "fully animated with everything" will otherwise eat a mid-range laptop alive.

| Budget | Target | Enforced by |
|---|---|---|
| Initial JS payload | **< 400 KB gzipped** | Vite bundle analysis in CI |
| Campaign pack (lazy-loaded JSON) | **< 1.5 MB** per chapter | Zod validation + size check |
| Texture atlas per scene | **≤ 2 × 4096²**, WebP | Atlas build script |
| Total GPU texture memory | **< 384 MB** | Runtime counter, warns in dev |
| Draw calls per frame | **< 250** via batching | PixiJS stats, asserted in dev builds |
| Frame time | **60 fps** at 1080p on integrated graphics; **30 fps floor** below that | Playwright perf smoke test |
| Rules engine tick | **< 0.5 ms** at 20 Hz | Vitest benchmark |
| Fate's Eye, 400 simulations | **< 80 ms**, in a Web Worker, off the main thread | Vitest benchmark |

**Quality ladder** — auto-detected, manually overridable: particle counts, shader complexity, parallax layer count, dynamic-light resolution and animation framerate (24 → 12 fps) all step down across four tiers. **The rules never change between tiers. Only the paint does.**

**Mobile:** touch is a first-class target. The Eye becomes a tap, the action economy maps to a radial, Fate's Eye becomes a long-press. The game should be *playable* on a phone even though keyboard and mouse are the intended experience.

## 3.7 Why the browser is the *right* platform for this game, not a compromise

Six of this design's headline features get **easier** on the web. That is not a consolation prize; it is a genuine argument for the platform.

| Feature | Why the web makes it better |
|---|---|
| **Fate's Eye (§3.5)** | 400 forked simulations of a pure, deterministic engine. In a browser that is a **Web Worker** — off the main thread, no frame drops, no engine plugin. In a native engine this is a threading headache. |
| **The Roll Ledger & Roll Rewind (§3.3)** | The engine already emits a pure JSON event log. On the web, **that log is a URL.** Every combat in the game becomes a shareable link. Bug reports become "here's the fight." Players trade ambushes like chess puzzles. This is essentially free and would need custom netcode anywhere else. |
| **Saves, permadeath, multiple slots (§11.3)** | IndexedDB. No accounts, no server, no backend cost, works offline. |
| **Zero-friction playtesting** | **A URL is the whole distribution.** This is the biggest one: the game-feel loop that decides whether this project lives or dies (§13.2, risk 1) runs at the speed of sending a link, not the speed of shipping a build. |
| **The Codex, Rules Inspector and Roll Ledger text** | It is all *text*. DOM gives real typography, real text selection, real screen readers, real find-in-page — the accessibility requirements in §11.3 are met by the platform instead of being hand-built. |
| **Campaign packs as data (§12)** | A chapter is a JSON file fetched on demand. Chapter 2 is literally a deploy, with no patch, no download, no store review. |

**And one honest downgrade:** no controller haptics, no true fullscreen exclusive mode, and asset streaming is bound by HTTP. None of these touch this design.

## 3.8 Building this with Arena AI — the method

Since the whole codebase is written in conversation, the *process* is part of the design. This is how it should actually be run.

**1. The rules engine first, and nothing else.** `packages/rules` is pure TypeScript with zero dependencies, so it can be written, run and **verified with `vitest` in the same conversation turn it is written in.** No renderer, no assets, no browser. Every rule in Part VI becomes a failing test first, then an implementation. This is the part of the project an AI is genuinely *best* at, and it is the part that everything else depends on.

**2. Greybox before beauty.** The vertical slice (M1) is built with placeholder rectangles on a PixiJS canvas — no painted art at all. It proves the one thing that could kill the project: that real-time D&D is fun. Art is expensive; a rectangle is free.

**3. Live preview as the playtest loop.** The dev server runs in the workspace and is viewable immediately, so "does the ambush feel right?" is answered in the same session it is asked. Every game-feel question in this document — the initiative ribbon, the die's weight, the knockout input — gets settled by playing, not by arguing.

**4. Small, verified increments.** Each turn should end with `pnpm test` and `pnpm build` green and a named code path actually executed. A rules engine with 10,000 passing tests is a project an AI can keep working on for months without it collapsing, because the tests catch drift.

**5. Art is the real dependency, and it is not code.** The painted Emberlight layers (§2.2), the part sprites for the rigs, the VFX frames and the audio all need to be *generated or commissioned*. The plan is written so the game is **fully playable and fully fun with placeholder art**, and the paint drops in on top without touching a line of logic.

**6. The one thing to decide early:** the **rig format and the animation curve format** (§3.6.1). Everything in Part IX depends on those two schemas. Get them right in week one and 150 animation states become a content problem; get them wrong and every one has to be redone.

---

# PART IV — BASIC MECHANICS

Five systems. Everything else in the game is a composition of these five.

## 4.1 SYSTEM 1 — THE D20 PIPELINE

One function. Every check, save and attack in the game flows through it, in this exact order.

```
ROLL D20 TEST (actor, kind, ability, proficiency?, target?, dc?, advSource[], disSource[])

 1. GATHER       build the modifier list
                   ability modifier            (from score, PHB Ability Modifiers table ✅)
                 + proficiency bonus           (+2 at level 1 ✅ — only if proficient and only ONCE ✅)
                 + situational bonuses         (cover, Help, Bless, magic)
 2. ADVANTAGE    if advSource[] non-empty and disSource[] non-empty → CANCEL to a normal roll ✅
                 else if advSource[] non-empty → roll 2d20, keep HIGHER
                 else if disSource[] non-empty → roll 2d20, keep LOWER
                 (they never stack, no matter how many sources ✅)
 3. ROLL         roll the d20(s), seeded
 4. NAT RULES    nat 20 on an attack roll = automatic hit + critical hit ✅
                 nat 1  on an attack roll = automatic miss ✅
 5. TOTAL        kept die + modifiers
 6. COMPARE      attack → total >= target's AC  (AC includes cover bonus ✅)
                 check/save → total >= DC
 7. EMIT         write the full breakdown to the event log and the Roll Ledger
 8. RESOLVE      apply damage / condition / state change / flag
 9. FEED         trigger the animation + audio + VFX director
```

**Design consequence:** because every single resolution goes through one function, the game can render *every* resolution identically — and can therefore make the die the star without a thousand special cases.

### The 4-question gate (the DMG's own adjudication rules, made into code) ✅

Before any roll, the DM Director asks:

1. **Is a D20 Test warranted?** If the task is trivial or impossible → **no roll, just narrate.** (The game will not ask you to roll to walk across a room.) A roll is only called when there is a real chance of both success and failure *and* a meaningful consequence for failure.
2. **What kind of test?** Acting → ability check (or attack roll). Reacting → saving throw. ✅
3. **Which ability?** Most influential ability, plus any relevant skill or tool proficiency. The game is *generous* with proficiency, exactly as the DMG instructs ✅ — and it will even let you **argue for a different skill** (see 6.2.5).
4. **What's the DC?** From the DMG's Typical DCs table ✅:

| Task | DC |
|---|:---:|
| Very easy | 5 |
| Easy | 10 |
| Moderate | 15 |
| Hard | 20 |
| Very hard | 25 |
| Nearly impossible | 30 |

And **calculated DCs** use the standard formula **DC = 8 + ability modifier + Proficiency Bonus** ✅ — which is how a grapple, a shove, a hidden creature's Stealth-vs-Perception, and every monster ability get their numbers.

**These four questions are exposed to the player verbatim in the Codex.** The game teaches you to think like a DM.

### The die presentation (the payoff for all of the above)

| Roll event | Screen behaviour | Sound |
|---|---|---|
| Roll called | World slows to 40% speed over 8 frames; a parchment wash vignettes in; the die rises from the bottom edge | A single low drum hit + paper rustle |
| Advantage | Second die, gold-rimmed, counter-spinning | A rising fifth interval |
| Disadvantage | Second die, rust-rimmed, counter-spinning | A falling fifth interval |
| Loser crumbles | The discarded die turns to dust and blows away | Sand hiss |
| Result revealed | Die locks, numeral scales 1.15× and holds 0.35 s; the *total* writes itself in ink beside it | A struck bell, pitched by margin of success |
| **Nat 20** | Gold leaf ignites across all 20 faces; shockwave ring; camera punches 4%; hitstop 90 ms | Full orchestral stab + a choir hit |
| **Nat 1** | Hairline crack races across the die; numeral flickers and dies; camera drops 2° | A cello string snapping |
| Heroic Inspiration spent | The third gold die dives in and *replaces* the rolled one mid-air | A held breath, then release |

**Heroic Inspiration** ✅ — you can expend it to reroll any die immediately after rolling and you must use the new roll; you can never hold more than one; **Human characters start each day with it** ✅ (via Resourceful — ⚠️ verify trait name in the Human species entry, which is an embed stub in the attached PHB). This is a **once-per-day "no, actually"** button, and it is the most satisfying single input in the game.

## 4.2 SYSTEM 2 — THE ACTION ECONOMY

The 2024 action list, ✅ verbatim from the PHB, and what each one becomes as an input.

| Action | Rules summary ✅ | Input | Animation |
|---|---|---|---|
| **Attack** | Attack with a weapon or Unarmed Strike | **LMB / RT** (hold for target selection) | Full weapon-specific combo set |
| **Dash** | Extra movement equal to Speed for the rest of the turn | **Double-tap move / B** | Sprint burst, dust kick, motion smear |
| **Disengage** | Movement doesn't provoke Opportunity Attack for the rest of the turn | **Shift+move / A** | Low, guarded sidestep; a faint blue ward trails the character |
| **Dodge** | Until your next turn, attacks against you have Disadvantage and you make Dex saves with Advantage; lost if Incapacitated or Speed 0 | **Q / LB** | Defensive stance, weapon up, weight back; a shimmering outline |
| **Help** | Help another creature's ability check or attack roll, or administer first aid | **H / D-pad up** (target an ally) | A shouted gesture + pointing; a gold thread draws from you to them |
| **Hide** | Make a Dexterity (Stealth) check | **C / stick click** (must be out of sight) | Crouch into cover, breath held, the character's colours drain to match the background |
| **Influence** | Make a Cha (Deception, Intimidation, Performance, Persuasion) or Wis (Animal Handling) check to alter a creature's attitude | **Dialogue stances** (see 6.3) | Four distinct social animations, one per stance |
| **Magic** | Cast a spell, use a magic item, or use a magical feature | **1–4 / X Y** | Spell-specific |
| **Ready** | Prepare an action in response to a trigger you define | **R / D-pad right** (opens trigger picker) | Weapon raised, coiled, held; a hairline amber trigger-line draws from you to the condition |
| **Search** | Make a Wis (Insight, Medicine, Perception, Survival) check | **E** (context) | Kneel, scan, hand to the ground |
| **Study** | Make an Int (Arcana, History, Investigation, Nature, Religion) check | **V** (context) | Lean in, narrow eyes, a lens of ink forms in the air |
| **Utilize** | Use a nonmagical object | **F** (context) | Object-specific |

Plus:
- **Bonus Action** — only exists when a feature grants one ✅. For a level-1 Fighter that means **Second Wind** (⚠️ verify) and the **Light property extra attack** (⚠️ verify, unless the weapon has **Nick** mastery, which folds it into the Attack action ✅).
- **Reaction** — one per round, refreshes at the start of your next turn ✅. **Opportunity Attack** is the common one ✅: when a creature you can see leaves your reach, you may spend your reaction on one melee attack, and it happens *right before* it leaves your reach ✅.
- **Free object interaction** — one per turn, during your move or action; a second one costs the Utilize action ✅. Rendered as: a small amber pip beside your portrait that is spent when you open a door mid-stride.
- **Communicating** — brief utterances and gestures are free; a detailed explanation or an attempt to persuade costs an action ✅. **This is why dialogue is an action economy.** (Pillar 2.)

### The input philosophy
**No action is ever buried in a menu during combat.** Every action in the table above has a button, because Pillar 3 says a rule that isn't a verb doesn't exist. The radial menu is a *fallback and a teaching tool*, never the primary path.

## 4.3 SYSTEM 3 — TIME, INITIATIVE & THE ROUND

### Order of combat ✅
1. **Establish Positions** — the DM (game) determines where everyone is from marching order or stated positions.
2. **Roll Initiative** — a **Dexterity check** for everyone; identical monsters share one roll ✅.
3. **Take Turns** — highest to lowest; order is fixed for the whole fight ✅.

### Surprise ✅
A surprised combatant has **Disadvantage on their Initiative roll.** That is the whole 2024 rule, and it is *brilliant* for a video game, because it means surprise is not a lost turn — it's a **bad position in the order**, which is a spatial, readable, recoverable thing.

**Presentation:** at the moment combat begins, the initiative ribbon unfurls across the top of the screen and the portraits *physically race* into position. Surprised combatants visibly stumble backwards in the queue, their portrait desaturated and wobbling. **The player sees the ambush cost them, in one second, without a tutorial text.**

### Ties ✅
DM decides among tied monsters; players decide among tied characters; DM decides between a monster and a character. **In this game the player is handed the tie-break** — a tiny drag-handle appears on the ribbon and you order your own side. Free tactical depth, zero rules cost.

### The round clock
The round is a **6-second bar** under the initiative ribbon. Your portrait's marker travels along it. When it reaches your slot, your queued action fires and the portrait **stamps** with a wax seal. Rounds tick over with a soft page-turn.

### Ending combat ✅
Combat ends when one side is defeated — killed, knocked out, surrendered or fled — or both sides agree to stop. **The game never forces you to kill everything.** When the last goblin's morale breaks, the music drops out and the encounter simply *ends*, mid-round.

## 4.4 SYSTEM 4 — POSITION, SPACE & THE GRID

- **Square grid, 5 ft per square** ✅. Speed 30 = **6 squares** ✅.
- **Entering a square** costs 1 square of movement; **Difficult Terrain** costs 2 ✅; you can't cross the corner of a wall or large tree diagonally ✅.
- **Ranges** are counted by shortest route from an adjacent square ✅.
- **Breaking up your move** ✅ — move, act, move. This is the single most important tactical rule in the game and it is *free* in real time.
- **Creature sizes** ✅ — Tiny 2½ ft (4 per square), Small and Medium 5 ft, Large 10 ft (2×2), Huge 15 ft, Gargantuan 20 ft.
- **Moving through other creatures** ✅ — you can pass through an ally, an Incapacitated creature, a Tiny creature, or one two sizes larger/smaller; another creature's space is Difficult Terrain; you can't willingly end your move in an occupied space (if forced, you go Prone).
- **Dropping Prone** is free and costs no movement ✅; standing costs half your movement (⚠️ verify glossary).

### The grid is felt, not seen
At Z2 the grid is **never drawn as lines.** It is expressed through:
1. **Painted ground detail** — ruts, stones, grass tufts at exactly 5-ft intervals, so the eye locks on subconsciously.
2. **Reach rings** — when you hover an enemy, a soft amber wash appears on every tile you can reach *this turn*, accounting for Difficult Terrain and Opportunity Attack zones (which are washed cold).
3. **Cover tinting** — tiles in Three-Quarters Cover from the selected attacker get a hard shadow gradient.
4. **On demand** — hold `G` and the grid draws itself in ink, like a cartographer laying a ruler over the painting.

## 4.5 SYSTEM 5 — DAMAGE, HIT POINTS & DEATH

### Hit Points
- Level 1 Fighter max HP = **10 + Constitution modifier** ✅. With Con 14 (+2) that is **12 HP**.
- **Temporary Hit Points** ✅ are a separate buffer that absorbs damage first, doesn't stack (you take the higher), and isn't healed.
- **Resistance** halves damage (round down ⚠️ verify), **Vulnerability** doubles it, **Immunity** zeroes it ✅.
- **Damage types** ✅: acid, bludgeoning, cold, fire, force, lightning, necrotic, piercing, poison, psychic, radiant, slashing, thunder. Each has a distinct **impact VFX and sound** — see Part IX.

### Critical hits ✅
A nat 20 on an attack roll is a critical hit: you roll **all the attack's damage dice twice** and add modifiers once.

**Presentation:** the damage dice physically double in the air — one set spins gold, the other spins normally — then they collapse into a single gold total. The target gets a unique **critical reaction animation** (a full-body stagger, not just a flinch) and 120 ms of hitstop.

### Dropping to 0 HP ✅
- **Monsters die instantly at 0 HP** ✅ — *unless* the DM decides otherwise, or unless **you choose to knock them out.**
- **Knocking Out a Creature** ✅ — when you would reduce a creature to 0 HP with a **melee attack**, you can instead reduce it to 1 HP and give it the **Unconscious** condition. It starts a Short Rest; the condition ends if it regains HP or if someone administers first aid with a successful **DC 10 Wisdom (Medicine)** check.

> **This single rule is the pivot of the entire Tutorial.** Knocking out a goblin instead of killing it is the difference between learning nothing and learning everything the Cragmaws know. It is a *melee-only, deliberate, thumb-press decision made in the split second before impact.* **It gets its own input:** hold `LMB` at the moment of the killing blow and the weapon flips to a pommel strike, the impact sound dulls from a slash to a thud, and the goblin drops limp instead of dissolving. **This is the game's most important interaction and it is taught in the first fight.** (See 8.9.)

- **Instant Death / Massive Damage** ✅ — if damage remaining after reaching 0 HP equals or exceeds your max HP, you die.
- **Death Saving Throws** ✅ — at the start of your turn with 0 HP, roll 1d20: 10+ is a success. **Three successes = Stable. Three failures = dead.** A **1 counts as two failures**; a **20 restores 1 HP**. Taking any damage at 0 HP causes one failure (two if it's a critical hit); if that damage equals or exceeds your max HP, you die.
- **Stabilising** ✅ — Help action + successful DC 10 Wisdom (Medicine) check. A stable creature that isn't healed regains 1 HP after 1d4 hours.

**Presentation:** three skull pips and three flame pips drawn in ink around the downed character's portrait. Each failure **stamps** a skull in oxblood with a wet thud. Each success kindles a small flame. The world's audio ducks to a heartbeat. The camera drops to Z1 and tilts. **When the third skull stamps, the screen goes to Inkwash and holds for two full seconds before the death card.** This is the most solemn sequence in the game and it must never be skipped.

### Healing & resting ✅
- **Short Rest** — spend Hit Dice to recover (⚠️ verify exact 2024 wording in the glossary stub).
- **Long Rest** — full restoration (⚠️ verify), 8 hours, interrupted by strenuous activity.
- **Potion of Healing** — an object, used with the Utilize action or as a Bonus Action (⚠️ verify).

**Rest is diegetic** — see 6.4.7. You choose where to camp on the actual map, and the *place* determines the quality of the rest.

---
---

# PART V — CHARACTER CREATION

**Locked for the Tutorial:** Species = **Human**, Class = **Fighter**, Background = **Soldier**. Everything else is greyed but *visible*, because a locked option that teaches you what's coming is worth more than a hidden one.

Character creation is not a menu. It is **a scene at the muster yard outside Neverwinter's gate at dawn**, shot at Z1, in which you walk between five stations and a dwarf named Gundren Rockseeker follows you around making small talk. Total target length: **8–12 minutes**, skippable to ~90 seconds via a "Quick Muster" button that applies the recommended build.

## 5.0 Before all of it — THE CAMPAIGN SELECTOR

The game opens on **the Adventurer's Ledger**: a vast leather campaign journal lying open on a table, lit by a single candle. You flip it with the mouse. Each campaign is a spread: a hand-painted plate on the left, the campaign card on the right.

### The selector card

```
┌────────────────────────────────────────────────────────────────┐
│  THE ADVENTURER'S LEDGER                            ✦  ✦  ✦    │
├──────────────────────────┬─────────────────────────────────────┤
│                          │  THE TUTORIAL                       │
│   [hand-painted plate:   │  A Dangerous Journey                │
│    the bend in the road, │  ─────────────────────────────────  │
│    two riderless horses] │  Source   Phandelver & Below, Ch. 1 │
│                          │  Levels   1                         │
│                          │  Party    1 hero (+1 recruitable)   │
│                          │  Length   45–75 min                 │
│                          │  Teaches  The complete core game    │
│                          │                                     │
│                          │  DIFFICULTY   ● ○ ○ ○               │
│                          │  Story / Standard / Veteran / Lethal│
│                          │                                     │
│                          │  RULE DENSITY  ● ● ● ○ ○            │
│                          │  [Guided] [Standard] [Purist]       │
│                          │                                     │
│                          │  ☐ Fate's Eye    ☐ Roll Rewind      │
│                          │  ☐ Lethal Damage ☐ Ironman Save     │
│                          │                                     │
│                          │         [ BEGIN THE JOURNEY ]       │
├──────────────────────────┴─────────────────────────────────────┤
│  CHAPTER II — TROUBLE IN PHANDALIN            🔒  locked        │
│  CHAPTER III — THE SPIDER'S WEB               🔒  locked        │
│  CHAPTER IV — WAVE ECHO CAVE                  🔒  locked        │
└────────────────────────────────────────────────────────────────┘
```

Locked chapters are **not hidden**. They are visible with a one-line teaser each, so the player knows exactly what they are working toward and how long this game intends to run. This is the cheapest, most effective retention device available and it costs nothing.

### The difficulty dial — mapped straight onto the DMG ✅
The DMG's 2024 encounter-difficulty model uses **XP Budget per Character** at three tiers: **Low / Moderate / High** ✅. That maps onto four player-facing settings with total honesty:

| Setting | Encounter budget | Fate's Eye | What it means |
|---|---|---|---|
| **Story** | DMG **Low** (Level 1: **50 XP/character** ✅) | Free | You will not die. You will see the story. |
| **Standard** | DMG **Moderate** (Level 1: **75 XP/character** ✅) | Free | The default. Sharp, survivable, occasional down. |
| **Veteran** | DMG **High** (Level 1: **100 XP/character** ✅) | Costs Heroic Inspiration | Real D&D. Bad positioning kills. |
| **Lethal** | DMG **High** × 1.5 | Off | Monsters don't pull punches; no knockouts; Death Saves are unforgiving. |

**Rule Density** is a separate, orthogonal dial:
- **Guided** — the game names every rule the first time it fires, auto-suggests actions, and holds your hand through the first three rolls.
- **Standard** — the game names rules in the Roll Ledger and the Codex but never interrupts.
- **Purist** — no rule names anywhere. Just numbers, dice, and consequences. (For actual D&D players who want the game to shut up.)

**These two dials are the single most important accessibility decision in the game**, and they are on the campaign card, before character creation, where they belong.

## 5.1 Station One — THE MUSTER TENT (name, body, face)

A canvas tent flap lifts. Inside: a polished steel breastplate on a stand, a bowl of water, a stool.

**Name.** A quill writes your name onto a muster roll as you type. Surname generator offers period-appropriate Sword Coast names.

**Body & face.** Not a slider system — a **layered painterly part system**, because that's what Emberlight is good at and what keeps silhouettes readable:

| Layer | Options at launch | Notes |
|---|---|---|
| Frame | 5 builds (gaunt → heavy) | Affects the rig's bone lengths, not a scale slider. Silhouette must stay readable. |
| Skin | 12 tones | All hand-painted with subsurface warmth, never flat fills |
| Face | 8 base heads × 6 nose × 6 jaw × 6 brow × 5 ear | Combinatorial, not sculpted |
| Hair | 14 styles × any colour | On secondary dynamics |
| Beard | 10 (including none) | On secondary dynamics |
| Eyes | 8 shapes × full colour wheel | Iris gets a hand-painted specular |
| **Marks** | 14 scars, 9 tattoos, 6 burns, 4 missing teeth | **Each Mark is a story seed** — picking one prompts a one-line "how did you get this?" which writes to the ledger and can surface in dialogue later |
| Voice | 6 timbres × pitch | With a "greeting" audition line |

**The silhouette test:** as you build, a live **silhouette preview** sits in the corner showing your character as a pure black shape at 20% scale. If you can't read the build and the weapon from the silhouette, the character fails the art test. The game will gently warn you. (This is a real production guardrail disguised as a feature.)

## 5.2 Station Two — THE CLASS BANNER (Fighter)

You walk to a rack of banners. Eleven hang there. Ten are **greyed, moth-eaten, and locked**, each with a one-line flavour from the PHB's Class Overview ✅ — Barbarian ("Storm with Rage"), Bard ("Perform spells"), Cleric ("Invoke divine magic"), and so on. One is bright: **FIGHTER — "Master all weapons and armor."** ✅ Primary ability: **Strength or Dexterity.** Complexity: **Low.** ✅

> **Design note:** the PHB's own Class Overview table lists Fighter complexity as **Low** ✅. That is precisely why it is the right Tutorial class: the fewest subsystems, the most verbs, and the most *physical* gameplay. It teaches the action economy, positioning, cover and weapon masteries without a single spell slot to explain. **Every one of the game's core mechanics is reachable with a Fighter.**

### What the Fighter gives you at level 1 (⚠️ verify against the PHB class table, which is an embed stub in the attached file)
- **Armor training:** Light, Medium, Heavy, Shields
- **Weapon proficiency:** Simple and Martial weapons
- **Saving throws:** Strength, Constitution
- **Fighting Style** — one of the Fighting Style feats
- **Second Wind** — a Bonus Action self-heal
- **Weapon Mastery** — you unlock two weapons' mastery properties
- **Hit Die:** d10

### Choice A — Fighting Style (Fighting Style feats ✅, from the PHB's own list)
Eleven options exist ✅: **Archery, Blind Fighting, Defense, Dueling, Great Weapon Fighting, Interception, Protection, Thrown Weapon Fighting, Two-Weapon Fighting, Unarmed Fighting**, plus one more in the list. The Tutorial surfaces the four that matter for a level-1 human fighter, and **each one changes how the game physically plays**:

| Style | Feels like |
|---|---|
| **Dueling** | You and one blade. Big single-target damage. The camera tightens on duels; your attacks get longer wind-ups and heavier impact. **The cinematic choice.** |
| **Defense** | +1 AC while wearing armor (⚠️ verify value). You are the wall. The game teaches you to stand in doorways and in Three-Quarters Cover. **The tactical choice.** |
| **Great Weapon Fighting** | Huge two-handed swings, wide arcs, Cleave and Topple come online. Slower, more committal, more spectacular. **The satisfying choice.** |
| **Archery** | You stay out of the light. The game becomes about sight lines, Dim Light Disadvantage, and Opportunity Attack zones. **The clever choice.** |

**Each choice is previewed live** on the practice dummy before you commit — a 5-second looping animation of your character fighting in that style. You *see* the build before you take it.

### Choice B — Weapon Masteries ✅
The PHB's eight mastery properties, ✅ verbatim, and exactly how each one animates:

| Mastery | Rule ✅ | On screen |
|---|---|---|
| **Cleave** | On a melee hit, attack a second creature within 5 ft of the first and within your reach; on a hit it takes the weapon's damage **without** your ability modifier (unless negative); once per turn | Your swing doesn't stop — it carries through into the second target in one continuous arc, with a trailing smear connecting both |
| **Graze** | On a **miss**, deal damage equal to the ability modifier used, same type; only increasable by increasing that modifier | The blade skips off armour with a spark and a shallow cut — a *miss that still hurts*. A distinct "shink" and a thin red line |
| **Nick** | The extra attack from the **Light** property can be made as part of the Attack action instead of a Bonus Action; once per turn | A flick of the off-hand, folded seamlessly into the main combo — no separate animation beat, no Bonus Action pip spent |
| **Push** | On a hit, push the target up to 10 ft straight away if it is Large or smaller | A shoulder-driven impact; the target physically slides two tiles, kicking dust, and can be pushed **off the embankment** |
| **Sap** | On a hit, the target has Disadvantage on its next attack roll before the start of your next turn | A pommel crack to the helm; the target's head snaps back and a cold verdigris haze settles over its weapon arm |
| **Slow** | On a hit that deals damage, reduce the target's Speed by 10 ft until the start of your next turn; doesn't stack beyond 10 ft | A hamstring cut; the target's run animation drops a gear and its reach ring visibly shrinks |
| **Topple** | On a hit, the target must succeed on a **Constitution save (DC 8 + your ability modifier + Proficiency Bonus)** ✅ or gain the **Prone** condition | A sweep of the haft; the target's feet leave the ground and it lands flat with a full-body impact and a dust burst |
| **Vex** | On a hit that deals damage, you have Advantage on your next attack against that target before the end of your next turn | A feint that opens a gap; a gold chevron marks the target and your next die visibly gains its gold twin |

**The mastery picker is the game's first real "build" moment.** It is presented as **two weapon silhouettes on a whetstone**, and picking one *unlocks that weapon's property in the Codex* — the first entry the player ever earns.

### Choice C — The weapon itself
From the PHB's weapon tables ✅. The recommended Tutorial kit and the numbers that fall out:

| Weapon | Damage ✅ | Properties ✅ | Mastery ✅ |
|---|---|---|---|
| **Longsword** | 1d8 Slashing | Versatile (1d10) | **Sap** |
| **Warhammer** | 1d8 Bludgeoning | Versatile (1d10) | **Push** |
| **Greataxe** | 1d12 Slashing | Heavy, Two-Handed | **Cleave** |
| **Battleaxe** | 1d8 Slashing | Versatile (1d10) | **Topple** |
| **Greatsword** | 2d6 Slashing | Heavy, Two-Handed | **Graze** |
| **Rapier** | 1d8 Piercing | Finesse | **Vex** |
| **Handaxe** | 1d6 Slashing | Light, Thrown (20/60) | **Vex** |
| **Shortbow** | 1d6 Piercing | Ammunition (80/320), Two-Handed | **Vex** |
| **Spear** | 1d6 Piercing | Thrown (20/60), Versatile (1d8) | **Sap** |
| **Shield** | +2 AC ✅ | Utilize action to don or doff ✅ | — |

**Recommended Tutorial build:** **Longsword (Sap) + Handaxe (Vex)** — one mastery that controls the enemy's next turn, one that guarantees Advantage on your next attack. Together they teach the player that **D&D combat is a chain of small advantages**, which is the single most important lesson in the game.

## 5.3 Station Three — THE SOLDIER'S KIT (background)

A footlocker. You open it and the kit lays itself out on the canvas, item by item, each one animating in with a soft thud.

### What the Soldier background gives (⚠️ verify — the background entries are embed stubs in the attached PHB; the following is the standard 2024 Soldier package)
- **Ability score increases:** Strength, Dexterity, Constitution are the three listed abilities ✅ (the PHB's own *Ability Scores and Backgrounds* table lists Soldier under **all three** of Strength, Dexterity and Constitution ✅). You increase one by 2 and a different one by 1, **or** all three by 1 ✅ — the game offers both and shows the resulting modifiers live.
- **Skills:** Athletics, Intimidation (⚠️ verify)
- **Tool proficiency:** one gaming set (⚠️ verify)
- **Feat:** one Origin feat
- **Equipment:** including a **rank insignia** — and the rank insignia is *mechanically meaningful in this game*: it is a small permanent bonus to Intimidation against soldiers, guards and mercenaries, and a small penalty with criminals and deserters. **Your background is a social weapon.**
- **Languages:** Common + one ✅

### Origin feat picker
Five candidates, each demonstrated on the dummy before you choose:

| Feat | Why it's here |
|---|---|
| **Tough** | More HP. The forgiving choice. Teaches that HP is a resource. |
| **Savage Attacker** | Reroll damage dice. Teaches the damage pipeline. |
| **Alert** | Better Initiative. Teaches that the initiative ribbon is a battlefield. |
| **Skilled** | Three more proficiencies. Teaches that skills are the exploration layer. |
| **Magic Initiate** | Two cantrips and a 1/day spell. Teaches that magic exists and is coming. **The most tempting and the most complicated — surfaced last.** |

*(Exact feat mechanics ⚠️ verify against the PHB feat entries.)*

### The equipment layout — and the encumbrance lesson
The kit is laid out on a grid of painted canvas squares. **You physically drag items into your pack.** Weight is shown as the pack visibly sagging. This is the encumbrance system taught in fifteen seconds with no numbers: **if you take everything, you are Slow, and Slow means you don't see traps.**

The Tutorial's default kit: **chain mail** (AC 16, Str 13, Stealth Disadvantage ✅), **shield** (+2 ✅), **longsword**, **4 handaxes**, **explorer's pack**, **5 lanterns and a barrel of oil are on the wagon, not on you** ✅, and a **trinket** ✅ (rolled from the PHB trinket table, presented as a small velvet pouch Gundren hands you "for luck").

**The chain mail choice is the game's first real trade-off and it is deliberately unavoidable:** AC 18 with a shield is fantastic, but you have **Disadvantage on Stealth** ✅ forever. The game shows you both numbers side by side and lets you choose. If you take it — most players will — then the ambush in 20 minutes is going to go *loud*, and the game will remember that you chose it.

## 5.4 Station Four — THE ABILITY SCORES

Not a table. **A hex ring.**

Six facets of a bronze soldier's medal lie on a table. You drag six numbers onto them. The numbers are the PHB's **Standard Array for a Fighter: 15 / 14 / 13 / 12 / 10 / 8** ✅.

As you place each number, the medal facet lights and **every derived number in the world updates live and audibly**:
- Place 15 on Strength → the longsword's attack total ticks from +4 to +5 with a soft *tink*.
- Place 14 on Dexterity → Initiative ticks up; a faint amber ring appears around your character's feet showing the reach of your move.
- Place 13 on Constitution → your HP heart fills to 12.

Then the background increases apply on top ✅ and the medal **stamps** with a wax seal.

### The recommended build, and the exact numbers it produces

**Standard Array (Fighter) ✅: Str 15, Dex 14, Con 13, Int 8, Wis 10, Cha 12**
**Soldier increases ✅: +2 Strength, +1 Constitution**

| Ability | Score | Modifier ✅ |
|---|:---:|:---:|
| Strength | **17** | **+3** |
| Dexterity | **14** | **+2** |
| Constitution | **14** | **+2** |
| Intelligence | **8** | **−1** |
| Wisdom | **10** | **+0** |
| Charisma | **12** | **+1** |

Derived, at level 1:

| Stat | Value | Derivation |
|---|---|---|
| **Proficiency Bonus** | **+2** | ✅ PHB, level 1 |
| **Hit Points** | **12** | 10 + Con mod ✅ |
| **Armor Class** | **18** | Chain Mail 16 ✅ + Shield 2 ✅ |
| **Initiative** | **+2** | Dex mod |
| **Speed** | **30 ft (6 squares)** | Human ✅ |
| **Longsword attack** | **+5** | +2 prof + 3 Str |
| **Longsword damage** | **1d8 + 3** (1d10 + 3 two-handed ✅) | Versatile ✅ |
| **Handaxe attack / damage** | **+5 / 1d6 + 3** | Light, Thrown 20/60 ✅ |
| **Strength save** | **+5** | Fighter save proficiency ⚠️ |
| **Constitution save** | **+4** | Fighter save proficiency ⚠️ |
| **Athletics** | **+5** | +3 Str +2 prof (Soldier skill ⚠️) |
| **Intimidation** | **+3** | +1 Cha +2 prof (Soldier skill ⚠️) |
| **Passive Perception** | **10** | 10 + Wis mod 0 |
| **Heroic Inspiration** | **1** | Human, each day ✅ |

**That Passive Perception of 10 is a deliberate, painful design decision.** It means the player **will not automatically spot the goblins, the snare, or the pit.** They will have to *actively Search*. The game teaches the difference between passive and active perception by making the player feel the absence of the passive one. (See 6.4.4.)

## 5.5 Station Five — THE OATH (alignment, bond, and Gundren)

A brazier. Gundren Rockseeker is here, and this is the first time you ever speak to an NPC — which means **this is the first time the player uses the Influence action without knowing it.**

**Alignment.** ✅ In 2024 this is a roleplaying shorthand, not a mechanic. Presented as **two sliding scales** — Lawful↔Chaotic, Good↔Evil — that write a single sentence onto your sheet: *"You believe in the order of the shield wall, and in the people behind it."* The sentence is editable. It surfaces in dialogue.

**The Bond.** Three prompted questions, each with four choices and a free-text option:
1. *Why did you leave the army?*
2. *Who did you fail?*
3. *What are you carrying that isn't yours?*

Each answer writes a **flag** to the world ledger. In the Tutorial these surface exactly twice — but they surface, and the player notices.

**How you know Gundren.** ✅ The adventure explicitly asks the DM to establish this. The game offers four pre-written hooks plus free text:
- He paid your discharge fee.
- He pulled you out of a river.
- He was your sergeant's brother.
- You don't know him. You needed ten gold.

This flag drives Gundren's opening attitude toward you (Friendly vs Indifferent) and therefore the **DC of every social check involving him for the rest of the campaign.** Character creation is already doing mechanical work.

**Then Gundren speaks.** He explains the job — a wagonload of provisions to **Barthen's Provisions** in Phandalin, **10 gp each** on delivery ✅ — and he's excited and secretive about it, saying only that he and his brothers found "something big." ✅ He rides ahead with **Sildar Hallwinter**. ✅ You take the wagon.

**The contract writes itself in ink across the screen and a wax seal stamps it.** That is the quest log entry being created, and it is diegetic.

## 5.6 Station Six — THE PRACTICE YARD (the tutorial that isn't one)

Before the road, there is a **ten-yard square of packed dirt behind the tent**, with a straw dummy, a lantern on a post, and a length of embankment.

Gundren says: *"Show me you can hold a line."*

In ninety seconds, without a single tutorial prompt, the player is taught:

| Beat | What it teaches |
|---|---|
| Walk to the dummy | Movement, the reach ring, the grid |
| Swing at it | **The die.** First roll of the game. Full cinematic. +5 vs a dummy AC of 10. |
| It doesn't die | Damage, HP bars, that combat is a sequence |
| Swing until it breaks | Critical hit chance, the nat-20 presentation (guaranteed to roll a 20 once here, seeded — the *only* rigged roll in the game, and it's in the practice yard) |
| Gundren: *"Now the clever bit. Don't kill it."* | **The knockout input.** The dummy drops to 1 HP and goes Unconscious ✅ |
| Gundren: *"What can you see from there?"* | Light. Walk out of the lantern's Bright radius and the screen cools; the *Dim Light* rule is shown, not told |
| Gundren: *"Talk me out of it."* | **Influence.** One dialogue stance, one die. The player learns that talking is a roll before they ever meet an enemy who talks back |
| Gundren: *"Right. Get in the wagon."* | The game is on |

**No tutorial text appears anywhere in this sequence.** Everything is Gundren talking and the world responding. This is Pillar 2 and Pillar 3 working together, and it is the sequence that will make or break the game's first impression.

## 5.7 The character sheet

Open it with `I` at any time. It is a **real 2024 D&D character sheet**, hand-illustrated on parchment, with the actual layout — because the promise is that you can play at a real table afterward.

Every number on it is **live and hoverable**: hover `+5` on Strength and a parchment tooltip unfurls showing `+3 (ability) + 2 (proficiency) = +5`, with the rule name **Proficiency Bonus** ✅ inked beneath it in small caps. Click the rule name and the **Codex** opens at that entry.

The sheet has one extra element no paper sheet has: a thin **amber thread** connecting related numbers. Hover your AC and threads light up to your armor, your shield, and the cover tile you're standing on — **showing you, graphically, that AC is a sum of the world.**

---
---

# PART VI — VIDEO-GAMEIFYING THE RULES

The core translation. Each section takes a block of the 2024 ruleset and answers three questions: **what is the rule**, **what becomes a verb**, and **what does the player see.**

## 6.1 THE SIX ABILITIES ✅

The PHB and DMG agree on the six and on what each measures ✅:

| Ability | Measures ✅ | In this game it drives |
|---|---|---|
| **Strength** | Physical might | Melee damage, carrying, shoving, breaking things, **Strength (Intimidation)** |
| **Dexterity** | Agility, reflexes, balance | Initiative ✅, AC in light armour, Stealth, Acrobatics, ranged attacks, every Dex save |
| **Constitution** | Health and stamina | HP, concentration, endurance, resisting poison |
| **Intelligence** | Reasoning and memory | **Study** checks, Investigation, History, Arcana, Nature, Religion |
| **Wisdom** | Perceptiveness and mental fortitude | **Search** checks, Perception, Insight, Survival, Medicine, every Wis save |
| **Charisma** | Confidence, poise, charm | **Influence** checks, Persuasion, Deception, Intimidation, Performance |

**The design move:** the six abilities are mapped onto the three verbs the player actually presses.
- **Search** = Wisdom (Insight, Medicine, Perception, Survival) ✅
- **Study** = Intelligence (Arcana, History, Investigation, Nature, Religion) ✅
- **Influence** = Charisma (Deception, Intimidation, Performance, Persuasion) or Wisdom (Animal Handling) ✅

**These are three of the twelve actions in the PHB's own action table ✅.** The ruleset already told us how to structure the interaction menu. We just obeyed it.

## 6.2 EXPLORATION AS AN INTERACTION SYSTEM

### 6.2.1 The contextual prompt — "the eye"
Approach anything interactable and a small **hand-drawn eye** rises above it, ringed by the ability it wants:

```
        ◉  WIS (PERCEPTION)      Search the saddlebags
        ◉  INT (INVESTIGATION)   Study the arrow fletching
        ◉  STR (ATHLETICS)       Right the wagon
        ◉  CHA (PERSUASION)      Speak to the goblin
        ◉  DEX (STEALTH)         Hide behind the embankment
        ◉  —                     Take the map case   (no roll needed ✅)
```

**The last line is the most important one in the whole system.** When the DMG says a task is trivial, **no roll happens** ✅ — and the game must *show* that. An unringed eye means "just do it." This trains the player to read the ring, which means the moment a ringed eye appears they feel the stakes.

### 6.2.2 Active vs passive checks ✅
- **Passive Perception** = 10 + all modifiers ✅. The DMG explicitly invites extending this to **Passive Insight** and other passives ✅. The game implements **Passive Perception, Passive Insight and Passive Investigation** as always-on background sweeps.
- **When your passive beats a hidden thing's DC, you get a *whisper*, not an answer:** the music dips a semitone, a dust mote hangs in the air where the trap is, the edge of the screen cools by 2%. **Nothing is revealed.** You now know to Search.
- **Active Search** is an Action (costs a turn in combat, costs ~10 seconds out of it) and rolls the die.

**The lesson this teaches:** *noticing is a skill, and looking is a choice.* Which is exactly what the DMG's Perception section is about ✅.

### 6.2.3 Trying again, and the cost of time ✅
The DMG's rule: sometimes failing makes retrying impossible; sometimes the only cost is time; and if failure has no consequences and retrying is free, **skip the check and just say how long it takes** ✅.

The game implements this literally:
- **Consequential** (picking a lock with a guard coming) → one roll, failure has teeth.
- **Time-costed** (forcing a stuck door) → the roll determines **how long it takes**: a low success takes three minutes and the world clock advances; the light moves; the encounter may find you.
- **Trivial** → no roll. The door opens.

**This is why time is a first-class resource in this game.** There is a world clock, always running, always visible as the position of the sun and the length of the shadows. Lingering has a cost.

### 6.2.4 Group checks ✅
Everyone rolls; if **at least half** succeed, the group succeeds ✅. The DMG says don't use them where one failure is catastrophic (stealth) or where one success suffices (finding a hidden compartment) ✅.

The game uses them for exactly the DMG's examples: **research, roped-together climbs, and social situations** ✅ — and the presentation is glorious: a row of dice all thrown at once across the screen, landing one by one, the tally inked up beneath them.

### 6.2.5 Arguing for your proficiency
The DMG says be **generous** with proficiency and even allows a Strength (Intimidation) check instead of Charisma ✅, and invites the player to **negotiate** which proficiency applies ✅.

So the game lets you do that. When a prompt offers `CHA (PERSUASION)`, hold `Alt` and a second ring of your *other* proficiencies appears. Pick **Athletics** and you get `STR (ATHLETICS) — you crack your knuckles instead of making a speech`. **This is the single most beloved rule at real tables and it costs one modifier to implement.**

### 6.2.6 Saving throws as a defensive layer
The DMG: a saving throw is a reaction to something bad, almost never by choice ✅.

The game gives every telegraphed threat a **tell** — the goblin draws back, the rope creaks, the ground cracks — and a **0.6-second reaction window** in which pressing the right defensive input grants Advantage on the resulting save. Not "dodge the attack": **dodge the *save*.** This makes Constitution and Dexterity saves feel like skill without changing a single number.

## 6.3 SOCIAL INTERACTION AS A TACTICAL ENCOUNTER

The brief said: don't make social a separate mode. Here is how it isn't one.

### 6.3.1 The battlefield is a person
When you enter a conversation, the camera drops to **Z1 (The Eye)**, the background dims and blurs, and **the NPC's portrait becomes a stat block** — but rendered as a *person*, not a box:

```
              ┌──────────────────────┐
              │   [painted portrait]  │
              │                       │
              │  GOBLIN, BOUND        │
              │  Attitude  HOSTILE ◄──┼── the attitude track, ✅ DMG
              │  ──────────────────── │
              │  ❤ 1 HP  (Unconscious)│
              │  ? Fears              │  ← revealed by Wis (Insight)
              │  ? Wants              │  ← revealed by Cha (Persuasion)
              │  ? Knows              │  ← the actual objective
              │  ? Lying about        │  ← revealed by a failed Deception
              └──────────────────────┘
```

**Four hidden layers: Fears, Wants, Knows, Lies.** You do not fight the goblin's HP; you fight its **Resistance**.

### 6.3.2 Attitude ✅
Straight from the DMG: every DM-controlled creature has an attitude of **Friendly, Indifferent, or Hostile** ✅, and characters can shift it by words or actions ✅, and **the DM must describe the shift when it happens** ✅.

The attitude track is a **physical slider on the portrait** — a wax token that slides along a rule from Hostile to Friendly. When it moves, it moves *visibly and with sound*, and the NPC's idle animation changes: a Hostile goblin bares its filed teeth; an Indifferent one stops struggling; a Friendly one starts talking with its hands.

The DMG's **Initial Attitude** table ✅ (1d12: 1–4 Hostile, 5–8 Indifferent, 9–12 Friendly, with different dice for different creature types) is used verbatim by the AI director for any creature the adventure hasn't pre-set.

### 6.3.3 Stances are weapons
The **Influence action** ✅ is the attack. The four Charisma skills are the four weapons, and they behave *differently*, with different risk profiles — exactly like choosing a Fighting Style:

| Stance | Skill | Damage to Resistance | Risk |
|---|---|---|---|
| **Persuade** | Cha (Persuasion) | Moderate | **Low.** Failure costs time. Repeated success compounds. The honest weapon. |
| **Intimidate** | Cha (Intimidation) | **High** | **High.** Fast results, but every point of "damage" writes a permanent **Grudge** flag. This goblin will remember. Its band will hear about it. |
| **Deceive** | Cha (Deception) | **Very high** | **Delayed.** A successful lie plants a **false belief** the target acts on — and which can be *discovered later*, converting every point of "damage" into Hostility at the worst possible moment. |
| **Perform** | Cha (Performance) | Low, but hits **everyone present** | An AoE. Useless on one bound goblin; devastating on a crowd. |

**And the non-Charisma options the DMG explicitly encourages ✅:**
- **Strength** — pick the goblin up by its ankles. `STR (ATHLETICS)`.
- **Dexterity** — talk from hiding; or lift its purse while it's distracted ✅.
- **Intelligence** — catch it in a contradiction it can't parse ✅.
- **Wisdom (Insight)** — the **Search action of conversation**: reveal one hidden layer, and see through a lie ✅.

**Switching stance mid-conversation is switching weapon mid-fight.** Same input model, same die, same Roll Ledger, same animations. **This is Pillar 2 made literal.**

### 6.3.4 The Resistance roll
```
INFLUENCE CHECK
  d20 + Cha mod + proficiency (if the stance's skill is proficient)
  + Attitude modifier   (Friendly +2 / Indifferent +0 / Hostile −2)
  + Leverage            (what you're offering: gold, mercy, a knife, the truth)
  + Help                (an ally's Help action ✅)
  vs.  the target's Resistance DC
```

**Resistance DC = 8 + the target's relevant ability modifier + its Proficiency Bonus** ✅ — the DMG's own **Calculated DC** formula ✅. A goblin's is low. A bugbear's is not. A king's is enormous.

### 6.3.5 Failure is not a dead end — it's a consequence ✅
The DMG insists that failure must have meaningful consequences and that the adventure must keep moving ✅. So:

| Failure | Consequence |
|---|---|
| **Marginal fail (1–4 short)** | The target's Resistance goes up by 2 for this conversation. You learn one hidden layer anyway — you pushed too hard and it showed. |
| **Clean fail (5–9 short)** | Attitude drops one step. The target clams up; the conversation can be resumed but the DC is permanently higher. |
| **Catastrophic fail (10+ short, or nat 1)** | Attitude goes **Hostile** and stays there. In combat, this means the target fights to the death instead of surrendering. **The nat 1 in a conversation is a real, permanent loss** — and it is the reason players will hold their breath on a Persuasion roll. |

**No conversation can be reloaded.** The world clock runs, the ledger records, and there is always another way to get the information — the DMG's **"Multiple Ways to Progress"** ✅ principle. You failed to talk the goblin down? Fine. Search its body. Track its friends. Wait for it to wake up frightened. **The adventure moves.**

### 6.3.6 The Help action in conversation ✅
The DMG says: when a character Helps another influence an NPC, **encourage the player to actually contribute to the conversation** ✅.

So the game requires it. Pressing Help opens a one-line contribution prompt with three options and a free-text box. The *content* you choose determines the size of the Advantage. Pick the wrong angle and you get nothing. **A teammate's Help is a real tactical choice, not a free +bonus.**

### 6.3.7 Rumours, knowledge, and the "knows / knew" split
Two separate ledgers, because the distinction matters enormously in a mystery-driven campaign:
- **`world.knows`** — what is true.
- **`party.knew`** — what the player has actually learned, and *when*.

The **Journal** renders `party.knew` as handwritten entries that appear in ink at the moment of learning, with a timestamp and a source (*"Learned from a bound goblin, Triboar Trail, afternoon"*). This means the game can always answer "how do you know that?" — and can build entire quests around information the player *missed*.

### 6.3.8 Haggling, services and lifestyle
The PHB's **Services**, **Lifestyle Expenses** and **Food, Drink, and Lodging** tables ✅ become the economy layer. In the Tutorial the only merchant is Gundren's contract and whatever you loot, but the system is live: prices are data, haggling is an Influence check with a Leverage modifier, and your coin pouch is a physical object that gets lighter.

## 6.4 EXPLORATION, TRAVEL & THE WILDERNESS

### 6.4.1 One world, three camera heights
Pillar 2's technical expression. There are **no separate maps, no loading screens, no scene transitions.** The overland map *is* the tactical map, zoomed out — and as you zoom, the painted world **cross-fades into the hand-drawn cartographer's version of itself**, ink lines drawing in over the paint. Zoom back in and the ink dissolves back into trees.

### 6.4.2 Travel pace ✅
The PHB's Travel Pace table, verbatim:

| Pace | Per minute | Per hour | Per day ✅ | Effect ✅ |
|---|---|---|---|---|
| **Fast** | 400 ft | 4 miles | 30 miles | **Disadvantage** on Wisdom (Perception or Survival) and Dexterity (Stealth) checks |
| **Normal** | 300 ft | 3 miles | 24 miles | **Disadvantage** on Dexterity (Stealth) checks |
| **Slow** | 200 ft | 2 miles | 18 miles | **Advantage** on Wisdom (Perception or Survival) checks |

**The game makes this a physical dial** in the corner of the screen: a small painted boot with three positions. Move it to Fast and the camera pulls back, the music quickens, the party's animation shifts to a jog, and a cold verdigris wash creeps in over the perception icons to show you what you've given up. **You feel the trade-off in the animation before you ever read the rule.**

### 6.4.3 Terrain ✅
The DMG's **Travel Terrain** table ✅ gives maximum pace, encounter distance, and Foraging / Navigation / Search DCs per terrain type. For the Tutorial's ground:

| Terrain | Max pace ✅ | Encounter distance ✅ | Foraging DC ✅ | Navigation DC ✅ | Search DC ✅ |
|---|---|---|:---:|:---:|:---:|
| **Forest** (Neverwinter Wood) | Normal | 2d8 × 10 ft | 10 | 15 | 15 |
| **Hill** (the Sword Mountains foothills) | Normal | 2d10 × 10 ft | 15 | 10 | 15 |
| **Grassland** (the approaches) | Fast | 6d6 × 10 ft | 15 | 5 | 15 |

**Encounter distance is the game's spawn-distance rule** ✅ — how far away a random encounter appears is rolled, not scripted. Two playthroughs of the same road will never ambush you from the same distance.

### 6.4.4 Marching order ✅
The PHB calls this out in a sidebar: **the marching order determines who is affected by traps, who spots hidden enemies, and who is closest if a fight breaks out** ✅. The adventure itself instructs the DM to ask the players for their marching order before the trapped trail ✅.

**This is the single best under-used rule in D&D and it becomes a headline mechanic.** Before any journey, the player **drags tokens into a line.** The lead token is the one who rolls to spot the snare. The second is the one who takes the arrow. The last is the one who has to Dash to reach the fight.

**Presentation:** at Z3 the marching line is drawn on the map as a dotted ink trail with numbered tokens. Change the order and the trail redraws. **In the Tutorial this is taught by the wagon:** you can't all ride, so someone walks, and *who walks* is your choice.

### 6.4.5 Hiding, stealth and sound ✅
- **Hide action** = Dexterity (Stealth) check ✅. **Your Stealth total becomes the DC for anyone's Wisdom (Perception) check to find you** ✅ (the DMG's Calculated DC pattern, and the DMG uses hiding as its own example ✅).
- **Unseen attackers** ✅: attacking a target you can't see = Disadvantage; a target that can't see you = you have Advantage; **if you're hidden when you attack, you reveal your position whether you hit or miss** ✅.
- **Heavy armour imposes Disadvantage on Stealth** ✅ (chain mail, scale mail, half plate, ring mail, splint, plate).
- **Audible distance** ✅ (DMG): trying to be quiet = 2d6 × 5 ft; normal noise = 2d6 × 10 ft; very loud = 2d6 × 50 ft.

**Sound is simulated as a field.** Every action emits a noise radius: walking in mail = normal; running = loud; a nat-1 on Stealth = very loud; a drawn sword = quiet. The field is rendered (when you hold the stealth key) as **expanding ink ripples** on the ground, in the Emberlight style. Enemies have **hearing cones** that light up as a ripple enters them.

**This is the stealth system.** It is not a detection meter. It is D&D's actual numbers, drawn as ink on parchment.

### 6.4.6 Traps ✅
Three states, and the transition between them is the game:

```
   UNDETECTED  ──passive Perception beats trap DC──►  WHISPERED
        │                                                │
        │ (you trigger it)                    (you take the Search action)
        ▼                                                ▼
    TRIGGERED ◄──────── fails the check ──────── DETECTED ──► DISARMED
```

- **Whispered** is the state no other game has: you don't know what or where, only *that*. The screen cools 2%, the music drops a semitone, a dust mote hangs. **This is passive Perception made felt.**
- **Detected** triggers the **Sight** render mode for 0.6 s: the world desaturates and the trap glows ember.
- **Disarming** is a Dexterity check with thieves' tools or an improvised solution, and **you can also just avoid it**, or **trip it deliberately on an enemy** — which the AI director will absolutely do to you.
- The DMG's **Traps** section ✅ supplies the DC and severity tables; the game's trap editor is data-driven so later chapters can add traps without code.

### 6.4.7 Resting and camping
Rest is not a button. It is **a place you choose on the map.**

Set camp and the game evaluates the site: distance from the road, cover, light discipline (do you light a fire?), whether you set a watch, whether the ground is dry. Each factor modifies the rest:

| Camp quality | Result |
|---|---|
| **Good** (hidden, watched, dark) | Full Long Rest benefits ✅ |
| **Poor** (off the road, no watch) | Rest completes but roll on the encounter table |
| **Bad** (on the road, fire lit) | **Interrupted.** ✅ A Long Rest interrupted grants reduced benefit; you get a Short Rest at best |

**Lighting a fire at night is a decision with a real cost**, and the game makes you feel it: the fire is *beautiful*, warm, animated, and it paints your position in ember for two miles. The player will light it anyway, the first time. That's the lesson.

### 6.4.8 The Journal and the Codex
Two books, one binding, opened with `J`.

- **The Journal** (diegetic): Quests, People, Places, Rumours, Bestiary. Written in ink, in your character's hand, with sketches that *draw themselves* the first time you meet something.
- **The Codex** (the rules): **starts empty.** Every time a rule fires for the first time, its entry writes itself into the Codex with a small gold ✦ and a soft chime. After the ambush the player's Codex contains: *Attack Roll, Ability Modifier, Proficiency Bonus, Advantage, Disadvantage, Armor Class, Cover, Opportunity Attack, Initiative, Surprise, Critical Hit, Unconscious, Death Saving Throw, Passive Perception, Difficult Terrain, Lightly Obscured.*

**The Codex filling up is the game's real progression system for the first hour.** Players will open it between fights just to see what they've unlocked. It is the most powerful teaching device available and it costs one event hook.

## 6.5 COMBAT, IN FULL DETAIL

### 6.5.1 The attack roll ✅
```
d20 + ability modifier + Proficiency Bonus  ≥  target's AC
```
- Proficiency applies only with a weapon you're proficient with ✅ — a level-1 Fighter is proficient with **all simple and martial weapons** ⚠️ (verify), so this never bites in the Tutorial, but the system supports it.
- **Nat 20 = automatic hit and critical ✅. Nat 1 = automatic miss ✅.**
- **Cover is added to AC** ✅ (see 6.5.2).
- **Unseen attacker rules** apply ✅ (see 6.4.5).

### 6.5.2 Cover ✅ — computed from geometry
| Degree ✅ | Benefit ✅ | Offered by ✅ |
|---|---|---|
| **Half** | +2 to AC and Dexterity saving throws | A creature or object covering at least half the target |
| **Three-Quarters** | +5 to AC and Dexterity saving throws | An object covering at least three-quarters of the target |
| **Total** | Can't be targeted directly | An object covering the whole target |

Only the **most protective** degree applies; they don't add ✅. Cover only helps against attacks originating on the **opposite side** of the cover ✅.

**Implementation:** the game raycasts from attacker to target through a per-tile cover mask, at three heights (low/mid/high) so crouching and elevation matter. The result is cached per attacker-target pair and re-computed on any movement.

**Presentation:** when you select a target, the tiles granting it cover glow with a hard shadow gradient and a small `+2` or `+5` inked beside the target's AC. **Move two feet and the number changes.** That is the whole tactical game of D&D, made visible.

### 6.5.3 Ranged combat ✅
- **Normal range / long range** ✅ — beyond normal range you have **Disadvantage**; beyond long range you **can't attack at all** ✅.
- **Ranged attacks in close combat** ✅ — **Disadvantage** if you're within 5 ft of an enemy who can see you and isn't Incapacitated.
- **Ammunition** ✅ is a real, counted resource with a **Loading** property on crossbows ✅.

**Presentation:** the ground is painted with two concentric arcs around you — a solid amber arc at normal range, a dashed cold arc at long range. Beyond the dashed arc the target reticle **greys out and refuses to close.** The player learns range bands in one shot, forever.

### 6.5.4 Melee combat ✅
- **Reach** is normally 5 ft ✅; some weapons (Glaive, Halberd, Lance, Pike, Whip) have **Reach** ✅.
- **Opportunity Attacks** ✅ — see 4.2. **Disengage** avoids them ✅; so does teleporting or being moved without using your own movement ✅.
- **Two-weapon fighting** via the **Light** property, with **Nick** mastery folding the extra attack into the Attack action ✅.

### 6.5.5 Conditions ✅ — the full 2024 list, and what each one looks like

The PHB's complete condition list ✅: **Blinded, Charmed, Deafened, Exhaustion, Frightened, Grappled, Incapacitated, Invisible, Paralyzed, Petrified, Poisoned, Prone, Restrained, Stunned, Unconscious.** Conditions don't stack ✅ — you have it or you don't — **except Exhaustion**, whose effects worsen ✅.

Every condition gets: **a rules entry, an icon, a unique animation state, and a unique sound.** This is a hard content requirement and it is in the animation budget (Part IX).

| Condition | On screen | Sound |
|---|---|---|
| **Blinded** | Eyes painted over in ink; the world renders as an audio-only waveform edge | Muffled low-pass on all world audio |
| **Charmed** | A rose-gold thread from charmer to charmed; the charmed creature's colours warm | A soft, wrong major chord |
| **Deafened** | The sound layer *literally drops out* for that creature's POV cues | Silence, then tinnitus |
| **Exhaustion** | The character's animation drops a gear per level; posture sags; sweat | Heavier breathing, layered |
| **Frightened** | The creature recoils away from the source and cannot move closer; a cold verdigris tremor | A high thin string |
| **Grappled** | Two rigs interlock with a shared constraint; both movement speeds change | Cloth and struggle |
| **Incapacitated** | The creature goes still; its action icon greys and crosses out | A dull thud |
| **Invisible** | The creature renders as a heat-haze distortion and a footprint trail only | Only footsteps remain |
| **Paralyzed** | Frozen mid-frame, rigid, with a faint amber crackle | Total absence of motion sound |
| **Petrified** | A stone shader creeps from the feet up over 1.2 s | Grinding mineral |
| **Poisoned** | A green film on the skin, a slow drip particle | A wet cough |
| **Prone** | Full-body ground state; **half movement to stand ⚠️; attacks from within 5 ft have Advantage ⚠️** | A body hitting dirt |
| **Restrained** | Bound with rope/net/vines; **Speed 0 ⚠️** | Straining rope |
| **Stunned** | The creature drops what it's holding — *literally, the weapon object spawns and falls* | Clattering steel |
| **Unconscious** | Limp, **Prone ⚠️**, unaware; the knockout state used for captures | A soft exhale |

### 6.5.6 The AI Director — how monsters actually behave

The DMG's **Monster Behavior** section ✅ is the design spec. Every monster in the game carries:

- **An initial attitude** ✅ (Friendly / Indifferent / Hostile), pre-set by the adventure or rolled on the DMG's table ✅.
- **A personality** ✅ from the DMG's Monster Personality table (1d8): *Cowardly — surrenders easily; Greedy — wants treasure; Boastful — makes a show of bravery but runs from danger; Disorderly — poorly trained and easily rattled; Fanatical — ready to die fighting* ✅ …
- **A morale curve** driven by: allies downed, HP lost, whether it's the last one standing, whether it's been Intimidated, whether it has an escape route.
- **A behaviour tree** with tactical nodes: `flank`, `take_cover`, `use_mastery`, `focus_fire_weakest`, `disengage_and_reposition`, `flee`, `surrender`, `take_hostage`.

**The Cragmaw goblins in the Tutorial are `Disorderly`** ✅ — "poorly trained and easily rattled" — which is *exactly* why the adventure says the last one flees ✅. The behaviour isn't scripted; it **emerges from the personality the DMG told us to give them.** That is the correct way to build a D&D AI.

### 6.5.7 The Encounter Director — pacing
A meta-system above the AI that manages **tension across the whole scene**, using the DMG's own encounter-design guidance ✅: changes in elevation, defensive positions, hazards, mixed monster groups, and **reasons to move** ✅. It:
- Tracks the party's HP and resource state and adjusts spawn timing within the adventure's authored bounds.
- Decides when a fight should *end* (morale break, flee, surrender) rather than run to the last HP.
- Enforces the DMG's **"Many Creatures"** warning ✅: at levels 1–2, more than two creatures per character means those creatures must be fragile.

## 6.6 PROGRESSION

- **XP** is awarded per the DMG/monster XP values, **and the game supports milestone advancement** — which is what the adventure itself uses: *"The characters gain a level when they finish exploring the Cragmaw hideout"* ✅. The Tutorial therefore ends at level 1, and the level-up is the reward for the *next* scene.
- **Level-up** is a full diegetic ceremony: the journal closes, a wax seal stamps **LEVEL 2** across it in gold leaf, the camera drops to Z1, and the character does a single, unique, hand-animated "settle into your new skin" animation. HP increases, Proficiency stays +2 until level 5 ✅, and new features unlock as **new Codex entries**.
- **Weapon Masteries** expand at level 4 ⚠️ (verify) — the mastery picker returns and you choose again. This is the game's mid-campaign build hook.
- **Feats / Ability Score Improvements** arrive on the 2024 schedule ⚠️ (verify).
- **Renown** (DMG ✅) and **Marks of Prestige** (DMG ✅) are the reputational layer: Phandalin's factions will have Renown tracks, and the Tutorial writes the first entries.

---
---

# PART VII — ONE GAME, NOT THREE

The brief's hardest requirement: exploration, social interaction and combat must not be modes. They must be one game. Here is the actual mechanism that makes that true, not just aspirational.

## 7.1 The unifying insight

**All three pillars of D&D are the same loop with different opponents.**

```
        ┌──────────────────────────────────────────────────────┐
        │  PERCEIVE → DECIDE → COMMIT → ROLL → CONSEQUENCE     │
        └──────────────────────────────────────────────────────┘
                 ▲                                    │
                 └────────────────────────────────────┘
```

| Pillar | The opponent | The weapon | The win condition |
|---|---|---|---|
| **Exploration** | A Difficulty Class | Wisdom, Dexterity, Strength | Information or passage |
| **Social** | A person's Resistance | Charisma (or anything, argued) | A shift in Attitude |
| **Combat** | An Armor Class | A weapon | 0 HP — **or Unconscious** |

Same loop. Same die. Same Roll Ledger. Same animation budget. **The only thing that changes is the tempo** — and tempo is a camera height and a music layer, not a state machine.

## 7.2 The Tempo Dial — the technical answer

The game has exactly one "mode" variable, and it is continuous:

```
TEMPO 0.0 ─────────────── 0.5 ─────────────── 1.0
  THE ROAD                 THE EDGE              THE ROUND
  hours per tick        minutes per tick      6 seconds per round
```

- At **0.0** the world clock runs in hours. You can't be attacked. Music is ambient. Camera is free.
- At **0.5** something is wrong. The clock runs in minutes. Enemies may be near. Music gains a pulse layer. Camera tightens to Z2.
- At **1.0** Initiative has been rolled. The clock runs in rounds. Combat UI is live.

**Crucially, the player can be at 0.5 and never go to 1.0** — and can go from 0.0 to 1.0 in a single frame when the goblins spring. **And can go from 1.0 back to 0.5 mid-fight** the moment the last goblin's morale breaks, without a victory screen.

**Social interaction runs at whatever tempo the scene is already at.** You can Influence a goblin at Tempo 1.0 — it costs an Action, the initiative ribbon keeps moving, the other goblins keep coming. **This is the single most important implementation detail in the game**, and it is why conversation never feels like a pause.

## 7.3 Six worked transitions, all with no screen change

| From → To | What the player sees |
|---|---|
| **Exploration → Combat** | You approach the horses. The world clock stops. A heartbeat. The four goblin silhouettes ignite with verdigris rim light *simultaneously*. The camera snaps to Z2 and the initiative ribbon unfurls. **Total elapsed: 1.2 s. No fade.** |
| **Combat → Social** | The last goblin drops its scimitar and raises its hands. Its portrait slides from the initiative ribbon into the conversation frame *without leaving the screen*. The battlefield stays live behind it, blurred. Tempo drops to 0.5. |
| **Social → Combat** | You roll a nat 1 on Intimidate. The goblin's attitude token slams to Hostile with a crack. Its portrait slides *back* onto the initiative ribbon. Tempo snaps to 1.0. **The die that caused this is still on screen.** |
| **Combat → Exploration** | The last goblin dies. The music's combat layers fade over 3 s. The initiative ribbon rolls itself up like parchment. The camera relaxes. The world clock resumes. Loot prompts rise as eyes. **No victory screen. No XP popup.** XP writes itself silently into the Journal. |
| **Exploration → Social** | You round a bend and there's a traveller. The camera drifts to Z1 over 1.5 s *while you keep walking*. The conversation starts mid-stride. |
| **Any → The Map** | You hold the zoom. The world cross-fades to ink. You drag your marching order. You release. The ink dissolves back into trees. |

## 7.4 Why the player will believe it

Three concrete, checkable guarantees:

1. **No loading screen anywhere in the Tutorial.** The entire campaign ships as one streamed world.
2. **No screen ever goes black between activities.** Every transition is a camera move.
3. **The Roll Ledger never clears.** It runs continuously from the practice yard to the end of the trail. Scroll back and you can see the Persuasion roll from ten minutes ago sitting right next to the attack roll from ten seconds ago, in the same font, in the same book.

That third one is the trick. **When the rules are rendered identically everywhere, the player stops perceiving categories.** They stop thinking "this is the talking part." They just think "I'm rolling."

---
---

# PART VIII — "THE TUTORIAL" CAMPAIGN

**Source:** *Phandelver and Below: The Shattered Obelisk*, Chapter 1 — "A Dangerous Journey." ✅
**Scope of this design:** the road, the bend, the ambush, the interrogation, and the finding of the trail. **Stops there.**

## 8.1 What the Tutorial must accomplish

| # | Job | How it's measured |
|---|---|---|
| 1 | Teach the d20 pipeline completely | Player can explain Advantage without the Codex |
| 2 | Teach the action economy | Player has used at least 8 of the 12 actions |
| 3 | Teach positioning: cover, reach, Opportunity Attacks, Difficult Terrain | Player has used cover to change an AC |
| 4 | Teach that **killing is a choice** | Player discovers the knockout input |
| 5 | Teach that **talking is a system** | Player wins at least one Influence check |
| 6 | Teach that **perception is a resource** | Player searches actively at least twice |
| 7 | Teach that **failure moves the story** | Player experiences at least one meaningful failure |
| 8 | Make the player care about Gundren and Sildar | Player chooses to pursue the trail unprompted |

**Eight jobs in 45–75 minutes, with no tutorial text.** That is the design brief for this chapter.

## 8.2 The frame — what the adventure establishes ✅

Read from the source:

- The heroes are **escorting a wagonload of provisions from Neverwinter to Phandalin** for **Gundren Rockseeker**, hired for **10 gp each** on delivery to **Barthen's Provisions**. ✅
- Gundren rode ahead with **Sildar Hallwinter**, a warrior escort, saying he had to "take care of business." He was excited and secretive: he and his brothers found **"something big."** ✅
- The party has come south down the **High Road** from Neverwinter and **turned east onto the Triboar Trail.** ✅
- They are a **half-day's march from Phandalin** when trouble finds them. ✅
- **The wagon** is pulled by **two oxen**, needs no skill to drive, and the oxen stop if no one holds the reins ✅. Cargo: a dozen sacks of flour, several casks of salted pork, two kegs of ale, about a dozen each of shovels/picks/crowbars, five lanterns, and a small barrel of oil (about fifty flasks). **Total cargo value: 100 gp.** ✅
- Advancement: characters start at **level 1** and level up on finishing the Cragmaw Hideout ✅ — i.e. **the Tutorial ends at level 1 by design.**

## 8.3 THE BEAT MAP

```
 00:00  ┌─ BEAT 1  THE CONTRACT          (created in Part V — the campaign selector,
        │                                 character creation, the practice yard)
 08:00  ├─ BEAT 2  THE HIGH ROAD         Tempo 0.0 · teaches travel, pace, the wagon
 14:00  ├─ BEAT 3  TRIBOAR TRAIL         Tempo 0.0→0.5 · marching order, light, weather
 20:00  ├─ BEAT 4  THE BEND              Tempo 0.5 · the scene. Investigation before contact
 24:00  ├─ BEAT 5  THE AMBUSH            Tempo 1.0 · 4 goblins. The first real fight
 31:00  ├─ BEAT 6  THE ROUT              the last goblin runs — or doesn't
 35:00  ├─ BEAT 7  THE AFTERMATH         loot, bodies, the two horses, the map case
 40:00  ├─ BEAT 8  THE INTERROGATION     Tempo 0.5 · social. The heart of the chapter
 50:00  ├─ BEAT 9  THE TRAIL             DC 10 Wisdom (Survival) ✅ · the thread is picked up
 55:00  └─ BEAT 10 THE SNARE             [optional coda] · teaches marching order's teeth
 60:00     END CARD → CHAPTER II
```

## 8.4 BEAT 2 & 3 — THE ROAD (the calm that makes the bend work)

**This section is not filler and must not be cut.** The ambush only lands if the player has been lulled.

### The High Road (Tempo 0.0, ~6 minutes)
- Camera free, Z2 default. Beautiful. Emberlight at its warmest — the *only* time in the Tutorial the palette is genuinely warm. **This is deliberate: the player must associate warmth with safety so that the bend feels cold.**
- **Teaching, incidentally:**
  - The **wagon** is an interactable object. Climb onto it, sit on it, drive it. The oxen stop when you let go of the reins ✅.
  - **Travel pace dial** ✅ — Gundren's foreman suggested you make good time. Go Fast and you'll blur past the first hint.
  - **A travelling NPC** — a pedlar heading the other way — who warns of "goblins on the trail, filed teeth, nasty little things." One Influence check to get more out of him. **This is the player's first real social encounter, against someone with no reason to lie, at zero stakes.**
  - **Weather** ✅ (DMG weather table: 1–14 normal, 15–17 colder with light rain, 18–20 hotter with heavy rain). Rain reduces visibility, makes the road Difficult Terrain in patches, and *douses the lantern*. Weather is a rules layer, not a shader.

### Triboar Trail (Tempo 0.0 → 0.5)
- The road narrows. The treeline closes. **The palette shifts toward verdigris over four minutes without the player noticing.**
- **The marching order prompt fires here.** ✅ The trail is too narrow for the wagon and the party abreast. Someone has to walk point. **You choose who.** (In the single-player Tutorial, "who" is: you on foot with the wagon behind, you on the wagon, or you scouting twenty feet ahead. Three formations, three different ambush outcomes.)
- **The music gains a pulse layer.** The player does not consciously notice. Their hands get slightly quieter on the controls. This is measured in playtests: if players don't slow down here, the section has failed.

## 8.5 BEAT 4 — THE BEND (the scene)

### The map — "Map 1.1: Goblin Ambush" ✅
Rebuilt as a hand-painted tactical plane, **34 × 22 tiles (170 ft × 110 ft)**.

```
        NORTH  (thick forest, rising ground, deep shadow — verdigris)
   ┌──────────────────────────────────────────────────────────────┐
   │ ▓▓▓▓▓▓▓▓▓▓▓▓▓▓  ▓▓  G1 ▓▓   ▓▓▓▓▓▓▓   ▓▓  G2 ▓▓  ▓▓▓▓▓▓▓▓▓ │  steep
   │ ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓ │  embankment
   │ ░░░░░░░░░░ thickets ░░░░░░░░░░░░░░░░░░░░ thickets ░░░░░░░░ │
   │ ═══════════════════════════════════════════════════════════ │  ← the road
   │ ═══════════════ 🐴        📜        🐴 ═══════════════════ │     bend
   │ ═══════════════════════════════════════════════════════════ │
   │ ░░░░░░░░░░ thickets ░░░░░░░░░░░░░░░░░░░░ thickets ░░░░░░░░ │
   │ ▓▓▓▓▓▓▓▓▓▓▓▓▓▓  ▓▓  G3 ▓▓   ▓▓▓▓▓▓▓   ▓▓  G4 ▓▓  ▓▓▓▓▓▓▓▓▓ │
   │ ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓ │
   └──────────────────────────────────────────────────────────────┘
        SOUTH  (the side road toward Phandalin, open, warmer light)

   🐴 = the two riderless horses, wandering        📜 = the empty leather map case
   G1–G4 = goblin hiding positions, two on either side of the road ✅
   ▓ = dense forest (Heavily Obscured ✅ = Darkness-equivalent for sight)
   ░ = thickets (Lightly Obscured ✅, Three-Quarters Cover ✅ from most angles)
   ═ = the road (open, no cover, Bright Light in daylight)
```

**Everything in this map is a rule made physical:**
- The **road** is the kill zone: open, no cover, Bright Light. The goblins chose it because it's the only place their arrows have no Disadvantage.
- The **thickets** give the goblins **Three-Quarters Cover (+5 AC ✅)** and are **Lightly Obscured** ✅, so your sight-based Perception checks to spot them have **Disadvantage** ✅.
- The **deep forest** behind them is **Heavily Obscured** ✅ — you cannot see into it, and neither can they, which is why they don't retreat there.
- The **embankment** is a 10-ft rise: elevation, Difficult Terrain to climb ✅, and a **Push** target.
- The **horses** are the trigger. ✅

### The approach — Tempo 0.5
As you come around the bend the camera holds wide and the scene plays out over ~15 seconds *before any prompt appears*:

- Two horses drift across the road, heads down, sniffing at scattered belongings. Manes blow. One shies at nothing.
- The ground is littered with **arrows, torn scraps of fabric, and odds and ends from Gundren's bags** ✅ — each one an interactable eye.
- **Any character who approaches identifies the horses as Gundren's and Sildar's** ✅ — this is automatic, no roll, because the adventure says so. The recognition is delivered as a **flashback Inkwash frame**: half a second of Gundren mounting that exact horse outside Neverwinter's gate. **No text. No dialogue. The player understands.**
- The music drops to a single sustained note. The palette goes cold.

### The investigation window — the scene's secret
**The goblins do not attack until someone approaches the horses.** ✅ That is a *generous* rule and the game honours it exactly: the player has an unbounded window to prepare.

Available before contact, all as contextual eyes:

| Action | Roll | Result |
|---|---|---|
| Search the saddlebags | — | **"The horses' saddlebags have been looted. An empty leather map case lies nearby."** ✅ |
| Study the arrows | Int (Investigation) DC 10 | They're goblin-made, crudely fletched, fired from the north side |
| Search the ground | Wis (Perception) DC 12 | Boot prints. Small. Many. **And a drag mark toward the northwest.** |
| Study the fabric | Int (Investigation) DC 10 | A torn sleeve in Sildar's colours |
| Search the thickets | Wis (Perception) **DC 14, with Disadvantage** (Lightly Obscured ✅) | **Spot a goblin.** Combat begins on *your* terms — and the goblins are Surprised ✅ |
| Hide | Dex (Stealth), **Disadvantage in chain mail** ✅ | Get into position unseen |
| Ready an action | — | Set a trigger: "when a goblin shows itself" |
| Take the map case | — | It goes in your pack. A flag is written. |
| Calm the horses | Wis (Animal Handling) DC 10 ✅ | They stop wandering and can be led — or ridden |

**This window is where the game is won or lost**, and the design intent is that a thoughtful player can turn a 4-on-1 ambush into a 4-on-1 ambush *of their own choosing*. The rules already allow it. The game just has to not get in the way.

## 8.6 BEAT 5 — THE AMBUSH

### Trigger ✅
*"Four goblins are hiding in the woods, two on either side of the road. They wait until someone approaches the horses and then attack."* ✅

**Presentation:** the player crosses an invisible 10-ft line around the horses. **Freeze frame, 6 frames.** Four verdigris rim-lights ignite in the thickets *simultaneously* with a single sharp string stab. The camera snaps to Z2. **Initiative is rolled.**

### Surprise ✅
If the player did not spot them, **the player has Disadvantage on their Initiative roll** ✅. The goblins do not.

If the player *did* spot them (a successful DC 14 Perception with Disadvantage), **the goblins have Disadvantage on theirs** ✅ — and the player gets a free action before the ribbon settles.

**The initiative ribbon is where the player learns what surprise means.** No text. Just portraits racing, and yours stumbling.

### The encounter budget — and the honest math
**Four goblins.** The DMG's encounter framework ✅:

| Party | Budget at Level 1 | 4 goblins cost | Verdict |
|---|---|---|---|
| **1 character** — Story (Low, 50 XP/char ✅) | 50 XP | ~200 XP ⚠️ | **Massively over.** Must be rescaled. |
| **1 character** — Standard (Moderate, 75 ✅) | 75 XP | ~200 XP ⚠️ | **Over.** |
| **1 character** — Veteran (High, 100 ✅) | 100 XP | ~200 XP ⚠️ | **Exactly double. Brutal but fair for a Fighter at AC 18 with 12 HP.** |
| **4 characters** — Veteran (High, 100 ✅) | 400 XP | ~200 XP ⚠️ | Comfortable. |

*(Goblin XP value ⚠️ — the attached Monster Manual is an embed-stub export; confirm the 2025 goblin's XP before balancing. The DMG's own worked examples do confirm **Bugbear Warrior = 200 XP** and **Twig Blight = 25 XP** ✅.)*

**Design decision:** the Tutorial ships with a **party of one hero** (per the campaign card), so the encounter must be **authored to the solo budget, not copied from the book.** Three variants:

| Difficulty | Encounter | Why |
|---|---|---|
| **Story** | **2 goblins**, both `Cowardly` ✅ — one flees at half HP | 100 XP ⚠️. Teaches the loop with no threat of death. |
| **Standard** | **3 goblins**, `Disorderly` ✅ — the last flees when the second falls ✅ | ~150 XP ⚠️. **The default.** |
| **Veteran / Lethal** | **4 goblins**, `Disorderly` ✅, exactly as written ✅ | ~200 XP ⚠️. The real thing. |

**This is the correct way to adapt a published adventure:** keep the *fiction* identical, retune the *numbers* to the party using the DMG's own budget table ✅, and never pretend otherwise.

### The goblins ✅
Small humanoids, **filed to jagged teeth** ✅, scavenged gear, shortbows and scimitars, **Nimble Escape** ⚠️ (verify the 2025 stat block — the 2024 rules made Disengage/Hide a Bonus Action for goblins), **Darkvision** ⚠️.

**Their tactics, from their `Disorderly` personality ✅:**
- Open with a **volley from Three-Quarters Cover** in the thickets (+5 AC ✅, and they're Lightly Obscured so your Perception to relocate them suffers ✅).
- Two close to melee, two hang back and shoot.
- They **focus-fire whoever is lowest**, because they're bullies.
- They **do not coordinate**. When one goes down, the others flinch — a full-body recoil animation — and their behaviour tree rolls a morale check.
- **When one goblin remains alive, that goblin flees toward the goblin trail.** ✅ This is authored, not emergent — it's the adventure's plot.

### The fight's teaching beats
Every fight teaches. This one teaches five things, in this order:

1. **Cover matters.** The goblins are at +5 AC in the thickets. The player will swing and miss and *see the +5*. Solution: move them out, or move around. **The game never explains this. It just shows the number.**
2. **Opportunity Attacks are real.** The first goblin that closes will try to Disengage. If it doesn't, and it moves away, the reaction fires. **The first time it happens the camera punches and the Roll Ledger highlights the entry in gold.**
3. **Weapon Masteries fire.** Sap gives the goblin Disadvantage on its next attack — visible as a verdigris haze on its weapon arm and a `DIS` pip over its next die. Vex gives *you* Advantage — visible as a gold twin die. Push shoves one **off the embankment** for falling damage.
4. **You can knock them out.** ✅ The third goblin down triggers a diegetic nudge — not a tutorial prompt, but **Gundren's voice in your head, or your own sergeant's memory from the practice yard**: *"Take one alive."* The knockout input glows once.
5. **You can lose, and it's fine.** See 8.11.

### Audio and motion during the fight
- **Music:** three adaptive layers — *Pulse* (percussion), *Threat* (low strings), *Clash* (brass and war drum) — cross-fading on threat level, all in the same key so they can enter and leave seamlessly.
- **Goblin vocalisation:** a bank of ~40 short barks, cackles, shrieks and panicked shouts, **never repeating within 90 seconds**, triggered by state not by script. Goblins sound like a mob, not a soundboard.
- **Impact:** every hit has a 3-frame impact freeze, a directional particle burst matched to the damage type ✅, a blood/ichor decal that persists on the ground for the rest of the scene, and a hitstop proportional to damage.

## 8.7 BEAT 6 — THE ROUT

*"The goblins fight to the death until one goblin remains alive; that goblin then flees and heads for the goblin trail."* ✅

**This is the most important single line in the chapter**, because it is the *plot hook physically running away from the player.*

### The player's options, all legal, all rules-supported

| Option | Rule used | Outcome |
|---|---|---|
| **Let it go** | — | You lose the interrogation. You can still track (Beat 9). The game does not punish this; it just gives you less. |
| **Opportunity Attack as it leaves your reach** | Reaction ✅ | Free melee attack, right before it leaves your reach ✅ |
| **Throw a handaxe** | Thrown (20/60) ✅, Attack action | Range band arcs are already on the ground; the player knows if they can reach |
| **Dash after it** | Dash ✅ — extra movement equal to Speed | A short, real-time chase across 6 tiles. **The only "chase" in the Tutorial and it costs one Action.** |
| **Ready an attack** | Ready ✅ | If you saw it coming |
| **Knock it out instead of killing it** | **Knocking Out a Creature** ✅ — melee only | **The best outcome in the chapter.** This goblin becomes the interrogation of Beat 8. |
| **Shove it off the embankment** | **Push** mastery ✅ — 10 ft straight away if Large or smaller ✅ | Falling damage + Prone ✅ + it can't reach the trail |
| **Trip it** | **Topple** mastery ✅ — Con save, DC 8 + Str mod + prof = **DC 13** for our build | Prone ✅; it loses half its movement standing ⚠️ |
| **Grapple it** | Grappled condition ✅ | Speed 0 ⚠️ |

**The game makes none of these obvious and all of them discoverable.** The Fate's Eye preview (hold `Tab`) shows the *chase outcome distribution* — how likely you are to catch it — which is the game teaching the player that **D&D is a game of probabilities you can read.**

### If you catch it alive
The goblin goes limp, Unconscious ✅, and starts a Short Rest ✅. It wakes "after a few minutes" ✅ and can be questioned. **A small ink entry writes itself into the Journal: *One goblin, alive. Bound to the wagon wheel.***

### If you killed all four
No interrogation. **But the chapter does not stall.** See Beat 9 — the trail is findable without them. The DMG's *Multiple Ways to Progress* ✅ principle, honoured.

## 8.8 BEAT 7 — THE AFTERMATH

Combat ends. Music layers fade over 3 s. The initiative ribbon rolls up. Tempo drops to 0.5. **No victory screen.**

The scene is now a **crime scene**, and it is the player's to read:

| Eye | Roll | Result |
|---|---|---|
| Search the goblin bodies | Wis (Perception) DC 10 | Looted coin, a crude charm, **one goblin carries a folded note in Goblin** (⚠️ the party may not read Goblin — Languages matter ✅) |
| Study the charm | Int (Arcana or Religion) DC 12 | A Cragmaw band token. First Bestiary entry: **Cragmaw Goblins** |
| Search the road | Wis (Survival) DC 10 | The ambush has been staged here **for some time** ✅ |
| Examine the map case | — | Empty. Leather, good quality, dwarf-made. **Gundren's.** |
| Calm / claim the horses | Wis (Animal Handling) DC 10 ✅ | *"It's up to the players to decide whether to bring the horses with them."* ✅ |
| Right the wagon / secure the oxen | — | *"The drivers can easily steer the wagon off the road and tie off the oxen."* ✅ |
| Tend your wounds | Short Rest, spend Hit Dice ⚠️ | Costs 1 hour on the world clock |
| Administer first aid to a downed ally | Help action, DC 10 Wis (Medicine) ✅ | Stabilises ✅ |

**XP is awarded silently** into the Journal. The Codex gains six or seven new entries, each with a gold ✦ and a chime. **This is the game's real reward moment** — not a loot popup, but a *book getting thicker.*

## 8.9 BEAT 8 — THE INTERROGATION (the heart of the chapter)

**This is the most important scene in the Tutorial**, because it is where the player learns that this game is not a combat game with talking in it.

### The setup
A goblin, bound to the wagon wheel. One lantern. Camera at Z1. The battlefield still visible behind, blurred, bodies still on the ground. **Tempo 0.5 — the world clock is running.**

Its attitude starts **Hostile** ✅ (or rolled on the DMG's Initial Attitude table ✅ if you prefer randomness). Its portrait shows four hidden layers: **Fears / Wants / Knows / Lies.**

### The four information packets ✅
The adventure specifies exactly what a captured Cragmaw can be persuaded to reveal ✅:

| Packet | Content ✅ |
|---|---|
| **Bugbear Leader** | Their leader is a **bugbear named Klarg**, who reports to **King Grol** at **Cragmaw Castle**, about **twenty miles northeast in Neverwinter Wood**. The goblins can give basic directions. |
| **Capturing Gundren** | A messenger goblin came from King Grol a few days ago: someone called **"the Spider"** is paying the Cragmaws to capture **Gundren Rockseeker** and send him **and everything he was carrying** to King Grol. Klarg obeyed. Gundren was taken **with his personal effects — including a map.** |
| **Sildar's Location** | Gundren's human companion is held in the **"eating cave"** (area H6). **About fifteen goblins** live in this hideout. |
| **Strange Goblins** | Recently, odd goblins have sometimes joined Cragmaw ambushes — **though not today.** They have **elongated skulls**, and **glowing green energy** surrounds their weapons. They cackle and leave. **The Cragmaws are afraid of them** and think you should be too. |

**That fourth packet is the seed of the entire campaign** — the psionic goblins of the Shattered Obelisk — and it is delivered by a terrified creature in a ditch. That is how you plant a mystery.

### The conversation as a tactical encounter

```
   RESISTANCE  ██████████████░░░░░░░░  14 / 20
   ATTITUDE    HOSTILE ◄──────○────────────── FRIENDLY
```

Each Influence check "damages" Resistance. At 0, the goblin talks — and **which packets you get depends on how you broke it:**

| Method | Packets revealed | Cost |
|---|---|---|
| **Persuade** (offer mercy, water, its life) | All four, and it *volunteers* the fourth | Slowest. Attitude ends **Indifferent** or better. |
| **Intimidate** (knife, threats) | All four, fast | **Grudge flag.** The goblin is Hostile forever. **If released, it warns the hideout.** |
| **Deceive** ("I'm with the Spider") | Three, plus a *false* fourth that is subtly wrong | Plants a **false belief** that can detonate later |
| **Search (Insight)** | Reveals **Lies** — you learn which packet it's hiding | Doesn't reduce Resistance, but guarantees you get the fourth |
| **Strength (Athletics)** — pick it up by its ankles | Two, immediately | Comedic, effective, writes a **Cruel** flag |

**Every method works.** None is "correct." The **consequences differ** — and the consequences are the game.

### The goblin's own agency
It is a character, not a vending machine. It:
- **Lies** about the number of goblins in the hideout (says eight; the truth is about fifteen ✅). Catching the lie requires **Wis (Insight)** ✅.
- **Bargains** — it will offer to **lead you to the hideout and guide you around the traps** ✅ *if* you let it go. This is authored in the adventure and it is a real choice with real consequences downstream.
- **Panics** if you draw a weapon, and its Resistance goes *up*.
- **Can be killed mid-conversation**, which ends the scene and writes a permanent flag.

### The failure state
Three failed Influence checks and the goblin clams up entirely — Attitude locked **Hostile**, Resistance immovable. **The scene ends.** But the trail is still there (Beat 9), and the Journal notes what you *didn't* learn. **The adventure moves.** ✅

## 8.10 BEAT 9 — THE TRAIL

*"On the north side of the road, characters can easily find a trail behind the thickets that leads northwest. A character who succeeds on a DC 10 Wisdom (Survival) check recognizes that about a dozen goblins have come and gone along the trail; the character also sees signs of two human-sized bodies being hauled from the ambush site."* ✅

**This is the last beat, and it is deliberately small.** No fanfare. The player walks to the north side of the road, finds the gap in the thickets, and rolls.

### Success (DC 10 ✅ — our Soldier has Survival untrained, Wis +0, so a straight d20 ≥ 10)
The camera drifts to Z3 and the world becomes **The Map**. An ink trail draws itself northwest across the parchment. **Two drag marks** appear in oxblood, drawn slowly, left to right.

The Journal writes:
> *A dozen goblins have used this path. Two bodies were dragged northwest. Gundren. Sildar.*

The quest updates. The compass needle swings. **The chapter's contract with the player is complete: they know what happened, they know who did it, and they know where they went.**

### Failure
The trail is found anyway — it's "easily found" ✅ — but the *reading* fails. You know a path exists; you don't know about the bodies. **You will find out at the hideout instead.** Information is never gated behind a single roll; only its *timing* is. ✅

### And then — the optional coda: THE SNARE
Ten minutes down the trail, the lead character meets a **hidden snare** ✅:
- **DC 15 Wisdom (Perception)** to notice it if searching ✅
- If unnoticed, it triggers: **DC 10 Dexterity saving throw** or the character is **flipped upside down and suspended 10 ft up**, with the **Restrained** condition ✅ until 1 point of slashing damage is dealt to the cord ✅
- Not lowered carefully = **3 (1d6) bludgeoning** from the fall ✅

**Why this is the perfect final beat:** it is *tiny*, it is *funny*, it is *physically spectacular* (the whole screen rotates 180° and the character hangs there, spinning slowly, while the UI stays upright — the single best joke in the game), and it teaches **three rules at once**: Perception DCs, Dexterity saves, and the Restrained condition. And it retroactively justifies the marching-order choice from Beat 3: *you put yourself in front.*

**The screen fades on the player hanging upside down, looking northwest up the trail, with the compass needle swinging. TITLE CARD: CHAPTER II — TROUBLE IN PHANDALIN.**

## 8.11 THE FAILURE STATE — losing the ambush ✅

The adventure provides it, and the game must too:

> *"In the unlikely event that the goblins triumph, they leave the characters unconscious, loot them and the wagon, then head to the Cragmaw hideout. The characters can continue to Phandalin, buy new gear at Barthen's Provisions, return to the ambush site, and find the goblins' trail."* ✅

**Implementation — and this is a design manifesto in miniature:**
- You wake on the road. No death screen. No game over.
- Your gear is gone. Your coin is gone. The wagon is stripped.
- The horses are gone — or one remains, because goblins are greedy and stupid.
- The world clock has advanced four hours. It is getting dark.
- The road to Phandalin is still there. **So is the trail.**
- A single Journal entry: *"They left you alive. That was a mistake."*

**The game has no Game Over screen anywhere in the Tutorial.** Death exists (Death Saves are real ✅), but the *default* failure is the one the adventure wrote: you lose, you get up, and the story is now *better* because you have something to prove.

## 8.12 FLAGS THE TUTORIAL WRITES (and Chapter 2 will read)

```
goblins_killed                0–4
goblins_captured              0–2
goblin_released               bool
goblin_warned_hideout         bool        ← consequence of Intimidate + release
knows.klarg                   bool
knows.king_grol               bool
knows.cragmaw_castle_bearing  bool
knows.spider                  bool
knows.gundren_taken_with_map  bool
knows.sildar_eating_cave      bool
knows.hideout_population      "8 (lied)" | "~15 (true)"
knows.strange_goblins         bool        ← the campaign's central mystery
horses_claimed                bool
wagon_stripped                bool
map_case_recovered            bool
pc_chose_chain_mail           bool        ← referenced by an NPC in Ch. 2
pc_alignment_sentence         string
pc_bond_answer                enum
pc_knows_gundren_via          enum
pc_cruel_flag                 bool
pc_merciful_flag              bool
snare_triggered               bool
```

**Twenty-four flags from one scene.** That is what Pillar 5 looks like in practice.

---
---

# PART IX — MOTION: THE COMPLETE ANIMATION PLAN

The brief says *fully animated with everything*. This part makes that a schedulable number instead of a vibe.

## 9.1 The animation philosophy: three laws

**Law 1 — Anticipation, Impact, Recovery.** Every action has three parts and the *anticipation* is the one that carries the game. A goblin's ear twitches 8 frames before it swings. A trap's rope creaks 12 frames before it snaps. **The player can learn to read the world.** This is what turns real-time combat from twitch into tactics — and it is the motion-layer expression of Pillar 4.

**Law 2 — Squash, Stretch, Smear.** Nothing moves rigidly. Every impact squashes the target 12% along the impact axis for 3 frames. Every fast swing leaves a 2-frame **smear** — a hand-painted arc, not a trail renderer. **Hand-painted smears are the single biggest visual-quality-per-dollar item in the whole plan.**

**Law 3 — Weight is a number.** Every character has a `mass` and every animation set is timed against it.
- **Goblin:** anticipation 4 f, contact 2 f, recovery 6 f. Twitchy, over-committed, off-balance.
- **Human fighter:** anticipation 8 f, contact 3 f, recovery 10 f. Grounded, deliberate.
- **Bugbear** (Ch. 1 boss, later): anticipation 14 f, contact 4 f, recovery 18 f. You can *see* the opening.

**At 24 fps base.** Secondary cycles (idle breathing, cloth, foliage) run at 12 fps to save budget without looking wrong.

## 9.2 The rig

**Skeletal 2D, hand-rolled, JSON-defined and code-animated** — the format specified in §3.6.1, rendered through PixiJS with a weighted-mesh vertex shader. No external editor at any point (see §3.6.1 for why that constraint is a feature). Per humanoid character:

| Component | Count | Notes |
|---|---|---|
| Body bones | 24 | Spine, pelvis, 2× (upper arm, forearm, hand, thigh, shin, foot), head, jaw |
| Facial bones | 6 | Brow L/R, eyelid L/R, mouth, jaw |
| Blend shapes | 12 | Blink, squint, brow raise, brow furrow, mouth open, snarl, smile, wince, shout, grit, fear, dead |
| Deform meshes | 8 | Torso, cape, tabard, 2× sleeve, 2× trouser, hair mass |
| Dynamic chains | 3 | Hair, cloak, belt pouch — driven by a spring solver, not keyframes |
| IK | 2 | Feet (ground-conforming), hands (weapon grip) |

**One rig, one skeleton, retargeted across all humanoid characters.** The goblin rig is a *scaled and re-proportioned variant* of the human rig — same bone names, different lengths and centres of mass. **This is what makes "fully animated" affordable: you animate once and retarget.**

## 9.3 The animation state list — per playable character

### Locomotion (14)
`idle_A`, `idle_B`, `idle_C` (weighted random, 3 variants minimum — Tier A standard ✅), `idle_combat`, `walk`, `walk_combat`, `run`, `sprint_dash`, `strafe_L`, `strafe_R`, `backpedal`, `turn_180`, `crouch_idle`, `crouch_walk`

### Combat — per weapon family (×5 families: sword, axe, hammer, polearm, bow)
`attack_light_1`, `attack_light_2`, `attack_light_3` (combo chain), `attack_heavy` (two-handed), `attack_charged`, `attack_air`, `riposte` (Opportunity Attack — a distinct, sharper, faster read), `block`, `parry_success`, `parry_fail`, `dodge_back`, `dodge_side`
**= 12 states × 5 families = 60**

### Weapon Mastery reactions (8) ✅
`mastery_cleave` (the carry-through arc), `mastery_graze` (the skip-off), `mastery_nick` (the folded off-hand flick), `mastery_push` (the shoulder drive), `mastery_sap` (the pommel crack), `mastery_slow` (the hamstring cut), `mastery_topple` (the haft sweep), `mastery_vex` (the opening feint)

### Reaction & defence (7)
`hit_light`, `hit_heavy`, `hit_critical` (full-body stagger, unique), `knockdown`, `get_up`, `death`, `knockout_limp` (the capture state ✅)

### Nonlethal set (4) — **the game's signature**
`pommel_strike` (the held-LMB variant of every light attack), `pommel_impact`, `target_collapse_limp`, `bind_prisoner`

### Social (8) — **Influence stances are animations** ✅
`persuade` (open palms, weight forward), `intimidate` (chin down, weapon low, weight over them), `deceive` (angled away, one hand hidden), `perform` (arms wide, facing the crowd), `listen`, `insight_narrow` (the Search-of-conversation), `help_gesture` (the pointing Help action ✅), `threaten_draw` (drawing steel mid-conversation — raises the target's Resistance)

### Interaction & exploration (12)
`search_ground`, `search_object`, `study_lean`, `utilize_generic`, `open_container`, `climb`, `jump`, `fall`, `land`, `swim`, `prone_idle`, `prone_crawl`

### Conditions (15) ✅ — one animation state per condition
`cond_blinded`, `cond_charmed`, `cond_deafened`, `cond_exhausted_1/2/3`, `cond_frightened`, `cond_grappled`, `cond_incapacitated`, `cond_invisible` (shader only), `cond_paralyzed`, `cond_petrified` (shader + freeze), `cond_poisoned`, `cond_prone`, `cond_restrained`, `cond_stunned`, `cond_unconscious`

### Emotes & life (10)
`sit`, `sleep`, `eat`, `drink`, `sharpen_blade`, `check_armor`, `stretch`, `laugh`, `mourn` (used at the dead horses), `look_at_camera`

**Subtotal: ~150 states per playable character.** At 24 fps with an average of 18 frames per state and skeletal deformation rather than frame-by-frame, this is **~14 working days of animation per character** for a good 2D animator — which is the number that makes the whole brief achievable.

## 9.4 The goblin state list

Same skeleton, retargeted, plus goblin-specific states:
`skitter_idle` (never still — a constant twitch), `cackle`, `file_teeth` (an idle that reinforces the Cragmaw detail ✅), `volley_loose`, `scramble_cover`, `flinch_ally_down` (the `Disorderly` personality made visible ✅), `break_morale` (the moment the last one turns to run ✅), `flee_panic`, `surrender_hands_up`, `bound_struggle`, `bound_talk`, `hung_upside_down` (the snare ✅), `wake_groggy`

**Plus the psionic variant** — the "strange goblins" with **elongated skulls and glowing green weapons** ✅. Same rig, a scaled skull deform bone, and a **verdigris→toxic-green energy shader on the weapon**. They do not appear in the Tutorial's fight ✅ but they appear in the *interrogation flashback* when the goblin describes them — a 6-second Inkwash frame that plants the campaign's central image.

## 9.5 Environment and prop animation (Tier B & C)

| Asset | Treatment |
|---|---|
| **The two horses** ✅ | Full anatomy rig. 3 collapse poses. Wind-driven mane and tail on a spring solver. Breathing. Ear flick. The most-painted assets in the Tutorial — they carry the emotional opening. |
| **The oxen** ✅ | Heavy idle, slow chew cycle, head-toss, harness creak. Stop dead when the reins drop ✅. |
| **The wagon** ✅ | Wheel rotation synced to ground speed, suspension bounce on ruts, cargo sway, canvas flap on secondary dynamics. |
| **Fire / torch / lantern** | 3-frame hand-drawn flicker loop, per-instance phase offset, plus a real light-buffer contribution. Ember particle emitter. |
| **Foliage** | Global wind vector, 3-frame sway cycle, per-instance offset. **Nothing in a scene may pulse in sync.** |
| **Water / stream** | UV drift + animated caustics + hand-authored ripple sprites on interaction. |
| **Weather** | Rain: 3 parallax sheets + ground splash emitters + a wetness shader that darkens the ground layer over 20 s. Mist: animated noise, drifting. |
| **Birds / insects** | Ambient flocking system, 6 species, triggered by time of day. They scatter when combat starts — **an unscripted tell that costs nothing.** |
| **Blood / ichor decals** | 12 hand-painted splat variants, persistent for the scene, colour-matched to faction (oxblood for humans, toxic green-black for goblins). |

## 9.6 UI animation

**No UI element may ever appear instantly.** ✅ (Pillar 6, Tier D.)

| Element | Animation |
|---|---|
| Panel open | Physically unfurls from a rolled state, 240 ms, with paper-creak audio |
| Quest entry | Ink writes itself left-to-right at ~40 chars/sec, with a nib-shaped cursor and a wet-ink sheen that dries over 1.5 s |
| Level-up | A wax seal **stamps** onto the page: 4 frames of impact, 2-frame screen shake, gold-leaf ignite |
| Damage number | Rises 24 px, scales 1.4→1.0, fades over 900 ms. Crits are gold, 2× size, with a 2-frame punch |
| Initiative ribbon | Portraits **race** into position on combat start; your slot **stamps** with a seal when it fires |
| The die | Full 3D rigid-body tumble with a scripted readable settle. 7 states (see 2.6) |
| Codex entry unlocked | The page turns itself, the entry writes in ink, a gold ✦ embosses, a soft chime |
| Tab change | Real page curl with a paper edge highlight |
| Hover | A 2-frame lift and a 4% scale, 80 ms |

## 9.7 VFX list

**Impact:** 13 damage-type-specific bursts ✅ (acid, bludgeoning, cold, fire, force, lightning, necrotic, piercing, poison, psychic, radiant, slashing, thunder), each hand-painted, 3-frame core + 12-frame decay.
**Mastery:** 8 unique VFX, one per mastery property ✅.
**Roll:** die material shader, Advantage gold rim, Disadvantage rust rim, dust crumble, nat-20 gold-leaf ignite + shockwave ring, nat-1 crack propagation, Heroic Inspiration gold dive.
**Condition:** 15 unique condition auras ✅.
**Environment:** torch flicker, campfire embers, god rays (volumetric, 4 layers), dust motes, fireflies, rain sheets, ground splash, mist, wind streaks.
**Render-mode transitions:** The Sight desaturation wash, Inkwash collapse, Darkvision cold pass, The Map ink-draw-on.

## 9.8 Camera motion

| Event | Camera |
|---|---|
| Roll called | Slow to 40% over 8 frames, vignette in, die rises |
| Nat 20 | Punch in 4%, 90 ms hitstop, 3-frame shake |
| Nat 1 | Drop 2°, tilt 1.5°, no shake — **stillness is the point** |
| Critical hit on you | Whip toward the attacker, 6 frames |
| Ally downed | Drop to Z1, tilt 4°, duck audio to a heartbeat |
| Combat starts | Snap to Z2 in 6 frames with a 2-frame overshoot |
| Combat ends | Relax to Z2 over 3 s, no snap |
| Conversation | Drift to Z1 over 1.5 s **while movement continues** |
| Third death-save skull | Cut to Inkwash, hold 2 s |

## 9.9 The production pipeline

```
CONCEPT (painted key frame — image model or illustrator)
   ↓
MODEL SHEET (turnaround + expression sheet + silhouette test at 20%)
   ↓
PAINT (flat colour → shadow pass → rim-light pass → line pass)   ← Emberlight's 4-pass order
   ↓                        exported as named PNG/WebP part sprites + a texture atlas
RIG (JSON: bones, slots, deform meshes, dynamic chains, IK)      ← §3.6.1 format, no GUI
   ↓
ANIMATE (curve sets in code/JSON: anticipation / impact / recovery, timed to mass)
   ↓
LIGHT PASS (per-animation rim light, baked into the sprites so they read on any background)
   ↓
RENDERER (retarget table, blend tree, IK solver, foot-fall audio sync, atlas load)
   ↓
PASS THE THREE LAWS  ← a checklist gate, not a vibe
```

**Every stage above is a text file or an image.** There is no `.psd`, no `.spine`, no `.aep` and no binary project file anywhere in the pipeline — which is precisely what makes it authorable by AI and reviewable in a GitHub diff. A change to frame 11 of `mastery_push` shows up in a pull request as a one-line change to a number.

**The gate:** no animation ships unless a reviewer can watch it muted, at 50% speed, and correctly name the anticipation frame, the impact frame, and the rule it represents.

---
---

# PART X — SOUND

## 10.1 The audio pillars

1. **The world is loud and specific.** Every footfall knows what it landed on. Every cloth layer rustles separately. Armour is a *layer*, not a sound: mail jingles, leather creaks, plate clanks — and the player learns that **mail is noisy**, which is the Stealth Disadvantage rule ✅ made audible.
2. **Music is a weather system, not a soundtrack.** Adaptive layers in one key, entering and leaving on threat level.
3. **The die has a voice.** A single carved-bone d20, recorded from twelve angles on a slate surface. Every roll in the game uses the same physical die. Players will recognise it.

## 10.2 The music system

| Layer | Enters at | Content |
|---|---|---|
| **Ground** | Always | Sustained pad, world tone |
| **Place** | Per location | Melodic identity — the road, the trail, the bend |
| **Pulse** | Tempo 0.5 | Sparse percussion, heartbeat register |
| **Threat** | Enemies aware | Low strings, dissonant seconds |
| **Clash** | Tempo 1.0 | War drum, brass stabs, on the initiative beat |
| **Solemn** | Ally at 0 HP | Everything drops out except a single cello |
| **Ink** | The Map, flashbacks | Solo hammered dulcimer |

**The rule:** layers cross-fade over 2 bars minimum, always on a barline, never mid-phrase. The player should never hear the system change — only feel it.

## 10.3 Voice

- **The player character** speaks only in short barks, grunts and combat calls (~120 lines). **Full voice would break the "you are this person" frame.**
- **NPCs** are fully voiced. Gundren is warm, fast, and says "aye" too much. The goblins are a chorus of 4 actors doing 40 short barks each, pitch-shifted ±15%.
- **The Codex** is narrated by a single calm voice, in the same room tone as the UI. It is the only voice that ever explains a rule, and it only does so in Guided mode.

---
---

# PART XI — UI, HUD & ACCESSIBILITY

## 11.1 The HUD rule

**The HUD is invisible until the die is in the air.** Four persistent elements, nothing else:

1. **The Ring** (bottom-left) — HP, Heroic Inspiration pip, active conditions as small inked icons. Grows only when relevant.
2. **The Ribbon** (top) — initiative order. Collapsed to nothing outside combat; unfurls on combat start.
3. **The Eye** (in-world) — contextual interaction prompts, floating over objects.
4. **The Ledger** (right edge) — the roll log. Collapsible to a thin amber thread.

Everything else is summoned: `I` character sheet, `J` journal + Codex, `Tab` Fate's Eye, `G` grid, `M` map.

## 11.2 The Rules Inspector — the game's best teaching tool

Hover **any** number anywhere in the game and a parchment slip unfurls showing its full derivation in plain language:

```
   ARMOR CLASS  18
   ─────────────────────────────────────────
   Chain Mail                        16
   Shield                            +2
   Three-Quarters Cover              +5   ← only vs. attacks from the north
   ─────────────────────────────────────────
   TOTAL vs. the goblin archer       23
   ─────────────────────────────────────────
   ◆ Armor Class     ◆ Cover
```

Click either ◆ and the Codex opens at that rule. **Every number in the game is one click from its own explanation.** This is Pillar 1's infrastructure.

## 11.3 Settings that matter

| Category | Options |
|---|---|
| **Rules** | Difficulty dial, Rule Density (Guided / Standard / Purist), Fate's Eye, Roll Rewind, Lethal Damage, Ironman, Grid Snap, Auto-pause triggers (7 individual toggles), Game Speed 0.5×–2× |
| **Presentation** | Palette (Emberlight / High-Contrast / 3 colourblind modes / Monochrome), Screen Shake, Hitstop, Blood, Flash Intensity, Camera Motion, Photo Mode |
| **Accessibility** | Full remap (keyboard / mouse / gamepad), Subtitles + speaker labels + size, **OpenDyslexic font**, Screen-reader support for the Roll Ledger and all rules text, Reduced Motion mode (kills shake, hitstop and smear; keeps readability), Colour-blind-safe condition icons (each condition also has a **distinct shape**, never colour alone) |
| **Audio** | Per-layer mix, Voice, SFX, Music, **Dice** (its own slider — some players will want it louder, and that's the point) |

**Reduced Motion deserves specific design attention:** this is an *animation-maximalist* game, so it must degrade gracefully. Every piece of information carried by motion must also be carried by a static icon, a colour, or text. **That constraint should be applied during design, not retrofitted.**

---
---

# PART XII — BUILDING THE NEXT CAMPAIGN

The whole point of a campaign selector is that campaigns are **content, not code.**

## 12.1 The split

| Layer | Ships as | Changes per campaign? |
|---|---|---|
| **Rules engine** | `@d20/rules` — an npm package of pure TypeScript | **Never.** One engine for all of D&D 2024. |
| **Rules data** | JSON (classes, species, backgrounds, feats, weapons, armor, spells, conditions, monsters) | Rarely — only with new sourcebooks |
| **Campaign pack** | JSON + painted sprites + audio | **Yes. This is the product.** |
| **Presentation** | The `apps/web` Vite bundle | Never |

A campaign pack is a directory of **plain files — no build step, no binary format**:

```
packages/content/campaigns/pabtso-ch1/
  manifest.json            ← name, level range, party size, difficulty mapping, locks
  scenes/
    00-muster.json         ← character creation stations
    01-high-road.json
    02-triboar-trail.json
    03-the-bend.json       ← the ambush map, spawns, triggers, eyes, DCs
    04-interrogation.json
    05-the-trail.json
  npcs/*.json              ← Gundren, Sildar, the goblins: attitudes, layers, dialogue graphs
  encounters/*.json        ← budget targets per difficulty, personalities, morale curves
  dialogue/*.json          ← stance trees, Resistance DCs, leverage, consequence flags
  flags.json               ← the ledger schema for this campaign
  rigs/*.json              ← any character rig unique to this chapter
  animations/*.ts          ← curve sets unique to this chapter
  art/                     ← painted layer PNGs/WebP + texture atlases (JSON manifests)
  audio/                   ← .ogg / .opus stems, one file per adaptive music layer
```

**Nothing in `campaigns/` is code that has to be compiled**, so a chapter is a **deploy, not a patch**: push the JSON and assets, Vercel rebuilds in about a minute, and the new chapter is live at the same URL. No download, no store review, no version mismatch — and because packs are lazy-loaded (§3.6.3), adding Chapter 2 does not make Chapter 1 slower to load.

**Schema versioning:** every pack carries a `schemaVersion`, validated by Zod at load. An old save against a new pack produces a readable migration, not a crash — which matters enormously when the whole codebase is being rewritten continuously by AI.

## 12.2 What unlocks as the campaign series progresses

| Chapter | New system it forces you to build |
|---|---|
| **Ch. 1 — The Tutorial** | Core loop: the die, the action economy, cover, light, social, traps |
| **Ch. 2 — Trouble in Phandalin** | **Town hub, faction Renown ✅, shops and economy ✅, recruitable companions, multi-character party control** |
| **Ch. 3 — The Spider's Web** | **Overland region map, random encounters ✅, weather ✅, a proper dungeon, Agatha (undead + social), Old Owl Well** |
| **Ch. 4 — Wave Echo Cave** | **Wandering monsters ✅, resource attrition, Darkvision as the primary light system, the Forge of Spells** |
| **Ch. 5–6** | **The psionic layer, the Obelisk, character transformation ✅, the campaign's real ending** |

**Every system in the game is justified by a specific chapter.** Nothing is built speculatively.

---
---

# PART XIII — ROADMAP, RISKS & LEGAL

## 13.1 Seven milestones — built by AI, in this order

The order matters more than the durations. Each milestone ends **green**: `pnpm test` passing, `pnpm build` clean, and one named code path demonstrably executed. Nothing starts until the thing before it is verified.

| # | Milestone | Deliverable | What proves it works |
|---|---|---|---|
| **M0** | **The rig and animation schemas** | The JSON rig format and the animation curve format (§3.6.1), plus a debug viewer that draws one skeleton doing one attack | These two schemas decide everything in Part IX. **Get them right before anything else.** |
| **M1** | **The Rules Engine** | `@d20/rules`: d20 pipeline, actions, initiative, conditions, cover, light field, combat resolution, world-state ledger. Pure TS, zero deps. **Thousands of unit tests against the SRD.** No graphics. | `vitest` green in Node. The engine can simulate the whole ambush headlessly in under 50 ms. |
| **M2** | **The Greybox Ambush** | The bend, fully playable in the browser, with **coloured rectangles for art.** Every verb in §4.2 works. Camera zoom, initiative ribbon, the die, cover, Opportunity Attacks, the knockout input. | **This is where the game is proven or killed.** If rectangles aren't fun, Emberlight cannot save it. |
| **M3** | **Emberlight Vertical Slice** | The ambush painted, lit, rigged, animated, scored. The six key frames of §2.7 realised in-engine. | **This is the trailer.** |
| **M4** | **Character Creation + The Road** | The full Part V sequence and Beats 2–3. | A new player reaches the bend with no instruction. |
| **M5** | **The Interrogation + The Trail** | Beats 7–10. The social system, the Codex, the flags. | The social system carries a scene with no combat in it. |
| **M6** | **The Tutorial, Shipped** | Accessibility pass (§11.3), performance budget met (§3.6.3), balance, mobile pass, deploy to Vercel. | A URL, and a stranger can finish it. |

**On durations:** these are not engineer-weeks, because there are no engineers. The honest way to size an AI-built project is by **verified increments**, not calendar time — and the rate-limiting factors are (a) how much art and audio has to be generated or commissioned, and (b) how much playtesting the game-feel needs. **M1 and M2 are almost entirely code and can move very fast. M3 is where the project's real cost lives**, because it is the art, and art is the one thing that cannot be typed into existence line by line.

## 13.2 The seven risks that could kill this

| Risk | Mitigation |
|---|---|
| **1. "Real-time D&D" doesn't actually feel good** | Build M2 greybox with zero art, early. **This is the project's one non-negotiable.** |
| **2. AI-authored code drifts into inconsistency** | This is the *new* top engineering risk, and it has a specific cure: **`strict: true`, the zero-dependency rules package, and a test suite that runs on every change.** Drift is a symptom of unverified code, not of AI authorship. Every session ends green or it ends incomplete. |
| **3. The browser can't hold the animation load** | The hard budget in §3.6.3, checked in CI, with a four-tier quality ladder. **Measure at M2, not M6** — a rectangle scene already tells you the draw-call and batch story. |
| **4. Animation scope explodes** | One shared humanoid skeleton retargeted by arithmetic (§9.2), the three-law gate (§9.9), and a hard state-count budget. **Cut states before cutting frames.** |
| **5. Art and audio are the real bottleneck** | The game must be **fully playable and fully fun with placeholder art** by the end of M2. The paint drops in on top without touching logic. Commission or generate the six key frames (§2.7) *first*, because everything derives from them. |
| **6. The player never notices the depth** | The Codex that fills itself, the Rules Inspector, the Roll Ledger and Fate's Eye are all *depth-revealing* systems. If a player can't say what makes this different by minute 20, they have failed. |
| **7. Legal exposure** | Sharper now, not softer — see below. A public GitHub repo and a public URL are the most visible thing you can do. |

## 13.3 Legal — read this carefully, and note that "public repo + public URL" changes the picture

This design is built on **Dungeons & Dragons 2024** and **Phandelver and Below: The Shattered Obelisk**, both **copyrighted by Wizards of the Coast.**

- **Mechanics** (the d20 system, ability scores, AC, the action list, conditions, cover values, DC tables) are broadly considered game *mechanics*, and the **Creative Commons CC-BY-4.0 SRD 5.1** covers the 2014 ruleset's expression. **The 2024 ruleset's licensing status is different and must be checked directly with WotC before shipping.**
- **Product identity is not open.** *Mind Flayer, the Forgotten Realms, Phandalin, Gundren Rockseeker, Sildar Hallwinter, the Cragmaws, the Black Spider, the Shattered Obelisk* — these are **copyrighted creative expression**, not mechanics.
- **The 2024 PHB, 2024 DMG and 2025 Monster Manual text is not in the SRD.** The four source files behind this project are exports of commercial products and **must not be shipped in the repository, quoted verbatim in the game, or used as a data source in the product.** They are research inputs for this design document only.
- **A public GitHub repository and a public Vercel URL are maximal visibility.** This is not a risk to ignore; it is the single strongest reason to build to Path B from the first commit.

**Three viable paths:**

| Path | What it means |
|---|---|
| **A. Official licence** | Approach Wizards of the Coast / Hasbro for a licensed D&D video game. Slow, expensive, and they have their own plans — but it is the only path that lets you ship Phandalin. |
| **B. SRD-only, original world** | Build the identical game on the CC-BY-4.0 SRD 5.1 with an **original setting, original monsters, and an original opening ambush.** You keep essentially all of this design and lose the proper nouns. **This is the realistic path for a public, AI-built, free-to-play browser game.** |
| **C. Fan / non-commercial** | Ship free, no monetisation, clear attribution, takedown-ready. Viable for a portfolio piece — and note that a browser game with no install and no paywall is *exactly* the shape this path assumes. |

**Recommendation:** build to **Path B** with a strict **content-abstraction layer** — every name, place and monster is a data reference in a campaign pack, never a hardcode. That is already how §12.1 is architected, so **the legal path and the technical path are the same decision**: if a licence is ever secured, the campaign pack swaps and the game is Phandelin. Until then it is a game about a soldier, a wagon, and an ambush on a road — which, as this document should have made clear, was always the actual content.

**Treat every proper noun in this document as a placeholder.**

---
---

# APPENDIX A — SOURCE VERIFICATION

An honest account of what was and wasn't readable in the four attached files.

## A.1 What the attached files are

All four are **5e.tools exports in Markdown.** The prose, tables and chapter structure export cleanly. However, 5e.tools renders **stat blocks, class tables, background entries, species entries, feat entries, individual condition definitions, and rules-glossary entries as lazy-loaded embed placeholders** — and those placeholders exported as HTML stubs reading `Loading "Fighter"…`, not as content.

Concretely, in the attached files:
- `Player's Handbook (2024).md` — **Fighter class** (line ~2904), **Soldier background** (line ~3643), **Human species**, **all conditions** including Exhaustion, **the entire Rules Glossary**, and **all feat entries** are stubs.
- `Monster Manual (2025).md` — **all ~1,500 stat blocks** are stubs. There is **no goblin stat block in the attached file.** The CR/XP framework prose *is* present.
- `Phandelver and Below_ The Shattered Obelisk.md` — **complete and fully readable**, including all of Chapter 1.
- `Dungeon Master's Guide (2024).md` — **complete and fully readable** for every section cited.

## A.2 ✅ VERIFIED — read directly from the attached files

| Item | Source |
|---|---|
| Ability Modifier table (1→−5 … 30→+10) | XPHB §Ability Modifiers |
| Proficiency Bonus is +2 at level 1; doesn't stack; can be doubled (Expertise) or halved once | XPHB §Proficiency |
| Level 1 HP: Fighter **10 + Con mod** | XPHB §Level 1 Hit Points by Class |
| Fighter Standard Array **Str 15 / Dex 14 / Con 13 / Int 8 / Wis 10 / Cha 12** | XPHB §Standard Array by Class |
| Fighter primary ability **Strength or Dexterity**, complexity **Low** | XPHB §Class Overview |
| Background ability increases: **+2 and +1 to two listed abilities, or +1 to all three**, max 20 | XPHB §Adjust Ability Scores |
| Soldier is listed under **Strength, Dexterity *and* Constitution** | XPHB §Ability Scores and Backgrounds |
| Character creation is 5 steps: Class → Origin → Ability Scores → Alignment → Details | XPHB §Create Your Character |
| The **12 Actions** table (Attack, Dash, Disengage, Dodge, Help, Hide, Influence, Magic, Ready, Search, Study, Utilize) | XPHB §Actions |
| Bonus Actions exist only when granted; one per turn | XPHB §Bonus Actions |
| Reactions: one per round, refresh at start of your next turn | XPHB §Reactions |
| **Heroic Inspiration** — expend to reroll any die, must use new roll, never hold more than one, **Humans start each day with it** | XPHB §Advantage/Disadvantage sidebar |
| Advantage/Disadvantage **cancel regardless of how many sources** | XPHB |
| Round = ~6 seconds; **Initiative is a Dexterity check**; identical monsters share one roll; order is fixed | XPHB §The Order of Combat |
| **Surprise = Disadvantage on the Initiative roll** | XPHB §Initiative |
| Tie-breaking rules | XPHB §Ties |
| Grid rules: 5 ft/square, Speed ÷ 5 = squares, Difficult Terrain = 2 squares, no diagonal through a filled corner, ranges by shortest route | XPHB §Playing on a Grid |
| Creature Size and Space table | XPHB |
| Breaking up your move; dropping Prone is free | XPHB §Movement and Position |
| **Cover: Half +2, Three-Quarters +5, Total = untargetable; only the best applies; only from the opposite side** | XPHB §Cover |
| Unseen attackers: Disadvantage to hit unseen; Advantage if they can't see you; attacking while hidden reveals you | XPHB §Cover sidebar |
| Ranged: normal/long range, Disadvantage beyond normal, impossible beyond long; **Disadvantage if an enemy is within 5 ft** | XPHB §Ranged Attacks |
| Reach is normally 5 ft; **Opportunity Attack** = reaction, one melee attack, right before the target leaves your reach; Disengage avoids it | XPHB §Melee Attacks |
| One free object interaction per turn; a second costs Utilize | XPHB §Interacting with Things |
| Communication is free if brief; persuading costs an action | XPHB §Communicating |
| Combat ends on defeat, surrender, flight, or mutual agreement | XPHB §Ending Combat |
| Death: monsters die at 0 HP; **Knocking Out a Creature** (melee → 1 HP + Unconscious, ends on a Short Rest or a DC 10 Wis (Medicine) first aid); Massive Damage; **Death Saving Throws** (10+ succeeds, 3/3, 1 = two failures, 20 = 1 HP, damage at 0 = one failure); **Stabilising** via Help + DC 10 Wis (Medicine); Stable regains 1 HP after 1d4 hours | XPHB §Dropping to 0 Hit Points |
| The **15 Conditions** list, and "conditions don't stack, Exhaustion excepted" | XPHB §Conditions |
| **Bright / Dim / Darkness**; Lightly Obscured = Disadvantage on sight-based Wis (Perception); Heavily Obscured = Blinded when trying to see | XPHB §Vision and Light |
| **Travel Pace table** (Fast 400 ft/min, 4 mi/hr, 30 mi/day; Normal 300/3/24; Slow 200/2/18) and its Advantage/Disadvantage effects | XPHB §Travel Pace |
| **Marching order** determines who is hit by traps, who spots enemies, who is nearest | XPHB §Marching Order sidebar |
| Hiding = Hide action = Dex (Stealth) check | XPHB §Hiding |
| Finding hidden objects requires a Wis (Perception) check *and* searching in the right place | XPHB §Finding Hidden Objects |
| **Full weapon table** with damage, properties and mastery for every simple and martial weapon | XPHB §Weapons |
| **All 8 Mastery Properties** verbatim: Cleave, Graze, Nick, Push, Sap, Slow, Topple, Vex | XPHB §Mastery Properties |
| **Full armor table** including Chain Mail AC 16 / Str 13 / Stealth Disadvantage, and Shield +2 with a Utilize action to don or doff | XPHB §Armor |
| Armor training: no training = Disadvantage on Str/Dex D20 Tests and no spells; Shield benefit needs training | XPHB §Armor Training |
| DMG **four adjudication questions** (Is a test warranted? What kind? Which ability? What DC?) | XDMG §Resolving Outcomes |
| DMG **Typical DCs** table (5/10/15/20/25/30) and the DC 10/15/20 shorthand | XDMG |
| DMG **Calculated DC = 8 + ability mod + Proficiency Bonus**; hiding as the worked example | XDMG |
| DMG **Trying Again**, **Group Checks** (half succeed = group succeeds), **Passive Checks** (Passive Perception, and the invitation to use Passive Insight) | XDMG §Ability Checks |
| DMG **Running Social Interaction**: goal-oriented; **Friendly / Indifferent / Hostile**; shifts must be described; **Help in conversation should require real contribution**; **use other ability scores** (Str/Dex/Int/Wis examples) | XDMG |
| DMG **Initial Attitude** table (1d12: 1–4 Hostile, 5–8 Indifferent, 9–12 Friendly) | XDMG §Monster Behavior |
| DMG **Monster Personality** table (1d8: Cowardly, Greedy, Boastful, Disorderly, Fanatical…) | XDMG |
| DMG **XP Budget per Character** table (Level 1: Low 50 / Moderate 75 / High 100) and **Low / Moderate / High** definitions | XDMG §Combat Encounter Difficulty |
| DMG worked examples confirming **Bugbear Warrior = 200 XP**, **Giant Wasp = 100 XP**, **Twig Blight = 25 XP** | XDMG §Step 3 |
| DMG **"Many Creatures"** warning for levels 1–2 | XDMG §Troubleshooting |
| DMG combat-interest features: elevation, defensive positions, hazards, mixed groups, reasons to move | XDMG §Combat Encounters |
| DMG **Keeping the Adventure Moving** (Adviser NPCs, Evil Intrusion, the DM's Role) and **Multiple Ways to Progress** | XDMG §Plan Encounters |
| DMG **Travel Terrain** table (Forest / Hill / Grassland rows used) | XDMG §Travel |
| DMG **Audible Distance** table (quiet 2d6×5, normal 2d6×10, loud 2d6×50) | XDMG §Perception |
| DMG **Weather** table (1d20) | XDMG |
| **Torch** — burns 1 hour, **Bright Light 20 ft radius, Dim Light +20 ft**, usable as a Simple Melee weapon for 1 Fire damage | XPHB §Torch |
| **Candle** — 1 hour, **Bright Light 5 ft, Dim Light +5 ft** | XPHB §Candle |
| **Tinderbox** — lighting a Candle/Lamp/Lantern/Torch is a **Bonus Action**; lighting any other fire takes **1 minute** | XPHB §Tinderbox |
| **Explorer's Pack** contents (Bedroll, 2 flasks of Oil, 10 days Rations, Rope, Tinderbox, **10 Torches**, Waterskin) | XPHB §Adventuring Gear |
| PaBTSO **Chapter 1 in full**: the contract, the wagon and cargo, the ambush text, the goblin positions, "fight to the death until one remains, then it flees", the Development section, **What the Cragmaws Know** (all four packets), the Goblin Trail, **DC 10 Wis (Survival)**, the **Snare** (DC 15 Perception, DC 10 Dex save, Restrained, 1 slashing to cut, 1d6 fall), the **Pit Trap**, level-up on finishing the hideout | PaBTSO Ch. 1 |

## A.3 ⚠️ UNVERIFIED — flag before implementation

These are used in this document from knowledge of the ruleset because the attached exports stubbed them out. **Every one must be confirmed against the printed books before it goes into the rules engine.**

| Item | Why it's unverified |
|---|---|
| **Fighter level-1 features** — Fighting Style, Second Wind, Weapon Mastery, d10 Hit Die, Str/Con save proficiency, all-armor + all-weapon training | XPHB class entry is an embed stub |
| **Soldier background package** — Athletics + Intimidation skills, gaming-set tool, origin feat, equipment, rank insignia | XPHB background entry is an embed stub |
| **Human species traits** — Resourceful (Heroic Inspiration), Skillful, Versatile, 30 ft Speed, Medium | XPHB species entry is an embed stub. *Note: the "Humans start each day with Heroic Inspiration" line **is** verified — it appears in the Advantage/Disadvantage sidebar.* |
| **Individual feat mechanics** — Tough, Savage Attacker, Alert, Skilled, Magic Initiate | XPHB feat entries are embed stubs (the *names* and the Fighting Style category are verified) |
| **Individual condition definitions** — exact effects of Prone, Restrained, Grappled, Unconscious, Exhaustion levels | XPHB glossary is an embed stub (the condition *list* is verified) |
| **Short Rest / Long Rest exact wording** | XPHB glossary stub |
| **Resistance rounding rule** (round down) | XPHB glossary stub |
| **Goblin, Wolf, Bugbear, Horse, Ox, Commoner stat blocks** — AC, HP, Speed, attacks, XP, Nimble Escape, Darkvision | **The attached Monster Manual contains no stat block text at all** — all ~1,500 are embed stubs |
| **Goblin XP value** (assumed 50 throughout §8.6) | Same as above. The DMG examples confirm Bugbear Warrior = 200 ✅ but say nothing about goblins |
| **Darkvision range** (assumed 60 ft) | XPHB glossary stub |
| **Hooded Lantern / Bullseye Lantern / Lamp radii**, and the **campfire** radius | The Torch and Candle entries *are* in the file and are now verified ✅ (§2.3); the lantern entries did not export as plain text. **Verify the lanterns and the campfire before implementing the light system.** |
| **Weapon Masteries gained at level 4** | XPHB class table stub |
| **Fighting Style feat numeric values** (e.g. Defense +1 AC, Dueling +2 damage) | XPHB feat entries stub |

**Action item:** re-export these files from 5e.tools with embeds expanded (or work from the printed books), then re-verify every ⚠️ row before M0 code freeze.

---
---

# APPENDIX B — THE NUMBERS OF THE AMBUSH

The reference build, fully derived. Everything here is what the game will actually compute.

## B.1 The hero — "the Sergeant", level 1 Human Fighter (Soldier)

```
ABILITY SCORES   Standard Array (Fighter) ✅  +  Soldier +2 Str / +1 Con ✅
  STR 15 → 17  (+3)      INT  8  (−1)
  DEX 14     (+2)        WIS 10  (+0)
  CON 13 → 14  (+2)      CHA 12  (+1)

DERIVED
  Proficiency Bonus        +2                        ✅ level 1
  Hit Point Maximum        12        (10 + 2)        ✅
  Armor Class              18        (Chain Mail 16 ✅ + Shield 2 ✅)
  Initiative               +2
  Speed                    30 ft / 6 squares          ✅
  Passive Perception       10        (10 + 0)
  Heroic Inspiration       1 per day                  ✅ Human

SAVING THROWS
  Strength                 +5   (proficient ⚠️)
  Constitution             +4   (proficient ⚠️)
  Dexterity                +2
  Intelligence             −1
  Wisdom                   +0
  Charisma                 +1

SKILLS (proficient)
  Athletics                +5   (3 + 2)              ⚠️ Soldier skill
  Intimidation             +3   (1 + 2)              ⚠️ Soldier skill

WEAPONS
  Longsword  (Sap ✅)      +5 to hit · 1d8+3 slashing · 1d10+3 versatile ✅
  Handaxe    (Vex ✅)      +5 to hit · 1d6+3 slashing · Thrown 20/60 ✅ · Light ✅
```

## B.2 The opening exchange, computed

The hero attacks a goblin in the thickets. **AC 15** ⚠️ **+ 5 Three-Quarters Cover** ✅ = **AC 20**.

```
ATTACK ROLL   1d20 + 3 (Str) + 2 (prof)  vs  AC 20
   roll 14 →  total 19 → MISS by 1
   → the +5 from cover is highlighted in the Rules Inspector
   → the player learns cover exists, by losing

MOVE 6 squares around the embankment to remove the cover line → AC 15

ATTACK ROLL   1d20 + 5  vs  AC 15
   roll 11 →  total 16 → HIT
DAMAGE ROLL   1d8 + 3   →  6 + 3  =  9 slashing
   → Sap ✅ fires: the goblin has Disadvantage on its next attack roll
   → a verdigris haze settles on its weapon arm; a DIS pip appears over its next die

GOBLIN'S TURN
   attack vs hero AC 18, WITH DISADVANTAGE ✅
   rolls 17 and 9 → keeps 9 → 9 + 4 ⚠️ = 13 → MISS
   → the rust-rimmed die crumbles to dust on screen
   → the player learns what Sap bought them, by surviving
```

**That is one exchange. It teaches four rules — attack rolls, cover, mastery properties, and Disadvantage — with no text, in about twenty seconds.**

## B.3 The encounter budget, restated

| Difficulty | DMG budget, 1 char at level 1 ✅ | Encounter | Cost ⚠️ |
|---|:---:|---|:---:|
| Story | 50 XP | 2 goblins, one Cowardly ✅ | ~100 XP |
| **Standard** | **75 XP** | **3 goblins, Disorderly ✅** | **~150 XP** |
| Veteran | 100 XP | 4 goblins as written ✅ | ~200 XP |
| Lethal | 150 XP (High × 1.5) | 4 goblins + 1 wolf | ~225 XP |

*The Standard tier deliberately runs ~2× the DMG budget. That is a considered choice, not an error: the DMG budget assumes a **four-character party** covering each other, and a solo AC-18 Fighter with 12 HP and Second Wind is far more durable than one quarter of a party. **Rebalance after the first twenty playtests** — and re-check the goblin XP value first (Appendix A.3).*

## B.4 The two DCs that matter most

| Check | DC ✅ | Our modifier | Needed on the die |
|---|:---:|:---:|:---:|
| Spot a hidden goblin in the thickets (Wis Perception, **Disadvantage** ✅ from Lightly Obscured ✅) | **14** | +0 | 14+ on the better of 2d20 → ~36% |
| Read the trail (Wis Survival) | **10** ✅ | +0 | 10+ → 55% |
| Notice the snare (Wis Perception) | **15** ✅ | +0 | 15+ → 30% |
| Resist the snare (Dex save) | **10** ✅ | +2 | 8+ → 65% |
| Topple a goblin (Con save vs our DC) | **13** (8+3+2 ✅) | — | goblin must roll under |

**Note the first row.** Passive Perception of 10 against a hidden-goblin DC of 14 means the hero **never passively spots the ambush.** That is intentional. The player must *choose* to Search, and Search costs an action, and the world clock is running. **The single most important lesson in the Tutorial is delivered by a number the player will never see.**

---
---

# APPENDIX C — DATA SCHEMA SKETCHES

Three sketches, to show that the architecture in Part XII is real.

## C.1 A creature

```json
{
  "id": "goblin",
  "name": "Goblin",
  "size": "Small", "type": "humanoid",
  "cr": 0.25, "xp": 50,                          // ⚠️ verify
  "ac": { "base": 15, "from": "leather armor, shield" },
  "hp": { "avg": 7, "dice": "2d6" },
  "speed": { "walk": 30 },
  "abilities": { "str": 8, "dex": 14, "con": 10, "int": 10, "wis": 8, "cha": 8 },
  "senses": { "darkvision": 60 },                 // ⚠️ verify
  "languages": ["Common", "Goblin"],
  "features": [
    { "id": "nimble_escape",
      "text": "Bonus Action: Disengage or Hide",   // ⚠️ verify 2025 wording
      "actionType": "bonus" }
  ],
  "attacks": [
    { "id": "scimitar", "type": "melee", "toHit": 4, "reach": 5,
      "damage": "1d6+2", "damageType": "slashing" },
    { "id": "shortbow", "type": "ranged", "toHit": 4, "range": [80, 320],
      "damage": "1d6+2", "damageType": "piercing" }
  ],
  "ai": {
    "personality": "disorderly",                  // ✅ DMG Monster Personality
    "initialAttitude": "hostile",                 // ✅ DMG
    "morale": { "breaksAt": 0.25, "fleesWhenLast": true },   // ✅ PaBTSO
    "tactics": ["volley_from_cover", "focus_weakest", "flank", "flee_to_trail"]
  },
  "social": {
    "resistance": { "base": 8, "ability": "cha", "proficient": false },  // ✅ formula
    "layers": ["fears", "wants", "knows", "lies"]
  },
  "animation": { "rig": "humanoid_small_goblin", "mass": 0.6 }
}
```

## C.2 A scene trigger — the ambush

```json
{
  "id": "the-bend/ambush",
  "trigger": {
    "type": "enterRadius",
    "target": "prop.horses",
    "radius": 10,                                  // ✅ "wait until someone approaches the horses"
    "once": true
  },
  "onTrigger": [
    { "type": "tempo.set", "value": 1.0 },
    { "type": "camera.snap", "zoom": "Z2", "frames": 6, "overshoot": 2 },
    { "type": "vfx.rimlight", "targets": ["goblin_01","goblin_02","goblin_03","goblin_04"] },
    { "type": "audio.stab", "cue": "ambush_reveal" },
    { "type": "surprise.evaluate",
      "surprisedIf": "not flags.party.spotted_goblins",
      "effect": "disadvantage.initiative" },        // ✅ XPHB
    { "type": "initiative.roll" },
    { "type": "codex.unlock", "entries": ["Initiative", "Surprise"] }
  ],
  "spawns": [
    { "id": "goblin_01", "at": "G1", "hidden": true, "cover": "three-quarters" },
    { "id": "goblin_02", "at": "G2", "hidden": true, "cover": "three-quarters" },
    { "id": "goblin_03", "at": "G3", "hidden": true, "cover": "three-quarters" },
    { "id": "goblin_04", "at": "G4", "hidden": true, "cover": "three-quarters" }
  ],
  "budget": {                                      // ✅ DMG XP Budget per Character
    "story":    { "goblins": 2, "personalities": ["cowardly", "disorderly"] },
    "standard": { "goblins": 3, "personalities": ["disorderly"] },
    "veteran":  { "goblins": 4, "personalities": ["disorderly"] },
    "lethal":   { "goblins": 4, "wolves": 1, "personalities": ["disorderly"] }
  },
  "endConditions": [
    { "when": "allEnemiesDefeated" },
    { "when": "lastEnemyAlive", "then": ["ai.flee", "flag.goblin_fled"] },   // ✅ PaBTSO
    { "when": "partyDefeated",  "then": ["scene.capture_wakeup"] }           // ✅ PaBTSO
  ]
}
```

## C.3 A social layer — the interrogation

```json
{
  "id": "interrogation.bound_goblin",
  "tempo": 0.5,
  "camera": "Z1",
  "target": "goblin_captive",
  "attitude": "hostile",                            // ✅ DMG
  "resistance": { "max": 20, "start": 14 },
  "stances": [
    { "id": "persuade",  "skill": "persuasion",  "damage": "1d6+cha",
      "onFail": { "resistanceDelta": +2, "attitudeDelta": -1 } },
    { "id": "intimidate","skill": "intimidation","damage": "1d10+cha",
      "onSuccess": { "flag": "goblin.grudge" },
      "onFail": { "attitude": "hostile_locked" } },
    { "id": "deceive",   "skill": "deception",   "damage": "1d12+cha",
      "onSuccess": { "plant": "false_belief.spider_ally" } },
    { "id": "strength",  "ability": "str", "skill": "athletics", "damage": "1d8+str",
      "onSuccess": { "flag": "pc.cruel" } }
  ],
  "rewards": [
    { "at": 15, "packet": "bugbear_leader" },        // ✅ PaBTSO
    { "at": 10, "packet": "capturing_gundren" },     // ✅
    { "at":  5, "packet": "sildars_location" },      // ✅
    { "at":  0, "packet": "strange_goblins",
      "requiresAnyOf": ["stance.persuade", "insight.revealed.lies"] }   // ✅
  ],
  "onComplete": { "flags": ["knows.spider", "knows.klarg", "knows.sildar_eating_cave"],
                  "journal": "A bound goblin, the Triboar Trail, afternoon" }
}
```

---

# CLOSING NOTE

**What this game is, in one paragraph.**

*D20: The Roll* is a hand-painted 2D action-RPG in which the Dungeons & Dragons 2024 ruleset is not adapted, simplified or hidden — it is **performed.** Real time runs at one foot per fifth of a second, so a six-second round takes six seconds and D&D is finally played at the speed it was written for. Every rule is a button, every number is one click from its own explanation, and the d20 is the most-animated object in the game. Exploration, conversation and combat are one continuous world at three tempos, with no mode switch anywhere. You begin as a human soldier with a longsword, a shield, twelve hit points and a Passive Perception of 10 — which is exactly why you do not see the goblins until it is too late. And the first thing the game teaches you, in a fight you will probably lose, is that you did not have to kill them.

**The four things to build first:**
1. **The rig and animation schemas** (§3.6.1) — two JSON formats that every animation in the game depends on (M0).
2. **The headless rules engine and its test suite** — pure TypeScript, zero dependencies, verified with `vitest` in Node (M1).
3. **The greybox ambush** — coloured rectangles in a browser, to prove real-time D&D is *fun* (M2).
4. **The six Emberlight key frames** in §2.7, to prove it is *beautiful* (M3).

Everything else follows. And because the whole thing is a URL, the moment M2 works you can send it to someone and find out whether you were right.

---

## Changelog

**v0.2 — platform rewrite.** The game was re-platformed from a native engine to **browser-native, engine-free, AI-authored code deploying to Vercel/Netlify**. Changed: §3.1 (architecture diagram and the `@d20/rules` npm package), **§3.6 fully rewritten** (TypeScript + Vite + PixiJS v8 + Web Audio + Lit + IndexedDB, with reasons and rejections), **§3.6.1 new** (hand-rolled JSON/code skeletal animation, replacing Spine's GUI editor, with the rig and curve formats specified), **§3.6.2 new** (hybrid canvas/DOM UI, revising the earlier "no HTML overlays" position), **§3.6.3 new** (hard browser performance budget and quality ladder), **§3.7 new** (why the web is the right platform — Fate's Eye in a Worker, replays-as-URLs, IndexedDB saves, zero-friction playtesting), **§3.8 new** (the Arena AI build method), §9.2 and §9.9 (rig and pipeline now GUI-free), §12.1 (campaign packs as plain files; a chapter is a deploy, not a patch), §13.1 (seven AI-buildable milestones), §13.2 (two new risks: AI code drift, and the browser performance ceiling), §13.3 (legal, sharpened for a public repo and public URL).
**Nothing in the design, the rules, the art direction, the animation ambition or the campaign content was cut.** The only design position that changed is the UI one in §3.6.2, and it changed in favour of accessibility.

**v0.1 — first draft.** Full vision, Emberlight art direction, core mechanics, rules translation, character creation, and the goblin ambush.

---
*D20: The Roll — Game Design Bible v0.2. Built from the Player's Handbook (2024), Dungeon Master's Guide (2024), Monster Manual (2025) and Phandelver and Below: The Shattered Obelisk, Chapter 1. See Appendix A for what is verified and what is not, and Part XIII for the licensing situation.*
