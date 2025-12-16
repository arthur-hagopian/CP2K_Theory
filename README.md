# CP2K Theory: From Molecules to Interfaces

This repository is a **guided walkthrough of the theory, numerical choices, and input structure of CP2K**, with a **direct focus on electronic properties** relevant for **water, metallic systems, and metal–water interfaces**.

Rather than providing ready-to-use *black-box* input files, this tutorial aims to build **physical and numerical intuition** for CP2K keywords and their impact on the computed electronic structure.

---

## Scope and philosophy

The tutorial follows a **bottom-up approach**, progressing from minimal systems to realistic electrochemical interfaces. Each step introduces new theoretical concepts and CP2K keywords in a controlled and physically transparent way.

### 1. Single water molecule (`01_molecule/`)

A single, isolated H₂O molecule is used as the **minimal reference system** to introduce the core ingredients of CP2K’s DFT implementation.

This section focuses on:
- the structure of `&DFT` and `&QS`
- Gaussian basis sets and GTH pseudopotentials
- plane-wave grids (`&MGRID`) and cutoff convergence
- SCF procedures and convergence criteria (`&SCF`, `EPS_SCF`)
- orbital energies and projected density of states (PDOS)

Because the system is simple and non-periodic, it allows direct comparison to **textbook molecular orbitals** and highlights how numerical settings affect electronic properties.

---

### 2. Metallic systems and periodicity

The second step moves to **metallic systems**, introducing periodic boundary conditions and the physics of extended electronic states.

Key topics include:
- periodic cells and k-point sampling
- metallic SCF convergence and electronic smearing
- Fermi level and occupation schemes
- work functions and surface electronic structure
- projected density of states in extended systems

This section establishes the conceptual tools required to move from molecules to surfaces.

---

### 3. Metal–water interfaces

The final part addresses **metal–water interfaces**, which are central to electrochemistry and interfacial AIMD simulations.

Here, the focus is on:
- slab models with explicit water layers
- vacuum separation and electrostatic boundary conditions
- surface dipole corrections
- charge neutrality and potential alignment
- CP2K tags specific to interfacial AIMD calculations

At this stage, CP2K is used to analyze **interfacial electronic structure and electrostatics**, rather than only atomic trajectories.

---

## Intended audience

This repository is intended for:
- students and researchers starting with CP2K
- users who already run CP2K but want a deeper theoretical understanding
- anyone working on **water, interfaces, or electrochemical systems**

## Status

🚧 This repository is under active development and will be expanded progressively.

