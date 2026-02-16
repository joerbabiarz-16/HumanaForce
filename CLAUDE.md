# CLAUDE.md — HumanaForce AI Innovation Library

## Project Overview

HumanaForce is a **static single-page React application** showcasing Humana's AI use cases powered by Salesforce Agentforce. It serves as a proof-of-concept / sales enablement tool featuring interactive healthcare AI use case demonstrations.

**Live deployment:** Hosted on Vercel as a static site.

## Tech Stack

| Layer          | Technology                                      |
|----------------|------------------------------------------------|
| Framework      | React 18 (CDN via unpkg, not bundled)          |
| JSX Transform  | Babel Standalone (in-browser transpilation)     |
| Styling        | Vanilla CSS with CSS custom properties          |
| Fonts          | Google Fonts (Sora, IBM Plex Sans)             |
| Hosting        | Vercel (static, `@vercel/static`)              |
| Build Tools    | None — no bundler, no package.json             |
| Language       | JavaScript (no TypeScript)                     |

## Repository Structure

```
HumanaForce/
├── index.html           # Main application — all React code, CSS, and data (~991 lines)
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
App (root component)
├── Header — hero section with Humana + Salesforce dual branding
├── Navigation — sticky nav bar
├── Main Content
│   ├── Filter Section — category filter buttons
│   └── Use Cases Grid — responsive card layout
└── Modal — detail view overlay
    ├── Overview tab (default)
    ├── Video Demo tab (conditional, when hasVideo)
    └── Presentation tab (conditional, when hasPDF)
```

### State Management

Three pieces of local React state via `useState`:

```javascript
const [selectedCategory, setSelectedCategory] = useState('All');
const [selectedUseCase, setSelectedUseCase] = useState(null);
const [activeTab, setActiveTab] = useState('overview');
```

No external state management library is used.

### Data Model

Use cases are defined in a `useCasesData` array within the script. Each use case has:

```javascript
{
  id: number,
  title: string,
  category: string,          // "Claims", "Member Services", "Care Coordination", etc.
  description: string,
  businessContext: string,
  technicalApproach: string,
  impact: string,            // "High Impact", "Very High Impact", "Medium Impact"
  timeToValue: string,       // e.g., "3 months"
  roi: string,               // e.g., "275%"
  technologies: string[],    // Array of tech names
  metrics: { [key]: string },
  image: string,             // Path to card image
  hasVideo?: boolean,        // Optional video support
  videoUrl?: string,
  hasPDF?: boolean,          // Optional PDF embed
  pdfUrl?: string
}
```

**Current use cases (7):** Intelligent Claims Processing, Member Support Chatbot, Predictive Care Management, Provider Network Optimization, Automated Prior Authorization, Personalized Wellness Programs, Data Cloud Member Experience Extension.

## Design System

### Brand Colors

```css
--humana-green: #00A758;
--humana-dark-green: #006B5E;
--salesforce-blue: #00A1E0;
--salesforce-dark-blue: #032D60;
--accent-orange: #FF6B35;
--background: #F8F9FB;
--surface: #FFFFFF;
--text-primary: #1A1F36;
--text-secondary: #697386;
--border: #E3E8EF;
```

### Typography

- **Headings:** Sora (weights: 300, 400, 600, 700)
- **Body:** IBM Plex Sans (weights: 300, 400, 500, 600)

### Responsive Breakpoint

- Mobile-first at **768px**
- Grid: `repeat(auto-fill, minmax(400px, 1fr))` → `1fr` on mobile

### Animations

- `fadeIn` / `fadeInUp` / `fadeInDown` — entrance transitions
- `slideUp` — modal entrance
- Staggered delays via `animation-delay` for progressive reveal

## Development Workflow

### Local Development

No build process is needed. To develop locally:

1. Open `index.html` directly in a browser, or
2. Use any static file server (e.g., `python3 -m http.server 8000`)

### Making Changes

All application code is in `index.html`:
- **CSS:** Inside the `<style>` block (lines ~11–350)
- **React/JSX:** Inside the `<script type="text/babel">` block (lines ~350–991)
- **Data:** The `useCasesData` array within the script block

### Deployment

Push to the repository. Vercel auto-deploys from `vercel.json` configuration:
- Static build using `@vercel/static`
- SPA routing: all paths redirect to `/index.html`

### No Tests, Linting, or CI/CD

This project has:
- No test framework or test files
- No ESLint / Prettier configuration
- No CI/CD pipeline files
- No pre-commit hooks

## Conventions for AI Assistants

### Code Style

- **Variables/functions:** camelCase
- **CSS classes:** kebab-case
- **Components:** Single `App()` function component
- **State:** React hooks (`useState`) — no class components

### When Modifying the Site

1. **All edits go in `index.html`** — there is no separate JS/CSS file structure
2. **Preserve dual branding** — Humana green + Salesforce blue gradient in the header
3. **Keep CDN approach** — do not introduce npm/bundler unless explicitly requested
4. **Maintain responsive design** — test at both desktop and mobile breakpoints
5. **Follow existing animation patterns** — use the defined keyframe animations
6. **Image paths** are relative: `images/slide_N.jpg`
7. **Keep `index-backup.html` and `index-old.html`** as reference — do not delete

### Adding a New Use Case

Add an entry to the `useCasesData` array following the existing schema. Categories are dynamically extracted from the data — new categories appear automatically in the filter bar.

### Common Pitfalls

- Babel standalone transpilation means syntax errors won't show at build time — check the browser console
- Large inline scripts can be hard to navigate — use search to find specific sections
- The modal relies on `selectedUseCase` state — setting it to `null` closes the modal
- Video and PDF embeds use iframes — ensure URLs are HTTPS for mixed-content compliance
