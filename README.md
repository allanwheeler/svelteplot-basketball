# svelteplot-basketball

A SveltePlot project that visualizes shot data for Stephen Curry and LeBron James. It's inspired by the R community and Observable-based projects (Example: [NBA Shot Charts](https://tanyaviz.com/projects/nba-shot-chart/)).

The court is drawn programmatically using SveltePlot's `<CustomMark>` and the `usePlot()` hook. The component reads the plot scales (e.g., `plot.scales.x.fn` / `plot.scales.y.fn`) to convert court coordinates to SVG positions, then draws lines, arcs, rects, and circles (baseline, sidelines, three-point arc, paint, rim, free-throw circle, etc.).

Example pattern:

```svelte
<CustomMark>
  {#snippet marks()}
    {@const xScale = plot.scales.x.fn}
    {@const yScale = plot.scales.y.fn}
    <!-- draw SVG shapes using xScale(...) and yScale(...) -->
  {/snippet}
</CustomMark>
```

---

## Quick start

### Prerequisites

- Node 18+
- pnpm (recommended; this repo includes `pnpm-lock.yaml`). You can use npm instead if you prefer.

### Install & run locally (pnpm)

```bash
pnpm install
pnpm dev
```

Open: `http://localhost:5173` (Vite default)

(Using npm: `npm install` → `npm run dev`)

### Build & preview

```bash
pnpm run build
pnpm run preview
```

---
