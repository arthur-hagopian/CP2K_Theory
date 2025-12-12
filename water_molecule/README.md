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
  RUN_TYPE GEO_OPT
&END GLOBAL

The &GLOBAL section controls how the calculation is run, independently of the physical system.

PROJECT H2O
Sets the project name. All output files will start with this prefix (e.g. H2O.out).

PRINT_LEVEL MEDIUM
Controls the verbosity of the output.
MEDIUM is a good compromise: enough information to diagnose convergence without overwhelming the output file.

RUN_TYPE GEO_OPT
Requests a geometry optimization, i.e. ionic positions are relaxed until forces vanish.
For a single water molecule, this allows us to verify that the electronic structure setup is stable and well converged.
