# Results - Single water molecule

This section presents the **projected density of states (PDOS)** for a **single water molecule in an empty (non-periodic) simulation box**, computed using CP2K. The computational parameters used for these calculations are those described in **README.md** and **input.inp**, and define the reference setup for the first set of results shown below.

In the following sections, several key parameters are systematically varied, namely `BASIS_SET`, `CUTOFF`, `EPS_SCF`, and `XC_FUNCTIONAL`, in order to assess their respective impact on the computed PDOS.

<p align="center">
  <img src="images/01_PDOS_O_H.png" width="80%">
</p>

<p align="center">
  Figure 1 — PDOS (projected on the different atom types) of an isolated H₂O molecule.
</p>

## Description

The PDOS is decomposed into atomic-orbital contributions (s, p, and d), allowing a clear identification of the molecular orbitals of water:

- The **occupied molecular orbitals** (e.g. 1b₂, 3a₁, 1b₁) appear at negative energies relative to the Fermi level and are dominated by **O p-character**, as expected for water.
- The **deep valence state** (2a₁) shows a strong **s-character**, reflecting its largely O 2s nature.
- At positive energies, a set of **unoccupied states** is visible. These correspond to antibonding molecular orbitals and to additional **artificial (added) MOs** included to stabilize the SCF procedure and enable analysis of the unoccupied DOS.

This simple molecular example serves as a reference for understanding how CP2K basis sets, added molecular orbitals, and projection schemes affect the DOS/PDOS, before moving to more complex systems such as metal/water interfaces.

<p align="center">
  <img src="images/01_PDOS_O_H_spd.png" width="80%">
</p>

<p align="center">
  Figure 2 — PDOS of an isolated H₂O molecule. Energies are referenced to the Fermi level.</em>
</p>


