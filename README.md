# Bezier_rope
PROJECT: Interactive Bézier Curve with Physics & Sensor Control

1. OVERVIEW
--------------------------------------------------------------------------------
This project implements a cubic Bézier curve simulation where the control points
behave like physical masses attached to springs. The simulation runs in a web 
browser using HTML5 Canvas and raw JavaScript. It demonstrates mastery of:
   - Manual Bézier math (no external graphics libraries).
   - Vector mathematics for tangent computation.
   - Spring-damper physics for natural motion.
   - Real-time rendering and user interaction loops.


2. MATHEMATICAL MODEL (BÉZIER CURVE)
--------------------------------------------------------------------------------
I implemented the Cubic Bézier equation manually to draw the curve point-by-point.
The curve B(t) is defined by 4 points: P0, P3 (Fixed Anchors) and P1, P2 (Dynamic).

Formula Used:
   B(t) = (1-t)³P0 + 3(1-t)²tP1 + 3(1-t)t²P2 + t³P3

   Where 0 <= t <= 1

Implementation Detail:
   The curve is approximated by iterating 't' from 0.0 to 1.0 in steps of 0.01.
   I store the P0..P3 coordinates in custom Vector objects and calculate the 
   weighted sum manually for each segment using the HTML5 'lineTo' command.


3. TANGENT VISUALIZATION (DERIVATIVE)
--------------------------------------------------------------------------------
To visualize the "flow" of the rope, I calculate the instantaneous slope of the
curve by taking the first derivative with respect to t:

   B'(t) = 3(1-t)²(P1-P0) + 6(1-t)t(P2-P1) + 3t²(P3-P2)

This vector is normalized to unit length and drawn as red lines at 10 distinct
intervals along the curve. This visually confirms the curvature direction updates
correctly as the control points move.


4. PHYSICS ENGINE & STABILITY
--------------------------------------------------------------------------------
The control points (P1, P2) are not directly mapped to the mouse. Instead, they 
follow a "target" position based on the mouse coordinates using a mass-spring-damper 
system.

Motion Equation:
   Acceleration = Force / Mass (Mass assumed to be 1)
   Force_total  = Force_spring + Force_damping

   Force_spring = -Stiffness * (Position - Target)
   Force_damping = -Damping * Velocity

Role of Damping & Stability:
   Without damping, the system would oscillate indefinitely (Simple Harmonic Motion).
   In a discrete-time simulation (using Euler integration), zero damping can cause
   instability where energy increases over time, causing the points to "explode" 
   off-screen. 
   
   I implemented a variable Damping coefficient (controlled via slider) to dissipate 
   energy, ensuring the rope settles smoothly. High damping creates a "jelly-like" 
   drag; low damping creates a bouncy, elastic effect.


5. DESIGN CHOICES & OPTIMIZATION
--------------------------------------------------------------------------------
* Language: HTML/JavaScript was chosen for its universal compatibility and the
  immediate feedback of the Canvas API.
* Optimization: Vector objects are reused where possible. The render loop uses
  'requestAnimationFrame' to sync with the display refresh rate (typically 60Hz),
  ensuring smooth motion without tearing.
* Clean Architecture: The code is separated into:
    - 'PhysicsPoint' class (handles movement).
    - 'Vec2' helper (handles vector algebra).
    - 'animate()' (handles rendering and loop logic).


6. HOW TO RUN & CONTROLS
--------------------------------------------------------------------------------
1. Click :- 
2. Move the mouse to pull the rope.
3. Use the UI controls in the top-right corner:
    - Stiffness Slider: Controls the tension (Higher = Snap back faster).
    - Damping Slider: Controls the friction (Lower = More wobble).

