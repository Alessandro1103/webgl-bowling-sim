# webgl-bowling-sim

A 3D bowling simulation built from scratch using WebGL and JavaScript, featuring a real-time mass-spring physics engine with multiple numerical integration methods, AABB collision detection, and interactive gameplay mechanics.

---

## Demo

Open `project.html` directly in a browser — no build step or server required.

---

## Features

- **3D rendering** via raw WebGL with per-fragment Phong lighting and texture mapping
- **Mass-spring physics** engine with configurable stiffness, damping, mass and gravity
- **Multiple integration methods** (switchable in code):
  - Explicit Euler
  - Implicit Euler
  - Verlet Integration *(active by default)*
- **AABB collision detection** between the bowling ball and each pin
- **Lane boundary enforcement** — ball is constrained within the lane width
- **Restitution & friction** — bouncing and ground drag on contact
- **Power bar mechanic** — hold to charge shot strength before releasing
- **Animated camera transition** — smooth dolly into the lane on play start
- **Directional light control** — interactive light widget
- **Texture support** — distinct textures for ball, pins, and floor (toggle on/off)
- **Adjustable parameters at runtime** via sliders: time step, gravity, mass, stiffness, damping, shininess

---

## Controls

| Input | Action |
|---|---|
| `Start Play` button | Begin a round (camera animates into position) |
| `←` / `→` arrow keys | Move the ball left or right before shooting |
| `↑` arrow key | Release the shot (stops the power bar and launches the ball) |
| Mouse drag (free view) | Rotate the scene |
| Scroll | Zoom |
| Double-click a slider | Reset to default value |

---

## Physics Overview

Each object (ball, pins, floor) is a `MassSpring` instance — a deformable mesh whose vertices are connected by springs. At every time step, the simulation:

1. Accumulates forces: gravity + spring forces (Hooke's law) + damping
2. Resolves lane boundary collisions (x-axis clamp with velocity reflection)
3. Resolves object-object collisions via AABB overlap + impulse response
4. Integrates positions and velocities (Verlet by default)
5. Updates the WebGL mesh buffers and redraws the scene

Collision response uses momentum-based impulse with a `dampingFactor` of 0.85 to model energy loss on impact. A timer fires after the first pin collision to automatically end the round.

---

## Project Structure

```
project.html        # Single-file application (HTML + JS + GLSL shaders)
Textures/
  ballTEX.png
  pinTEX.png
  floorTEX.png
*.obj               # 3D mesh definitions for ball, pin, floor, skybox
```

---

## Requirements

Any modern browser with WebGL support (Chrome, Firefox, Edge, Safari). No dependencies, no npm, no build tools.

---

## Configuration

All physics parameters can be tuned live via the control panel, or changed in code inside the relevant class constructors:

```js
// Ball defaults
this.gravity    = new Vec3(0, -2, 0);
this.mass       = 0.2;
this.stiffness  = 1;
this.damping    = 1;
this.restitution = 0.8;
```

To switch integration method, edit `simTimeStep()` in the `MassSpring` class and uncomment the desired function call:

```js
// SimTimeStep(...)                // Explicit Euler
SimTimeStep_Verlet(...)            // Verlet (default)
// SimTimeStep_ImplicitEuler(...)  // Implicit Euler
```

---

## License

MIT
