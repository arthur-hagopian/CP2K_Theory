# CP2K Theory: From Molecules to Interfaces

This repository is a **guided walkthrough of the theory, numerical choices, and input structure of CP2K**, with a **direct focus on electronic properties** relevant for **water, metallic systems, and metal–water interfaces**.

Rather than providing ready-to-use *black-box* input files, the aim is to develop **physical and numerical intuition** for:
- what each CP2K tag does,
- why it is needed,
- and how it affects the computed electronic structure.

The emphasis is on understanding CP2K as an **electronic-structure method**, not merely as a molecular dynamics engine.

---

## Scope and philosophy

The repository progresses from simple to more realistic systems:

1. **Single water molecule in a box**  
   Used as a minimal system to introduce and dissect:
   - `&DFT`, `&QS`, `&MGRID`, `&SCF`
   - basis sets and pseudopotentials
   - cutoffs, grids, and convergence parameters

2. **Metal / water interface**  
   Used to illustrate more advanced concepts:
   - periodic slabs and vacuum
   - surface dipole corrections
   - electrostatic boundary conditions
   - tags relevant for interfacial AIMD simulations

For each section, the CP2K input tags are explained in detail, with emphasis on:
- their **physical meaning**
- their **numerical role**
- common pitfalls and best practices

## Intended audience

This repository is intended for:
- students and researchers starting with CP2K
- users who already run CP2K but want a deeper theoretical understanding
- anyone working on **water, interfaces, or electrochemical systems**

## Status

🚧 This repository is under active development and will be expanded progressively.

