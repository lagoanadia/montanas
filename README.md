# 🏔️ Montañas del Mundo

An interactive geography game to learn the world's main mountain ranges. Tap the map where you think a range is: green if you got it, pink if you missed (it shows you where it really was). Includes a **Study** mode with a numbered legend.

## 📁 Project Structure

```
└── index.html   → Everything in one file: styles, world map data (TopoJSON), mountain list and game logic
```

External libraries loaded from CDN: **D3.js** (map projection, zoom, animations) and **TopoJSON** (land shapes).

## 🚀 How It Works

### 🎮 Play mode

1. The 15 ranges are shuffled into a queue
2. The game asks for one: *"Toca en el mapa dónde están: Alpes"*
3. On tap, it measures the distance from the tap to the range's line
4. Close enough → ✅ green + confetti and the streak 🔥 grows; otherwise ❌ the map shakes and the correct range is drawn in pink
5. At the end it shows the score (`⭐ ok/total`) with a trophy if you missed 2 or fewer

### 📖 Study mode

All ranges are drawn on the map with numbers; clicking a number or a legend chip zooms into that range.

### Mountain ranges as polylines

Each range is a list of `[longitude, latitude]` points projected with `d3.geoEquirectangular` and drawn as a smooth `d3.curveCatmullRom` line with little triangle "peaks" along it:

```js
{n:"Alpes", c:"Europa", p:[[6,44],[7,46],[10,46.6],[13,47.2],[15.5,47.5]]}
```

### Point-to-segment distance

To decide if a tap is a hit, `dist()` computes the shortest distance from the tap to every segment of the polyline (projection of the point onto the segment, clamped to `[0,1]`).

### Zoom-aware sizes

With `d3.zoom` you can pinch or scroll (Europe is small!). `escala()` divides stroke widths and font sizes by the zoom factor `K` so labels keep the same on-screen size.

## 🧠 Key Concepts

- **D3 geo projections** — turning lat/long into SVG coordinates
- **SVG paths + `getTotalLength` / `getPointAtLength`** — drawing animations and placing peaks
- **Geometry** — distance from a point to a polyline
- **Fisher–Yates shuffle** — random question order
- **CSS custom properties + `prefers-color-scheme`** — light and dark themes
- **Responsive / touch** — pinch zoom, safe-area insets, mobile layout

## 🛠️ How To Run

No build step. Open `index.html` in any browser (an internet connection is needed to load D3, TopoJSON and the fonts).
