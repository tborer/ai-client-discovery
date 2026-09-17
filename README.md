# AI Client Discovery Simulator

A single-page, zero-build web app that turns a three-question discovery
conversation into a formatted AI strategy brief. A user picks their industry,
their primary business goal, and the AI technology they are considering; the
app instantly composes a summary statement, a sample user story, a three-month
implementation roadmap, and a theoretical ROI metric tailored to those choices.

It is designed as a sales and consulting aid — something to put in front of a
prospective client to frame an AI initiative in concrete terms within seconds,
without a backend, an API key, or a model call. All content is generated from
deterministic templates defined in the page itself.

## Live demo

The site deploys automatically to GitHub Pages from `main`
(see `.github/workflows/deploy.yml`):
<https://tborer.github.io/ai-client-discovery/>

## Features

### Guided discovery wizard
- **One question at a time.** Three steps — Industry, Primary Goal, AI
  Technology — each presented on its own screen so the flow never feels like a
  form.
- **Progress bar with step indicators.** A numbered three-dot tracker fills in
  as the user advances, with completed steps switching to a checkmark.
- **Breadcrumb chips.** Earlier answers stay visible as icon-labeled chips above
  the current question, so the user always sees the context of their brief.
- **Auto-advance on select.** Choosing an option moves to the next step after a
  brief confirmation pause — no "Next" button to click.
- **Back navigation and Start Over.** Users can step backward at any point, and
  reset the whole session from the results view.
- **Emoji iconography.** Every industry, goal, and technology has its own icon
  in both the option cards and the breadcrumb chips.

### Discovery inputs
- **8 industries:** Healthcare, Logistics, Fintech, Manufacturing,
  Environmental, Non-Profit, E-commerce & Retail, Media & Entertainment.
- **6 business goals:** reduce operational costs, automate manual workflows,
  drive new revenue streams, ensure regulatory compliance, improve data-driven
  decision making, enhance customer personalization & retention.
- **5 AI technologies:** Generative AI (LLMs), Conversational AI (NLP),
  Computer Vision, Predictive Machine Learning, AI Workflow Automation.

### Generated strategy brief
- **Strategy summary.** A one-sentence initiative statement combining the
  selected technology, goal, and industry.
- **Sample user story.** A properly formed "As a… I want to… so that I can…"
  story that maps the industry to a realistic persona (e.g. Clinical Operations
  Director, Supply Chain Manager) and to the data that persona works with
  (patient records, shipment data, production telemetry, and so on).
- **Mock 3-month roadmap.** Three phased cards — Strategy & Data
  Infrastructure, Model Training & MVP Build, and UAT, Optimization & Launch —
  where the first two months are specific to the chosen technology and the third
  is framed around how the chosen goal will be validated.
- **Theoretical ROI metric.** A benchmark-style projection matched to the
  selected goal, with a disclaimer noting that estimates are illustrative.
- **Animated results view.** The brief fades up into place and the page scrolls
  back to the top when generated.

### Design and theming
- **Centralized theme file.** `theme.css` holds every color, gradient, shadow,
  and surface style as CSS custom properties, so the entire look can be
  rebranded by editing one file.
- **Tailwind mapped to the theme.** The Tailwind CDN config aliases its color
  scale (`brand`, `page`, `card`, `ink`, …) onto those CSS variables, so utility
  classes and component classes stay in sync.
- **Current palette** is drawn from ninetwothree.co: turquoise primary with a
  purple-to-pink signature gradient used on the header and the strategy brief
  bar.
- **Responsive layout.** Two-column option grids and a three-column roadmap
  collapse cleanly on small screens.

### Build and deployment
- **No build step.** A static `index.html` plus `theme.css`. Tailwind and
  Alpine.js load from CDNs; open the file in a browser and it runs.
- **Automatic GitHub Pages deploy.** Pushes to `main` publish the site via the
  official `actions/deploy-pages` workflow. The workflow can also be run
  manually with `workflow_dispatch`.

## Project structure

```
index.html              Full application — markup plus the Alpine.js simulator()
                        component that holds all wizard state and brief templates
theme.css               Theme tokens (CSS custom properties) and component styles
.github/workflows/      GitHub Pages deployment workflow
  deploy.yml
```

## Running locally

No dependencies and no build. Either open `index.html` directly in a browser, or
serve the directory:

```bash
python3 -m http.server 8000
# then visit http://localhost:8000
```

An internet connection is needed on first load so the Tailwind, Alpine.js, and
Inter font CDNs can be fetched.

## Tech stack

| Purpose | Tool |
| --- | --- |
| Reactivity / state | Alpine.js 3 (CDN) |
| Utility styling | Tailwind CSS (CDN, browser config) |
| Theming | Plain CSS custom properties in `theme.css` |
| Typography | Inter via Google Fonts |
| Hosting | GitHub Pages |

## Customizing

- **Change the branding:** edit the variables at the top of `theme.css`.
- **Add an industry, goal, or technology:** add the label to the matching array
  in the `simulator()` function in `index.html`, add an entry to the `icons` map,
  and add matching entries to the template maps used by `buildUserStory()`,
  `buildRoadmap()`, and `buildROI()`. Every lookup has a sensible fallback, so a
  new option still produces a complete brief if a template is missed.
- **Reword the generated brief:** all copy lives in the `build*` methods at the
  bottom of `index.html`.
