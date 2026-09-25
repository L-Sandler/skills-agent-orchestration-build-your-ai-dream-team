# Project Pulse implementation plan

## Summary

Build a lightweight static dashboard that helps Mona’s contributors see multiple projects, owners, status, recent activity, priority or risk, and a short contributor-friendly summary. The app consists of HTML, CSS, and JSON, with a VS Code launch configuration that serves the `app/` directory and opens `index.html`.

The repository already has a Project Pulse brief at `.github/project-pulse-brief.md`, an empty `app/` directory, and `.vscode/tasks.json` for the exercise terminal. The launch configuration and dashboard files do not exist yet. There is no app framework, package manifest, or dashboard test suite to extend.

## Ordered implementation steps and file ownership

| Step | Owner | File assignment | Work |
|---|---|---|---|
| 1. Confirm requirements and interface | Orchestrator, informed by Planner | No files | Use the brief and this plan to agree on data fields, markup hooks, and CSS states before parallel implementation. |
| 2. Design the dashboard | Designer | **Create** `app/styles.css` | Define the responsive card layout, visual hierarchy, status and priority treatments, spacing, typography, focus states, and accessible color contrast. Include `.dashboard`, `.project-card`, `border-radius`, and `box-shadow`. |
| 3. Implement the page and project data | Coder | **Create** `app/index.html` and `app/project-data.json` | Build semantic, accessible dashboard markup; load project data and render visible cards with `name`, `owner`, `status`, `recentActivity`, and `priority`. Reference `styles.css` and `project-data.json`. |
| 4. Add the runnable preview | Coder | **Create** `.vscode/launch.json` | Add the exact configuration name **Run Project Pulse Dashboard**. Run `python3 -m http.server 5500` with `${workspaceFolder}/app` as the working directory and open `http://localhost:%s/index.html` through `serverReadyAction`. Use strict JSON without comments. |
| 5. Integrate and verify | Orchestrator coordinates; Coder validates implementation | No additional files | Check the contracts across HTML, CSS, JSON, and launch configuration; run the checks below and resolve integration issues within the assigned files. |

**Responsibilities:** The Designer owns visual and accessibility decisions and implements only the stylesheet. The Coder owns the HTML, data, and launch configuration. The Orchestrator coordinates, keeps assignments non-overlapping, and verifies the integrated result. The Planner’s plan is the implementation guide; the Planner does not implement code.

## Dependencies and parallel work

Steps 1 must finish before implementation so the Designer and Coder share agreed markup hooks, data fields, and status/priority presentation. Once those contracts are clear, the Designer can create `app/styles.css` in parallel with the Coder creating `app/index.html` and `app/project-data.json`; their file assignments do not overlap. The Coder can also prepare `.vscode/launch.json` during that phase because it has a separate file scope.

Integration and browser verification must happen after all four implementation files exist. Although implementation can run in parallel, the HTML/CSS class contract and JSON property names must remain consistent.

## Edge cases and assumptions

- The brief requires a short summary but does not list a `summary` property among the required JSON fields. **Assumption:** add a concise `summary` per project while retaining all five required fields.
- No authoritative project dataset is provided. Use clearly illustrative sample projects unless Mona supplies real project information; do not present invented data as verified.
- Handle an empty project list and a failed JSON fetch with a clear, accessible message rather than a blank dashboard.
- Render data safely as text; account for long project names, activity descriptions, owners, and status labels without breaking the layout.
- Serve the app over HTTP for preview. A `fetch()` of JSON may fail when the page is opened directly as a `file://` URL.
- The current Codespaces extension list does not include the Python debugger. Confirm that the chosen launch type can run the server in this environment; if it requires an unavailable extension, use a supported alternative or identify the extension prerequisite. Do not change `.vscode/tasks.json` as part of this work.

## Validation expectations

1. Confirm all four assigned files exist. Parse both JSON files with `python3 -m json.tool app/project-data.json` and `python3 -m json.tool .vscode/launch.json`.
2. Check that `app/project-data.json` has a top-level `projects` array, multiple sample projects, and the required `name`, `owner`, `status`, `recentActivity`, and `priority` fields for each.
3. Check that `app/index.html` has the exact title **Project Pulse**, references both companion files, and renders project cards showing status, recent activity, and priority.
4. Check that `app/styles.css` includes `.dashboard` and `.project-card`, uses rounded cards and shadows, and remains usable at narrow viewport widths and with keyboard focus.
5. Launch **Run Project Pulse Dashboard** and verify that the server runs from `app/`, the browser opens `index.html` rather than a directory listing, the JSON loads, and the rendered cards match the data. Also verify the error/empty state and stop the preview server afterward.
6. The Step 3 workflow checks the required file paths, key phrases, JSON parsing, launch name, and launch target. The Step 2 workflow checks that this plan contains the required assignments, responsibilities, dependencies, parallel-work decisions, and validation. `scripts/validate-exercise.sh` is exercise/template-level validation, not a substitute for running the dashboard.

## Open question

Should the dashboard show real project data or illustrative sample data? Until clarified, use clearly labeled sample data.
