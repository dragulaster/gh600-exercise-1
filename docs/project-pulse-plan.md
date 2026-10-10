# Project Pulse implementation plan

## Summary

Build a lightweight static dashboard that lets contributors quickly see active
projects, owners, status, recent activity, priority or risk, and a short summary.
Use plain HTML, CSS, and JSON, with no framework or build step. The preview
configuration must serve the `app/` directory and open `index.html`, not a
directory listing.

## Ordered implementation steps

### 1. Confirm the data and UI contract

- **Owner:** Orchestrator, with input from Designer and Coder.
- **Files:** No application files.
- Confirm the brief's required project fields: `name`, `owner`, `status`,
  `recentActivity`, and `priority`. Include a short `summary` for the
  contributor-friendly description.
- Agree on the values and labels used for status and priority so the JSON,
  rendered badges, and visual treatments stay consistent.

### 2. Create the data and visual foundation (parallel)

- **Coder — `app/project-data.json`:** Provide a top-level `projects` array
  with realistic sample records. Every record includes all agreed fields,
  including `summary`; keep values readable and useful to contributors.
- **Designer — `app/styles.css`:** Create the responsive visual system and
  dashboard styling. Use `.dashboard` and `.project-card` hooks, clear status
  badges and priority treatment, readable spacing and typography, visible
  focus states, and polished card styling such as rounded corners and shadows.
- **Dependency:** Both tasks follow the field/value contract from step 1.
  They own separate files and can proceed in parallel.

### 3. Integrate the dashboard page

- **Coder — `app/index.html`:** Build semantic, accessible dashboard markup;
  link `styles.css`; load `project-data.json`; render project cards showing
  name, owner, status, recent activity, priority, and summary.
- **Dependency:** Use the data contract and CSS hooks agreed in step 1 and
  implemented in step 2. This integration follows the parallel work so the
  page uses the actual field names and selectors.
- Handle loading, empty-project, and data-load failure states. Insert project
  text as text rather than interpreting it as HTML.

### 4. Add the runnable preview

- **Coder — `.vscode/launch.json`:** Add strict JSON for a deterministic
  **Run Project Pulse Dashboard** configuration. Serve with the working
  directory set to `${workspaceFolder}/app` and open
  `http://localhost:<port>/index.html`. Use a local static server so the page
  can fetch its JSON; do not open the HTML through `file://`.
- **Dependency:** The preview target and app directory are established by the
  brief; verify the URL opens `index.html` rather than a server directory
  listing.

### 5. Integrate and hand off

- **Owner:** Orchestrator; Designer and Coder report their changes and
  validation.
- Review the page, styles, data, and launch configuration together. Resolve
  any mismatch in field names, status/priority values, CSS hooks, or launch
  paths with the agent who owns the affected file.

## Responsibilities and file assignments

| Agent | Assigned files | Responsibility |
|---|---|---|
| Orchestrator | `docs/project-pulse-plan.md` (this plan); integration review | Coordinate phases and ownership, keep interfaces aligned, review the integrated deliverable, and report results and blockers. |
| Planner | No implementation files | Research the brief and repository, identify phases, dependencies, parallel work, risks, and validation expectations for the Orchestrator. |
| Designer | `app/styles.css` | Shape the dashboard's hierarchy, visual design, responsiveness, and accessibility; implement the polished card layout, status/priority distinctions, and focus states. |
| Coder | `app/project-data.json`, `app/index.html`, `.vscode/launch.json` | Implement deterministic sample data, accessible HTML that loads and renders the data with useful states, and a strict-JSON launch configuration that serves and opens the dashboard. |

## Dependencies and parallel work decisions

- The field names and status/priority vocabulary are a shared interface.
  Confirm them before the Designer and Coder build their separate outputs.
- After that contract is set, Designer styling and Coder data authoring can run
  in parallel because they have non-overlapping file ownership.
- Coder integrates those outputs in `app/index.html` after the CSS hooks and
  JSON shape are known.
- Coder creates `.vscode/launch.json` for the agreed app path and preview URL;
  the Orchestrator verifies it alongside the completed app.
- Integration review is sequential and follows all implementation work.

## Edge cases and risks

- JSON may be malformed, unavailable, or contain an empty `projects` array;
  show an informative error or empty state rather than a blank dashboard.
- Missing or unexpected project values should not break card rendering.
  Preserve readable labels and provide a neutral treatment for unknown
  status/priority values.
- Ensure status and priority are conveyed by text as well as color, and keep
  keyboard focus visible.
- The data fetch requires HTTP serving; opening `index.html` as a local file
  can fail due to browser fetch restrictions.
- Keep the launch command, working directory, and URL consistent so the
  configuration does not serve a directory listing or target the wrong path.

## Validation expectations

- Parse `app/project-data.json` and `.vscode/launch.json` as JSON. Confirm that
  data has a top-level `projects` array and each sample has `name`, `owner`,
  `status`, `recentActivity`, `priority`, and `summary`.
- Check `app/index.html` is semantic, references `styles.css` and
  `project-data.json`, and displays each required project field in its cards.
- Check `app/styles.css` includes `.dashboard` and `.project-card`, provides a
  responsive layout, and distinguishes statuses and priorities accessibly.
- Start the **Run Project Pulse Dashboard** configuration and verify that
  `index.html` loads over HTTP, the project cards appear, and no directory
  listing, console errors, or failed data request occurs.
- Exercise the empty-data and data-load-failure states where practical, and
  check the layout at narrow and wide viewport sizes.
- Run `git diff --check` before handoff.
