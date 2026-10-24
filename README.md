# NACA 2412 Airfoil: CFD Aerodynamic Analysis (ANSYS Fluent)

Steady-state RANS CFD of a NACA 2412 airfoil. The geometry was built from the published NACA 4-digit equations in SOLIDWORKS (no imported geometry files) and analysed in ANSYS Fluent.

**Status: force results are being re-extracted and a multi-angle study is being added.** Search this file for `TODO` before publishing.

## Airfoil Definition

- NACA 2412: 2% maximum camber at 40% chord, 12% maximum thickness
- Chord 200 mm, span 600 mm
- Coordinates generated from the NACA 4-digit camber-line and thickness equations and entered in SOLIDWORKS using Curve Through XYZ Points (40 points)

## Setup

| Item | Value |
|---|---|
| Solver | ANSYS Fluent, 3D, double precision, pseudo-transient, 4 processes |
| Turbulence model | SST k-omega |
| Freestream velocity | TODO m/s |
| Reynolds number | TODO (V x 0.2 m / kinematic viscosity of air) |
| Angle of attack | TODO (state the exact angle, not "approximately 0") |
| Reference area | 0.12 m² (chord x span) |
| Air density | 1.225 kg/m³ |
| Mesh | TODO cells, 0.5 mm element size, student cap 512,000 cells |
| Inflation layers | TODO: state yes or no, number of layers and first-layer height |
| y+ on airfoil | TODO |
| Iterations | 200 |

The domain is a rectangular box with the airfoil subtracted. Named selections: inlet (velocity inlet), outlet (pressure outlet), wall (top, bottom, sides), wall-solid (airfoil surface).

## Results

**Pressure field.** At low angle of attack the mid-span plane shows a high-pressure stagnation region at the leading edge (up to about +270 Pa), a suction region over the upper surface (down to about -180 Pa) and pressure recovery toward the trailing edge, as expected for a cambered profile.

**Force coefficients**

| AoA | Cl | Cd | Cl (reference) | Cl error |
|---|---|---|---|---|
| TODO | TODO | TODO | TODO | TODO % |

TODO: report Cl and Cd from Fluent report definitions using the reference values above, run at several angles of attack (for example -4 to +12 degrees in 2 degree steps). Earlier force values in this README (1.658e-05 N lift, -4.473e-09 N drag) were removed because they are far below what the dynamic pressure and planform area imply, which points to a unit or scale error in the extraction.

## Validation

Compare Cl and Cd against data at a similar Reynolds number. Abbott and Von Doenhoff (1959) give the standard NACA 2412 data, but mostly at Reynolds numbers of about 3 million and above, which is higher than this case. For a fairer comparison use a low-Reynolds source such as Matsson et al. (2016), ASEE Annual Conference, which compares NACA 2412 experiments with Fluent at Re of about 426,000, or XFOIL run at your Reynolds number.

## Limitations

- Mesh sensitivity not studied. TODO: run 2 to 3 mesh sizes and report the change in Cl and Cd.
- y+ not yet reported. SST k-omega wants y+ of about 1 or below.
- 3D wing between side walls, so end effects are present. A 2D section would allow a much finer mesh inside the cell cap.
- Geometry built from 40 points, which limits leading-edge accuracy.
- Student-licence mesh cap.

## Next Steps

1. Fix and re-extract forces, then report Cl and Cd with correct reference values.
2. Run an angle-of-attack sweep and plot Cl and Cd against alpha.
3. Compare with Re-matched reference data and report the error.
4. Add a mesh study and report y+.
5. Optionally repeat as a 2D section and compare with the 3D result.

## Images

Geometry (SOLIDWORKS profile and 3D wing, DesignModeler domain), mesh (surface close-up, wing, full domain), solution setup (boundary conditions, residuals), post-processing (pressure contours, force extraction). TODO: replace screenshots with clean exports and crop out the Windows taskbar and software interface.

## Tools

SOLIDWORKS, ANSYS DesignModeler, ANSYS Meshing, ANSYS Fluent, ANSYS CFD-Post

## Author

TODO: your full name, MSc Automotive Engineering, University of Hertfordshire
GitHub: https://github.com/rupeshwaran67-prog
