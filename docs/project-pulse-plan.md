# Project Pulse implementation plan

## Goal

Build Mona's Project Pulse as a small static dashboard that helps contributors see active projects, owners, status, recent activity, priority or risk, and a short contributor-friendly summary. Follow `.github/project-pulse-brief.md` and the Step 3 requirements in `.github/outline/agent-orchestration-build-your-ai-dream-team-outline.md`.

## Agent responsibilities

- **Planner:** Research the brief and repository conventions, define file ownership and dependencies, identify edge cases, and set validation expectations before implementation.
- **Orchestrator:** Coordinate the phases below, keep file scopes explicit, sequence overlapping work, and verify the integrated dashboard and handoff.
- **Designer:** Own the page's information hierarchy, semantic structure, accessibility, visual design, and responsive behavior. Design visible project cards, status badges, priority treatment, readable spacing, and `.dashboard` / `.project-card` styling hooks.
- **Coder:** Own the project data, data-to-UI integration, and launch configuration. Implement deterministic rendering and explicit error/empty states, preserve the Designer's structure and styles, and validate the runnable app.

## Ordered implementation steps

### 1. Confirm requirements and the data contract

- Review `.github/project-pulse-brief.md`, the exercise outline, and the Step 3 instructions and grading checks.
- Use a top-level `projects` array in `app/project-data.json`. Each project includes `name`, `owner`, `status`, `recentActivity`, and `priority`; include `summary` for the contributor-friendly explanation in the brief.
- Agree on how statuses and priorities are presented, and how the page handles no projects, missing values, or failed data loading.
- **Owner:** Planner proposes the contract; Designer and Coder confirm it before parallel work.
- **Dependency:** This step precedes implementation.

### 2. Define the dashboard structure and presentation

- In `app/index.html`, establish the semantic document structure, dashboard heading, project-list region, and clear hooks for cards and their fields.
- In `app/styles.css`, implement responsive project cards, readable spacing and typography, visible status and priority treatment, and accessible focus and contrast. Include `.dashboard` and `.project-card` selectors, rounded corners, and shadows as required by the exercise.
- **Owner:** Designer owns both files for the initial structure and presentation.
- **Dependency:** Step 1. The Designer can work in parallel with the Coder's data authoring once the contract is agreed.

### 3. Author representative project data

- Create `app/project-data.json` with a useful set of representative projects and all agreed fields, including `summary`.
- Keep the file valid JSON and make each status and priority unambiguous.
- **Owner:** Coder owns `app/project-data.json`.
- **Dependency:** Step 1; can proceed in parallel with Step 2.

### 4. Connect data and prepare the launch configuration

- After the Designer hands off `app/index.html`, have the Coder connect it to `app/project-data.json` using the agreed loading approach. Keep any required client-side logic in `index.html`; the planned deliverable does not add another app file.
- Render the agreed project fields and provide a visible, understandable message for empty data or a data-loading/parse failure.
- Create `.vscode/launch.json` as strict JSON with a **Run Project Pulse Dashboard** configuration. Serve from `${workspaceFolder}/app` on a deterministic local port and open `index.html`, rather than opening a directory listing. Preserve any unrelated launch entries if present.
- **Owner:** Coder owns the integration changes to `app/index.html` after the Designer's handoff, `app/project-data.json`, and `.vscode/launch.json`.
- **Dependencies:** Step 3's data contract and the Designer's Step 2 handoff. Launch configuration work can happen in parallel with Steps 2–3 once the repository's preview mechanism is confirmed.

### 5. Integrate and validate

- Review all four deliverable paths together and resolve mismatches in field names, hooks, and launch behavior.
- **Owners:** Coder validates data loading and launch; Designer validates visual hierarchy, accessibility, and responsive behavior; Orchestrator verifies the complete result.
- **Dependency:** Steps 2–4.

## File assignments

| File | Primary assignment | Scope |
| --- | --- | --- |
| `app/index.html` | Designer, then Coder | Designer creates semantic dashboard structure and stable hooks; after handoff, Coder adds data loading and rendering without changing design decisions. |
| `app/styles.css` | Designer | Responsive layout, project cards, status and priority treatment, readable spacing, and accessible visual states. |
| `app/project-data.json` | Coder | Valid representative data using the agreed `projects` contract. |
| `.vscode/launch.json` | Coder | Strict-JSON launch configuration that serves `app/` and opens `index.html`. |

## Dependencies and parallel work decisions

- Requirement review and the data contract are sequential prerequisites for implementation.
- Once the contract is agreed, Designer work on `app/index.html` and `app/styles.css` can run in parallel with Coder work on `app/project-data.json`; these are independent file scopes.
- Launch configuration can be prepared in parallel after confirming the repository's local preview mechanism; it does not depend on the finished card styling.
- Coder's data-loading changes to `app/index.html` must follow the Designer's initial markup because both assignments touch that file. Do not run those edits in parallel.
- Integrated validation is sequential and follows all implementation work.

## Risks and edge cases

- A static `fetch` of JSON needs an HTTP server; the launch configuration must serve from `app/` and open `/index.html`, not rely on opening the file directly.
- Keep the JSON property names aligned with the rendering code, including camel-case `recentActivity`.
- Show useful empty and failure states instead of silently presenting an empty dashboard if data cannot be loaded.
- Long project names, summaries, or activity text must remain readable and not overflow narrow layouts.
- Status and priority must be understandable without color alone.
- Do not add app files outside the requested scope without a demonstrated need; preserve existing launch configurations and repository patterns.

## Validation expectations

- Confirm `app/project-data.json` parses as JSON and contains a top-level `projects` array with `name`, `owner`, `status`, `recentActivity`, `priority`, and contributor-friendly `summary` values.
- Confirm `app/index.html` identifies Project Pulse, references `styles.css` and `project-data.json`, and renders project cards with status, recent activity, and priority.
- Confirm `app/styles.css` contains `.dashboard`, `.project-card`, `border-radius`, and `box-shadow`; inspect narrow and wide layouts and keyboard focus/contrast.
- Confirm `.vscode/launch.json` parses as strict JSON, includes **Run Project Pulse Dashboard**, serves from `app/`, and opens `index.html`.
- Run **Run Project Pulse Dashboard** and verify the browser shows the dashboard rather than a directory listing; check successful data rendering and the agreed empty/error behavior.
- Run the repository's relevant validation checks after implementation and fix any failures before handoff.
