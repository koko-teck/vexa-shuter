# VEXA — ART DIRECTION BIBLE

> **Status:** AUTHORITATIVE. Every asset, system, UI screen and VFX added to VEXA must be checked against this document.
> **Source of truth:** the eleven images in `imagenes-de-referencia-/`. Those images are **reference material only** — they are never shipped as textures, sprites, skyboxes, backgrounds or models (brief §4). All final assets are built from scratch to reproduce the *look* they describe.

---

## 0. One-Line Identity

**VEXA — Voxel Tactical Unit.** A blocky/voxel FPS where military-grade tactical technology has been buried in desert dust. Hard-edged geometry, warm powdered light, sandstone ruins, and gear the colour of sand.

**Never:** photoreal military shooter, grey sci-fi corridor shooter, neon cyberpunk, generic brown-mud FPS.

---

## 1. Visual Style

### 1.1 The two pillars
| Pillar | Meaning | Reference source |
|---|---|---|
| **VOXEL / BLOCKY** | Everything is built from visible cuboid blocks. Silhouettes are read block by block. Straight edges, right angles, stair-stepped damage. No smooth organic curves, no bevels, no cloth simulation, no soft-body anything. | All character sheets, both FPS screenshots, weapon sheet |
| **DUST TOWN** | Warm desert-tan world. Sandstone, rubble, sandbags, timber crates, dry scrub, powdered airborne dust. Sun-bleached, wind-worn, half-collapsed architecture. | Ruins at sunset, warehouse interiors, Map 03 |

### 1.2 The core tension (VEXA's signature)
The logo reference defines it: **a tactical circuit board printed on sand.** VEXA is *not* post-apocalyptic rust and *not* clean futuristic chrome — it is **precision military technology slowly being reclaimed by the desert.** Circuit traces on a dusty PCB, a soldier whose gear colour matches the ground he stands on.

### 1.3 Voxel construction rules
- **Voxel grid:** `1 block = 0.1 m` (10 cm). Confirmed on the sniper sheet.
- **Flat shading.** Faces take one light value each. No smooth-gradient shading across a block face. Nearest-neighbour/no-mip-blur texture sampling — texels stay crisp and readable.
- **Rigid voxel groups.** Meshes do **not** deform. Limbs and gear are rigid block clusters that rotate at joints (the rig sheet labels this *"Mesh Integrity: Non-deforming, Rigged as Rigid Voxel Groups"*).
- **Stair-stepped damage.** Destruction, chipping and erosion always resolve as blocky steps and missing cubes — never smooth fractures or cloth tears.
- **Visible cube seams.** Slight per-block value variation so the grid reads even on large flat surfaces. Uniform flat walls are forbidden.
- **Gear is blocky too.** Pouches, straps, magazines, boots, ghillie strands and sandbags are all cuboid forms with no rounding.

---

## 2. Colour Palette

### 2.1 Core world palette
| Name | Hex | Use |
|---|---|---|
| **Sand Beige** | `#C2B280` | Primary sand/terrain, weapon body tint, UI paper. *Confirmed on the weapon sheet.* |
| **Dust Town Tan** | `#BFA074` | Faction colour, distant sand haze, dry-grass base. *Confirmed on the sniper sheet.* |
| **Sandstone** | `#C9B596` | Cut building blocks, walls, stairs. |
| **Warm Highlight** | `#E4D1A8` | Sun-facing wall tops, sand ridges, brightest sand. |
| **Clay / Terracotta** | `#B07A50` | Roof timber, doors, rusted metal accents. |
| **Coyote Brown** | `#8A6A46` | Weapon furniture, grips, stocks, crate wood. |
| **Wood Timber** | `#A9784C` | Crates, planks, beams, ladders. |
| **Khaki** | `#9C8E63` | Fabric, uniforms, webbing, sandbags. |
| **Olive Drab** | `#4E5233` | Ammo crates, foliage, ghillie grass, camo blotches. |
| **Deep Shade** | `#6E5C40` | Undersides, interior shadow, crevices. |

### 2.2 Faction accents
| Name | Hex | Use |
|---|---|---|
| **Team Blue** | `#2E6BB8` | Spawn A, friendly markers, friendly UI. *(From Map 03.)* |
| **Team Orange** | `#E07B2C` | Spawn B, hostile markers, hostile UI. *(From Map 03.)* |

### 2.3 Character skin / natural tones
| Name | Hex | Use |
|---|---|---|
| **Skin Base** | `#C98F63` | Blocky hands/face. |
| **Skin Shadow** | `#A8703F` | Block undersides of exposed skin. |
| **Dry Straw** | `#BFA074` | Ghillie strands, dead grass. |
| **Ghillie Dark** | `#8F7448` | Ghillie depth layers. |

### 2.4 UI chrome
| Name | Hex | Use |
|---|---|---|
| **UI Panel** | `#E8DCC0` @ 88% | HUD plates, menus — dusty parchment, not glossy black glass. |
| **UI Panel Edge** | `#6E5C40` | 2–3 px hard border — the *logo outline* colour. |
| **UI Ink** | `#3A2E1E` | Primary text. |
| **UI Ink Muted** | `#7A6A50` | Secondary/labels. |
| **UI Warning** | `#E07B2C` | Low ammo, damage, objective. |
| **UI Critical** | `#B33A2E` | Death, critical health. |
| **UI Positive** | `#4E5233` | Health bar, squad status. |

### 2.5 Palette laws
1. **No pure black and no pure white anywhere.** Darkest is `#3A2E1E`; lightest is `#E4D1A8`.
2. **No saturated blues, cyans, magentas or neons** in the world. Blue and Orange exist *only* as faction/UI signalling.
3. **No cold/cool greys.** Any "metal grey" is a warm desaturated brown-grey.
4. **Sand dominates.** Every scene should read 60–80 % warm tan by pixel area. Vehicles, crates and foliage are accents, not the field.
5. Camouflage is **desert DPM**: irregular blocky blotches of Sandstone / Tan / Khaki / Olive over a Beige base — never digital pixel-grid camo, never woodland.

---

## 3. Scale & Proportions

### 3.1 Units
- `1 block = 0.1 m`. Everything is authored on this grid.
- **1 Unity unit = 1 metre.** (Sub-0.1 m detail is allowed for trim, but the read stays blocky.)

### 3.2 Character heights
| Class | Height | Blocks | Model | Silhouette |
|---|---|---|---|---|
| **Heavy** | 1.90 m | ~19 | VEXA-02 | Widest. Bulky plate carrier, drum magazine, heavy helmet. Reads as a wall. |
| **Infiltration** | 1.85 m | ~18–19 | VEXA-09 | Medium/lean. Comms headset, slung rifle, slim vest. |
| **Sniper** | 1.95 m | ~19–20 | VEXA-07 | Tallest and narrowest. Ghillie mass widens the head/shoulder line; body itself is slim. |

- **Proportion law:** blocky soldier proportions are **slightly stocky** — roughly 5.5–6 heads tall, thicker torso and forearms than realism. Boots are chunky and square. Hands are oversized blocks (5–6 blocks across) so they read in first-person.
- **Head is oversized** relative to realism so the helmet, goggles, headset and ghillie hood silhouette survive at distance.

### 3.3 World scale
| Element | Size |
|---|---|
| Standard door opening | 2.2 m H × 1.2 m W (blocky lintel above) |
| Standard ceiling / floor height | 3.0 m |
| Window opening | 1.0 m W × 1.2 m H, square-cut, often smashed out to rubble |
| Interior crate | 0.7 m cube |
| Large supply crate | 1.0 m cube |
| Sandbag | 0.5 m long, stacked in stepped rows |
| Oil drum | 0.9 m tall |
| Concrete barrier | 1.0 m H (bulletproof per Map 03 legend) |
| Palm tree | 6–9 m |
| Scrub / dry bush | 0.4–0.8 m |
| Playable map footprint | 4v4 arena, 3 lanes, ~90 × 70 m |
| Interior camera eye height | 1.65 m |

---

## 4. Materials

| Material | Look | Roughness | Notes |
|---|---|---|---|
| **Sand / packed earth** | Fine grainy noise, faint wind ripples, footprint-free. Warm `#C2B280`–`#E4D1A8`. | 1.0 | Brightest thing outdoors. |
| **Sandstone block** | Cut rectangular blocks, chipped corners, hairline cracks, mortar joints as recessed grid lines. | 0.95 | Light passes almost no spec. Damaged blocks show missing cubes. |
| **Concrete** | Smoother than sandstone, slightly cooler tan-grey, larger block scale, bulletproof (spalling chips = blocky pits). | 0.9 | Used for Map 03's central warehouse and hard cover. |
| **Wood** | Visible plank seams, grain streaks, splinters as blocky chips. Warm `#A9784C`. | 0.85 | Crates, beams, ladders, doors, window frames. |
| **Woven fabric / canvas** | Sandbags, tarps, ghillie — matte, no sheen, small-scale weave pattern. | 1.0 | Never glossy. |
| **Tactical fabric** | Uniforms, vests, pouches. Matte cordura, subtle thread-block texture. | 0.95 | Desert DPM camo blotches. |
| **Rigid polymer** | Weapon bodies, magazines, goggles, helmet shells. Sand/kaki with a **very** soft sheen. | 0.7–0.8 | The *only* mildly reflective class. Still flat-shaded voxel. |
| **Dark metal** | Barrels, muzzle devices, bolts, rails, buckles. Warm gunmetal brown-grey (never cold steel blue). | 0.45–0.6 | Micro-specular steps per block face. |
| **Rusted metal** | Barrels, fences, rebar. Terracotta-brown rust streaks over metal. | 0.9 | Edge-of-map and ruin dressing. |
| **Dry vegetation** | Scrub, straw, ghillie strands. Papery, matt, sharply lit. | 1.0 | High light/dark split so clusters read. |

**Forbidden materials:** glass with real refraction, chrome/mirror, wet/rain surfaces, emissive neon, holographic screens, cloth physics, PBR micro-detail maps that break the voxel read.

---

## 5. Lighting

### 5.1 The look: *warm, powdery, low-sun*
- **Key light:** strong warm-white sun, `~5000–5600 K` biased hard toward amber. Elevation **15–35°** for a long-shadow "afternoon into sunset" feel. This is the default mood of *every* reference that isn't an asset sheet.
- **Ambient:** warm bounce fill, `#C2B280` tinted, high enough that shadow interiors stay legible (never crushed to black).
- **Shadows:** hard-edged, long, and clearly blocky — shadow silhouettes show the stair-stepped voxel geometry. Contact shadows tight and dark-warm.

### 5.2 Dust atmosphere — the signature
- **Volumetric god-rays** through doorways, window holes and collapsed roofs. Visibly stepped as they pass block edges.
- **Suspended dust motes** drifting and catching the light (prominent in both interior references).
- **Distance haze:** exponential warm fog, tan `#BFA074`, that desaturates and lightens far geometry. Long sightlines should fade into glowing gold, not into grey.
- **Heat shimmer** optional and subtle, distant only.

### 5.3 Time-of-day variants
| Preset | Key | Ambient | Haze | Use |
|---|---|---|---|---|
| **High Noon** | Near-vertical, harsh, high contrast | Bright warm | Thin | Competitive clarity |
| **Afternoon Dust** *(default)* | 25–35°, warm | Medium warm | Medium | Standard matches |
| **Sunset / Ruins** | 15°, deep amber, long shadows | Low orange | Heavy gold | The "Ruinas del desierto" mood, lobby backdrop |

### 5.4 Lighting laws
- Light is **always warm.** No cool moonlight, no blue fill, no colour-graded teal shadows.
- Interiors are never pitch black — bounced warm light and god-rays keep them playable and readable.
- Emissive is limited to thin tactical-tech accents: a circuit-trace line, a tiny optic dot, a radio LED. **Never** glowing scenery.
- No dynamic global illumination, bloom glare or lens flares. Performance-first (brief §5: *lightweight for web*).

---

## 6. Architecture — Dust Town

### 6.1 Kit of parts
All buildings are assembled from a fixed voxel kit:
- **Sandstone block wall** (1.0 m and 0.5 m modules), with rubble/damaged variants
- **Concrete wall** — bulletproof hard cover (Map 03)
- **Floor slab / rubble floor**
- **Square-cut window frame**, broken variants
- **Doorway with wooden door**, removed-door variants
- **Wooden plank beam** (lintels, roof joists)
- **Stone staircase**, blocky and stepped
- **Raised/upper floor + balcony** with timber railing
- **Flat roof / parapet**
- **Wall stub** (partial ruined wall, chest-to-head height)
- **Rubble pile** (stepped scatter of loose blocks)

### 6.2 Construction grammar
- **Two-storey roofless shells** are the signature structure — upper floor open to the sky, timber joists protruding.
- **Asymmetry and collapse.** Floors partially missing, walls cut back to jagged block edges, corners knocked out.
- **Interiors are furnished with cover**, not decoration: crate stacks, sandbag nests, shelving. Every prop is potentially gameplay cover.
- **Multiple exits per structure** — doorways AND blown-open walls, to serve the anti-camper design.
- **Upper floors connect via exterior staircases** so rooftops are part of the lane flow, not dead ends.
- Palm trees and scrub break silhouette; utility poles/power lines thread along the perimeter.

### 6.3 Map 03 — Dust Town layout law
Recreate this structure when building the map:
- **4v4**, rectangular arena.
- **Three lanes:** Top Lane (flank route + left tunnel), Mid Lane (**warehouse**, high traffic, multiple doors), Bot Lane (street route + right alley).
- **Central warehouse** = concrete, bulletproof, the map's anchor and hot zone.
- **Perimeter marked "out of bounds"** with ruins dressing, not invisible walls alone.
- **Spawn A (Blue)** bottom-left, **Spawn B (Orange)** bottom-right, mirrored rotationally.
- **Anti-camper:** multiple routes between lanes, cross-lane sightlines, no single dominant perch, overlapping cover so no angle is safe forever.
- **Cover language:** wooden crates (soft, breakable-feel), concrete walls (hard, bulletproof). Scatter both to make cover reading instant.
- **Open sightlines** down each lane with crate/sandbag cover at intervals.

---

## 7. Characters

### 7.1 Shared construction
- **Desert DPM uniform** (Sandstone / Tan / Khaki / Olive blotches on Beige).
- **Combat boots** — chunky, blocky, laced in block steps.
- **Knee pads** — rigid blocks strapped over the trouser.
- **Combat gloves** — oversized blocky hands.
- **Face/head is always covered** — goggles, mask, or ghillie. No exposed character faces; the mask is the face.

### 7.2 Class silhouettes
**Heavy — VEXA-02**
- Heavy helmet with side rails, broad goggles, breather mask.
- Bulky **tactical vest with armour plates** and chest rig.
- Large **backpack** with radio antenna(s).
- Big thigh pouches, bulky shoulders.
- Carry: **VEXR LMG-09 block LMG, 60-round drum, optic sight.**
- Reads: immovable suppressor.

**Infiltration — VEXA-09**
- Medium helmet + **comms headset** with boom mic.
- Standard tactical vest, pouches, knee pads.
- Slimmer **field pack** with the VEXA logo patch.
- Shoulder squad patch.
- Carry: **VX-416 block rifle, 30-round mag, desert tan sight.**
- Reads: mobile flanker.

**Sniper — VEXA-07**
- **Ghillie hood** and **ghillie cloak** — layered *dry-grass voxel clusters* in straw/tan/brown, with visible strands.
- Black **NV mask** under the hood (no face).
- Modular-pouch vest with circuit lining + armour plates.
- Stealth supply backpack with circuit/NV battery blocks.
- Carry: **VEXA-07 SRS bolt-action sniper, 8× block scope, 1200 m range.**
- Reads: a bush that shoots.

### 7.3 Rigging
- Rigid voxel groups, non-deforming. Joint nodes: shoulders, elbows, wrists, hips, knees, ankles, neck — matching the asset sheet's red node layout.
- Rotation only; no stretch, no squash-morph that would round blocks.
- Gear is rigid-parented (vest to chest, pouches to vest, backpack to spine).
- First-person arms are separate rigid block models, sized slightly larger than world arms for readability.

---

## 8. Weapons

### 8.1 The four-weapon roster (from the weapon sheet)
| Name | Type | Spec |
|---|---|---|
| **VEXA-AR9** | Assault Rifle | Standard calibre · 30-round mag · tactical grip |
| **VEXA-SMG-3** | Submachine Gun | 9 mm · 25-round mag · compact · folding stock |
| **VEXA-SG12** | Shotgun | 12-gauge · 8-round tube · pump action |
| **VEXA-07 SRS** | Sniper Rifle | Bolt-action · 5-round mag · scoped · long barrel |

*Additional class weapon:* **VEXR LMG-09** — block LMG, 60-round drum, optic sight (Heavy loadout).

### 8.2 Visual rules
- **Blocky construction** — receivers, handguards, stocks and magazines are all cuboid; each part is a visible block cluster.
- **Palette:** Sand Beige `#C2B280` body, Coyote Brown `#8A6A46` furniture (grip, stock, handguard), Khaki accents, dark-metal barrel/bolt/rail.
- **No electronics** on guns — no holographic screens, no glowing reticles, no digital readouts. Optics are physical tubes and iron-style blocks. *(Explicit on the weapon sheet.)*
- **Slab-side markings:** brief stencilled model text (e.g. `VEXA-AR9`) as a decal on the receiver.
- **Readable silhouettes:** each weapon must be identifiable from its block outline alone at a distance — long thin barrel (SRS), fat drum (LMG-09), stubby folded profile (SMG-3), tube magazine (SG12).
- **Modular attachments** snap on as additional block clusters (scope, drum, suppressor, grip), never as smooth curved add-ons.

### 8.3 First-person presentation
- Blocky gloved hands grip the weapon; both arms visible.
- Weapon held slightly canted with the muzzle toward screen centre, occupying the lower-right quadrant (per the FPS reference).
- **ADS** brings the weapon up so the optic tube aligns screen-centre; still visibly blocky.

---

## 9. UI, HUD & Lobby

### 9.1 Interface philosophy
Panels look like **printed military card stock that got dusty** — parchment panels, hard 2–3 px dark outline, flat ink. Not glossy black sci-fi glass, not neon.

> **Note on `LOBY.png`:** that reference is the only image in the set whose *skin* contradicts VEXA's voxel-desert identity (it is a realistic, dark sci-fi shooter lobby). Per the brief's Priority Rule, its **layout and menu hierarchy are adopted**; its **realistic dark visual treatment is explicitly rejected** in favour of the voxel-desert UI described here.

### 9.2 HUD (from the FPS reference)
- **Bottom-left stack:** `AMMO 30/120` with a horizontal segmented ammo bar beneath, then `HP 100` with a segmented health bar.
- Fonts: bold condensed uppercase, high legibility, hard edges — stencil/military feel. Monospaced digits for ammo and health.
- Bars are **segmented into blocks** (consistent with the voxel grid), not smooth gradients.
- Colour: UI Ink `#3A2E1E` text on dusty panel, health bar `#4E5233`, low-ammo/hurt states flip to `#E07B2C` then `#B33A2E`.
- Elements: health, ammo/reserve, weapon + fire mode, grenade/utility slots, crosshair (minimal blocky cross that opens with spread), hitmarker (small blocky X), damage direction indicators, killfeed, squad list with Blue/Orange team accents.
- **No diegetic helmet visor overlay, no scanlines, no HUD chrome clutter.** Clean card panels only.

### 9.3 Crosshair & feedback
- Default: four short blocky ticks around a centre dot.
- Expands as a block-stepped animation on movement/fire. Never a smooth circle.
- Hitmarker: blocky X. Headshot/kill: same mark in Team Orange.
- Damage indicators: blocky arrow wedges at screen edge.

### 9.4 Lobby / main menu
Adopt the LOBY **structure**:
- **Left:** player card — rank/level, name, season progress, currency, daily/event prompts.
- **Character stage:** full-body 3D voxel soldier, front-facing, idle animation, on a **desert-ruins backdrop at sunset** (the "Ruinas del desierto al atardecer" mood — sandstone rubble, palm silhouettes, heavy gold haze, drifting dust).
- **Right:** vertical mode list — *Patria clasificatoria / Multijugador / Battle Royale / DMZ: Recon / Torneo* — as stacked tab plates.
- **Bottom-right:** a primary action button (e.g. **ARMAMENTO**) to open the loadout screen.
- **Top-right:** settings, notifications, currency, social icon.
- **Loadout / Armory screen:** weapon list on the left, large blocky voxel weapon preview centre on a dusty neutral backdrop, stat block and attachment slots on the right — styled as the weapon-collection reference sheet come to life.

### 9.5 UI consistency laws
- Panel background is always dusty parchment; border always dark warm brown.
- Every icon is a small **voxel/blocky pictogram**, not a smooth vector glyph.
- Corner treatments are **hard 90° or stepped** (never rounded corners).
- Team colours only: Blue friendly, Orange hostile. No third accent.
- All UI art respects the no-black/no-white/no-neon palette law.

---

## 10. VFX & Combat Feel

### 10.1 Impact & destruction
- **Bullet impacts:** a burst of small **blocky debris cubes** plus a blocky dust puff. On sandstone: tan chips. On concrete: pale grey-tan chips. On wood: brown splinters. On metal: dark sparks as tiny square flecks.
- **Surface decals:** square/stepped bullet holes, not round ones. Decals age and fade.
- **Destructible cover:** vitality shown by progressive removal of blocks — corners chip, edges go jagged, the piece finally collapses into a **pile of cubes** that settles and fades.
- **Ricochet:** single square fleck with a short tan tracer streak.

### 10.2 Weapon VFX
- **Muzzle flash:** short, blocky, star-ish burst of a few square flares — readable for 1–2 frames, never a soft glowing bloom.
- **Smoke:** blocky puff clusters that grow in steps, drifting on the warm wind and catching the sun.
- **Tracers:** thin bright warm lines with a slight blockiness, fading fast.
- **Shell casings:** small cuboid brass blocks ejecting and settling on the sand.
- **Suppressed fire:** tiny flash, extra dust kick, no smoke plume into the skyline.

### 10.3 Movement & world
- **Footfalls:** small blocky dust puffs on sand, tiny debris flecks on rubble.
- **Landing:** ring of cubes and a sand spray.
- **Sand cascading** off ledges, off ruined walls, and off collapsing rubble.
- **Ambient drift:** constant slow dust motes in god-rays; occasional dust devil wisps crossing open lanes.
- **Blood:** minimal, muted warm red, treated as small blocky flecks — restrained and non-gory.

### 10.4 Feel rules
1. Feedback must be **instant** — flash, dust and debris on the same frame as the impact.
2. All particle shapes are **square/pixel** sprites, never soft round ones.
3. VFX must never break the voxel read; every effect resolves into visible cubes.
4. **Performance first:** short lifetimes, pooled emitters, aggressive culling. This is a *lightweight web* target (Map 03 explicitly names browser-scale budgets).

---

## 11. Audio & Ambience

- **Ambience bed:** low desert wind with sand hiss, distant debris settling, occasional creaking metal, far-off crumbling stone.
- **Combat:** gunfire is **hard, dry, cracky and punchy** with real tail-off into the desert — no sci-fi whooshes, no laser tones, no electronic beeps.
- **Impacts:** distinct material signatures — sandstone crack, concrete thud-and-chip, wood splinter, metal clang-and-ricochet.
- **Mechanical:** magazine in/out, bolt rack, pump action, drum rattle, boots on sand vs. rubble vs. timber upper floors.
- **UI:** dry, weighty clicks and thumps — stamped-metal and paper sounds, not digital chirps.
- **Music:** sparse, percussive, tense; warm low strings / horns for lobby and result screens. No EDM, no synthwave.
- **Vocal comms:** clipped, radio-filtered callouts consistent with the tactical-unit fiction.

---

## 12. Visual Consistency Rules (checklist for every new asset)

Before any asset is accepted, all must be true:

1. **Blocky read.** Can the silhouette be described in cubes? If it needs a smooth curve — rebuild it.
2. **On grid.** Snapped to the 0.1 m voxel grid; no off-grid floaters.
3. **Palette-compliant.** Only colours from §2. No pure black/white, no neon, no cool grey.
4. **Warm light only.** Asset looks correct under 15–35° warm sun with tan haze.
5. **Flat-shaded.** No smooth gradient shading across a block face.
6. **Rigid.** No deformation, no cloth sim, no soft-body.
7. **Correct scale.** Cross-checked against §3 tables and a reference character.
8. **Material-honest.** Uses one of the §4 material definitions; not a novel glossy/sci-fi surface.
9. **Cover-aware** (for props/architecture): reads instantly as hard or soft cover.
10. **No electronics on weapons.** Physical blocks only.
11. **Nearest-neighbour texturing.** Texels stay crisp; no blurry mip mush.
12. **Performance budgeted.** Triangle count, texture size and particle count suit a lightweight web target.
13. **Screenshot test.** A screenshot of the new asset placed in Dust Town still reads as *the same game* as the references.

If any check fails — fix it before moving on. If a reference is silent on a detail, **invent it inward from these rules**, never outward toward generic FPS realism.

---

## 13. Asset Reference → Deliverable Map

| Reference image | Informs | Produces |
|---|---|---|
| `Gemini_Generated_Image_20tw2s20tw2s20tw.jpg` (logo on sand/PCB) | Branding, palette, core tension | Logo/wordmark treatment, loading + lobby branding, UI colour system |
| Rig & texture asset sheet | Voxel rig, flat shading, UV/atlas strategy | Character rig spec, texture atlas layout, gear modularity |
| Heavy VEXA-02, Infiltration VEXA-09, Sniper VEXA-07 sheets | Character classes, gear, proportions | Three playable soldier models + loadouts |
| `Ruinas del desierto al atardecer.png` | Building kit, lighting mood, materials | Dust Town modular architecture set, sunset lighting preset |
| FPS warehouse / street screenshots | First-person view, HUD, VFX in-context | FP arms + weapon pose, HUD layout, dust god-rays, impact VFX |
| `Voxel weapon collection` sheet | Weapon roster + visual language | VEXA-AR9, VEXA-SMG-3, VEXA-SG12, VEXA-07 SRS models |
| Map 03 Dust Town | Arena layout, lanes, cover, spawns | Playable 4v4 three-lane map |
| `LOBY.png` | Lobby/menu **structure only** (skin rejected) | Lobby, mode select, armory/loadout screens |

---

## 14. Enforcement

`ART_DIRECTION.md` is the project's visual contract. It outranks personal style preference, generic FPS convention and convenience. When the brief's Priority Rule applies, resolve conflicts in this order:

1. Explicit written instructions from the project brief.
2. **VEXA's identity** as defined in this document.
3. The reference images in `imagenes-de-referencia-/`.
4. Performance.
5. Stability.
6. Modular architecture.
7. PC FPS player experience.

**The reference images are consulted, never shipped.**
