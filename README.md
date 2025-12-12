# CP2K_Theory

This repository is a guided walkthrough of the **theory and input structure of CP2K**, with a focus on understanding **what each tag does and why it is used**.

The goal is not to provide “black-box” input files, but to build **physical and numerical intuition** behind CP2K settings commonly used in electronic structure and ab initio molecular dynamics calculations.

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

