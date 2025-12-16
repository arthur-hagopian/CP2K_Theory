# Single water molecule

This document explains, step by step, the CP2K input used to describe a **single isolated water molecule**.
The goal is to build physical and numerical intuition for the main CP2K tags.

The emphasis is on the tags that matter most for **electronic-structure analysis**. In particular, this guide is written with the goal of understanding how CP2K settings impact the **density of states (DOS)** and **projected density of states (PDOS)**, and how to set up calculations that yield reliable DOS/PDOS for molecules, surfaces and interfaces.  

The full input discussed below is reproduced in the corresponding `input.inp` file.

---

## 1. GLOBAL section: job control

```text
&GLOBAL
  PROJECT H2O
  PRINT_LEVEL MEDIUM
  RUN_TYPE GEO_OPT
&END GLOBAL
```

The `&GLOBAL` section controls **how the calculation is run**.

- `PROJECT H2O`  
  Sets the project name. All output files will start with this prefix (e.g. `H2O.out`).

- `PRINT_LEVEL MEDIUM`  
  Controls the verbosity of the output.  
  `MEDIUM` is a good compromise: enough information to diagnose convergence without overwhelming the output file.

- `RUN_TYPE GEO_OPT`  
   Performs a **geometry optimization** where atomic positions are relaxed until the force-based convergence criteria defined in `&MOTION / &GEO_OPT` are satisfied.

  `RUN_TYPE CELL_OPT`  
   Performs a **cell optimization** in which both the **cell vectors and atomic positions** are optimized.

  `RUN_TYPE ENERGY`
  Performs a **single-point energy calculation** at fixed atomic positions and cell parameters.

  `RUN_TYPE ENERGY_FORCE`
   Performs a **single-point energy calculation** where both the **total energy and atomic forces** are computed.

---

## 2. FORCE_EVAL section: how forces and energies are computed

```text
&FORCE_EVAL
  METHOD Quickstep
```

`&FORCE_EVAL` defines **how energies and forces are evaluated**.

- `METHOD Quickstep`  
  Activates CP2K’s **Quickstep** module, which is the core engine for density-functional theory calculations in CP2K.  
  Quickstep implements the **Gaussian and Plane Waves (GPW)** method, where Kohn–Sham orbitals are expanded in **localized Gaussian basis functions**, while the electronic density is mapped onto an **auxiliary plane-wave grid**.  

  This mixed representation allows CP2K to treat systems with **thousands of atoms** efficiently: Gaussian functions keep the Hamiltonian sparse and enable linear-scaling techniques, while the plane-wave grid ensures an accurate and systematically convergent description of the electrostatics and exchange–correlation energy. As a result, Quickstep combines the efficiency of localized basis sets with much of the robustness of plane-wave approaches, making it particularly well suited for molecular dynamics and interfacial simulations.

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
  Specifies a **triple-zeta valence Gaussian basis set with two polarization functions**.  
  “Triple-zeta” means that each valence orbital is represented by three basis functions, allowing for a flexible description of bonding and charge redistribution.  
  “Double polarization” indicates that two sets of higher-angular-momentum functions are added, enabling the electron density to distort and polarize more accurately, which is particularly important for describing anisotropic bonding and electronic states relevant for DOS/PDOS analysis.  

  For lighter or exploratory calculations, smaller basis sets such as `SZV-GTH` (single-zeta valence) or `DZVP-GTH` (double-zeta valence with polarization) can be used at reduced computational cost, albeit with lower accuracy.

- `POTENTIAL GTH-PBE-qX`  
  Specifies a **Goedecker–Teter–Hutter (GTH) norm-conserving pseudopotential** constructed consistently with the PBE exchange–correlation functional.  
  Core electrons are replaced by an effective potential, reducing the number of explicitly treated electrons and thus the computational cost, while retaining an accurate description of valence electronic structure.  
  The label `qX` indicates the number of valence electrons treated explicitly (e.g. `q6` for O, `q1` for H).

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
  EPS_DEFAULT 1.0E-12
&END QS
```

Controls numerical settings specific to the **Gaussian and Plane Waves (GPW)** formalism.

- `METHOD GPW`  
  Selects the GPW scheme, in which Kohn–Sham orbitals are expanded in Gaussian basis functions while the electronic density is represented on a plane-wave grid. Other Quickstep methods exist (e.g. `GAPW`, `RIGPW`, `OFGPW`, semiempirical and tight-binding variants), but `GPW` is the standard and most appropriate choice when Kohn–Sham orbitals and reliable DOS/PDOS are required.  

- `EPS_DEFAULT`  
  Global numerical threshold controlling the accuracy of many internal operations (e.g. integral screening and matrix elements).  
  Tight values reduce numerical noise in the electronic structure, which is important for obtaining smooth and well-converged DOS/PDOS.


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

Controls the **real-space multigrid representation** of the electronic density in the GPW formalism.

- `CUTOFF`  
  Sets the plane-wave cutoff (in Ry) of the **finest grid** used to represent the electron density.  
  Higher values increase spatial resolution and improve the accuracy of energies and electronic states, which is important for converged DOS/PDOS calculations.

- `REL_CUTOFF`  
  Defines the cutoff for the coarser auxiliary grids relative to the finest one.  
  This parameter controls the balance between computational efficiency and numerical accuracy in the multigrid scheme.

- `NGRIDS`  
  Specifies the number of grids used in the multigrid hierarchy.  
  Multiple grids allow CP2K to efficiently treat both smooth and rapidly varying components of the electron density.

---

## 8. SCF section: self-consistent field procedure

```text
&SCF
  SCF_GUESS ATOMIC
  EPS_SCF 1.0E-6
  MAX_SCF 200
  ADDED_MOS 5
  CHOLESKY RESTORE
```

Defines how the **self-consistent field (SCF)** cycle is performed, i.e. how the electronic density is iteratively converged.

- `SCF_GUESS ATOMIC`  
  Initializes the electronic density as a superposition of isolated atomic densities.  
  This provides a stable and physically meaningful starting point for molecular and interfacial systems.

- `EPS_SCF`  
  Convergence threshold for the SCF cycle, typically based on the change in total energy or density between iterations.  
  Tight values reduce numerical noise in orbital energies and are important for obtaining smooth and well-converged DOS/PDOS.

- `MAX_SCF`  
  Maximum number of SCF iterations allowed before the calculation is aborted.  
  Larger values increase robustness for difficult cases but do not affect the final converged solution.

- `ADDED_MOS`  
  Adds extra unoccupied molecular orbitals beyond those required by the electron count.  
  This improves numerical stability and is particularly important when analyzing unoccupied states and the conduction-band region of the DOS.

- `CHOLESKY RESTORE`  
  Uses a Cholesky decomposition of the overlap matrix to improve numerical stability and efficiency of the SCF procedure.


---

## 9. XC section: exchange–correlation

```text
&XC
  &XC_FUNCTIONAL PBE
  &END XC_FUNCTIONAL
```

Uses the PBE exchange–correlation functional.

---

## 10. XC section: exchange–correlation

```text
&PRINT
  &PDOS ON
    NLUMO 5
    &LDOS
      LIST 1
    &END LDOS
    &LDOS
      LIST 2
        &END LDOS
        &LDOS
          LIST 3
        &END LDOS
      &END PDOS
    &END PRINT
  &END DFT
&END FORCE_EVAL

## 11. MOTION section: geometry optimization settings

```text
&MOTION
  &GEO_OPT
    TYPE MINIMIZATION
    OPTIMIZER BFGS
    MAX_ITER 200
    MAX_FORCE [eV/angstrom] 1.0E-3
    RMS_FORCE [eV/angstrom] 5.0E-4
    MAX_DR    [angstrom]    1.0E-2
    RMS_DR    [angstrom]    5.0E-3
  &END GEO_OPT
&END MOTION
```

The `&MOTION` section controls **ionic motion**, i.e. how atomic positions are updated during the calculation.

- `TYPE MINIMIZATION`  
  Specifies that a geometry optimization is performed, consistent with `RUN_TYPE GEO_OPT`.

- `OPTIMIZER BFGS`  
  Uses the BFGS quasi-Newton algorithm, which efficiently updates atomic positions using an approximate Hessian.

- `MAX_ITER`  
  Maximum number of geometry optimization steps.

- `MAX_FORCE` and `RMS_FORCE`  
  Convergence criteria based on the maximum and root-mean-square atomic forces, respectively.  
  These thresholds ensure that residual forces on the atoms are sufficiently small.

- `MAX_DR` and `RMS_DR`  
  Convergence criteria based on the maximum and root-mean-square atomic displacements between optimization steps.  
  Including displacement-based criteria prevents premature convergence when forces are small but atomic positions are still evolving.

---

## Final remarks

This water molecule example provides a **minimal, well-controlled reference system**.
Most of the tags introduced here will reappear in more complex simulations,
such as **metal/water interfaces**, where periodicity and electrostatics play a central role.
