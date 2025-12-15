# Results - Single water molecule

This section presents the **projected density of states (PDOS)** for a **single water molecule in an empty (non-periodic) simulation box**, computed using CP2K. The computational parameters used for these calculations are those described in **README.md** and **input.inp**, and define the reference setup for the first set of results shown below.

In the following sections, several key parameters are systematically varied, namely `BASIS_SET`, `CUTOFF`, `EPS_SCF`, and `XC_FUNCTIONAL`, in order to assess their respective impact on the computed PDOS.

## PDOS of a single water molecule

Figure 1 shows the PDOS of an isolated H₂O molecule, decomposed into contributions from **oxygen (O)** and **hydrogen (H)** atoms. Energies are referenced to the Fermi level.

<p align="center">
  <img src="images/01_PDOS_O_H.png" width="80%">
</p>

<p align="center">
  Figure 1 — PDOS (projected per atom-type) of an isolated H₂O molecule.
</p>

The **occupied states** at negative energies correspond to the well-known molecular orbitals of water. The deep-lying **2a₁** state exhibits a mixed contribution from both oxygen and hydrogen atoms, reflecting its bonding character involving O–H interactions. The valence orbitals **1b₂** and **3a₁** are dominated by oxygen character with a smaller hydrogen contribution, consistent with O–H bonding and hybridization. The **highest occupied molecular orbital (HOMO)**, labeled **1b₁**, is almost entirely oxygen-centered, in agreement with its largely non-bonding O 2p character.

At positive energies, the **unoccupied molecular orbitals** (**4a₁**, **2b₂**) become visible. These states exhibit increased hydrogen character, reflecting their antibonding nature. At higher energies, additional peaks appear that correspond to **artificial (added) molecular orbitals**, which are included to stabilize the SCF procedure and to enable analysis of the unoccupied part of the PDOS.

<p align="center">
  <img src="images/01_PDOS_O_H_spd.png" width="80%">
</p>

<p align="center">
  Figure 2 — PDOS (projected per orbital-type) of an isolated H₂O molecule for (left) O, and (right) H atoms.
</p>

- The **occupied molecular orbitals** (e.g. 1b₂, 3a₁, 1b₁) appear at negative energies relative to the Fermi level and are dominated by **O p-character**, as expected for water.
- The **deep valence state** (2a₁) shows a strong **s-character**, reflecting its largely O 2s nature.
- At positive energies, a set of **unoccupied states** is visible. These correspond to antibonding molecular orbitals and to additional **artificial (added) MOs** included to stabilize the SCF procedure and enable analysis of the unoccupied DOS.

## Impact of the 'CUTOFF' parameter




