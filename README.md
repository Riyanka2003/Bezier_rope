# Bezier_rope
PROJECT: Interactive Bézier Curve with Physics & Sensor Control

1. OVERVIEW
--------------------------------------------------------------------------------
This project simulates a rope using a Cubic Bézier curve where control points
behave as physical masses. It features a custom physics engine (Spring-Mass-Damper)
and real-time tangent visualization, built entirely in raw JavaScript/Canvas.

3. MATH & TANGENTS
--------------------------------------------------------------------------------
- Curve: Implemented manually using the Cubic Bézier formula B(t) with 4 points.
  B(t) = (1-t)³P₀ + 3(1-t)²tP₁ + 3(1-t)t²P₂ + t³P₃
- Tangents: Calculated via the first derivative B'(t). Normalized vectors are 
  drawn at intervals to visualize instantaneous direction.

3. PHYSICS MODEL
--------------------------------------------------------------------------------
Control points follow mouse input using Hooke's Law with damping:
  Force = -k * (Pos - Target) - c * Velocity

- Stiffness (k): Controls tension/snap-back.
- Damping (c): Simulates friction. Crucial for numerical stability to prevent 
  energy accumulation (instability) in the discrete integration loop.

4. IMPLEMENTATION
--------------------------------------------------------------------------------
- No external libraries used.
- Rendering: Optimized 60FPS loop using requestAnimationFrame.
- Architecture: Modularized into Vector math, PhysicsPoint objects, and Render loop.
  
5. HOW TO RUN & CONTROLS
--------------------------------------------------------------------------------
1. Click :- https://riyanka2003.github.io/Bezier_rope/
2. Move the mouse to pull the rope.
3. Use the UI controls in the top-right corner:
    - Stiffness Slider: Controls the tension (Higher = Snap back faster).
    - Damping Slider: Controls the friction (Lower = More wobble).

