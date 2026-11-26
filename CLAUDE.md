# CLAUDE.md: Nicholas Tipton engineering portfolio

Context for maintaining and iterating on this site.

## What this is

- A personal portfolio for job hunting in RF, hardware, and aerospace communications. It's linked from my résumé, so hiring managers are the audience.
- **Live at:** https://nicknamenms.github.io (GitHub Pages user site).
- **Deploy:** Pages serves `main` / root directly. There's no build step, framework, or package.json. Every push to `main` goes live within a minute or two.

## About me (for writing copy)

- BS Electrical Engineering Technology at Purdue, CNIT minor, May 2027. BA Physics (math minor) from Wabash College, 2023.
- Application Engineering intern at Texas Instruments (2026): RF front-end noise characterization for a 1.5 GSPS ADC. Now I build and rework production boards at Vetronix. Before that, about 2.5 years as a maintenance tech at Crestview (appliance, HVAC, and refrigeration repair).
- Ham callsign **KD9YXB**, General class (started as a Technician). I've made contact with the ISS.
- Clubs: Purdue Orbital avionics (DAQ subgroup, joined fall 2026), W9YB, IEEE, adapted toys for kids. I was president of the Society of Physics Students **at Wabash, not Purdue**.
- Contact links on the site: email `nicktipton1126@gmail.com`, LinkedIn `linkedin.com/in/Nick-Tipton11`.

## Files

```
index.html          ← the whole site: inline CSS, HTML, inline SVG, and JS (~400 KB)
images/             ← all photos, videos, and video posters (jpg/mp4)
Tipton-Resume.pdf   ← linked by the hero "Download résumé" button
README.md
CLAUDE.md           ← this file
```

- Everything lives in one `index.html`. Make **targeted edits**. Never regenerate or rewrite the whole file, because it holds many hand-tuned SVG diagrams.
- All paths are relative (`images/foo.jpg`). GitHub Pages is **case-sensitive**: filenames must match their references exactly. Use lowercase-kebab-case names.
- The only external dependency is Google Fonts.

## Design system: keep it consistent

**Vibe:** sci-fi / "mad scientist lab" engineering. Think a deep-space instrument panel, VFD phosphor glow, oscilloscope traces, schematics, and rack-mounted test gear. It should feel technical and crafted, not like a generic template.

### Colors (`:root` in index.html)

| Token | Hex | Use |
|---|---|---|
| `--space` | `#0A1424` | Page background, deep space blue |
| `--panel` | `#111F36` | Cards and panels |
| `--line` | `#22385A` | Borders, grid lines, rack dividers |
| `--phosphor` | `#62F2D0` | **Primary accent** (VFD teal): links, active states, highlights, main traces, solid buttons |
| `--amber` | `#FFB547` | **Second accent** (warning-lamp amber): callsign, subheads and dates, focus rings, second trace or channel, `.todo` notes |
| `--coral` | `#FF6B81` | **Third accent, reserved for signal processing**: DSP blocks, "after averaging" data. Not decorative. |
| `--text` | `#D9E4F4` | Body text |
| `--muted` | `#8497B4` | Secondary text |

Hard-coded supporting colors: schematic wires and component outlines are `#9DB4D8`. Inner SVG boxes are `#16294A`. Card and photo backgrounds are `#0D182B`, `#0B1628`, and `#08111F`.

Color meaning is consistent across diagrams: **teal = channel / signal A, amber = channel / signal B, coral = processed / averaged.** Keep that mapping in any new plot or diagram.

### Type

- Display and headings: **Chakra Petch** (500/600/700) → `var(--display)`
- Body: **IBM Plex Sans** (400/500/600) → `var(--body)`, 17px/1.6
- Mono (callsign, equations, readouts, SVG labels): **IBM Plex Mono** → `var(--mono)`

### Signature motifs (reuse these; don't invent new styles)

- Faint engineering-paper **48px grid** on the body, faded by a radial gradient.
- **Hero:** a full-width `<canvas id="scope">` FM-modulated waveform, with a glowing teal trace plus a fainter amber one. It sweeps once on load, then drifts slowly. The amber `KD9YXB · on the air` callsign has a glowing dot. The name is "Nicholas" solid with "Tipton" as **outlined/ghost teal text**. A readouts strip (`dl.readouts`) sits below it. Don't change the hero's character.
- **Buttons:** `.btn` (solid teal) and `.btn.ghost` (outline) with **clipped corners** (`clip-path` polygon).
- Brand mark in the sticky top bar: `N/TIPTON`, with the slash in teal.
- **Section subheads** (`.bg-head`): teal label followed by a hairline rule.
- **Toolkit "rack":** units separated by 1px `--line` gaps, each with a glowing teal LED dot in the corner.
- **Expandables:** `<details>` with a square `+`/`−` indicator box (teal outline, filled when open).
- **Schematics:** inline SVG with scope-probe "screens" (`.scope`) showing the waveform at each node, animated dashed current flow (`.cur`/`.lcur`), and hop-overs where wires cross.
- Glows come from `box-shadow` / `drop-shadow` in the accent color. Keep them subtle.
- **Footer mascot:** an **original** line-art tracked robot (treads, articulated arm, waveform-grin face, blinking amber antenna LED) that drives across the footer. Don't make it resemble any commercial robot (e.g., Anki Vector).

### Responsive and motion

- Breakpoints in use: 1300, 1200, 1100, 900, 820, 800, 760, 720, 700, 640, 620px. The project grid is 1 column, then 2 at ≥720px and 3 at ≥1200px.
- Below 760px the career flow diagram is hidden. Below 620px the header schematics are hidden. Below 640px some nav links hide.
- **Every animation must respect `prefers-reduced-motion`**, as the existing ones do. Animated loops pause when off-screen (IntersectionObserver).
- Accessibility: SVGs get `role="img"` with a `<title>`, decorative ones get `aria-hidden`, and focus-visible rings are amber.

## Page structure

1. **Top bar** (sticky): Background · Internship · Projects · Contact.
2. **Hero** (`.hero`): name, one-line role, buttons (See my projects / Download résumé), readouts.
3. **Background** (`#background`):
   - "Experience, as a signal chain": an SVG DSP block diagram. Juneau, Alaska → Wabash physics as a *filter* → TI as the *main amplifier*, with a parallel branch of Crestview (*delay*) and Vetronix (*gain stage*) rejoining at the output. Blocks link to the experience cards.
   - Education (expandable; Purdue and Wabash coursework tagged by application area, e.g. RF, DSP, embedded).
   - Clubs & activities (click a chip to open a panel; some have photos or video).
   - Toolkit (rack units: RF & test, Design software, Embedded & code, Build & repair).
   - Licenses (FCC General and Technician, KD9YXB).
4. **Featured work: Texas Instruments** (`#internship`): a ground-station uplink SVG, then a tabbed **lab notebook** (`#tilab`): 1 The problem · 2 Theory · 3 Hand calculation · 4 Test configurations · 5 Results. Includes a live animated SNR demo and real FFT data re-plotted in site colors.
5. **Projects** (`#projects`): a precision full-wave rectifier schematic in the header, category filter buttons, a photo-card grid, and an **"On the bench"** row (with a Chebyshev LC filter schematic) for drafts.
6. **Contact**, then the footer robot lane, then the footer (`© 2026 Nicholas Tipton · KD9YXB · 73`).
7. `<dialog id="detail">`: the project detail popup.

## Projects: how they work

The data lives in the `<script>` near the bottom; search for `PROJECT LIST`. The order in `PROJECTS` is the order on the page.

```js
const CATEGORIES = ["RF & Communications", "Embedded & Digital", "Power & Motion",
                    "High Voltage & Vintage", "CAD & Fabrication", "Field Repair"];
{
  title:    "…",
  category: "…",            // must exactly match a CATEGORIES entry
  image:    "file.jpg",     // in images/; "" if none
  image2:   "file2.jpg",    // optional second photo in the popup
  video:    "file.mp4",     // optional; plays muted and looped on the card, with `image` as the poster
  summary:  "1–2 sentences on the card",
  details:  "plain-text popup body; blank line = new paragraph",
  html:     "…",            // optional rich popup body (figures, diagrams, galleries); used instead of details
  wide:     true,           // optional: the card spans 2 columns
  draft:    true,           // puts it in the "On the bench" row instead of the photo grid
}
```

**Current projects:**
- Finished: FM Superheterodyne Receiver (wide, rich html, demo video) · Ham Radio & ISS Tracking · Reverse-Engineered VFD Clock · Weather Station (wide, gallery) · Purdue Orbital Test Stand DAQ (protoboard; PCB in design) · Class D Audio Amplifier (video, full schematic) · HPS Wall Pack Rebuild (Field Repair) · RV Rooftop A/C Repair (Field Repair) · Motor Drive PCB System (2 images) · Spark-Gap Tesla Coil · Single-Cylinder Engine (NX)
- Drafts ("On the bench"): Yagi-Uda Antenna · Calculator · Lights & Switches Board · Motor Driver Board · Electromagnet · CRT TV

## Media rules

- Put the file in `images/` and reference it exactly.
- Compress video: `ffmpeg -i in.mov -vf "scale=-2:720,fps=30" -c:v libx264 -preset slow -crf 26 -pix_fmt yuv420p -movflags +faststart out.mp4`. Keep audio only when it matters (e.g., the Class D amp). Pair each video with a `*-poster.jpg`.
- Resize and compress photos for the web. Crop out phone-model watermarks.
- Everything in the repo is public, even if no page uses it. Don't commit anything with other people's names or private chats, copyrighted content (e.g., game ROMs on screens), or employer work-site photos that haven't been cleared.

## Accuracy and content rules (important)

- **Technical accuracy comes first.** Common past mistakes: wire routing that doesn't match the real circuit, signals entering the wrong op-amp pin, wrong diode, polarity, or speaker orientation, splitter arrow direction, and mislabeled parts. The FPGA board is an **Altera DE1 (Cyclone II), not a DE1-SoC**.
- Draw schematics from the actual circuit (Multisim files, lab reports, photos). If something is ambiguous, ask instead of guessing.
- **Never invent specs**, numbers, results, or dates. If a detail is missing, leave an HTML comment `<!-- EDIT: … -->` or a `.todo` amber note, and say so.
- Plots use real data, re-styled in the site palette.
- Copy is first person, plain, specific, and honest: what I built, how, and what I measured. No hype or buzzword padding. Don't overstate solo work vs. team or kit-based work.
- All art and diagrams must be original. Don't recreate logos, characters, or commercial product designs.

## How to work on it

1. Make a small, targeted edit in `index.html`. Keep CSS near related rules; reuse existing classes and variables.
2. **Verify visually before calling it done.** Serve locally (`python -m http.server`) and screenshot with Playwright at **390px (phone), ~1000px (laptop), ~1440px, and ~1900px**. Check:
   - no horizontal overflow (`document.documentElement.scrollWidth <= innerWidth`)
   - no label collisions in SVGs, and no console errors
   - reduced-motion mode still looks right
   - every referenced file exists with matching case
3. Summarize the change in a sentence or two and suggest a commit message.

## Backlog

- 6 draft cards still say "Write-up coming." Filling these in (or hiding them) matters more for credibility than adding new cards.
- "Motor Driver Board" may duplicate "Motor Drive PCB System". Ask before merging them.
- Unused images in the repo (still publicly visible): `rv-board.jpg`, `wx-anemometer.jpg`, `wx-board.jpg`. Use them or delete them.
- `<!-- EDIT -->` placeholders: grant dates on both licenses, and a link to the TI application note once it's published.
- W9YB and IEEE club panels are one-liners and need real content.
- DAQ card: add PCB renders and results as the board comes together. It switched from NAU7802 + ADS1115 to an ADS131M04 for faster simultaneous multi-channel sampling without QFN parts. Add a hot-fire thrust curve if one becomes available.
- Candidate additions: SX1280 + nRF52840 2.4 GHz mesh radio board (in design), coursework builds (ECET 36400 antenna labs, C5515 FIR filters, DE1 VGA/UART/SPI labs), Ben Eater 8-bit computer on protoboard, macro pad, plasma speaker. Add a GitHub link once code is published.
