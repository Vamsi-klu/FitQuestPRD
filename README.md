
# FitQuest PRD — Interactive Requirements

An interactive, single‑page Product Requirements Document (PRD) for the FitQuest fitness app. It turns a static doc into a fast, navigable, and visual experience — now with AI enhancement in every section.

## Live Demo

Open the hosted version:

- https://vamsi-klu.github.io/FitQuestPRD/

## Highlights

- Single‑page navigation with a clean sidebar and responsive layout (Tailwind CSS).
- Charts for quick comprehension of activities and gamification (Chart.js).
- AI Enhance in every section: type a prompt, see staged “Parsing → Searching → Composing”, then streamed results.
- Modern loader and streaming UI (animated dots, typewriter, fade‑in items).
- Reliable backend handoff: POST → sendBeacon fallback → offline queue with auto‑flush.
- Accessibility first: `aria-live` updates, sensible focus/animation defaults.

## Quick Start

- Local: open `index.html` in your browser.
- Or serve locally: `python3 -m http.server` then visit `http://localhost:8000`.
- GitHub Pages: use the Live Demo link above.

## Using AI Enhance

Each major section includes an “AI Enhance this section” card.

1) Enter a prompt describing what you want (e.g., “Add seasonal challenges and co‑op goals”).
2) Click Enhance. You’ll see:
   - Status: Parsing → Searching → Composing
   - Streaming output: a titled plan with bullets renders progressively
3) Save status appears automatically:
   - “Saved to backend ✔️” when the API is reachable
   - “Saved via beacon ✔️” if the browser’s background send succeeds
   - “Backend unreachable. Queued locally ⏳” when offline or API down (auto‑flush later)

The original AI “Dynamic Challenge Generator” demo remains in the AI section and now streams results too.

## Backend Integration

The app can POST every enhancement to your backend for storage/auditing. Configure in:

- `index.html:419` — set `CONFIG.backendURL` to your API origin (e.g., `https://api.example.com`). Defaults to `null` (no network writes).

Defaults

- `endpointPath`: `/api/ai/enhance`
- Timeout: 4000ms
- Local queue key: `aiEnhancementQueue`

Example payload

```json
{
  "id": "k3s9abc123xyz",
  "section": "gamification",
  "prompt": "Add seasonal challenges and co-op goals",
  "output": {
    "title": "Gamification Ideas",
    "bullets": [
      "Seasonal quests and streak boosts",
      "Co-op goals with shared milestones",
      "Badge sets with tiered rarity",
      "Dynamic difficulty and anti-burnout",
      "Theme: Add seasonal challenges and co-op goals"
    ]
  },
  "at": "2025-08-28T12:34:56.000Z"
}
```

Server expectations

- Accept `POST ${backendURL}/api/ai/enhance` with JSON body; respond `2xx` on success.
- If offline/unreachable: the app tries `navigator.sendBeacon` and finally queues locally, retrying periodically when `backendURL` is set.

Minimal Node/Express stub

```js
import express from 'express';
const app = express();
app.use(express.json());
app.post('/api/ai/enhance', (req, res) => {
  console.log('[AI Enhance]', req.body);
  res.status(204).end();
});
app.listen(3000, () => console.log('Listening on http://localhost:3000'));
```

## Technology

- HTML5
- Tailwind CSS (CDN)
- Vanilla JavaScript
- Chart.js

## Accessibility

- Live regions announce progress and streamed results.
- Reduced motion friendly animations.
- Clear focus states and readable contrasts.

## Development Notes

- Main entry: `index.html`
- Configure backend: `index.html:419` (`CONFIG.backendURL`)
- Contributions welcome via PRs.

