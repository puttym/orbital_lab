# Orbital Mechanics Simulation – Configurable Design Notes

This document captures which aspects of an orbital-mechanics simulation platform should be configurable, and at what stage.  
The goal is to enable future flexibility without introducing premature complexity.

---

## Design Principle

Make **environment, physics scope, and numerics configurable**.

Avoid configuring:
- Internal math steps
- Class structures
- Data layouts
- Algorithm internals (in early phases)

Configurability should increase reuse and architectural safety, not cognitive load.

---

# High-Value Configurables (Day-1 Worthy)

These are inexpensive to add early and difficult to retrofit later.

## 1. Central Body
**Purpose:** Maximum reuse with near-zero complexity cost.

Configurable fields:
- Gravitational parameter (μ)
- Mean radius
- J2 coefficient (even if unused initially)
- Rotation rate (optional)
- Name / identifier

Enables Earth, Moon, Mars, Sun, etc. without rewriting equations.

---

## 2. Time Representation / Epoch
**Purpose:** Prevent hidden assumptions and improve reproducibility.

Configurable:
- Epoch (start time)
- Time unit (seconds / days)
- UTC vs Julian Date (internally normalized if needed)

Full astronomical precision is not required initially — explicitness is.

---

## 3. Reference Frame (Abstract Layer)
**Purpose:** Enforce linear-algebra discipline early.

Configurable:
- Inertial frame label
- Body-fixed frame label
- Transformation interface (even if identity at start)

Avoid hard-coding a single frame.

---

## 4. Units System
**Purpose:** Prevent silent scaling bugs.

Configurable:
- Length unit (km vs m)
- Time unit (s vs day)
- Angle unit (rad vs deg for input/output)

Internally you may standardize, but config prevents accidental mixing.

---

# Medium-Value Configurables (Phase 1.5 / Phase 2)

Introduce after the two-body core is stable.

## 5. Force Model Selection
**Purpose:** Natural entry into perturbations.

Configurable switches:
- Two-body only
- + J2
- + Atmospheric drag (simplified)
- + Solar radiation pressure (optional later)

This is a controlled list, not arbitrary physics.

---

## 6. Integrator Choice
**Purpose:** Numerical experimentation and learning.

Configurable:
- RK4
- RK45 / adaptive
- Fixed vs adaptive step
- Step size / tolerances

Requires a clean solver interface.

---

## 7. Initial State Representation
**Purpose:** Convenience and broader test coverage.

Configurable input mode:
- State vector (r, v)
- Classical orbital elements
- (Later) Equinoctial elements

Internally convert to a canonical form.

---

# Low-Value Early Configurables (Avoid Day-1)

These add complexity without foundational learning benefit.

## 8. Propulsion / Maneuvers
- Burns, thrust curves, mass change

## 9. Multi-Body Gravity
- N-body / ephemerides  
- Architect indirectly via central-body abstraction, but do not implement early

## 10. High-Fidelity Atmospheres
- Exponential drag is sufficient initially

## 11. Mission Optimization
- Lambert solvers, targeting, transfer windows

---

# Structural / Platform Configurables (Cheap but Valuable)

These improve usability and experimentation without deep physics changes.

## 12. Output / Logging Resolution
- Sampling interval
- Variables to record

## 13. Visualization Options
- 2D vs 3D
- Display frame
- Scaling / aspect lock

## 14. Numerical Tolerances
- Root-finder tolerance
- Convergence criteria
- Angle tolerances
- Eccentricity tolerances

---

# Semantic / Classification Thresholds (Optional but Useful)

Used only for logging, reporting, and visualization — **never for dynamics**.

Examples:
- Near-equatorial inclination band
- Near-polar inclination band
- Circular orbit eccentricity threshold
- Parabolic boundary tolerance
- Retrograde boundary angle

These are interpretive, not physical.

---

# Minimal Practical Day-1 Configuration Set

If starting with tight scope, include only:

1. Central body parameters  
2. Epoch / time unit  
3. Units (km/s vs m/s)  
4. Integrator type + step size  
5. Input mode (state vs elements)

Everything else can grow naturally once the two-body and basic propagation pipeline is stable.

---

## Summary

Configure:
- Representation
- Environment
- Numerics
- Interpretation (optionally)

Delay:
- Complex physics
- Optimization engines
- Multi-body dynamics

This preserves flexibility while keeping the initial system intellectually manageable and technically clean.
