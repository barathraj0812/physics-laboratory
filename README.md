# SP — Digital Physics Laboratory

![Status: Planning](https://img.shields.io/badge/status-planning-orange)
![Experiment: Projectile Motion](https://img.shields.io/badge/experiment-projectile%20motion-22D3EE)
![TypeScript](https://img.shields.io/badge/TypeScript-strict-3178C6?logo=typescript&logoColor=white)
![Vite](https://img.shields.io/badge/Vite-build%20tool-646CFF?logo=vite&logoColor=white)
![Canvas 2D](https://img.shields.io/badge/rendering-Canvas%202D-0B1120)
![Tests: Vitest](https://img.shields.io/badge/tests-Vitest-6E9F18?logo=vitest&logoColor=white)
![Platform: Web](https://img.shields.io/badge/platform-web-111C2E)
![Backend: none](https://img.shields.io/badge/backend-none-lightgrey)

An interactive, browser-based educational physics laboratory. Students can change physical parameters, watch motion unfold, inspect vectors, read graphs, study the governing equations, and compare theory with results — all in a polished dark scientific workstation interface.

The first experiment is the **Projectile Motion Laboratory**.

> **Project status:** Planning and specification. The product concept and the Projectile Motion Laboratory v1 UI/UX specification are finalized. The development environment and project baseline have not yet been verified, and no feature listed below should be considered implemented until it has been built and tested.

---

## Goals

- Scientific correctness before decorative effects.
- One physics model feeding the simulation, measurements, graphs, data table, and results, so they always agree.
- Understandable for beginners by default, with deeper scientific detail available on demand.
- Responsive on desktop, tablet, and mobile.
- Accessible from the first implementation.
- Small, verifiable milestones, with tests for the math and edge cases.

## Planned v1 Features

**Simulation**
- Canvas 2D viewport with projectile, launch point, ground, axes, grid, trajectory, and launch-angle indicator
- Independently selectable overlays: velocity, acceleration, and their horizontal and vertical components
- Playback: play, pause, resume, reset, single-step, and playback speed
- Fixed physics timestep (proposed 0.01 s) with rendering scheduled independently

**Parameters** (slider plus exact numeric input, with units and validation)

| Parameter | Default | Min | Max | Unit |
|---|---|---|---|---|
| Initial speed | 20 | 0 | 100 | m/s |
| Launch angle | 45 | 0 | 90 | degrees |
| Launch height | 0 | 0 | 100 | m |
| Gravity magnitude | 9.81 | 1 | 25 | m/s² |

Changing a parameter pauses the run and recomputes from the initial conditions.

**Analysis**
- Live measurements: time, position, velocity, speed, and acceleration
- Graphs below the simulation: trajectory, position vs. time, velocity vs. time, acceleration vs. time, with a synchronized time cursor
- Data table alternative for every graph
- Typeset equations with symbol definitions, units, and assumptions
- Theoretical results: flight time, maximum height, time to apex, and horizontal range

## Physics Model

An ideal projectile under constant gravity, with air resistance neglected. Position and velocity are computed directly from time, not by numerical integration.

$$x(t) = x_0 + v_0\cos\theta \, t$$

$$y(t) = y_0 + v_0\sin\theta \, t - \tfrac{1}{2} g t^2$$

$$v_x(t) = v_0\cos\theta, \qquad v_y(t) = v_0\sin\theta - g t$$

$$a_x = 0, \qquad a_y = -g$$

Derived results:

$$t_{\mathrm{apex}} = \frac{v_0\sin\theta}{g}, \qquad H_{\max} = y_0 + \frac{(v_0\sin\theta)^2}{2g}$$

$$T = \frac{v_0\sin\theta + \sqrt{(v_0\sin\theta)^2 + 2 g y_0}}{g}, \qquad R = v_0\cos\theta \, T$$

Edge cases (zero speed, horizontal and vertical launch, launch from ground level or height, degenerate trajectories, invalid input) are handled explicitly. Undefined results are shown as such, never as `NaN` or `Infinity`.

## Design

Dark scientific theme with a cyan accent, driven by centralized CSS custom properties.

| Token | Value |
|---|---|
| Main background | `#0B1120` |
| Panel background | `#111C2E` |
| Border | `#263449` |
| Accent cyan | `#22D3EE` |
| Primary text | `#F1F5F9` |
| Secondary text | `#94A3B8` |

Layouts: persistent sidebar on desktop (1024px and up), collapsible navigation on tablet (768–1023px), and stacked controls on mobile (below 768px) with the simulation shown before secondary panels.

## Accessibility

Keyboard-operable controls with visible focus, semantic HTML, accessible labels and units, sufficient contrast, reduced-motion support, no hover-only essential information, text and table alternatives to graphs, and distinctions that do not rely on color alone.

## Tech Stack

- TypeScript (strict)
- Vite
- Semantic HTML and CSS custom properties
- Canvas 2D (simulation) and lightweight SVG graphs
- Vitest for unit tests
- Git

No large UI framework or state-management library unless a concrete requirement justifies one.

## Out of Scope for v1

Air resistance, 3D/WebGL, other physics experiments, backend or database, authentication, cloud sync, AI tutor, native Android app, and multiplayer or gamification features.

## Roadmap

Development proceeds through ordered milestones, each verified before the next begins.

- [ ] **Milestone 0 — Environment and baseline:** verify Node.js, npm, and Git; inspect the existing repository; initialize the Vite/TypeScript project if missing; confirm baseline behavior; record a Git baseline

Further milestones will be added here as they are defined.

## Getting Started

Setup instructions will be added once Milestone 0 is complete and the baseline project is verified.

## Project Documentation

The master project brief is the authoritative source of truth for requirements, locked decisions, and constraints. If a proposed change conflicts with a locked decision, the conflict should be raised before any changes are made.

## License

To be decided.
