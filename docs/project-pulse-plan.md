# Project Pulse Dashboard Implementation Plan

## Summary

Build Mona’s Project Pulse dashboard as a lightweight static web app for contributors. The dashboard will present active projects, ownership, status, recent activity, priority or risk, and concise contributor-oriented context through a polished, accessible card-based interface.

The implementation will use the existing custom-agent workflow:

- **Orchestrator** coordinates phases, delegates bounded file scopes, resolves dependencies, and integrates the result.
- **Planner** defines this implementation plan and validation strategy.
- **Designer** defines the information hierarchy, visual system, responsive behavior, and accessibility expectations.
- **Coder** implements the HTML, CSS, project data, and VS Code launch configuration.

No application framework, package manager, or existing frontend source is present. The solution should therefore remain dependency-free and run with Python’s built-in HTTP server.

## File assignments

| File | Owner | Responsibilities |
|---|---|---|
| `app/index.html` | Coder, guided by Designer | Create the complete Project Pulse page, including semantic structure, exact `Project Pulse` title, dashboard heading and summary context, project-card markup, status/priority presentation, and client-side loading/rendering of `project-data.json`. Reference `styles.css` and `project-data.json`. |
| `app/styles.css` | Designer, implemented by Coder | Define the visual design system, layout, typography, spacing, color contrast, status badges, priority treatment, responsive behavior, hover/focus states, and polished card styling. Include deterministic `.dashboard` and `.project-card` selectors, rounded corners, and shadows. |
| `app/project-data.json` | Coder, using Designer’s content/display guidance | Provide valid JSON with a top-level `projects` array. Every project object must include `name`, `owner`, `status`, `recentActivity`, and `priority`; use several realistic projects so the dashboard demonstrates multiple cards. |
| `.vscode/launch.json` | Coder | Create strict JSON with no comments. Add a configuration named `Run Project Pulse Dashboard`, run `python3 -m http.server 5500`, set `cwd` to `${workspaceFolder}/app`, and configure `serverReadyAction` to open `http://localhost:%s/index.html` rather than the directory root. |
| `docs/project-pulse-plan.md` | Planner/Orchestrator | Persist this plan. It must remain documentation only and must not contain implementation code. |

## Responsibilities

### Designer

1. Define the information hierarchy for a contributor’s first view:
   - Dashboard title and purpose.
   - Project count or overview context where useful.
   - Project cards as the primary content.
   - Clear grouping of owner, status, recent activity, and priority.
2. Specify a visual system that makes status and risk scannable without relying on color alone.
3. Define accessible semantic and interaction expectations:
   - Appropriate heading hierarchy.
   - Meaningful labels for status and priority.
   - Sufficient color contrast.
   - Visible keyboard focus styles.
   - Responsive layout for narrow and wide viewports.
   - Readable spacing and typography.
4. Recommend card, badge, grid, and empty/error-state behavior to Coder.
5. Review the integrated HTML/CSS for visual coherence and accessibility before handoff.

### Coder

1. Implement only the assigned application and launch files.
2. Build semantic, deterministic HTML in `app/index.html`.
3. Load `app/project-data.json` from the page and render visible project cards using the `project-card` class.
4. Ensure every rendered card exposes the project’s `name`, `owner`, `status`, `recentActivity`, and `priority`.
5. Handle data-loading or parsing failures explicitly in the UI rather than silently showing an empty dashboard.
6. Implement the Designer’s visual direction in `app/styles.css`, including `.dashboard`, `.project-card`, `border-radius`, `box-shadow`, responsive layout, and accessible focus/contrast treatments.
7. Create valid project data in `app/project-data.json`.
8. Create and validate `.vscode/launch.json` with the required working directory, command, launch name, and `index.html` URL.
9. Validate the implementation before reporting completion and identify any remaining browser-only checks.

## Dependencies

- `app/project-data.json` is the source of truth for project content and must define the shape consumed by `app/index.html`.
- `app/index.html` depends on `app/styles.css` for the dashboard presentation and on `app/project-data.json` for project cards.
- The page’s data-loading behavior requires an HTTP server; opening `index.html` directly from the filesystem may block `fetch` requests.
- `.vscode/launch.json` depends on the final app location and filename, especially `app/index.html`, so its launch target must be checked after the app files are created.
- The final visual review depends on the integrated HTML, CSS, and JSON rather than either Designer or Coder output in isolation.
- No external runtime dependencies are required. Python 3 and VS Code’s built-in launch support are sufficient for local preview.

## Ordered implementation phases

### Phase 1: Confirm scope and inspect the repository

**Owner:** Orchestrator and Planner  
**Files:** Read `.github/project-pulse-brief.md`, `.github/agents/*.agent.md`, repository structure; write `docs/project-pulse-plan.md`.

- Confirm that the expected deliverables are `app/index.html`, `app/styles.css`, `app/project-data.json`, and `.vscode/launch.json`.
- Preserve the existing repository conventions and avoid introducing a framework or unrelated tooling.
- Record this plan in `docs/project-pulse-plan.md`.

### Phase 2: Produce the design direction

**Owner:** Designer  
**Files:** Design guidance only initially; implementation authority remains with Coder for the assigned files.

- Define the page structure, card anatomy, typography, spacing, color/status treatments, responsive grid behavior, and accessibility requirements.
- Specify how status and priority remain understandable for users who cannot distinguish colors.
- Identify loading, empty, and data-error presentation expectations.
- Hand the design decisions to the Orchestrator and Coder.

### Phase 3: Prepare project data

**Owner:** Coder  
**File:** `app/project-data.json`

- Create a valid top-level `projects` array.
- Add multiple representative projects.
- Include `name`, `owner`, `status`, `recentActivity`, and `priority` on every project.
- Keep values concise enough for cards while retaining useful contributor context.

### Phase 4: Implement the dashboard page and styles

**Owner:** Coder, incorporating Designer guidance  
**Files:** `app/index.html`, `app/styles.css`

- Create semantic page structure with the exact title `Project Pulse`.
- Link `styles.css` and reference/load `project-data.json`.
- Render visible project cards with the exact `project-card` class.
- Display project name, owner, status, recent activity, and priority for every project.
- Include explicit loading and error states for the JSON request.
- Implement `.dashboard`, `.project-card`, responsive layout, readable spacing, status badges, priority indicators, `border-radius`, and `box-shadow`.
- Ensure the initial page is recognizable as a finished dashboard rather than a plain document.

### Phase 5: Add the runnable preview configuration

**Owner:** Coder  
**File:** `.vscode/launch.json`

- Use strict JSON with no comments.
- Add the exact configuration name `Run Project Pulse Dashboard`.
- Use `python3 -m http.server 5500`.
- Set `"cwd": "${workspaceFolder}/app"`.
- Configure `serverReadyAction` to open `http://localhost:%s/index.html`.
- Ensure the browser opens the dashboard page and not a server directory listing.

### Phase 6: Integrate and review

**Owner:** Orchestrator, Designer, and Coder  
**Files:** All four implementation files

- Compare the implementation against this plan and the Project Pulse brief.
- Designer reviews visual hierarchy, responsiveness, accessibility, and readability.
- Coder fixes implementation, data-shape, loading, or launch-configuration issues.
- Orchestrator confirms that file ownership boundaries were respected and that all required surfaces are wired together.

### Phase 7: Validate and hand off

**Owner:** Orchestrator  
**Files:** All implementation files; later documentation may record the result in `docs/final-handoff.md`.

- Run structural, syntax, and local preview checks.
- Confirm the launch configuration opens `index.html`.
- Record validation results, limitations, and the contributions of Planner, Designer, Coder, and Orchestrator in the final handoff.

## Parallel work decisions

The following work can run in parallel after Phase 1 establishes the shared requirements:

- **Designer** can define layout, accessibility, visual tokens, card anatomy, and responsive behavior in parallel with **Coder** drafting `app/project-data.json`.
- **Coder** can prepare the initial `.vscode/launch.json` in parallel with data preparation because the required server command and `app/` working directory are already known.
- Structural validation commands for JSON, CSS selectors, HTML references, and launch configuration can be prepared independently.

The following work must remain sequential:

1. The repository and brief must be inspected before design or coding assignments are finalized.
2. Designer guidance must be available before the final HTML/CSS integration review.
3. `app/project-data.json` must be finalized before confirming that the page renders every required field correctly.
4. `app/index.html` and `app/styles.css` must exist before the launch configuration can be functionally previewed.
5. All application files and `.vscode/launch.json` must be integrated before browser validation.
6. Any fixes found during validation must be completed before the final handoff is written.

Avoid assigning overlapping edits to Designer and Coder in the same phase. Designer owns design decisions; Coder owns the implementation files unless the Orchestrator explicitly reassigns a bounded change.

## Edge cases and risks

- **Direct-file loading:** `fetch("project-data.json")` may fail when `index.html` is opened with a `file://` URL. Always preview through the configured HTTP server.
- **Missing or invalid data:** Show a clear user-facing error state if the JSON cannot be loaded or parsed; do not silently render an empty dashboard.
- **Incomplete project objects:** Guard against missing field values so one malformed project does not create inaccessible or confusing output. Validation should verify all required fields are present.
- **Empty projects array:** Provide a meaningful empty-state message rather than a blank content area.
- **Long content:** Owner names, activity text, project names, or priority labels may wrap. Cards must not overflow horizontally.
- **Color dependence:** Status and risk must have text labels and accessible contrast, not color alone.
- **Small screens:** The card grid must collapse cleanly without horizontal scrolling.
- **Keyboard access:** Interactive elements, if any are added, require visible focus states and meaningful labels. Avoid making non-interactive cards appear clickable.
- **Port conflicts:** Port `5500` may already be occupied. Report the conflict explicitly and stop the previous preview server or choose an approved alternative only if the launch contract is intentionally updated.
- **Launch target mistakes:** A `cwd` of the repository root or a URL ending at `/` can expose a directory listing. Verify both the working directory and explicit `/index.html` target.
- **Strict JSON:** Comments or trailing commas in `.vscode/launch.json` and `app/project-data.json` will break parsing.

## Validation expectations

### Static validation

- Confirm all required files exist:
  - `app/index.html`
  - `app/styles.css`
  - `app/project-data.json`
  - `.vscode/launch.json`
- Validate JSON:
  - `python3 -m json.tool app/project-data.json`
  - `python3 -m json.tool .vscode/launch.json`
- Confirm `project-data.json` contains a top-level `projects` array and that every item contains:
  - `name`
  - `owner`
  - `status`
  - `recentActivity`
  - `priority`
- Confirm `index.html`:
  - Contains the exact text `Project Pulse`.
  - References `styles.css`.
  - References or loads `project-data.json`.
  - Contains or creates `project-card` elements.
  - Presents status, recent activity, and priority values.
- Confirm `styles.css` contains:
  - `.dashboard`
  - `.project-card`
  - `border-radius`
  - `box-shadow`
  - Responsive layout rules and visible focus treatment.
- Confirm `.vscode/launch.json`:
  - Contains `Run Project Pulse Dashboard`.
  - Uses `python3 -m http.server 5500`.
  - Sets `cwd` to `${workspaceFolder}/app`.
  - Uses `serverReadyAction`.
  - Opens `http://localhost:%s/index.html`.

### Runtime validation

- Start the configured launch target or equivalent command from `app/`:
  `python3 -m http.server 5500`.
- Request `http://localhost:5500/index.html` and confirm the response is the dashboard HTML, not a directory listing.
- Open the page in a browser and confirm:
  - Project Pulse is visible immediately.
  - Multiple project cards render from JSON.
  - Every card shows name, owner, status, recent activity, and priority.
  - Loading and error states do not remain visible after successful data loading.
  - The layout remains readable at narrow and wide viewport sizes.
  - Keyboard focus and text contrast are usable.
- Stop the preview server after testing.
- Report any browser-only or environment-specific limitation rather than treating an unverified check as passed.

## Open questions

- The brief requires a short contributor-friendly summary but does not define a separate JSON field for it. Unless the requirements are clarified, use the dashboard introduction and concise `recentActivity` text to provide that context while keeping the required project data fields unchanged.
- The brief does not prescribe a specific number of projects, statuses, or priority vocabulary. Use enough representative records to demonstrate the UI and keep status/priority labels consistent.
- No automated browser-testing dependency exists in the repository. Browser rendering and accessibility checks will therefore be manual unless the Orchestrator explicitly approves adding test tooling.
- The repository does not define a visual brand system for Mona’s team. Designer should choose a restrained, accessible palette and document any notable design assumptions in the final handoff.
