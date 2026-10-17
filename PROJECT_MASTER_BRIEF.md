# SP — Digital Physics Laboratory
## Master Project Brief and Long-Term Source of Truth

**Document purpose:** Preserve the complete vision, product requirements, technical decisions, implementation strategy, and development constraints of the SP Digital Physics Laboratory project.

**Status:** Product concept and Projectile Motion Laboratory v1 UI/UX specification finalized. Development environment and project baseline not yet verified.

**Primary rule:** This document is the authoritative reference for the project. Do not rely on previous chat history to understand the requirements. If a future instruction conflicts with a locked decision in this document, identify the conflict before making changes.

---

# 1. Project Identity

**Project name:** SP — Digital Physics Laboratory

**Initial experiment:** Projectile Motion Laboratory

**Product type:** Interactive, browser-based educational physics laboratory.

**Primary objective:** Build a scientifically accurate, visually polished, accessible, interactive digital laboratory that allows students to explore physics experiments, manipulate physical parameters, observe motion, inspect vectors, analyze graphs, understand equations, and compare theoretical predictions with numerical results.

The application should feel like a professional scientific workstation, not a basic school-project webpage or a static collection of formulas.

The initial implementation will focus exclusively on projectile motion. The architecture should be maintainable and reusable enough to support additional physics experiments in the future, but those experiments are not part of the initial release.

## 1.1 Product principles

1. Scientific correctness takes priority over decorative effects.
2. The simulation, numerical measurements, graphs, equations, and calculated results must agree.
3. The interface must make complex physics understandable without overwhelming the user.
4. Essential controls and information must remain easy to find.
5. The application must work on desktop and adapt to tablet and mobile screens.
6. Accessibility is a requirement, not a later optional enhancement.
7. Code should be understandable, modular, tested, and maintainable.
8. Development must proceed through small, verifiable milestones.
9. Existing working code must be inspected and preserved before modifications.
10. No feature should be claimed as implemented or tested without verification.

# 2. Intended User Experience

The user should be able to open the laboratory and immediately understand:

- What experiment is running.
- Which physical parameters control it.
- How to start, pause, resume, step through, and reset the experiment.
- Where the projectile is and how its motion changes over time.
- How velocity and acceleration vectors affect the interpretation of motion.
- How position, velocity, and acceleration change over time.
- Which equations explain the observed behavior.
- How the observed results compare with theoretical predictions.

The interface should combine a clear visual simulation with deeper scientific controls and analysis that the user can reveal when needed.

The default view should be understandable to a beginner. Additional vector overlays, graphs, equations, and data should support more detailed investigation.

# 3. Locked UI/UX Decisions

These decisions are finalized. Do not change them without explicit approval.

| Decision | Locked choice |
|---|---|
| Application | Web application |
| Initial experiment | Projectile Motion Laboratory |
| Visual theme | Dark scientific theme |
| Main layout | Scientific workstation |
| Accent color | Cyan |
| Information density | Balanced |
| Simulation experience | Hybrid: clean default, optional scientific overlays |
| Vector behavior | Individually selectable vector overlays |
| Graph placement | Below the simulation |
| Initial rendering | Canvas 2D |
| Graph rendering | Lightweight reusable SVG-based graphs or an equally suitable lightweight implementation |
| Initial physics model | Analytical ideal-projectile model |
| Air resistance | Excluded from v1 |
| Backend | Not required for the initial application |
| Authentication | Excluded |
| Native Android application/APK | Excluded |
| 3D/WebGL | Excluded from v1 |
| AI tutor | Excluded from v1 |

## 3.1 Visual design system

Use centralized CSS custom properties rather than scattering colors and dimensions throughout the codebase.

Initial color tokens:

- Main background: `#0B1120`
- Panel background: `#111C2E`
- Border: `#263449`
- Accent cyan: `#22D3EE`
- Primary text: `#F1F5F9`
- Secondary text: `#94A3B8`

These colors establish the initial visual direction. Check text contrast and adjust secondary or interactive states when required for accessibility.

Design characteristics:

- Clear panel hierarchy.
- Consistent spacing.
- Restrained borders and shadows.
- Readable scientific typography.
- Distinct primary and secondary controls.
- Useful state indicators.
- Subtle transitions that respect reduced-motion preferences.
- No unnecessary decorative animation.
- No excessive gradients, glass effects, or visual clutter.
- Scientific information must remain readable against the dark background.

The name “PHYSICA” may be used as provisional interface branding. It is not a mandatory final brand name.

# 4. Application Layout

## 4.1 Desktop

For viewports at least 1024 CSS pixels wide:

- Compact top header.
- Persistent navigation sidebar.
- Large central simulation viewport.
- Parameter controls and live measurements arranged around or immediately beneath the simulation.
- Analysis section below the simulation.
- Sufficient space for graph inspection without making the simulation unnecessarily small.

## 4.2 Tablet

For viewports from 768 to 1023 CSS pixels:

- Narrow or collapsible navigation.
- Responsive parameter and measurement panels.
- Simulation remains prominent.
- Panels may stack when necessary.
- Graphs remain below the simulation.

## 4.3 Mobile

For viewports narrower than 768 CSS pixels:

- Collapsible navigation.
- Simulation appears before secondary analysis panels.
- Controls stack vertically.
- Numeric inputs remain usable.
- Graphs adapt to the available width.
- No essential controls depend exclusively on hover.
- Avoid horizontal scrolling for the main interface.

## 4.4 Main interface regions

1. **Header:** project identity, experiment title, simulation status and useful global actions.
2. **Navigation:** the current experiment and future experiment navigation structure. Do not implement future experiments.
3. **Simulation viewport:** projectile, coordinate axes, grid, trajectory and optional scientific overlays.
4. **Parameter controls:** initial speed, launch angle, launch height and gravitational acceleration.
5. **Live measurements:** current time, position, velocity and acceleration.
6. **Playback controls:** play, pause, resume, reset, single-step and playback speed.
7. **Analysis section:** tabs or equivalent navigation for Graphs, Data, Equations and Results.
8. **Status and feedback:** validation messages, simulation state and other useful contextual feedback.

# 5. Projectile Motion Laboratory

## 5.1 Scientific assumptions

The v1 experiment models an ideal projectile under constant gravitational acceleration.

Assumptions:

- Air resistance is neglected.
- Gravitational acceleration is constant.
- The horizontal axis is \(x\).
- The vertical axis is \(y\).
- The ground is represented by \(y=0\).
- Launch height is nonnegative.
- Gravity magnitude is positive.
- Angles are entered in degrees and converted to radians internally where necessary.
- Calculations retain full floating-point precision internally; rounding is applied only for display.

## 5.2 Default parameters

| Parameter | Default | Minimum | Maximum | Unit |
|---|---:|---:|---:|---|
| Initial speed | 20 | 0 | 100 | m/s |
| Launch angle | 45 | 0 | 90 | degrees |
| Launch height | 0 | 0 | 100 | m |
| Gravity magnitude | 9.81 | 1 | 25 | m/s² |

Every parameter should have:

- A slider.
- An exact numeric input.
- A visible unit.
- Clear minimum and maximum constraints.
- Validation feedback.
- A single authoritative value shared by the simulation, measurements, graphs and calculations.

Changing a parameter should pause the current run, return the projectile to the launch state and recompute the experiment. Do not silently change the launch conditions halfway through an existing trajectory.

## 5.3 Mathematical model

Let:

- \(x_0\) = initial horizontal position.
- \(y_0\) = launch height.
- \(v_0\) = initial speed.
- \(\theta\) = launch angle.
- \(g\) = positive gravitational acceleration magnitude.
- \(t\) = time after launch.

Initial velocity components:

\[
v_{0x}=v_0\cos\theta
\]

\[
v_{0y}=v_0\sin\theta
\]

Position:

\[
x(t)=x_0+v_0\cos\theta\,t
\]

\[
y(t)=y_0+v_0\sin\theta\,t-\frac12gt^2
\]

Velocity:

\[
v_x(t)=v_0\cos\theta
\]

\[
v_y(t)=v_0\sin\theta-gt
\]

Acceleration:

\[
a_x(t)=0
\]

\[
a_y(t)=-g
\]

Speed:

\[
v(t)=\sqrt{v_x(t)^2+v_y(t)^2}
\]

The simulation must derive all these values from the same physics model. Do not independently approximate the graph, the displayed measurements and the projectile position using unrelated calculations.

## 5.4 Derived results

Time to the apex, when the apex occurs during the trajectory:

\[
t_{\mathrm{apex}}=\frac{v_0\sin\theta}{g}
\]

Maximum vertical height:

\[
H_{\max}=y_0+\frac{(v_0\sin\theta)^2}{2g}
\]

Positive ground-impact time:

\[
T=\frac{v_0\sin\theta+
\sqrt{(v_0\sin\theta)^2+2gy_0}}{g}
\]

Horizontal displacement at impact:

\[
R=v_0\cos\theta\,T
\]

The implementation must define and test edge cases rather than assuming every formula produces a meaningful display value in every situation.

Important cases include:

- Zero initial speed.
- Horizontal launch.
- Vertical launch.
- Launch from ground level.
- Launch from positive height.
- Immediate impact or degenerate trajectories.
- Zero horizontal velocity.
- The apex coinciding with launch or impact.
- Invalid or out-of-range user input.

A vertical launch must not create division-by-zero errors in trajectory calculations. Results that are physically undefined should be represented explicitly rather than as `NaN` or `Infinity`.

## 5.5 Scientific interpretation

The interface should explain that:

- Horizontal velocity is constant under the ideal model.
- Vertical velocity decreases linearly with time.
- Acceleration is constant and directed downward.
- At the apex, vertical velocity is zero, but horizontal velocity is not necessarily zero.
- Complementary launch angles produce the same range only when launch and landing heights are equal under the ideal model.
- A zero-speed launch does not behave like an ordinary launched projectile.

Explanations must remain consistent with the active parameters and model assumptions.

# 6. Simulation Experience

## 6.1 Default viewport

Initially display:

- Projectile.
- Launch point.
- Ground line.
- Coordinate axes.
- Grid.
- Trajectory.
- Launch-angle indicator.
- Current simulation time.

Keep secondary scientific overlays hidden initially so that the trajectory remains visually clear.

## 6.2 Selectable vector overlays

Allow independent controls for:

- Velocity vector.
- Horizontal velocity component.
- Vertical velocity component.
- Acceleration vector.
- Horizontal acceleration component.
- Vertical acceleration component.

Each overlay must be independently selectable where scientifically meaningful. A selected overlay should be clearly distinguishable from other overlays.

The visual vector length may use a documented display scale. It must not imply that the drawn vector length is a literal measurement of magnitude unless the scale makes that relationship clear.

Avoid color-only distinctions. Use suitable labels, line styles, arrowheads and accessible control names.

## 6.3 Coordinate view

- Maintain equal physical scale on both axes by default.
- Use an adaptive grid and readable tick labels.
- Keep units visible.
- Allow zoom and pan only if implemented reliably.
- Zoom and pan must change the camera mapping, not the physical simulation.
- Resizing the browser must not change the physical trajectory.
- Do not distort the trajectory to fit the screen without clearly explaining the scaling behavior.

## 6.4 Playback

Required controls:

- Play.
- Pause.
- Resume.
- Reset.
- Advance exactly one physics step.
- Playback speed selection.

Rules:

- Pause preserves the exact current simulated state.
- Resume continues from that state.
- Reset restores default parameters, initial simulation time and default view.
- A parameter change pauses and recomputes from initial conditions.
- Playback speed changes how quickly the simulation is displayed in real time; it must not change the physical equations or results.
- Graph selection and vector visibility changes do not reset physics.
- Rendering and physics calculations must not depend on each other in a way that changes the result when the frame rate changes.

Use a fixed physics timestep initially, proposed as 0.01 seconds, with rendering scheduled independently. Handle accumulated time carefully to prevent unstable or unbounded update loops.

For the initial analytical model, calculate position and velocity directly from time rather than introducing unnecessary numerical integration error.

# 7. Measurements and Analysis

## 7.1 Live measurements

Display:

- Simulation time, seconds.
- Horizontal position, metres.
- Vertical position, metres.
- Horizontal velocity, metres per second.
- Vertical velocity, metres per second.
- Speed, metres per second.
- Horizontal acceleration, metres per second squared.
- Vertical acceleration, metres per second squared.

Do not round internal values. Format values for readability in the interface.

Analytical model outputs must not be falsely described as experimental measurements.

## 7.2 Graphs

Graphs appear **below the simulation**.

Required graphs:

1. Trajectory: vertical position versus horizontal position.
2. Horizontal and vertical position versus time.
3. Horizontal and vertical velocity versus time.
4. Horizontal and vertical acceleration versus time.

Graph requirements:

- Clearly labeled axes and units.
- Readable grid and tick labels.
- Legends.
- Appropriate automatic scaling.
- A clear indication of the selected variable.
- Cursor inspection where practical.
- A synchronized time cursor linked to the simulation.
- A data-table alternative.
- Distinct line styles if analytical and numerical results are eventually compared.

Graph inspection must use the shared simulation model and state. It must not trigger a new, inconsistent calculation path.

## 7.3 Data table

Provide a table with columns for:

- Time.
- Horizontal position.
- Vertical position.
- Horizontal velocity.
- Vertical velocity.
- Horizontal acceleration.
- Vertical acceleration.

Every column must include a clear unit or an accessible association with its unit.

CSV export is a later analysis feature, not a prerequisite for the first static UI milestone.

## 7.4 Equations

The Equations section should contain:

- Typeset equations.
- Definitions of symbols.
- Units.
- Assumptions.
- Derived quantities.
- A clear connection between equations and the current parameters.

KaTeX may be used if it provides a clear benefit without adding unnecessary complexity.

## 7.5 Results

Display:

- Flight time.
- Maximum height.
- Time to apex when applicable.
- Horizontal displacement at impact.
- Theoretical results.
- Clear descriptions of the assumptions behind those results.

Numerical integration comparison and measured numerical error belong to a later milestone. Never fabricate numerical accuracy or show a claimed error value without calculating it.

# 8. Technical Architecture

## 8.1 Initial stack

Use the following stack unless a verified technical limitation requires a documented change:

- TypeScript with strict type checking.
- Vite for the development server and production build.
- Semantic HTML.
- CSS with centralized custom properties.
- Canvas 2D for the simulation viewport.
- SVG or another lightweight, accessible graph implementation.
- Explicit, understandable application state.
- Vitest for unit tests of mathematical and other pure functions.
- Browser interaction testing appropriate to the available environment.
- Git for version control.

Use no large UI framework or state-management library unless a concrete requirement justifies it. Avoid unnecessary dependencies and animation libraries.

## 8.2 Suggested separation of responsibilities

Organize the code so that these responsibilities remain distinguishable:

1. **Physics model:** pure functions for position, velocity, acceleration and derived results.
2. **Simulation controller:** playback, pause, resume, reset, stepping and current time.
3. **Rendering layer:** coordinate transformations, grid, trajectory, projectile and overlays.
4. **Parameter interface:** validated inputs and controls.
5. **Measurements:** formatted values derived from the model.
6. **Analysis:** graph data, data table, equations and results.
7. **Application shell:** header, navigation, layout, tabs and responsive behavior.
8. **Tests:** mathematical edge cases, state transitions and cross-component consistency.

These are responsibility boundaries, not a mandate to create a separate file for every item. Prefer a simple architecture that fits the actual size of the application.

## 8.3 State consistency

There must be one authoritative set of parameters and one authoritative simulation time/state.

The following must remain synchronized:

- Projectile position.
- Velocity and acceleration measurements.
- Vector overlays.
- Trajectory and time graphs.
- Graph cursor.
- Data table.
- Derived results.
- Equation interpretations.

Changing display options must not modify the physics model. Changing the physical parameters must trigger a controlled recomputation.

## 8.4 Performance

- Avoid unnecessary per-frame DOM updates.
- Redraw only what needs to be redrawn.
- Handle canvas resizing correctly.
- Avoid background animations that serve no purpose.
- Keep dependencies minimal.
- Prevent console errors.
- Do not sacrifice scientific accuracy to produce smoother-looking motion.

# 9. Accessibility

Accessibility is required from the first implementation.

Include:

- Keyboard-operable controls.
- Visible keyboard focus.
- Semantic HTML.
- Accessible labels and units.
- Sufficient contrast.
- Reduced-motion support.
- Easy access to pause and reset.
- No hover-only essential information.
- Graph alternatives in tabular or textual form.
- Distinctions that do not rely on color alone.
- Appropriate descriptions of validation errors.
- Responsive controls that remain usable on small screens.

Test actual keyboard navigation and responsive behavior rather than assuming accessibility from appearance alone.

# 10. Scope Boundaries

## Included in v1

- Dark scientific workstation UI.
- Responsive application shell.
- Projectile-motion parameter controls.
- Analytical physics model.
- Interactive Canvas 2D simulation.
- Playback controls.
- Selectable vector overlays.
- Live measurements.
- Graphs below the simulation.
- Data table.
- Equations and scientific explanations.
- Derived theoretical results.
- Tests and validation.
- Accessibility and performance improvements.

## Excluded from v1

- Native Android APK.
- Authentication and user accounts.
- Backend and database.
- Cloud synchronization.
- AI tutor.
- Air resistance.
- Three-dimensional simulation.
- Other physics experiments.
- Multiplayer or social features.
- Unnecessary gamification.
- Features added solely to increase the feature count.

Future ideas must not displace the core requirements or be implemented without explicit approval.

# 11. Development Roadmap

Development proceeds through these milestones in order. Each milestone must be verified before the next begins.

## Milestone 0 — Environment and baseline

Objectives:

- Verify Windows environment.
- Verify Node.js and npm.
- Verify Git.
- Confirm the actual project directory.
- Inspect existing files and repository state.
- Determine whether a Vite/TypeScript project already exists.
- Initialize only what is missing.
- Run the starter project and confirm its baseline behavior.
- Record a recoverable Git baseline.

Do not implement the laboratory UI until the baseline is verified.

**Current status: Not verified.**

## Milestone 1 — Static UI

Build only the visual structure:

- Theme and design tokens.
- Header and navigation.
- Responsive workstation layout.
- Placeholder simulation viewport.
- Parameter controls.
- Measurement panels.
- Playback control layout.
- Analysis tabs below the simulation.
- Initial accessibility and responsive behavior.

At this stage, visual placeholders are acceptable. Do not pretend that the physics engine is working.

## Milestone 2 — Physics model

Implement and test:

- Initial velocity components.
- Position and velocity functions.
- Acceleration.
- Speed.
- Flight time.
- Maximum height.
- Horizontal displacement.
- Edge cases and input constraints.

Keep the mathematical model separate from the UI.

## Milestone 3 — Simulation viewport

Implement:

- Coordinate system.
- Adaptive grid.
- Projectile and launch point.
- Trajectory.
- Time display.
- Playback controller.
- Selectable vector overlays.
- Reliable resize and coordinate mapping.

## Milestone 4 — Parameter integration

Connect all parameters to the shared model.

Verify that changing any parameter consistently updates the trajectory, measurements, graphs and derived results.

## Milestone 5 — Graphs and data

Implement:

- Trajectory graph.
- Position-time graphs.
- Velocity-time graphs.
- Acceleration-time graphs.
- Graph inspection and time cursor.
- Data table.
- Synchronization with simulation time.

Add CSV export only after the core analysis is reliable.

## Milestone 6 — Equations and interpretation

Add:

- Typeset equations.
- Definitions and units.
- Assumptions.
- Derived quantities.
- Parameter-linked scientific interpretation.

## Milestone 7 — Validation

Test:

- Mathematical functions.
- Edge cases.
- Playback state transitions.
- Parameter changes.
- Synchronization across all views.
- Responsive layouts.
- Analytical versus numerical comparison if numerical integration has been implemented.

Report only verified results.

## Milestone 8 — Polish and release

Verify:

- Accessibility.
- Keyboard navigation.
- Reduced motion.
- Browser console.
- Responsive layout.
- Performance.
- Scientific consistency.
- Build and deployment behavior.
- User-facing documentation.

The first release is complete only when the core requirements work reliably, not merely when the interface looks finished.

# 12. Working Method for AI Assistants and Codex

The user has limited daily development time and prefers guided, incremental progress. AI tools may generate substantial code, but their output must be reviewed and verified.

## 12.1 Mandatory workflow

For each task:

1. State the objective.
2. Inspect the current project before changing it.
3. Identify relevant files and existing behavior.
4. Make only the changes required for the current task.
5. Preserve working code.
6. Run appropriate checks.
7. Report actual results and unresolved problems.
8. Provide one clear next action.

Prefer tasks that can be reviewed and completed in approximately 20 minutes. Some tasks may require multiple sessions.

## 12.2 Instructions for Codex

Every Codex prompt must include:

- Project context.
- Current milestone.
- Exact objective.
- Scope boundaries.
- Locked design and technical decisions relevant to the task.
- Acceptance criteria.
- Required verification commands or tests.
- A requirement to report changed files and actual test results.

Codex must:

- Inspect before editing.
- Avoid broad rewrites.
- Avoid silently changing the architecture or adding dependencies.
- Avoid implementing future milestones prematurely.
- Avoid changing locked decisions without approval.
- Never claim that a command or test passed if it was not run.
- Identify assumptions and blockers explicitly.
- Preserve user work and inspect Git status before risky operations.
- Prefer small, recoverable changes.
- Report what was changed, what was verified, what remains incomplete and the next recommended action.

## 12.3 Instructions for future ChatGPT sessions

When this brief is supplied to a new assistant:

1. Read the entire document.
2. Treat it as the source of truth for the project.
3. Determine the current milestone from the user's latest verified report and actual project state.
4. Do not assume that an earlier task succeeded merely because it was proposed.
5. Ask for missing evidence when required.
6. Continue from the current verified state.
7. Do not restart planning or redesign the project unnecessarily.
8. Do not generate the entire application in one step.
9. Keep the user informed of risks and decisions.
10. Never claim access to the user's computer or files unless the available tools actually provide that access.

# 13. Verification and Acceptance Criteria

The project is not complete merely because it builds.

## Interface

- [ ] Dark scientific theme with cyan accent.
- [ ] Workstation layout.
- [ ] Balanced information density.
- [ ] Responsive desktop, tablet and mobile layouts.
- [ ] Graphs below the simulation.
- [ ] Usable parameter and playback controls.

## Physics

- [ ] Analytical model implemented and unit-tested.
- [ ] Position, velocity and acceleration agree with the equations.
- [ ] Derived results are correct.
- [ ] Edge cases are handled.
- [ ] Parameter changes recompute consistently.
- [ ] Playback does not alter the underlying physics.
- [ ] No invalid numerical values appear in normal supported scenarios.

## Analysis

- [ ] Live measurements agree with the model.
- [ ] Vector overlays use the current simulation state.
- [ ] Graphs agree with the same model.
- [ ] Graph inspection synchronizes with simulation time.
- [ ] Data table agrees with graph values.
- [ ] Equations and results state their assumptions.

## Engineering

- [ ] TypeScript strict checks pass.
- [ ] Automated tests pass.
- [ ] Production build succeeds.
- [ ] No unresolved browser console errors.
- [ ] Keyboard navigation works.
- [ ] Reduced-motion behavior works.
- [ ] Responsive behavior is tested.
- [ ] Git history preserves meaningful, recoverable milestones.
- [ ] Documentation reflects the actual implementation.

Check items only after verification. Do not mark a requirement complete based on a design intention.

# 14. Project Status and Next Action

**Known status:**

- Product concept: defined.
- Projectile Motion Laboratory v1 UI/UX: finalized.
- Technical direction: selected.
- Development environment: not yet verified for this project.
- Project directory: not yet confirmed.
- Git repository: not yet inspected.
- Vite/TypeScript setup: not yet inspected.
- Application code: implementation has not been verified as started.

**Next action: Milestone 0, Step 1.**

Open VS Code and verify the environment using PowerShell commands that check:

- Windows version and architecture.
- Node.js version.
- npm version.
- Git version.
- Current working directory.
- Files in the current directory.

Return the complete terminal output. Use it to determine the next step. Do not install packages, initialize Vite, create application files or begin UI implementation until the environment and project directory are confirmed.

# 15. Final Guiding Principle

Build the smallest verified version of the right product, then expand it through controlled milestones.

Scientific accuracy, coherent interaction, accessibility, maintainability and verified progress matter more than raw code volume or a long feature list.

**This document preserves the project vision. Actual files, tests and Git history determine implementation status.**