# The Field Journal — Explore

A data-driven, browser-based scene explorer. One generic engine (`index.html`)
reads everything else from plain TSV tables — no rebuild needed to add scenes or
touch points, just new rows.

## What's in this repo

```
index.html          ← the engine. Never needs editing for ordinary content changes.
/scenes/
  garden.jpg         ← ready
  bedroom.jpg         ← ready
/data/
  locations.tsv        ← one row per scene
  dilemmas.tsv           ← one row per touch-point dilemma
  exits.tsv                ← one row per scene-to-scene connection (empty for now)
```

Two scenes are wired and working right now: **Garden** and **Bedroom**. Eight more
are scoped in `locations.tsv` with `status = pending` — they'll show a "still being
built" placeholder until an image and rows are added for them. See
`EXPLORE_ENGINE_SCOPE.md` (in the project files, not this repo) for the full plan.

---

## First-time setup — get this live on GitHub Pages

1. **Create the repo on GitHub** (via the website): New repository → name it
   (e.g. `field-journal-explore`) → keep it **Public** (GitHub Pages needs this on
   a free account) → don't initialise with a README (you already have one).

2. **From this folder, push it up.** Open a terminal in this exact folder and run:
   ```bash
   git init
   git add .
   git commit -m "Initial Explore engine — Garden and Bedroom scenes"
   git branch -M main
   git remote add origin https://github.com/YOUR-USERNAME/YOUR-REPO-NAME.git
   git push -u origin main
   ```
   Replace `YOUR-USERNAME` and `YOUR-REPO-NAME` with the real values from step 1.

3. **Turn on GitHub Pages:** on the repo's GitHub page → **Settings** → **Pages**
   (left sidebar) → under "Build and deployment", Source = **Deploy from a
   branch** → Branch = **main**, folder = **/ (root)** → Save.

4. **Wait about a minute**, then your live URL will be:
   ```
   https://YOUR-USERNAME.github.io/YOUR-REPO-NAME/
   ```
   That's the link to open on any device — phone, tablet, whatever — from here on.

---

## Adding a new scene later (no code, ever)

1. Get the image (see `SCENE_ART_BRIEF.md`), name it e.g. `hallway.jpg`, drop it
   in `/scenes/`.
2. Open `data/locations.tsv` in a spreadsheet app (or any text editor), find the
   row for that scene id, change `status` from `pending` to `ready`, fill in
   `image` (e.g. `scenes/hallway.jpg`) and `intro`.
3. Add touch points: open `data/dilemmas.tsv`, add a new row — `scene` must match
   the location's `id`, `x`/`y` are percentage coordinates (use the coordinate
   picker tool to find these from the actual image).
4. Save, commit, push:
   ```bash
   git add .
   git commit -m "Add hallway scene"
   git push
   ```
5. Refresh the live page. Done — no engine changes.

## Adding a new touch point to an existing scene

Just add a row to `data/dilemmas.tsv` with the right `scene` id and coordinates.
Nothing else to touch.

## Connecting two scenes (so you can walk between them)

Add a row to `data/exits.tsv`: `scene` = where the exit appears, `x`/`y` = its
position, `target_scene` = where it leads, `label` = the button text (e.g. "Go
upstairs"). Currently empty — Garden and Bedroom aren't connected yet since
nothing links them narratively without a Hallway scene between them.

---

## A note on file formats

Everything here is **tab-separated** (TSV), not comma-separated (CSV) — this is
deliberate. The activity text is full of natural commas ("no explaining, no big
moment"), and commas are exactly what CSV uses as its delimiter, which means every
field would need careful quoting or the file silently breaks. Tabs never appear in
normal prose, so there's nothing to quote or worry about. Open these files in
Excel, Google Sheets, or Numbers — they'll all recognise `.tsv` correctly.

## Local testing without pushing to GitHub every time

Opening `index.html` by double-clicking **will not work** — browsers block a local
file from reading other local files for security reasons, and that's exactly what
this engine needs to do. To test changes before pushing, run a tiny local server
from this folder:
```bash
python3 -m http.server 8000
```
then open `http://localhost:8000` in a browser. Stop it with Ctrl+C when done.

---
*Engine v1 — Garden and Bedroom proven working. Ready for Hallway, Kitchen, Back
Path, and the Street/School levels as art and content are added.*
