# CLAUDE.md — HumanaForce AI Use Case Library

## Project Overview

HumanaForce is a **static single-page React application** serving as a Salesforce AI Use Case Library for Humana. Inspired by [agentforge.tools](https://agentforge.tools), it provides a browsable collection of healthcare-focused AI use cases with business context, technical guidance, metrics, video demos, and multi-dimensional filtering.

**Live deployment:** Hosted on Vercel as a static site.

## Tech Stack

| Layer          | Technology                                      |
|----------------|------------------------------------------------|
| Framework      | React 18 (CDN via unpkg, not bundled)          |
| JSX Transform  | Babel Standalone (in-browser transpilation)     |
| Styling        | Vanilla CSS with CSS custom properties          |
| Fonts          | Google Fonts (Inter)                           |
| Hosting        | Vercel (static, `@vercel/static`)              |
| Build Tools    | None — no bundler, no package.json             |
| Language       | JavaScript (no TypeScript)                     |

## Repository Structure

```
HumanaForce/
├── index.html           # Main application — all React code, CSS, and data
├── index-backup.html    # Backup version of the site
├── index-old.html       # Previous version with different styling
├── vercel.json          # Vercel deployment configuration (static SPA routing)
├── presentation.pdf     # 13-page presentation document
├── images/              # Image assets (slide_2.jpg through slide_10.jpg)
│   └── slide_*.jpg      # 9 JPG images used as use-case card images
└── CLAUDE.md            # This file
```

## Architecture

### Single-File SPA

The entire application lives in `index.html`. This is intentional — the project prioritizes simplicity and shareability over traditional enterprise patterns.

- **No build step** — open the file directly or serve statically
- **No npm dependencies** — React and Babel loaded from CDN
- **No server-side logic** — purely client-side rendering

### React Component Structure

```
App (root)
├── TopNav — sticky top navigation bar with brand + links
├── Hero — gradient hero section with stats counters
├── FilterBar — sticky search + dropdown filters (Department, Cloud, Impact)
│   └── Dropdown — reusable dropdown filter component
├── Main Grid — responsive card grid
│   └── UseCaseCard — individual use case card
├── Footer — site footer
└── DetailModal — tabbed detail overlay (Overview, Technical, Metrics, Video, PDF)
```

### State Management

Five pieces of local React state via `useState`:

```javascript
const [search, setSearch] = useState('');       // Text search query
const [department, setDepartment] = useState('All');  // Department filter
const [cloud, setCloud] = useState('All');      // Salesforce Cloud filter
const [impact, setImpact] = useState('All');    // Impact level filter
const [selectedUC, setSelectedUC] = useState(null);   // Selected use case for modal
```

Filtering is memoized with `useMemo` for performance.

### Data Model

Use cases are defined in a `useCasesData` array within the script. Each use case has:

```javascript
{
  id: number,
  title: string,
  department: string,      // "Claims", "Member Services", "Care Coordination", etc.
  cloud: string,           // "Health Cloud", "Service Cloud", "Data Cloud", etc.
  description: string,
  businessContext: string,
  technicalApproach: string,
  impact: string,          // "High", "Very High", "Medium"
  timeToValue: string,     // e.g., "3 months"
  roi: string,             // e.g., "275%"
  technologies: string[],  // Array of Salesforce tech names
  metrics: { [value]: label },  // Key = metric value, Value = metric description
  isNew: boolean,          // Shows "New" badge
  hasVideo?: boolean,
  videoUrl?: string,
  hasPDF?: boolean,
  pdfUrl?: string,
  image?: string           // Optional card image path
}
```

**Departments (7):** Claims, Member Services, Care Coordination, Provider Networks, Data & Analytics, Compliance, Sales

**Salesforce Clouds (7):** Health Cloud, Service Cloud, Experience Cloud, Marketing Cloud, Data Cloud, Tableau, Sales Cloud

**Current use cases: 16**

## Design System

### Brand Colors

```css
--humana-green: #00A758;
--humana-dark-green: #006B5E;
--humana-light-green: #E8F5EE;
--salesforce-blue: #00A1E0;
--salesforce-dark-blue: #032D60;
--salesforce-light-blue: #E8F4FD;
--accent-orange: #FF6B35;
--background: #F8FAFB;
--surface: #FFFFFF;
--text-primary: #111827;
--text-secondary: #6B7280;
--text-tertiary: #9CA3AF;
```

### Typography

- **Font:** Inter (weights: 300, 400, 500, 600, 700, 800)

### Impact Level Color Coding

- **Very High:** Green badge (`#DCFCE7` bg, `#166534` text)
- **High:** Amber badge (`#FEF3C7` bg, `#92400E` text)
- **Medium:** Indigo badge (`#E0E7FF` bg, `#3730A3` text)

### Responsive Breakpoint

- Mobile-first at **768px**
- Grid: `repeat(auto-fill, minmax(380px, 1fr))` → `1fr` on mobile

### Animations

- `fadeIn` / `fadeInUp` / `fadeInDown` — entrance transitions
- `slideUp` — modal entrance
- `pulse` — hero badge dot
- Staggered card animations via `animation-delay`

## Development Workflow

### Local Development

No build process is needed. To develop locally:

1. Open `index.html` directly in a browser, or
2. Use any static file server (e.g., `python3 -m http.server 8000`)

### Making Changes

All application code is in `index.html`:
- **CSS:** Inside the `<style>` block
- **React Components:** Inside the `<script type="text/babel">` block
- **Data:** The `useCasesData` array within the script block

### Deployment

Push to the repository. Vercel auto-deploys from `vercel.json` configuration.

### No Tests, Linting, or CI/CD

This project has no test framework, linting, or CI/CD pipeline.

## Conventions for AI Assistants

### Code Style

- **Variables/functions:** camelCase
- **CSS classes:** kebab-case (BEM-like naming: `.card-title`, `.modal-head-metric`)
- **Components:** Named function components (`function App()`, `function UseCaseCard()`)
- **State:** React hooks (`useState`, `useMemo`, `useEffect`, `useRef`)
- **CSS custom properties** for all colors and design tokens

### When Modifying the Site

1. **All edits go in `index.html`** — there is no separate file structure
2. **Preserve dual branding** — Humana green + Salesforce blue throughout
3. **Keep CDN approach** — do not introduce npm/bundler unless explicitly requested
4. **Maintain responsive design** — test at both desktop and mobile breakpoints
5. **Follow existing animation patterns** — use the defined keyframe animations
6. **Keep `index-backup.html` and `index-old.html`** as reference — do not delete
7. **Use CSS custom properties** — never hardcode colors directly

### Adding a New Use Case

Add an entry to the `useCasesData` array following the schema above. Departments, clouds, and impacts are dynamically extracted — new values appear automatically in filters.

### Adding a New Filter Dimension

1. Add the field to the use case data model
2. Create a `useState` for the filter
3. Add the filter to the `useMemo` filtering logic
4. Add a `<Dropdown>` component in the filter bar

### Common Pitfalls

- Babel standalone transpilation means syntax errors won't show at build time — check the browser console
- Dropdowns use a `mousedown` event listener for click-outside-to-close — test this behavior
- The modal locks body scroll on open (`overflow: hidden`) and restores on close
- Video and PDF embeds use iframes — ensure URLs are HTTPS for mixed-content compliance
- The `metrics` object uses the metric value as the key and description as the value (reversed from typical patterns)
