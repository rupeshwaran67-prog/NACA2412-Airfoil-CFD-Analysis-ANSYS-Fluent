# NACA 2412 Airfoil: CFD Aerodynamic Analysis (ANSYS Fluent)

Steady-state RANS CFD of a NACA 2412 airfoil. The geometry was built from the published NACA 4-digit equations in SOLIDWORKS (no imported geometry files) and analysed in ANSYS Fluent and CFD-Post.

---

## Airfoil Definition

- **Profile:** NACA 2412 (2% maximum camber at 40% chord, 12% maximum thickness)
- **Intended Dimensions:** Chord $c = 200\text{ mm}$ ($0.2\text{ m}$), span $b = 600\text{ mm}$ ($0.6\text{ m}$)
- **Geometry Generation:** Coordinates generated from the NACA 4-digit camber-line and thickness equations and entered in SOLIDWORKS using *Curve Through XYZ Points* (40 points)

---

## Setup

| Item | Value |
| :--- | :--- |
| **Solver** | ANSYS Fluent 2026 R1, 3D, pressure-based, double precision, pseudo-transient (Automatic time step, scale factor 1.0), 4 processes |
| **Turbulence Model** | SST $k\text{-}\omega$ |
| **Freestream Velocity** | $20\text{ m/s}$ (verified from CFD-Post velocity field; peak local velocity $25.18\text{ m/s}$) |
| **Reynolds Number** | $\text{Re} \approx 2.74 \times 10^5$ at intended $0.2\text{ m}$ chord ($\nu = 1.46 \times 10^{-5}\text{ m}^2/\text{s}$); $\text{Re}_{\text{sim}} \approx 274$ in imported domain ($0.0002\text{ m}$ chord due to mm-to-m import scaling) |
| **Angle of Attack** | $0^\circ$ (chord line aligned horizontally with freestream $+X$ flow) |
| **Reference Area** | $0.12\text{ m}^2$ intended ($0.2\text{ m} \times 0.6\text{ m}$); $1.2 \times 10^{-7}\text{ m}^2$ in imported domain ($0.0002\text{ m} \times 0.0006\text{ m}$) |
| **Air Density** | $1.225\text{ kg/m}^3$ ($q = \frac{1}{2}\rho V^2 = 245\text{ Pa}$) |
| **Mesh** | Unstructured tetrahedral mesh, $0.5\text{ mm}$ target element size (within 512,000-cell ANSYS Student licence cap) |
| **Inflation Layers** | Not applied in baseline run (queued for boundary-layer refinement study) |
| **$y^+$ on Airfoil** | Not extracted in baseline run (target $y^+ \le 1$ planned with inflation layer addition) |
| **Iterations** | 200 max set; scaled residuals converged to solver tolerance at iteration 42 |

The domain is a rectangular wind-tunnel box ($8\text{ mm}$ length in imported scale) with the airfoil subtracted. Named selections: `inlet` (velocity-inlet), `outlet` (pressure-outlet), `wall` & `ground` (outer tunnel walls), and `wall-solid` (airfoil surface).

---

## Results

### Pressure & Velocity Field ($0^\circ$ AoA)

- **Pressure Field:** At $0^\circ$ angle of attack, the mid-span plane (`Plane 1`) shows a clear leading-edge stagnation region reaching **$+266.3\text{ Pa}$**, acceleration over the cambered upper surface creating a suction trough down to **$-179.8\text{ Pa}$**, and gradual pressure recovery toward the trailing edge.
- **Velocity Field:** Freestream flow enters at **$20.0\text{ m/s}$** and accelerates over the upper surface suction peak to **$25.18\text{ m/s}$**.

### Force Extraction & Coefficients

Forces were extracted in ANSYS CFD-Post Function Calculator on `wall-solid` (`Variable: Pressure`):
- **Lift Force (Global $Y$):** $F_y = 1.65848 \times 10^{-5}\text{ N}$
- **Pressure Drag Force (Global $X$):** $F_x = 6.41367 \times 10^{-6}\text{ N}$ *(Note: the previously recorded $-4.47306 \times 10^{-9}\text{ N}$ was the spanwise Global $Z$ force, confirming spanwise symmetry rather than drag.)*

Normalising by the actual imported planform area ($A_{\text{sim}} = 1.2 \times 10^{-7}\text{ m}^2$) and dynamic pressure ($q = 245\text{ Pa}$, $q A_{\text{sim}} = 2.94 \times 10^{-5}\text{ N}$):

| AoA | $C_l$ | $C_d$ (Pressure) | $C_l$ (Reference) | $C_l$ Error |
| :--- | :--- | :--- | :--- | :--- |
| $0^\circ$ | $0.564$ | $0.218$ | $0.240$ | $+135.0\%$ |

---

## Validation & Engineering Diagnosis

- **Reference Baseline:** For a NACA 2412 airfoil at $\alpha = 0^\circ$ and $\text{Re} \approx 2.74 \times 10^5\text{–}4.26 \times 10^5$, XFOIL and low-Reynolds experimental data (Matsson et al., 2016, *ASEE Annual Conference*) give $C_{l,\text{ref}} \approx 0.240$, whereas Abbott & Von Doenhoff (1959) report high-Reynolds data ($\text{Re} \ge 3 \times 10^6$).
- **Root Cause of Discrepancy ($+135.0\%$):** Post-processing inspection of the CFD-Post scale bars revealed that the SOLIDWORKS geometry imported into Fluent at $1/1000\text{th}$ linear scale ($0.2\text{ mm}$ chord instead of $200\text{ mm}$). While surface pressure magnitudes ($+266.3\text{ Pa}$ to $-179.8\text{ Pa}$) match $20\text{ m/s}$ dynamic pressure, the $1000\times$ smaller chord reduced the effective Reynolds number to $\text{Re}_{\text{sim}} \approx 274$, placing the boundary layer in an overly viscous regime that inflates pressure drag ($C_{d,p} = 0.218$) and alters effective camber circulation. Additionally, CFD-Post `Pressure` force integration excludes viscous shear drag.

---

## Limitations

- **Unit Scaling Mismatch:** CAD geometry imported at $1/1000\text{th}$ scale (`mm` interpreted as `m`), lowering the effective Reynolds number by three orders of magnitude.
- **Mesh Sensitivity & Near-Wall Resolution:** Single baseline mesh without inflation layers; $y^+$ was not enforced to $y^+ \le 1$ for SST $k\text{-}\omega$, and grid convergence across 2–3 refinement levels has not yet been completed.
- **Single Angle of Attack:** Baseline results cover $\alpha = 0^\circ$ only; multi-angle polar sweep ($-4^\circ$ to $+12^\circ$) is queued next.
- **3D Wall Effects & Geometry Resolution:** 3D span bounded by tunnel side walls introduces end effects, and 40 coordinate points limit leading-edge curvature smoothness within the 512,000-cell Student licence limit.

---

## Next Steps

1. Scale geometry by $1000\times$ in DesignModeler (or set Fluent units/scaling to true $0.2\text{ m}$ chord $\times$ $0.6\text{ m}$ span) and extract total forces (pressure + viscous) directly via Fluent Report Definitions.
2. Add boundary-layer inflation layers targeting $y^+ \le 1$ and perform a 3-level mesh independence study.
3. Run an angle-of-attack sweep ($\alpha = -4^\circ$ to $+12^\circ$ in $2^\circ$ increments) to plot $C_l\text{-}\alpha$ and $C_d\text{-}\alpha$ polars against XFOIL / Matsson et al. (2016).
4. Replace raw full-screen captures with cropped viewport exports.

---

## Tools

SOLIDWORKS, ANSYS DesignModeler, ANSYS Meshing, ANSYS Fluent (2026 R1), ANSYS CFD-Post

---

## Author

**Rupeshwaran Paranthaman**, MSc Automotive Engineering, University of Hertfordshire  
GitHub: [https://github.com/rupeshwaran67-prog](https://github.com/rupeshwaran67-prog)
