# SP — Digital Physics Laboratory (PHYSICA)

![Status: v1 UI built](https://img.shields.io/badge/status-v1%20UI%20built-2EA043)
![Experiment: Projectile Motion](https://img.shields.io/badge/experiment-projectile%20motion-22D3EE)
![TypeScript](https://img.shields.io/badge/TypeScript-strict-3178C6?logo=typescript&logoColor=white)
![Vite](https://img.shields.io/badge/Vite-build%20tool-646CFF?logo=vite&logoColor=white)
![Canvas 2D](https://img.shields.io/badge/rendering-Canvas%202D-0B1120)
![Tests: Vitest](https://img.shields.io/badge/tests-Vitest-6E9F18?logo=vitest&logoColor=white)
![Platform: Web](https://img.shields.io/badge/platform-web-111C2E)
![Backend: none](https://img.shields.io/badge/backend-none-lightgrey)

An interactive, browser-based educational physics laboratory, presented under the **PHYSICA — Digital Laboratory** brand. Students can change physical parameters, watch motion unfold, inspect vectors, read graphs, study the governing equations, and compare theory with results — all in a polished dark scientific workstation interface.

The first experiment is the **Projectile Motion Laboratory** (Experiment 01). The page title is *Projectile Motion Laboratory — PHYSICA*.

> **Project status:** The v1 Projectile Motion Laboratory page is built and runs entirely in the browser. This README describes what the current page implements. Unit-test results and the development environment baseline have not been re-verified as part of this update; see [Roadmap](#roadmap).

---

## Goals

- Scientific correctness before decorative effects.
- One physics model feeding the simulation, measurements, graphs, data table, equations, and results, so they always agree.
- Understandable for beginners by default, with deeper scientific detail available on demand.
- Responsive on desktop, tablet, and mobile.
- Accessible from the first implementation.
- Small, verifiable milestones, with tests for the math and edge cases.

## Features (v1)

### Simulation

- Canvas 2D viewport on a coordinate grid with minor and major gridlines, labelled axes, the full trajectory path, launch point, launch-angle (θ) indicator, and a glowing projectile marker
- Auto-scaling view that fits the trajectory and keeps the projectile in frame
- Live status badge: **Ready**, **In motion**, **Paused**, or **Impact**
- Header readout of the current launch settings (speed, angle, gravity)
- Six independently selectable vector overlays under a **Vectors** toggle:
  - velocity and its horizontal and vertical components
  - acceleration and its horizontal and vertical components
  - on-screen display scales are stated: velocity 0.28 m per (m/s), acceleration 0.34 m per (m/s²)

### Playback

- **Play**, **Pause**, **Resume**, single **Step**, and **Reset** (returns time to zero)
- **Time scrubber** to seek to any moment between launch and impact
- Playback speed: 0.25×, 0.5×, 1×, 2×, 4×
- Fixed physics timestep of **0.01 s**, with rendering scheduled independently through `requestAnimationFrame`. Large frame gaps are clamped so a background tab cannot cause a jump.
- Controls disable themselves when they do not apply (for example, Play is disabled while playing, Step is disabled after impact)

### Parameters

Each parameter has a slider and an exact numeric input, with its symbol, unit, range labels, and an inline validation message.

| Parameter | Symbol | Default | Min | Max | Unit |
|---|---|---|---|---|---|
| Initial speed | v₀ | 20 | 0 | 100 | m/s |
| Launch angle | θ | 45 | 0 | 90 | degrees |
| Launch height | y₀ | 0 | 0 | 100 | m |
| Gravity magnitude | g | 9.81 | 1 | 25 | m/s² |

- Changing any parameter restarts the experiment from its launch state: time returns to zero and playback is paused.
- Out-of-range or non-numeric entries are rejected with an accessible error message and do not change the experiment.
- **Reset all to defaults** restores the parameters, playback speed, vector overlays, and the Graphs tab.

### Live measurements

Five cards update with simulation time: time, position (x, y), velocity (vₓ, vᵧ), speed (|v|), and acceleration (aₓ, aᵧ).

### Theoretical results panel

Shown beside the parameters and updated instantly: flight time, maximum height, time to apex, and horizontal range.

### Analysis

A tabbed section (Graphs, Data, Equations, Results) with a synchronized time cursor. All views are computed from the same analytical model.

| Tab | Contents |
|---|---|
| **Graphs** | Four SVG graphs — trajectory, position over time, velocity over time, acceleration over time — each with a legend, labelled axes, a cursor and point markers that follow simulation time, and a synchronized time readout. Series are distinguished by line style (solid or dashed) as well as color. |
| **Data** | A table alternative to the graphs: t, x, y, vₓ, vᵧ, aₓ, aᵧ sampled every 0.10 s, with the exact impact state included. The row nearest the current simulation time is highlighted and marked `aria-current`. |
| **Equations** | Typeset equations filled in with the current launch values: resolving the launch velocity, position, velocity and acceleration, impact time and range, and apex time and height. Includes a symbols-and-units list and model assumptions. |
| **Results** | Six result cards: flight time, maximum height, time to apex, horizontal displacement, impact velocity (components), and impact speed. A plain-language "What this means" section explains the current launch, including vertical, horizontal, and immediate-impact cases. |

## Physics Model

An ideal projectile under constant gravity, with air resistance neglected. The launch point is x₀ = 0 and the ground is y = 0. Position and velocity are computed directly from time, not by numerical integration.

$$x(t) = v_0\cos\theta \, t$$

$$y(t) = y_0 + v_0\sin\theta \, t - \tfrac{1}{2} g t^2$$

$$v_x(t) = v_0\cos\theta, \qquad v_y(t) = v_0\sin\theta - g t$$

$$a_x = 0, \qquad a_y = -g$$

Derived results:

$$t_{\mathrm{apex}} = \frac{v_0\sin\theta}{g}, \qquad H_{\max} = y_0 + \frac{(v_0\sin\theta)^2}{2g}$$

$$T = \frac{v_0\sin\theta + \sqrt{(v_0\sin\theta)^2 + 2 g y_0}}{g}, \qquad R = v_0\cos\theta \, T$$

Here R is the horizontal displacement from launch to impact at y = 0. The impact velocity and speed are evaluated at t = T.

### Edge cases

- Exactly 90° gives zero horizontal velocity, and exactly 0° gives zero vertical velocity, so no rounding noise appears from cos 90° or sin 0°.
- Zero speed at ground level gives immediate impact (T = 0), a single-point trajectory, and no apex.
- When the initial vertical velocity is zero, the apex is at launch (0.00 s).
- Launching from a height is supported for every angle, including horizontal launches.
- Time to apex is shown as "—" when it is undefined, rather than as `NaN` or `Infinity`. Tiny values near zero display as `0.00`.
- Parameters must be finite numbers within their ranges; invalid input raises a validation error and is never passed to the model.

## Layout and Responsiveness

- **Wide screens:** fixed left sidebar (232 px) with the PHYSICA brand, experiment navigation, an "Ideal model — air resistance off" status card, and a session footer. The simulation and measurements sit beside the launch-parameters panel, with the analysis section below.
- **Narrower screens:** the layout reflows at 1120 px, 900 px, and 600 px, with further adjustments at 760 px, 460 px, and 400 px. Below the sidebar breakpoint a menu button opens the navigation, which closes on link selection or the Escape key.
- A minimum page width of 320 px is supported.

## Design

Dark scientific theme with a cyan accent, driven by centralized CSS custom properties.

| Token | Value |
|---|---|
| Main background (`--bg`) | `#0B1120` |
| Panel background (`--panel`) | `#111C2E` |
| Secondary panel (`--panel-2`) | `#0E1929` |
| Border (`--border`) | `#263449` |
| Soft border (`--border-soft`) | `#1D2A3E` |
| Accent cyan (`--cyan`) | `#22D3EE` |
| Primary text (`--text`) | `#F1F5F9` |
| Secondary text (`--muted`) | `#94A3B8` |
| Faint text (`--faint`) | `#64748B` |

Typography uses DM Sans for text, DM Mono for numeric readouts, and Manrope for the brand, each with system-font fallbacks. The page does not load external font files, so the fallbacks apply unless the fonts are installed locally.

Vector and series colors are consistent across the canvas, legends, and graphs:

| Quantity | Color |
|---|---|
| Velocity / trajectory | cyan |
| Horizontal components | blue |
| Vertical velocity | violet |
| Acceleration (vertical) | amber |
| Horizontal acceleration | coral |

## Accessibility

- Semantic landmarks and headings, labelled controls, units attached to every parameter, and `aria-describedby` links to units, ranges, and errors
- Validation errors announced with `role="alert"` and `aria-invalid`
- Every graph has an `aria-label`, and the Data tab provides a full table alternative with a caption and a keyboard-focusable scroll region
- The simulation canvas has a live text description of time, position, and velocity
- The time scrubber exposes its value in seconds through `aria-valuetext`
- Analysis tabs follow the ARIA tabs pattern: arrow keys, Home, and End move between tabs
- Navigation toggle exposes `aria-expanded` and closes with Escape
- Reduced-motion media query supported
- Series are distinguished by dash pattern and labels, not by color alone
- No essential information depends on hover

## Tech Stack

- TypeScript (strict)
- Vite
- Semantic HTML and CSS custom properties
- Canvas 2D (simulation) and lightweight inline SVG graphs
- Vitest for unit tests
- Git

No large UI framework or state-management library is used. The physics model, the playback clock, the graph and table builders, and the canvas renderer are separate modules.

## Out of Scope for v1

Air resistance, 3D/WebGL, other physics experiments, backend or database, authentication, cloud sync, AI tutor, native Android app, and multiplayer or gamification features.

The sidebar lists a single experiment. Navigation to further experiments is not part of v1.

## Roadmap

Development proceeds through ordered milestones, each verified before the next begins.

- [ ] **Milestone 0 — Environment and baseline:** verify Node.js, npm, and Git; inspect the existing repository; confirm baseline behavior; record a Git baseline
- [ ] **Verification pass:** run the Vitest suite against the physics model and playback clock (including the edge cases above), and test keyboard, screen-reader, and responsive behavior on real devices

Further milestones will be added here as they are defined.

## Getting Started

The v1 page ships as a single self-contained `index.html` with its script and styles inlined. Open it in a modern browser to use the laboratory; no server or backend is required.

Development setup instructions (install, run, build, and test commands) will be added once Milestone 0 is complete and the baseline project is verified.

## Project Documentation

The master project brief is the authoritative source of truth for requirements, locked decisions, and constraints. If a proposed change conflicts with a locked decision, the conflict should be raised before any changes are made.

## License

To be decided.
