# Daily Flow — Blanca's Recovery & Routine Tracker

## Project Goal
Build a single-page HTML web app called **"Daily Flow"** and publish it via GitHub Pages so it's accessible from a phone home-screen icon (Add to Home Screen), fully portable and independent of any chat platform.

This recreates a working prototype (attached separately as `blanca_daily_flow.html` if provided) — preserve its structure, content, and visual style exactly unless noted below. The only functional change needed is the storage mechanism (see Data Persistence).

## Who this is for
Blanca, 63, recovering from a total knee replacement (May 2026) and hyperparathyroid surgery, managing ADHD, and rebuilding daily structure after months of disrupted routines. The tool must feel gentle, non-punishing, and low-friction — never like a productivity app that shames missed days.

## Design Direction
- **Palette:** sage green (#7C8B6F, #5C6952), warm stone (#A89A87), deep brown (#4A3B2E), gold (#B8934A), plum/purple accent (#6B5271), cream background (#F4F0E6), paper white (#FBF9F3).
- **Typography:** 'EB Garamond' (serif, for headers, italic subheads), 'Tangerine' (script, for one hero line only), 'Inter' (sans-serif, for body/UI/data). Load from Google Fonts.
- **Tone:** Taoist/Wu Wei inspired — flowing, accepting, non-judgmental. A small wave/brushstroke SVG divider between sections is the signature visual element.
- **Layout:** single column, mobile-first, max-width ~640px centered, generous padding, rounded card sections (10px radius), soft borders.
- Fully responsive down to small phone screens. Visible focus states for accessibility.

## Page Structure (in order)
1. **Header** — script-font line "I flow with the river of life" + italic subline "trusting its direction, one gentle step at a time"
2. **Date row** — auto-displays today's actual date (e.g. "Friday, July 10") + a small free-text "mood tag" input (one word for the day)
3. **"The Floor"** section — the non-negotiable minimum: checkboxes for "Meditation" and "Warm drink, slow moment (matcha)". Include note: "If only these two happen today, today still counts."
4. **Wave divider** (decorative SVG, repeating gentle sine wave in gold, low opacity)
5. **"Knee — PT & Care"** section — subtitle noting the goal ("0° extension · 125° flexion by birthday"). Checkboxes: Home PT (assertive, not aggressive), Jacuzzi (waterproof Band-Aid on), Ice/rest as needed, Movement/short walk
6. **"Positive Mental Awareness"** section — checkboxes: "Checked in with how I actually feel", "Psychiatrist search — one small step". Plus a free-text textarea: "What's underneath today, if anything…"
7. **"Connection"** section — three toggle buttons (not checkboxes): Honey, Krystn, Dave — tap to mark as visited/connected today, toggle active state with plum background
8. **"Sleep"** section — checkbox: "Hypnosis playlist tonight" (healing + weight-release playlist)
9. **"Focus Timer"** section — a simple countdown timer. Dropdown to select 25/15/10/5 minutes. Start/Pause button and Reset button. Large timer display in gold serif font. On completion, display "done 🌿" instead of 00:00.
10. **"Today's Win"** — visually distinct card (sage gradient background, white/cream text) with a textarea prompting: "Even meditation + matcha on a hard day counts here…"
11. **Footer** — italic line: "Whatever comes up is fine — I accept this moment exactly as it is."

## Data Persistence (IMPORTANT — differs from prototype)
The original prototype used a Claude-artifact-only `window.storage` API. That API does **not exist** in a standalone GitHub Pages site. Replace it with:

- **`localStorage`**, keyed by date: `flow:YYYY-MM-DD` (same key format as before), storing a JSON blob of all checkbox states, text fields, and toggle states for that day.
- On page load: read today's date, look up `localStorage.getItem('flow:' + todayISODate)`, and populate the form if data exists. If not, start blank.
- On every change (checkbox, textarea input, toggle click): debounce ~250ms, then save the full state object back to localStorage under today's key.
- Show a small transient "saved ✓" status message near the bottom after each save, fading out after ~1.8s.
- Because this is a static site with no backend, data lives only in that browser/device's localStorage. That's fine and expected — this is a personal single-user tool. Do NOT add any backend, database, or account system.
- Optional nice-to-have (only if trivial): a simple way to export/view past days' entries from localStorage (e.g. a small "history" link that lists saved dates) — not required for v1.

## Technical Requirements
- Single `index.html` file (inline CSS and JS is fine and preferred for simplicity/portability), OR a minimal `index.html` + `style.css` + `script.js` if you prefer separation — either is acceptable, but keep it to these files only. No build step, no framework, no npm dependencies required to run it.
- Must work fully offline after first load except for the Google Fonts CDN request.
- No tracking, no analytics, no external calls besides Google Fonts.

## Deployment
1. Initialize a git repo, add the files.
2. Push to a new GitHub repository (ask the user for the repo name/visibility if not specified, default to a public repo named `daily-flow`).
3. Enable **GitHub Pages** from the repository settings, serving from the `main` branch root (or `/docs` if that's simpler to set up — pick one and document it).
4. Confirm the live GitHub Pages URL works and report it back clearly at the end, e.g.:
   `https://<username>.github.io/daily-flow/`
5. Remind the user how to add it to their iPhone home screen once live:
   - Open the URL in Safari
   - Tap the Share icon → "Add to Home Screen" → Add

## What NOT to change
- Do not add login/auth, ads, tracking, or unrelated features.
- Do not make the tone clinical, gamified with streaks/points, or punishing for missed days — this must stay gentle and floor-based, never guilt-based.
- Do not remove "The Floor" section or its no-pressure framing — it's the emotional core of the tool.
