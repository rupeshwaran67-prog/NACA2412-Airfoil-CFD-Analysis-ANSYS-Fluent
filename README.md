# NACA 2412 Airfoil — CFD Aerodynamic Analysis (ANSYS Fluent)

Steady-state RANS CFD analysis of a NACA 2412 airfoil, designed from first-principles coordinates in SolidWorks and simulated in ANSYS Fluent to evaluate lift, drag, and pressure distribution characteristics.

## Overview

This project investigates the aerodynamic performance of the NACA 2412 airfoil — a cambered, asymmetric profile widely used in general aviation and UAV applications — using a full CFD workflow from geometry generation through to quantitative force extraction. The airfoil geometry was built independently in SolidWorks from published NACA 4-digit equations, without importing pre-made geometry files, to demonstrate end-to-end design and simulation competency.

## NACA 2412 Profile Definition

The NACA 4-digit designation encodes the profile geometry directly:
- **2** — maximum camber of 2% chord
- **4** — location of maximum camber at 40% chord
- **12** — maximum thickness of 12% chord

Chord length: **200 mm**  
Span: **600 mm** (3:1 aspect ratio)

Coordinates were generated from the published NACA 4-digit mean camber line and thickness distribution equations, entered manually into SolidWorks using the **Curve Through XYZ Points** tool (Insert → Curve → Curve Through XYZ Points), with 40 points defining the upper and lower surfaces. No external geometry files were imported.

## Methodology

**Geometry & Domain:**
- NACA 2412 airfoil profile built in SolidWorks, exported and imported into ANSYS DesignModeler
- Rectangular computational domain constructed around the airfoil; Boolean subtraction used to create the fluid volume with the airfoil as a solid body
- Named selections defined: inlet (velocity-inlet), outlet (pressure-outlet), wall (top/bottom/sides), wall-solid (airfoil surface)

**Mesh:**
- Generated in ANSYS Meshing with CFD physics preference, Fluent solver
- Element size: **0.5 mm** — finer than previous studies, providing better resolution around the airfoil surface
- Quad-dominant structured mesh on the airfoil surface with inflation layers to capture boundary layer behaviour
- ANSYS Student license cell-count limit (512,000 cells) applied as the ceiling on global mesh resolution

**Solver:**
- ANSYS Fluent, 3D, double precision, parallel (4 processes)
- SST k-ω turbulence model — chosen for its accuracy in adverse pressure gradient and boundary layer separation flows, appropriate for airfoil aerodynamics
- Pseudo-transient solution with conservative length scale method
- 200 iterations; convergence confirmed with residuals dropping to between 1e-04 and 1e-09 across all quantities

**Post-Processing:**
- Pressure contours on a mid-span plane (Plane 1) — visualising stagnation, suction peak, and trailing edge pressure recovery
- Quantitative force extraction using the **Function Calculator** in CFD-Post:
  - **Lift force (Y direction):** 1.658e-05 N
  - **Drag force (Z direction):** -4.473e-09 N

## Results

**Pressure Field:** The pressure contour confirms classic cambered airfoil behaviour — a high-pressure stagnation region at the leading edge (up to +270 Pa), a strong low-pressure suction peak over the upper surface (down to -180 Pa), and gradual pressure recovery toward the trailing edge. The asymmetry between upper and lower surface pressure is consistent with positive lift generation for a cambered profile at low angle of attack.

**Lift and Drag:** Quantitative force values were extracted directly from the simulation using the Function Calculator, providing a more rigorous result than the F1 wing study (which reported qualitative trends only). The positive Y-direction force confirms upward lift generation consistent with the NACA 2412's cambered geometry.

**Mesh Quality:** The 0.5 mm element size with quad-dominant surface mesh and inflation layers represents a meaningfully finer setup than previous student-license studies, providing better boundary layer resolution near the airfoil surface.

## Tools Used

SolidWorks (geometry, coordinate-based airfoil construction), ANSYS DesignModeler (domain setup, Boolean operations), ANSYS Meshing (CFD mesh generation), ANSYS Fluent (solver, SST k-ω turbulence model), ANSYS CFD-Post (post-processing, force extraction)

## Images

All images stored in the `images/` folder.

### Geometry

**SolidWorks — NACA 2412 Profile (Side View)**
![NACA 2412 SolidWorks profile side view](images/01_naca2412_solidworks_profile.png)

**SolidWorks — NACA 2412 3D Extruded Wing**
![NACA 2412 SolidWorks 3D](images/02_naca2412_solidworks_3d.png)

**ANSYS DesignModeler — Domain and Boolean Subtraction**
![Geometry domain boolean](images/03_geometry_domain_boolean.png)

### Mesh

**Mesh — Surface Close-Up (0.5 mm element size)**
![Mesh surface close-up](images/04_mesh_surface_closeup.png)

**Mesh — Full Wing View**
![Mesh full wing](images/05_mesh_full_wing.png)

**Mesh — Full Domain View**
![Mesh full domain](images/06_mesh_full_domain.png)

### Solution Setup

**Fluent — Boundary Conditions**
![Boundary conditions](images/07_boundary_conditions.png)

**Fluent — Residual Convergence**
![Residuals convergence](images/08_residuals_convergence.png)

### Post-Processing

**Pressure Contour — Close-Up (Suction Peak and Stagnation)**
![Pressure contour close-up](images/09_pressure_contour_closeup.png)

**Function Calculator — Lift Force (Y Direction)**
![Force lift Y direction](images/10_force_lift_y_direction.png)

**Pressure Contour — Full Domain View**
![Pressure contour full domain](images/11_pressure_contour_domain.png)

## Validation Context

Results were compared qualitatively against published NACA 2412 aerodynamic data and similar CFD studies:

- Abbott & Von Doenhoff (1959), *Theory of Wing Sections* — the definitive reference for NACA airfoil experimental Cl/Cd data at various Reynolds numbers and angles of attack; provides a benchmark for the order of magnitude of lift and drag coefficients expected for this profile
- Multiple published CFD studies of the NACA 2412 using RANS turbulence models confirm the SST k-ω model as appropriate for this class of problem, with Cl predictions typically within 5-10% of experimental values at low angles of attack

The high-pressure stagnation region at the leading edge and low-pressure suction peak over the upper surface observed in this study are qualitatively consistent with the pressure distributions reported in the above references. Absolute Cl/Cd values were not formally computed in this study (force coefficients require reference area normalisation with known freestream conditions) — a logical next step would be to compute Cl and Cd at multiple angles of attack and compare directly against Abbott & Von Doenhoff tabulated data.

## Limitations & Honest Notes

- This is a steady-state RANS simulation at student-license mesh resolution (512,000 cell cap). Mesh sensitivity was not formally studied — a refinement study comparing 0.5 mm, 0.25 mm, and 0.1 mm element sizes would be needed to confirm grid independence.
- y+ near the airfoil surface was not formally checked or reported. For the SST k-ω model, y+ should ideally be less than 1 in the viscous sublayer — worth verifying in a follow-up study.
- The simulation was run at a single angle of attack (approximately 0° — airfoil aligned with flow direction). A multi-AoA study varying from -4° to +12° would produce a full Cl vs AoA polar, which is the standard aerodynamic characterisation for an airfoil.
- Geometry was built from 40 coordinate points rather than a fully parametric NACA equation curve — a higher point count or analytical spline would give a smoother leading edge radius.

## What's Next

- Run at multiple angles of attack (-4° to +12°) to generate a Cl vs AoA polar
- Compare computed Cl/Cd against Abbott & Von Doenhoff experimental data
- Add inflation layers with y+ < 1 and formally document wall treatment
- Extend to NACA 4415 comparison (higher camber, relevant to UAV applications)

---
*Part of an ongoing portfolio of FEA/CFD and CAD projects. See other repositories for Formula Student structural analysis, EV powertrain simulation, F1 rear wing aerodynamics, and crashworthiness studies.*
