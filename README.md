# Atomity Frontend Engineering Challenge

This project is my interpretation of **Option B** from the reference video, specifically the sequence shown between **0:45 and 0:55**.

I chose that section because it has a clear product story: separate cloud estates first appear as disconnected systems, then collapse into a shared optimization view, and finally narrow into a concrete cluster-level action. That arc felt like the strongest opportunity to build a section that is both visually engaging and easy to explain from a product point of view.

## Live demo

- GitHub repository: `add-your-public-repo-link-here`
- Deployed demo: `add-your-live-demo-link-here`

## What I built

I built a single-page React and TypeScript experience centered around one scroll-driven section. The section is designed as a three-beat narrative:

1. **Provider mesh**: cloud providers appear as separate nodes with their own spend and active clusters.
2. **Shared spectrum**: the focus shifts to a central resource board that represents a consolidated optimization surface.
3. **Cluster drill-down**: the final state narrows to one hotspot cluster and shows usage, requests, and estimated monthly savings.

I did not try to copy the video literally. I used it as a structural reference, then pushed the scene toward a denser and more product-like multi-cloud visualization.

## Animation approach

The section is driven by scroll progress instead of page-load animation. I used a tall section with a sticky frame so the user moves through the story while the scene stays anchored.

Framer Motion handles the motion system:

- provider nodes move through stage-based positions
- the central board scales and shifts slightly as the story changes
- metric values animate numerically
- the drill-down panel becomes more prominent in the final stage

I kept the motion intentionally restrained. The goal was to make the sequence feel controlled and readable instead of decorative. I also respected `prefers-reduced-motion` in both CSS and motion logic so the section still works when animation should be simplified.

## Tokens and styling

I used CSS variables as the source of truth for color, spacing, radius, fonts, and shadows in `src/styles/tokens.css`. A small token map in `src/lib/tokens.ts` mirrors the key values where component logic needs token references.

I chose **CSS Modules** for styling because the task only required one focused page and I wanted strong local scoping without bringing in a utility framework. The section also uses modern CSS features where they made sense:

- container queries for component-level responsiveness
- native CSS nesting
- `:has()` for parent-aware interaction states
- `color-mix()` for token-based color variation
- logical properties for layout
- `clamp()` for fluid typography and spacing

## Data fetching and caching

The content is not hardcoded. Data is fetched from JSONPlaceholder and transformed into a cloud optimization story inside `src/hooks/useOptimizationData.ts`.

Source endpoints:

- `https://jsonplaceholder.typicode.com/users`
- `https://jsonplaceholder.typicode.com/todos`

I used **TanStack Query** for caching and async state management. The query is configured with:

- `staleTime: 10 minutes`
- `gcTime: 30 minutes`
- no refetch on window focus
- no refetch on reconnect

That gives the first visit a normal loading state, but avoids unnecessary repeat requests during the stale window. The section also has explicit loading and error states.

## Project structure

```text
src/
  app/
    App.tsx
    App.module.css
  components/
    AnimatedValue.tsx
    OptimizationSection.tsx
    OptimizationSection.module.css
    ThemeToggle.tsx
    ThemeToggle.module.css
  hooks/
    useOptimizationData.ts
    useTheme.ts
  lib/
    tokens.ts
  styles/
    global.css
    tokens.css
```

## Libraries used and why

- **React** for component structure and UI composition
- **TypeScript** for stronger typing across components and transformed API data
- **Framer Motion** for scroll-linked transitions, springs, and animated values
- **TanStack Query** for caching, async state, and request lifecycle management
- **Vite** for a fast local dev loop and simple production build setup

## Decisions and tradeoffs

- I focused the submission on one section rather than building a larger marketing page. The challenge explicitly rewarded polish over scope, so I treated the section itself as the product.
- I used transformed public API data instead of trying to find a cloud-specific API. That kept the data flow realistic enough for async handling while still letting me shape the result into something product-relevant.
- I built custom provider nodes and information cards instead of using logos or external UI kits. That kept the work consistent with the "build everything yourself" requirement.
- I included a light/dark theme toggle as a bonus, but I biased the section toward the darker presentation because it supported the feature mood more strongly.

## What I would improve with more time

- refine the choreography further so the transition from provider mesh to shared spectrum feels even more directional
- add a small amount of keyboard-specific stage navigation and stronger screen reader announcements
- add tests around the API transformation logic in `useOptimizationData`
- deploy the final build and replace the placeholder links at the top of this README

## Running the project

```bash
npm install
npm run dev
npm run build
```

If you are running this on Windows PowerShell with execution-policy restrictions, `npm.cmd` may be required instead of `npm`.
