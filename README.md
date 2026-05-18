# Top 10 Formula 1 Racers of All Time

A single-page static website showcasing the greatest drivers in Formula 1 history, built with pure HTML, CSS, and vanilla JavaScript — no frameworks or build tools required.

## Preview

The site features an F1-themed dark design with red accents, animated speed lines in the hero section, and interactive driver cards with stats and biographies.

## Drivers Featured

| Rank | Driver | Nationality | Championships |
|------|--------|-------------|---------------|
| 1 | Lewis Hamilton | British | 7 |
| 2 | Michael Schumacher | German | 7 |
| 3 | Juan Manuel Fangio | Argentine | 5 |
| 4 | Ayrton Senna | Brazilian | 3 |
| 5 | Alain Prost | French | 4 |
| 6 | Sebastian Vettel | German | 4 |
| 7 | Jackie Stewart | Scottish | 3 |
| 8 | Nigel Mansell | British | 1 |
| 9 | Niki Lauda | Austrian | 3 |
| 10 | Max Verstappen | Dutch | 4 |

## Features

- Animated hero section with dynamic speed lines and checkered flag bands
- Driver cards showing wins, pole positions, podiums, and fastest laps with relative progress bars
- Scroll-reveal animations using the Intersection Observer API
- Hover effects: card lift, photo zoom, and red glow
- Fully responsive layout (desktop → tablet → mobile)
- Carbon-fiber texture background
- Google Fonts: Titillium Web + Barlow Condensed (the official F1 typefaces)

## Project Structure

```
f1-site/
├── index.html          # Entire app — markup, styles, and script
└── f1_images/          # Driver portrait photos
    ├── hamilton.jpg
    ├── schumacher.jpg
    ├── fangio.jpg
    ├── senna.jpg
    ├── prost.jpg
    ├── vettel.jpg
    ├── stewart.jpg
    ├── mansell.jpg
    ├── lauda.jpg
    └── verstappen.jpg
```

## Usage

No installation or build step needed. Open `index.html` directly in a browser:

```bash
# macOS / Linux
open index.html

# Windows
start index.html
```

Or serve it with any static file server:

```bash
npx serve .
# then visit http://localhost:3000
```

## Data

All driver data is defined in the `drivers` array inside `index.html`. Each entry contains:

```js
{
  rank: Number,
  name: String,
  nationality: String,
  years: String,           // active F1 career span
  teams: String,
  championships: Number,
  stats: {
    wins: Number,
    poles: Number,
    podiums: Number,
    fastestLaps: Number
  },
  bio: String,
  img: String              // path relative to index.html
}
```

Stat bars are normalized against Lewis Hamilton's all-time records (103 wins, 102 poles, 191 podiums, 54 fastest laps).

## License

This project is for educational and personal use. Driver images sourced from Wikimedia Commons under their respective licenses.
