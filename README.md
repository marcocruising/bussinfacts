# bussin facts — coding agent brief

> **bussinfacts.com** — an interactive, scroll-driven infographic site for gen z.
> Clean & minimal (TikTok/Notion aesthetic). Data animates as you scroll. Three chapters: student debt, housing, dating.

---

## what this project is

A single-page website that tells data-driven stories about issues affecting gen z. Each section follows the same pattern:

1. **A big stat** — hits you immediately, animated counter on scroll
2. **One line of context** — reframes the stat
3. **An animated bar chart** — builds bar by bar as the section enters the viewport
4. **A personalisation widget** — pill buttons that trigger a personalised insight

The file `index.html` is the complete starter. It's fully functional with placeholder data. Your job is to enrich it with real verified data and optionally expand the interactivity.

---

## file structure

```
bussinfacts/
├── index.html          ← entire site (single file, no build step needed)
├── README.md           ← this file
```

No framework, no bundler, no dependencies. Vanilla HTML/CSS/JS only. Keep it that way unless adding charts (see below).

---

## design system

**Palette — three chapter accents:**

| Chapter | Light | Mid | Strong | Dark |
|---------|-------|-----|--------|------|
| debt (purple) | `#EEEDFE` | `#AFA9EC` | `#7F77DD` | `#534AB7` |
| housing (teal) | `#E1F5EE` | `#5DCAA5` | `#1D9E75` | `#0F6E56` |
| dating (coral/pink) | `#FBEAF0` | `#ED93B1` | `#D4537E` | `#993556` |

**Typography:** system sans-serif stack, weight 400 (body) / 500 (headings). Never 600 or 700. Letter-spacing `-0.02em` on large numbers.

**Borders:** `0.5px solid rgba(0,0,0,0.10)` everywhere. No box shadows.

**Border radius:** `--radius-pill` (100px) for nav/pills, `--radius-md` (12px) for cards, `--radius-sm` (8px) for metric tiles.

**Backgrounds:** white (`#ffffff`) for cards, off-white (`#f8f8f6`) for metric tiles and personalise bars.

**No gradients. No shadows. No coloured backgrounds on outer containers.**

---

## what needs enriching (all marked with `// TODO` in the HTML)

### chapter 1 — student debt & careers

**Hero stat:**
- Replace `data-target="45800"` with verified average English student debt on graduation
- Source: Student Loans Company annual report / IFS graduate debt analysis

**Bar chart — years to repay by career:**
- Replace the `data-width` percentages (visual bar fill, 0–100) and `.bar-val` text
- Careers in the chart: software engineer, nurse, teacher, social worker, artist/creative
- Need: starting salary, estimated annual repayment, resulting payoff timeline
- Source: IFS graduate outcomes, UCAS subject-level salary data, SLC repayment calculator

**Pill data (`debtData` object in JS):**
- 8 careers: software-engineer, nurse, teacher, lawyer, artist, journalist, social-worker, marketing
- Each needs: `headline` (punchy 1-liner), `body` (2–3 sentences with actual numbers)
- Include: starting salary range, monthly repayment estimate, rough payoff timeline

---

### chapter 2 — housing & rent crisis

**Hero stat:**
- Replace `data-target="9"` with verified house-price-to-salary ratio (current year)
- Source: ONS House Price Statistics, Nationwide / Halifax House Price Index

**Metric cards:**
- London avg rent (1 bed, current): verify against Zoopla / Rightmove / SpareRoom rental index
- Median grad salary UK (current): verify against HESA Graduate Outcomes / ONS ASHE

**Bar chart — rent as % of take-home by city:**
- 6 cities: London, Manchester, Bristol, Birmingham, Leeds, Glasgow
- Each needs: median 1-bed rent, median local salary, resulting % calculation
- Source: Zoopla rental market report, SpareRoom regional index, ONS regional pay data

**Pill data (`housingData` object in JS):**
- 8 cities: london, manchester, bristol, birmingham, leeds, glasgow, edinburgh, nottingham
- Each needs: `headline` (what's left after rent), `body` (rent figure, salary figure, context)

---

### chapter 3 — dating & relationships

**Hero stat:**
- Replace `data-target="63"` with verified figure for gen z relationship status / loneliness
- Source: Pew Research Center "Dating in America" / ONS loneliness survey

**Metric cards:**
- Average monthly spend on dating apps: verify (Bumble / Tinder / Sensor Tower spend data)
- Average reply rate / match conversion: verify (academic studies or app transparency reports)

**Bar chart — loneliness by age:**
- 5 age bands: 16–24, 25–34, 35–44, 45–54, 55+
- Source: ONS loneliness estimates, Campaign to End Loneliness, NHS survey data

**Pill data (`datingData` object in JS):**
- 5 options: tinder, hinge, bumble, no apps, irl only
- Each needs: `headline` + `body` with real stats on spend, match rates, satisfaction

---

## animation system (already built — don't break it)

**Scroll observer** — elements with `.animate-on-scroll` fade+slide in on intersection.

**Bar animation** — `.infographic-card` elements trigger `.bar-fill[data-width]` elements to animate their CSS width from 0 to the `data-width` value (as a percentage of the track). Bars stagger with 120ms delay between each.

**Counter animation** — `.stat-number[data-target]` elements count up from 0. Supports:
- `data-format="currency"` → `£45,800` (uses `toLocaleString`)
- `data-format="percent"` → `63%`
- `data-format="multiplier"` → `9x`
- `data-prefix` and `data-suffix` for custom formatting

**Active nav** — intersection observer highlights the correct nav pill as you scroll through chapters.

---

## potential enhancements (nice to have)

### interactive debt calculator
Replace the static pill result with a live slider:
```
[starting salary: £28,000 ────●──── £80,000]
→ At £28k you'd repay for ~12 years. Monthly: ~£45.
```
Logic: Plan 5 repayments = 9% of income above £25,000 threshold (England 2023+).

### timeline / line chart for housing
Show house price to salary ratio over time (1990 → now) as an animated line chart.
Use Chart.js (CDN: `https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.0/chart.umd.min.js`).

### add more chapters
Each chapter follows the same HTML pattern. To add a new one:
1. Copy a `.chapter` block
2. Add a new colour accent set in `:root`
3. Add a nav link
4. Populate data object in JS

### mobile polish
The layout is responsive but hasn't been tested below 375px. Check:
- `.bar-name` width (currently fixed 130px — may need to shrink)
- `.two-col` grid (stacks fine but metric card text may overflow)
- Hero font size (uses `clamp` — should be fine)

---

## sources to pull from (Cursor chat)

When you bring in the research from your other Cursor chat, map it to these exact JS objects and HTML attributes:

| Data point | Where it goes in HTML/JS |
|---|---|
| avg student debt figure | `data-target` on `.stat-number` in #debt |
| career salary + repayment data | `debtData` object in `<script>` |
| bar chart visual ratios | `data-width` on each `.bar-fill` in #debt-chart |
| house price:salary ratio | `data-target` on `.stat-number` in #housing |
| city rent % figures | `data-width` + `.bar-val` in #housing-chart |
| city-level rent + salary | `housingData` object in `<script>` |
| loneliness % by age | `data-width` + `.bar-val` in #dating-chart |
| app spend / match rates | `datingData` object in `<script>` |

---

## rules — don't change these

- No framework. No bundler. No npm. Single HTML file.
- Don't add inline gradients, box shadows, or coloured page backgrounds.
- Keep all font weights at 400 or 500 only.
- Don't change the colour palette — it maps to the nav and tags throughout.
- Borders are always `0.5px solid rgba(0,0,0,0.10)` — not 1px, not solid black.
- The `observer` and animation logic is in the last `<script>` tag. Don't split it out.
- All `// TODO` comments are data placeholders — replace the values, not the structure.
