# AMIER CARS — Private Collection 3D Showroom

A bilingual (EN / العربية RTL) single-file 3D car showroom. It opens on a **presentation gate** — enter the showroom and inspect up to **8 cars** in full 3D: every angle, engine bay, wheels, cabin and paint — dressed in a deep-black + gold + silver identity.

## The fleet (7 of max 8)

| # | Car | Powertrain | Output | 0–100 | Model file |
|---|---|---|---|---|---|
| 01 | BMW F10 535i (2013) | 3.0L N55 twin-turbo inline-6 · RWD | 306 hp / 400 Nm | 5.9 s | `car.glb` |
| 02 | BMW M5 E34 (1992) | 3.6L S38 inline-6 · RWD | 311 hp / 360 Nm | 6.3 s | `bmw_m5_e34.glb` |
| 03 | Porsche 911 Carrera 4S (2019) | 3.0L flat-6 twin-turbo · AWD | 450 hp / 530 Nm | 3.4 s | `free_porsche_911_carrera_4s.glb` |
| 04 | BMW M2 G87 ADRO (2023) | 3.0L S58 twin-turbo inline-6 · RWD | 460 hp / 550 Nm | 3.9 s | `2023_bmw_m2_g87_adro_carbon_fiber_aero_body_kit.glb` |
| 05 | BYD Yangwang U8 (2023) | Quad-motor e-4WD + 2.0T range extender | 1,200 hp / 1,280 Nm | 3.6 s | `2023_byd_yangwang_u8.glb` |
| 06 | BYD Yangwang U9 (2024) | Quad-motor e-4WD (960 kW) | 1,300 hp / 1,580 Nm | 2.4 s | `2024_byd_yangwang_u9.glb` |
| 07 | BYD Han DM-p (2023) | 1.5T plug-in hybrid AWD (DM-p) | 489 hp / 850 Nm | 3.7 s | `2023_byd_han_dm-p.glb` |

## The entrance gate (presentation)

The site boots into a cinematic gate: AMIER CARS wordmark, the whole fleet as gold chips, and one CTA — **ENTER THE SHOWROOM** (ادخل المعرض). The gate has its own عربي/EN toggle; the 3D studio loads behind the blur, so entering feels instant. `body.gated` locks scrolling and hides the chrome until entry.

## Run

Any static server from the project root, e.g.:

```
python -m http.server 8765
# open http://127.0.0.1:8765/index.html
```

## What you can inspect on every car

- **Free explore** — drag-orbit the real model on the gold turntable (auto-orbit pauses on interaction).
- **Wheel study** — nearest wheel explodes into tire / rim / disc / caliper; rim finishes (Gloss Black, Silver, Bronze, Gold) apply to the rim materials.
- **Powertrain** — hood lifts (or the bay shows beside the body); ICE cars get the procedural X-ray engine with live pistons in firing order; **EV cars (U8/U9) get an e-drive bay**: battery floor, front/rear e-axles with spinning rotors and HV cabling — no pistons.
- **Underbody** — the car lifts, ghosts to glass and reveals the procedural drivetrain.
- **Driver space** — driver seat 360°, live digital cluster (speed/RPM dials scaled to each car's `tachMax`/`redline`), ambient light, iDrive screen, open/close door.
- **Paint & finish** — 10 swatches retint the body paint (auto-detected by material color or name) and re-theme the studio lighting.
- **Vehicle details** — full bilingual spec sheet from the car's profile, then **Book on WhatsApp** — the message includes the viewed car's name.

## Rebranding — one place to edit colors & fonts

Open `index.html` and find the `:root{…}` block at the top of the `<style>` section. Every color derives from these variables:

```css
:root{
 --bg:#07080A;          /* page base (near-black)      */
 --bg-2:#0D0F12;        /* elevated surfaces            */
 --gold:#D4AF37;        /* the brand accent             */
 --gold-deep:#B8892E;   /* darker gold (borders, hover) */
 --gold-soft:#F5E6A8;   /* highlight gold               */
 --accent:#B8892E;      /* swatch accent (paint sync)   */
 --accent-soft:#D4AF37;
 --silver:#C9CDD2;      /* secondary accent             */
 --text:#F2F2F0;        /* primary text                 */
 --muted:#9A9EA5;       /* secondary text               */
 --grad-gold:…;         /* buttons / titles gradient    */
 --grad-silver:…;       /* numerals gradient            */
 --grad-title:…;        /* the hero wordmark gradient   */
 --ease:cubic-bezier(.22,.68,.28,1);  /* motion curve  */
}
```

**Fonts** load from one Google Fonts `<link>` in `<head>`:

| Token | Family | Used for |
|---|---|---|
| Display | `Michroma` | Logo, h1, gate wordmark, section h3 |
| Editorial | `Cormorant Garamond` | h2 headings, big spec numerals |
| UI | `Manrope` | buttons, labels, small caps, canvas dials |
| Arabic | `Cairo` | everything once `body.ar` is set |

Canvas-drawn text (cluster, iDrive, engine badge) uses `Manrope` — update those `context.font` lines if you swap fonts. The 3D studio colors live in `buildLighting()` and the `studio` palette inside `setEnvironment()`.

## How to add a car (max 8)

1. Drop the GLB next to `index.html`.
2. Append an object to the `CARS` array (search `const CARS=[`). The showroom hard-caps at `MAX_CARS=8` (switcher tabs and collection cards both slice to it).

Template — copy an existing entry and edit:

```js
{
 brand:"Porsche", model:"911 Carrera 4S", modelUrl:"./free_porsche_911_carrera_4s.glb",
 tabLabel:"911 C4S",            // compact switcher pill label
 tag:"Sport Icon",              // collection card tag
 powertrain:"ice",              // "ice" | "ev" | "hybrid" — ev builds the e-drive bay
 hotspots:{grille:"…",eyes:"…",wheels:"…",exhaust:"…"},    // EN hotspot labels
 hotspotsAr:{grille:"…",eyes:"…",wheels:"…",exhaust:"…"},  // AR hotspot labels
 engine:"3.0L flat-6 twin-turbo (992)",
 engineAr:"محرك سداسي أفقي…",
 hp:450, torque:530, transmission:"8-speed automatic (PDK)",
 transmissionAr:"ناقل PDK من 8 سرعات", drive:"AWD",
 zeroTo100:3.4, topSpeed:306, year:2019, mileage:"38,000 km",
 color:"Imola Red",             // must match a PAINTS entry name
 price:"EGP 000,000", whatsapp:"201008655235",
 features:["…"], featuresAr:["…"],
 engineBadge:"TWIN-TURBO FLAT-6",   // valve-cover badge texture
 tachMax:8000, redline:7500,        // cluster dial range
 engineDesc:{en:"…",ar:"…"},        // powertrain-mode description
 aspiration:{en:["…","…"],ar:["…","…"]},
 exteriorCopy:{en:"…",ar:"…"},
 labelOverrides:{                   // engine-bay part labels (anchors: valve/turbo/block/pipes/fan/battery)
  valve:{en:["Rear deck","Flat-6 · 450 hp"],ar:["غطاء المؤخرة","بوكسر سداسي · ٤٥٠ حصان"]}
 }
}
```

3. That's it — switcher tab, collection card, live 3D thumbnail (auto-captured on first load), marquee, WhatsApp message, cluster, bento, hotspots and all Arabic copy generate from this object. Wheels are detected by name (`wheel/tire/tyre/rim/hub`) or positionally (4 round parts near the corners); **if the model ships wheels merged into whole-axle meshes** (e.g. the free Porsche 911), the showroom synthesizes per-corner anchors so the wheel study, explode, rim finishes and facts still work — the `?debug=1` panel remaps anything else the heuristics miss. First load of a heavy GLB (15–26 MB) parses on the spot; the collection card then shows the live thumbnail.

## Structure of the redesign (what changed, where)

- **Gate/entry**: `#gate` section + `setupGate()`/`applyGateCopy()`; `body.gated` locks scroll and hides chrome; `#gate-enter` releases it.
- **Fleet data**: `CARS[]` profiles drive everything; `MAX_CARS=8` caps the UI; `powertrain` field selects ICE bay vs EV e-drive bay (`buildEVBay`); `rebuildEngineBay()` rebuilds the bay per car load; per-car `hotspots/hotspotsAr` sync into `COPY[lang].hotspots`.
- **Identity**: AMIER CARS tokens (`:root`), gold gradients, Michroma/Cormorant/Manrope/Cairo, favicon, loader.
- **Icons**: inline SVG sprite after `<body>` (`i-360 … i-vol-off` + `stGold/stSilver` gradients); glass `.ig` chips; `setBt/setBtEl` keep labels icon-safe across language switches.
- **Collection**: `buildCollection()` from `CARS` — glass cards, live thumbnails, `.many` grid for >5 cars, `.fleet` copy width.
- **Switcher**: top-center pill, gold glider (`moveDockIndicator()`), single scrollable row when >5 tabs.
- **Scene**: gold platform light + turntable rim, silver key/rim lights, reflective floor, gold cluster/iDrive/engine badge; paint detection by color **or** material name.
- **Chrome**: scroll progress hairline, boot stagger, tilt/magnetic cards, reduced-motion respected.

## Offline / CDN resilience

- **Core too**: `three` and `gsap` import from the map first, then fall back to **local `vendor/three.module.js` / `vendor/gsap.core.js` (+ `gsap-core.js` + `CSSPlugin.js`)** — so the showroom boots even when every CDN is unreachable.
- Optional modules fall back jsdelivr → unpkg → local `vendor/` + `shaders/` + `utils/` copies.
- Result: the gate, loader and full showroom work in a fully offline presentation.

## Debug & tests

- `index.html?debug=1` — mapping panel + role remap. `__AMIER_GET_SCENE__()`, `window.CARS/CAR`, `window.__AMIER_MODEL_INFO__` (per-car dims, mesh count, roles) for live inspection.
- `node test-page.mjs` — full F10 regression (boot, 7 modes, paint sync, ignition, cluster, bento, gallery, AR RTL, debug remap) expecting zero console/page errors.
