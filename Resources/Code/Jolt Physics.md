---
tags:
  - software
  - utility
created: 21.04.2025
up: "[[Code]]"
---
[https://github.com/jrouwe/JoltPhysics](https://github.com/jrouwe/JoltPhysics)

A multi core friendly rigid body physics and collision detection library. Suitable for games and VR applications. Used by Horizon Forbidden West.

![[68747470733a2f2f6a726f7577652e6e6c2f6a6f6c742f486f72697a6f6e5f466f7262696464656e5f576573742e706e67.png]]
![[68747470733a2f2f696d672e796f75747562652e636f6d2f76692f707779435730794e4b4d412f687164656661756c742e6a7067.jpg]]
For more demos and [videos](https://www.youtube.com/watch?v=pwyCW0yNKMA&list=PLYXVwtOr1CBxbA50jVg2dKUQvHW_5OOom) go to the [Samples](https://github.com/jrouwe/JoltPhysics/blob/master/Docs/Samples.md) section.

## Design considerations
Why create yet another physics engine? Firstly, it has been a personal learning project. Secondly, I wanted to address some issues that I had with existing physics engines:

- Games do more than simulating physics. These things happen across multiple threads. We emphasize on concurrently accessing physics data outside of the main simulation update:
    - Sections of the simulation can be loaded / unloaded in the background. We prepare a batch of physics bodies on a background thread without locking or affecting the simulation. We insert the batch into the simulation with a minimal impact on performance.
    - Collision queries can run parallel to adding / removing or updating a body. If a change to a body happened on the same thread, the change will be immediately visible. If the change happened on another thread, the query will see a consistent before or after state. An alternative would be to have a read and write version of the world. This prevents changes from being visible immediately, so we avoid this.
    - Collision queries can run parallel to the main physics simulation. We do a coarse check (broad phase query) before the simulation step and do fine checks (narrow phase query) in the background. This way, long running processes (like navigation mesh generation) can be spread out across multiple frames.
- Accidental wake up of bodies cause performance problems when loading / unloading content. Therefore, bodies will not automatically wake up when created. Neighboring bodies will not be woken up when bodies are removed. This can be triggered manually if desired.
- The simulation runs deterministically. You can replicate a simulation to a remote client by merely replicating the inputs to the simulation. Read the [Deterministic Simulation](https://jrouwe.github.io/JoltPhysics/#deterministic-simulation) section to understand the limits.
- We try to simulate behavior of rigid bodies in the real world but make approximations. Therefore, this library should mainly be used for games or VR simulations.

## Features
- Simulation of rigid bodies of various shapes using continuous collision detection:
    - Sphere
    - Box
    - Capsule
    - Tapered-capsule
    - Cylinder
    - Tapered-cylinder
    - Convex hull
    - Plane
    - Compound
    - Mesh (triangle)
    - Terrain (height field)
- Simulation of constraints between bodies:
    - Fixed
    - Point
    - Distance (including springs)
    - Hinge
    - Slider (also called prismatic)
    - Cone
    - Rack and pinion
    - Gear
    - Pulley
    - Smooth spline paths
    - Swing-twist (for humanoid shoulders)
    - 6 DOF
- Motors to drive the constraints.
- Collision detection:
    - Casting rays.
    - Testing shapes vs shapes.
    - Casting a shape vs another shape.
    - Broadphase only tests to quickly determine which objects may intersect.
- Sensors (trigger volumes).
- Animated ragdolls:
    - Hard keying (kinematic only rigid bodies).
    - Soft keying (setting velocities on dynamic rigid bodies).
    - Driving constraint motors to an animated pose.
    - Mapping a high detail (animation) skeleton onto a low detail (ragdoll) skeleton and vice versa.
- Game character simulation (capsule)
    - Rigid body character. Moves during the physics simulation. Cheapest option and most accurate collision response between character and dynamic bodies.
    - Virtual character. Does not have a rigid body in the simulation but simulates one using collision checks. Updated outside of the physics update for more control. Less accurate interaction with dynamic bodies.
- Vehicles
    - Wheeled vehicles.
    - Tracked vehicles.
    - Motorcycles.
- Soft body simulation (e.g. a soft ball or piece of cloth).
    - Edge constraints.
    - Dihedral bend constraints.
    - Tetrahedron volume constraints.
    - Long range attachment constraints (also called tethers).
    - Limiting the simulation to stay within a certain range of a skinned vertex.
    - Internal pressure.
    - Collision with simulated rigid bodies.
    - Collision tests against soft bodies.
- Water buoyancy calculations.
- An optional double precision mode that allows large worlds.

## Supported platforms
- Windows (Desktop or UWP) x86/x64/ARM32/ARM64
- Linux (tested on Ubuntu) x86/x64/ARM32/ARM64/RISC-V64/LoongArch64/PowerPC64LE
- FreeBSD
- Android x86/x64/ARM32/ARM64
- Platform Blue (a popular game console) x64
- macOS x64/ARM64
- iOS x64/ARM64
- MSYS2 MinGW64
- WebAssembly, see [this](https://github.com/jrouwe/JoltPhysics.js) separate project.
- 