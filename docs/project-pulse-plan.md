# Project Pulse Dashboard — Implementation Plan

## Summary

Build Mona’s Project Pulse as a small, dependency-free static dashboard. The page will load project records from `app/project-data.json`, render accessible project cards, and use a polished, responsive visual design. A VS Code launch configuration will serve the app from `app/` and open `index.html` rather than a directory listing.

Repository research found the dashboard brief in `.github/project-pulse-brief.md`, the agent role definitions in `.github/agents/`, and acceptance checks in `.github/workflows/3-step.yml` and `.github/steps/3-step.md`. There is no existing app framework or application code to extend; `app/` is currently empty. The repository specifies the launch command and outcome: run `python3 -m http.server 5500` from `app/` and open `http://localhost:%s/index.html`. The workflow checks that the app files and launch JSON exist, that project data parses as JSON, and that required markup, styles, data fields, and launch-name text are present.

Use no framework or additional dependency unless the team identifies a concrete need. Keep implementation scoped to the listed files and the exercise’s static-app requirements.

## Ordered implementation steps

### 1. Confirm the design and data contracts

Before implementation, agree on the information hierarchy, responsive layout, accessible status and priority treatments, JSON shape, and CSS hooks.

- **Designer:** Propose the information hierarchy, responsive layout, accessible status/priority treatments, and CSS class hooks. Do not edit implementation files in this step.
- **Coder:** Review the agreed contracts before implementation; avoid competing design or schema decisions while coding.
- **Planner:** Use this plan to clarify phase ownership and dependencies.

Agree that the JSON document has a top-level `projects` array and that each record has `name`, `owner`, `status`, `recentActivity`, and `priority`. The brief also asks for a short contributor-friendly summary, although the exercise’s explicit field list and workflow checks do not include one. Recommended resolution: add a `summary` field to each example record and display it. Keep `status` and `priority` as readable strings; agree on sample values and ensure CSS treatments remain understandable without color alone. The page should load JSON over HTTP through the preview server rather than rely on `file://` fetches.

**Dependency:** This agreement is a prerequisite for parallel implementation. If the team chooses a different schema or design, update both implementation assignments before coding.

### 2. Implement the dashboard

After Step 1, these tasks can run in parallel because their file scopes do not overlap and the interfaces have been agreed.

**Coder owns:**

- `app/index.html`
  - Create accessible page structure with the exact visible title **Project Pulse**.
  - Reference `styles.css` and `project-data.json`.
  - Load and render project cards from the `projects` array rather than hard-code cards as the data source.
  - Give each card the `project-card` class and show the project name, owner, status, `recentActivity`, priority, and the agreed summary field.
  - Render data values as text rather than inserting them as HTML.
  - Provide useful loading feedback and clear user-facing feedback if data cannot be loaded or parsed.
- `app/project-data.json`
  - Provide valid JSON with a top-level `projects` array and at least two realistic example records.
  - Include `name`, `owner`, `status`, `recentActivity`, `priority`, and the agreed `summary` field in each record.
  - Keep values concise and consistent with the agreed status and priority treatments.
- `.vscode/launch.json`
  - Use strict JSON with no comments.
  - Add the exact configuration name **Run Project Pulse Dashboard**.
  - Serve from `${workspaceFolder}/app` using `python3 -m http.server 5500`.
  - Open `http://localhost:%s/index.html`, not the server root.
  - Select a launch type supported by the Codespace’s installed VS Code extensions/debug adapters. The brief specifies the command and browser destination but does not prescribe or verify a launch type.

**Designer owns:**

- `app/styles.css`
  - Implement the agreed visual direction, responsive layout, readable spacing and typography, card styling, and status/priority affordances.
  - Style the agreed page/card hooks, including `.dashboard` and `.project-card`.
  - Use `border-radius` and `box-shadow` for polished card styling.
  - Make status and priority distinctions perceivable without color alone; include visible keyboard focus treatment where interactive elements exist.
  - Keep styling self-contained; do not edit HTML, JSON, or launch configuration.

**Responsibilities:** The Designer leads visual and interaction design in the stylesheet; the Coder leads page structure, data loading/rendering, sample data, and runnable-app configuration. Both follow the shared contracts and stay within their assigned files.

### 3. Integrate and validate

- **Coder:** Resolve integration defects in `app/index.html`, `app/project-data.json`, or `.vscode/launch.json`.
- **Designer:** Resolve visual or responsive defects in `app/styles.css`.
- **Orchestrator:** Review the integrated result against the brief, file boundaries, and validation expectations; report remaining limitations.

Check that markup, data fields, and CSS hooks agree. Parse both JSON files and verify the launch configuration is strict JSON. Start **Run Project Pulse Dashboard** in VS Code and confirm it opens the page at the configured `index.html` URL, served from `app/` rather than the repository root. Review the page at narrow and wide viewport sizes, check keyboard navigation and readable contrast, and confirm status/priority labels are visible. Stop the preview server after validation.

If the exercise handoff is in scope, the Orchestrator records the result and validation in `docs/final-handoff.md`. This is a closeout deliverable, not a dashboard runtime dependency.

## Dependencies and parallel work

- **Sequential:** Agree on the data contract, design direction, and CSS hooks before implementation. Page and JSON field names must agree; page and stylesheet class names must agree.
- **Parallel after agreement:** Designer implements `app/styles.css` while Coder implements `app/index.html`, `app/project-data.json`, and `.vscode/launch.json`. The file scopes do not overlap.
- **Sequential after implementation:** Integration review and launch/browser validation require all assigned files to exist and work together. Fixes should retain the Designer/Coder ownership above.
- **Optional closeout:** `docs/final-handoff.md` follows successful validation; it is not required to build or launch the dashboard.

## Edge cases and risks

- Missing, malformed, or unavailable JSON: show a clear error state rather than leaving the page blank or exposing a raw exception.
- Empty `projects` array: show a helpful empty state.
- Missing or blank record fields: avoid rendering broken labels or `undefined`; validate and report the issue or use a consistent fallback.
- Unexpected status or priority values: preserve readable text and use a neutral visual treatment.
- HTML-like text in data: render as text, not markup.
- Direct `file://` use: JSON fetching may fail; use the prescribed local HTTP server.
- Port 5500 already in use: the launch may fail or reach the wrong server. Report the conflict; if changing ports, update the launch URL and ready action consistently.
- Launch adapter availability: verify the selected type in the target Codespace rather than assuming an adapter is installed.
- Design/markup drift: agree on hooks before parallel work and use an integration correction pass if needed.

## Validation expectations

- **Repository acceptance checks:** Confirm all four required files exist. Run `python3 -m json.tool app/project-data.json` and `python3 -m json.tool .vscode/launch.json`. Check the required strings/selectors: `Project Pulse`, `styles.css`, `project-data.json`, `project-card`, `.dashboard`, `.project-card`, `border-radius`, `box-shadow`, the five required data fields, the launch name, and `index.html`.
- **Functional check:** Run the named VS Code configuration and verify the browser displays the dashboard—not a directory listing—served from `app/`.
- **UI/accessibility check:** Confirm multiple cards render from JSON, required values are visible, layout works at narrow and wide widths, status/priority are understandable without color alone, and loading/error/empty states are legible.
- **Tooling limitation:** `scripts/validate-exercise.sh` checks repository/exercise structure, not dashboard behavior or launch execution. Run it as an additional repository check if appropriate, but not as a substitute for browser and launch checks.

## Open questions

1. Should `summary` be required for every project, as recommended, or omitted to follow the exercise’s narrower five-field schema literally?
2. Which exact status and priority values should sample data use? The brief requires the fields but does not define enumerations.
3. Which launch type/debug adapter is available in the target Codespace? The repository specifies the Python command and URL but does not identify or verify the adapter.
4. Is `docs/final-handoff.md` part of this dashboard implementation request, or should it remain a separate exercise closeout step?
