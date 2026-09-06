# Project Pulse Dashboard Implementation Plan

## Summary

Build Mona’s lightweight **Project Pulse** dashboard as a static app for contributors. The current repository is an exercise starter: `app/` does not yet contain the dashboard files, `.vscode/launch.json` does not yet exist, and the repository has no application framework or package manifest to extend. The implementation should therefore use plain HTML, CSS, JSON, and browser-native JavaScript.

The dashboard must make active projects, owners, status, recent activity, priority or risk, and contributor-friendly summaries easy to scan. It must open through the VS Code **Run Project Pulse Dashboard** configuration at `app/index.html`, not show a server directory listing.

## Repository constraints and existing patterns

- The source brief is `.github/project-pulse-brief.md`.
- Custom agent definitions are in `.github/agents/`:
  - `orchestrator.agent.md` coordinates work and integration.
  - `planner.agent.md` produces plans and identifies dependencies.
  - `designer.agent.md` owns visual hierarchy, accessibility, responsive behavior, and polished dashboard styling.
  - `coder.agent.md` implements assigned files and explicitly supports creating `.vscode/launch.json`.
- `.vscode/tasks.json` already uses strict JSON and supports the repository’s Codespaces workflow; do not modify it.
- `scripts/validate-exercise.sh` checks file existence, JSON parsing, required dashboard selectors and fields, and launch configuration content.
- The repository has no application dependencies to install. Previewing should use the existing `python3 -m http.server` command.

## File assignments

| File | Owner | Responsibilities |
|---|---|---|
| `app/index.html` | Coder, informed by Designer | Create the accessible page structure, exact `Project Pulse` title, stylesheet link, JSON data reference, inline rendering logic, project card markup, status/activity/priority presentation, and visible loading/error states. |
| `app/styles.css` | Coder implementing Designer’s direction | Define the polished visual system, including `.dashboard` and `.project-card`, responsive layout, typography, spacing, status and priority treatments, `border-radius`, `box-shadow`, focus states, and sufficient contrast. |
| `app/project-data.json` | Coder | Provide valid JSON with a top-level `projects` array. Every project must include `name`, `owner`, `status`, `recentActivity`, and `priority`; include a contributor-friendly `summary` as well. Use multiple realistic projects so the card layout is visibly demonstrated. |
| `.vscode/launch.json` | Coder | Create strict JSON with the `Run Project Pulse Dashboard` configuration, serve from `${workspaceFolder}/app`, run `python3 -m http.server 5500`, and open `http://localhost:%s/index.html` through `serverReadyAction`. |
| `docs/project-pulse-plan.md` | Planner | Preserve this implementation plan as the orchestration handoff before implementation begins. |

## Agent responsibilities

### Orchestrator

- Read this plan and `.github/project-pulse-brief.md` before delegating.
- Keep file ownership explicit and prevent Designer and Coder from making conflicting edits.
- Ask Designer for the visual, information-architecture, accessibility, and responsive decisions first.
- Pass Designer’s decisions to Coder as implementation requirements.
- Assign Coder the four implementation files listed above.
- Review the integrated result against the brief and this plan.

### Planner

- Produce and maintain the implementation plan.
- Identify the four target files, their owners, dependencies, parallel work, edge cases, and validation expectations.
- Call out that the starter repository has no existing app code to preserve or extend.
- Do not implement application code.

### Designer

- Provide the dashboard information hierarchy:
  - Page header with `Project Pulse` title and a concise contributor-oriented description.
  - A responsive project-card grid as the primary content.
  - Consistent metadata groups for owner, status, recent activity, and priority.
  - A short summary that can be read without opening a detail view.
- Define accessible status and priority treatments. Color may reinforce meaning but must not be the only indicator; badges must also contain readable text.
- Specify responsive behavior for narrow screens, keyboard focus visibility, readable line lengths, spacing, contrast, and reduced-motion-safe interactions.
- Recommend polished card styling using rounded corners, shadows, clear borders or surfaces, and predictable hover/focus affordances.
- Return design guidance to the Orchestrator; do not edit Coder-owned files during the implementation phase unless the Orchestrator explicitly assigns a later review pass.

### Coder

- Implement the static dashboard within the assigned files only.
- Use browser-native HTML, CSS, JSON, and JavaScript; do not add a framework, package manifest, build step, or external dependency.
- Make `index.html` reference both `styles.css` and `project-data.json`.
- Load the `projects` array from `project-data.json` and render visible `.project-card` elements from that data rather than duplicating project content only in HTML.
- Make the rendered UI visibly include each project’s `name`, `owner`, `status`, `recentActivity`, `priority`, and `summary`.
- Create `.vscode/launch.json` as strict JSON without comments, with the exact launch name and URL requirements from this plan.
- Validate the resulting files and report any remaining browser or launch limitation explicitly.

## Ordered implementation phases

### Phase 1: Confirm scope and inspect the starter

**Owner:** Orchestrator and Planner

1. Read `.github/project-pulse-brief.md`, the four custom agent definitions, `.vscode/tasks.json`, and `scripts/validate-exercise.sh`.
2. Confirm that `app/index.html`, `app/styles.css`, `app/project-data.json`, and `.vscode/launch.json` are new outputs rather than existing files requiring migration.
3. Treat this plan as the source of truth for ownership and acceptance criteria.

**Dependency:** Must finish before delegation so all agents receive the same constraints.

### Phase 2: Produce the design direction

**Owner:** Designer

1. Define the page hierarchy and card content order.
2. Define semantic HTML and accessible labeling expectations.
3. Define the responsive grid behavior and visual tokens.
4. Define status and priority badge conventions that remain understandable without color.
5. Return a concise implementation handoff to the Orchestrator.

**Files:** No repository implementation file is modified in this phase. This avoids a conflict with Coder’s ownership of the final app files.

**Can run in parallel:** The Designer can work in parallel with the Coder’s initial data modeling only if the Coder does not begin final HTML or CSS integration before receiving the design handoff.

### Phase 3: Create the data contract

**Owner:** Coder

1. Create `app/project-data.json`.
2. Use a top-level `projects` array with multiple project records.
3. Include these required properties on every record:
   - `name`
   - `owner`
   - `status`
   - `recentActivity`
   - `priority`
4. Include `summary` for the contributor-friendly project explanation.
5. Use consistent string values and realistic content that demonstrates active, completed, at-risk, or planning states.

**Files:** `app/project-data.json`

**Can run in parallel:** This can begin while Designer is preparing visual guidance because the required data contract is already defined by the brief. Final integration remains dependent on the data file’s shape.

### Phase 4: Implement the dashboard frontend

**Owner:** Coder, using Designer’s handoff

1. Create `app/index.html` with:
   - The exact title text `Project Pulse`.
   - A stylesheet reference to `styles.css`.
   - A reference to `project-data.json`.
   - Semantic header, main content, and project collection structure.
   - An accessible loading state and an explicit error state if the JSON cannot be loaded.
   - Rendering logic that creates one `.project-card` per project.
   - Visible owner, status, recent activity, priority, and summary content.
2. Create `app/styles.css` with:
   - A `.dashboard` selector for the main dashboard layout.
   - A `.project-card` selector for each project card.
   - Responsive grid behavior that works on mobile and wider screens.
   - `border-radius` and `box-shadow`.
   - Clear typography, spacing, badge styling, focus states, and contrast.
3. Avoid relying on a directory index, server-side rendering, or build tooling.

**Files:** `app/index.html`, `app/styles.css`

**Dependency:** Must follow the Designer’s handoff and use the data contract from Phase 3. The HTML and CSS should be implemented together because the markup hooks and styling selectors are coupled.

### Phase 5: Add the runnable preview configuration

**Owner:** Coder

1. Create `.vscode/launch.json` as strict JSON with no comments.
2. Add a configuration named exactly `Run Project Pulse Dashboard`.
3. Use the `app/` directory as the working directory through `"cwd": "${workspaceFolder}/app"`.
4. Run the deterministic command `python3 -m http.server 5500`.
5. Configure `serverReadyAction` to open `http://localhost:%s/index.html`.
6. Ensure the URL explicitly targets `index.html`, preventing a directory listing from being shown.

**Files:** `.vscode/launch.json`

**Dependency:** The launch configuration can be authored independently of the frontend, but functional preview validation must wait until `app/index.html` exists.

### Phase 6: Integrate and review

**Owner:** Orchestrator, with Coder and Designer review input

1. Review all four implementation files together.
2. Confirm the HTML data-loading behavior matches the JSON property names.
3. Confirm every CSS hook used by the HTML exists and every required visual hook is present.
4. Confirm the launch working directory and URL target are consistent with the app location.
5. Resolve any integration issue through the agent that owns the affected file.

**Dependency:** Must follow Phases 2–5. This is sequential because it validates cross-file behavior.

## Dependencies

- No npm packages, third-party libraries, or build tools are required.
- `python3` must be available for the static preview server; it is expected in the repository’s Codespaces/dev-container environment.
- `app/index.html` depends on:
  - `app/styles.css` for presentation.
  - `app/project-data.json` for project records.
- The inline rendering logic depends on the exact JSON property names.
- `.vscode/launch.json` depends on the app being served from `${workspaceFolder}/app` and must target `index.html`.
- Browser loading of `project-data.json` should be tested through the HTTP server, not by opening `index.html` directly from the filesystem, because browser security rules can block `fetch()` for `file://` URLs.
- No changes are needed to `.vscode/tasks.json`, `.devcontainer/`, workflows, or custom agent definitions.

## Parallel and sequential work decisions

### Work that can run in parallel

- Designer can prepare visual and accessibility guidance while Coder creates the initial `project-data.json`.
- Coder can draft `.vscode/launch.json` while the HTML and CSS are being implemented because it has a fixed command, working directory, port, and URL contract.
- Orchestrator can review the existing validation script while Designer and Coder work.

### Work that must be sequential

1. The Orchestrator must establish scope before delegation.
2. Designer’s decisions must reach Coder before final HTML/CSS integration.
3. The JSON data contract must be fixed before rendering logic is finalized.
4. The app files must exist before the launch preview can be run.
5. All files must be integrated before final validation and handoff.
6. Any changes to shared markup/data assumptions must be resolved by the owning agent before the Orchestrator performs the final review.

## Edge cases and error handling

- Empty or missing `projects` array: show an explicit empty-state message instead of silently rendering a blank page.
- Malformed or unavailable `project-data.json`: show a visible, contributor-friendly error message in the dashboard.
- Missing fields in a project record: use a clear fallback such as `Not provided` while preserving the card layout; do not let one incomplete record prevent other valid records from rendering.
- Long project names, owner names, activity text, or summaries: allow wrapping without horizontal scrolling or clipped content.
- Unknown status or priority values: display the text safely with a neutral visual treatment rather than assuming a fixed color mapping.
- Status and priority must remain understandable for users with color-vision deficiencies or reduced contrast.
- Keyboard users must be able to see focus indicators for any interactive elements.
- Small screens must stack or reflow cards without requiring horizontal scrolling.
- Opening the app through the launch configuration must use `/index.html`; opening the server root alone is not an acceptable result.
- The HTTP server port may already be occupied. Report the launch conflict explicitly and use the configured port consistently rather than silently changing the URL contract.
- `launch.json` must remain valid JSON; comments or trailing commas are not permitted.

## Validation expectations

### Static file validation

Run the repository’s existing checks where applicable:

- `bash scripts/validate-exercise.sh`
- `python3 -m json.tool app/project-data.json`
- `python3 -m json.tool .vscode/launch.json`

Confirm that:

- `app/index.html` exists and contains the exact `Project Pulse` title.
- `app/index.html` references `styles.css` and `project-data.json`.
- `app/index.html` contains the `project-card` hook and renders status, `recentActivity`, and priority content.
- `app/styles.css` contains `.dashboard`, `.project-card`, `border-radius`, and `box-shadow`.
- `app/project-data.json` contains a top-level `projects` array.
- Every project includes `name`, `owner`, `status`, `recentActivity`, `priority`, and `summary`.
- `.vscode/launch.json` contains `Run Project Pulse Dashboard`, the app working directory, `python3 -m http.server 5500`, `serverReadyAction`, and `index.html`.

### Runtime validation

1. Start **Run Project Pulse Dashboard** from VS Code Run and Debug.
2. Confirm the server starts on port `5500`.
3. Confirm the browser opens `http://localhost:5500/index.html`, not the server directory root.
4. Confirm the page visibly shows multiple project cards.
5. Confirm each card shows the project name, owner, status, recent activity, priority, and summary.
6. Confirm the layout remains readable at narrow and wide viewport sizes.
7. Confirm loading and data-fetch failure states are visible and understandable.
8. Stop the preview server after validation.

## Open questions

There are no blocking open questions. The brief does not prescribe exact project names, colors, typography, or status vocabulary, so Designer and Coder should choose a cohesive contributor-focused visual system while preserving the required field names, selectors, launch name, command, port, and URL contract.
