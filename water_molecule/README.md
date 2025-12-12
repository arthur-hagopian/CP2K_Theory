# Water molecule: CP2K input walkthrough

This document explains, step by step, the CP2K input used to describe a **single isolated water molecule**.
The goal is to build physical and numerical intuition for the main CP2K tags, starting from the simplest possible system.

The full input discussed below is reproduced in the corresponding `input.inp` file.

---

## 1. GLOBAL section: job control

```text
&GLOBAL
  PROJECT H2O
  PRINT_LEVEL MEDIUM
  RUN_TYPE ENERGY
&END GLOBAL
```

The `&GLOBAL` section controls **how the calculation is run**.

- `PROJECT H2O`  
  Sets the project name. All output files will start with this prefix (e.g. `H2O.out`).

- `PRINT_LEVEL MEDIUM`  
  Controls the verbosity of the output.  
  `MEDIUM` is a good compromise: enough information to diagnose convergence without overwhelming the output file.

- `RUN_TYPE ENERGY`  
  Performs a **single-point energy calculation** at fixed atomic positions and cell parameters.

  `RUN_TYPE ENERGY_FORCE` performs a **single-point energy calculation** where both the **total energy and atomic forces** are computed.

  `RUN_TYPE GEO_OPT` activates a **geometry optimization**, where atomic positions are relaxed until the force-based convergence criteria defined in `&MOTION / &GEO_OPT` are satisfied.

  `RUN_TYPE CELL_OPT` performs a **cell optimization**, in which both the **cell vectors and atomic positions** are optimized.

---

## 2. FORCE_EVAL section: how forces and energies are computed

```text
&FORCE_EVAL
  METHOD Quickstep
```

`&FORCE_EVAL` defines **how energies and forces are evaluated**.

- `METHOD Quickstep`  
  Activates CP2K’s **Quickstep** module, which implements the **Gaussian and Plane Waves (GPW)** formalism.

---

## 3. SUBSYS section: atomic structure and simulation cell

### 3.1 Simulation cell

```text
&SUBSYS
  &CELL
    ABC [angstrom] 25.0 25.0 25.0
    ALPHA_BETA_GAMMA 90.0 90.0 90.0
    PERIODIC NONE
  &END CELL
```

- `ABC [angstrom] 25.0 25.0 25.0`  
  Cell vector lengths (cubic box of side 25 Å).

- `ALPHA_BETA_GAMMA 90.0 90.0 90.0`  
  Cell angles (orthogonal cell).

- `PERIODIC NONE`  
  Specifies a **non-periodic** (isolated) system.

---

### 3.2 Atomic coordinates

```text
&TOPOLOGY
  COORD_FILE_FORMAT XYZ
  COORD_FILE_NAME structure.xyz
&END TOPOLOGY
```

- `COORD_FILE_FORMAT XYZ`  
  Indicates that atomic coordinates are read from an XYZ file.

- `COORD_FILE_NAME structure.xyz`  
  File containing the positions of the O and H atoms.

---

### 3.3 Atomic kinds: basis sets and pseudopotentials

```text
&KIND O
  BASIS_SET TZV2P-MOLOPT-GTH
  POTENTIAL GTH-PBE-q6
&END KIND
```

```text
&KIND H
  BASIS_SET TZV2P-MOLOPT-GTH
  POTENTIAL GTH-PBE-q1
&END KIND
```

Each `&KIND` block defines how a given chemical element is treated.

- `BASIS_SET TZV2P-MOLOPT-GTH`  
  Triple-zeta valence basis set with polarization, optimized for molecular systems.

- `POTENTIAL GTH-PBE-qX`  
  GTH pseudopotentials consistent with the PBE functional.

---

## 4. DFT section: electronic structure setup

```text
&DFT
  BASIS_SET_FILE_NAME /path/to/BASIS_MOLOPT
  POTENTIAL_FILE_NAME /path/to/GTH_POTENTIALS
```

These lines tell CP2K **where to find** the basis sets and pseudopotentials.

---

### 4.1 Total charge

```text
CHARGE 0
```

Specifies the **total charge of the system**.

---

## 5. QS section: GPW numerical settings

```text
&QS
  METHOD GPW
  EPS_DEFAULT 1.0E-14
  EPS_PGF_ORB 1.0E-14
&END QS
```

Controls numerical thresholds in the GPW formalism.

---

## 6. POISSON section: electrostatics

```text
&POISSON
  PERIODIC NONE
  POISSON_SOLVER WAVELET
&END POISSON
```

Uses a wavelet-based Poisson solver suitable for isolated systems.

---

## 7. MGRID section: real-space grids

```text
&MGRID
  CUTOFF 600
  REL_CUTOFF 80
  NGRIDS 5
&END MGRID
```

Controls the plane-wave grids used to represent the electron density.

---

## 8. SCF section: self-consistent field procedure

```text
&SCF
  SCF_GUESS ATOMIC
  EPS_SCF 1.0E-8
  MAX_SCF 200
  ADDED_MOS 5
  CHOLESKY RESTORE
```

Defines how the electronic self-consistency cycle is performed.

---

## 9. XC section: exchange–correlation and dispersion

```text
&XC
  &XC_FUNCTIONAL PBE
  &END XC_FUNCTIONAL
```

Uses the PBE exchange–correlation functional.

---

### 9.1 van der Waals interactions

```text
&VDW_POTENTIAL
  POTENTIAL_TYPE PAIR_POTENTIAL
  &PAIR_POTENTIAL
    TYPE DFTD3
    REFERENCE_FUNCTIONAL PBE
  &END PAIR_POTENTIAL
&END VDW_POTENTIAL
```

Adds long-range dispersion interactions using the DFT-D3 correction.

---



---

## 10. MOTION section: geometry optimization settings

```text
&MOTION
  &GEO_OPT
    TYPE MINIMIZATION
    OPTIMIZER BFGS
    MAX_ITER 200
    MAX_FORCE [eV/angstrom] 1.0E-4
    RMS_FORCE [eV/angstrom] 5.0E-5
  &END GEO_OPT
&END MOTION
```

The `&MOTION` section controls **ionic motion**, i.e. how atomic positions are updated.

- `TYPE MINIMIZATION`  
  Specifies that a geometry optimization (energy minimization) is performed, consistent with `RUN_TYPE GEO_OPT`.

- `OPTIMIZER BFGS`  
  Uses the **Broyden–Fletcher–Goldfarb–Shanno (BFGS)** quasi-Newton algorithm.  
  This is a robust and efficient optimizer for small molecular systems.

- `MAX_ITER 200`  
  Maximum number of geometry optimization steps.

- `MAX_FORCE [eV/angstrom] 1.0E-4`  
  Convergence criterion on the **maximum atomic force**.  
  The optimization stops when the largest force component falls below this threshold.

- `RMS_FORCE [eV/angstrom] 5.0E-5`  
  Convergence criterion on the **root-mean-square force** over all atoms.  
  Using both `MAX_FORCE` and `RMS_FORCE` ensures a well-relaxed molecular geometry.

---

## Final remarks

This water molecule example provides a **minimal, well-controlled reference system**.
Most of the tags introduced here will reappear in more complex simulations,
such as **metal/water interfaces**, where periodicity and electrostatics play a central role.
