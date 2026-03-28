# Matrix · Probability Intelligence Engine

A heatmap-driven probability simulation engine. Input decisions, assign probabilities, explore infinite branching scenarios.

## Deploy to Render (2 minutes)

1. Push this folder to a new GitHub repo
2. Go to [render.com](https://render.com) → New → Static Site
3. Connect your GitHub repo
4. Set **Publish directory** to `.` (root)
5. Click Deploy

That's it. No build step, no dependencies.

## Local preview

Just open `index.html` in any browser. Everything runs client-side.

## Usage

- **Add event** — type any decision or event, assign a probability (1–99%), hit Enter or + Add
- **Heatmap** — tiles colour by probability (green = high confidence, red = collapse risk)
- **Branch** — click any tile to expand 6 MCMC-simulated scenarios
- **Drill** — click any scenario card to go deeper. Breadcrumb trail tracks your position
- **Polymarket** — click "fetch live" to pull prediction market signals and import them as tiles
- **Persistence** — events are saved in localStorage so they survive page refreshes

## Connecting real Polymarket data

The current Polymarket integration uses mock data (CORS blocks direct browser requests).
To connect live data, add an Express proxy:

```js
// server.js
const express = require('express');
const fetch = require('node-fetch');
const app = express();

app.get('/api/polymarket', async (req, res) => {
  const data = await fetch('https://gamma-api.polymarket.com/markets?active=true&closed=false');
  const json = await data.json();
  res.json(json.slice(0, 20));
});

app.use(express.static('.'));
app.listen(3000);
```

Then change `POLY_MOCK` in `index.html` to fetch from `/api/polymarket`.

## Roadmap ideas

- Real MCMC simulation engine (actual Markov chain math)
- Multi-user shared probability boards
- Scenario export as PDF / shareable link
- Time-series drift tracking
- Event clustering (correlated events move together)
