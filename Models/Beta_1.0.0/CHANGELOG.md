# M3HighResBeta: High-Resolution Martini 3 Protein Model

**Current version:** `beta_1.0.0` (Oct 2026) · **Force field ID:** `M3HighResBeta` · **License:** Apache 2.0

> **Please cite:**\
> L. Borges-Araujo, et al., 2026 (in preparation)\
> P.C.T. Souza, et al., *Nat. Methods*, 2021. DOI: [10.1038/s41592-021-01098-3](https://doi.org/10.1038/s41592-021-01098-3)
>
> See `force_fields/M3HighResBeta/citations.bib` for full references.

---

## Changelog
### [beta_1.0.0] - Beta release - Oct 2026
This beta version is mostly based on 033, with additional stability upgrades and improved modified terminals.
#### Added
 - Added Apache 2.0 license.
 - Added description headers to all force field, mapping and .itp files, and Apache license headers to the force field and mapping files.
 - Added `[ citations ]` to the force field files, so martinize2 now lists the model and Martini 3 papers in generated topologies.
#### General
 - Model ID changed from martini3007 to M3HighResBeta.
 - Introduced placeholder citation in citations.bib.
 - General file structure cleanup.
 - General file cleanup.
 - Reformatted .ff files: consistent section banners, aligned columns, trailing whitespace removed.
 - Removed commented-out code blocks (unused C-terminal angle tweak, polyply build bonds, HIS tautomer modifications).
#### Modified
 - Reverted ARG to 033.
 - Reverted TRP to 033.
 - Coil CMAP resolution decreased from 45x45 to 24x24 for stability. Thanks Fran.
#### Fixed
 - D-amino acid residue lists in the dihedral files referenced ARG0 instead of DARG0.
 - N-ter, NH2-ter and NACE-ter dihedrals were labelled with the C-TER group in generated topologies. Labels only; no parameter changes.
 - Citation entries in citations.bib reformatted so vermouth can parse them.
   
### [0.3.5.WIP.0] - Alpha release - May 2026
#### Modified
 - Reverted ARG to the default Martini 3 Protein model mapping, with newly fitted bonded parameters.
 - Made terminal Ryckaert-Bellemans dihedral potential terms sum to zero. No changes to potential shape.
 - Minor fixes to mapping files.
 - ARG SC1 bead changed from SC3 to SC4. Should improve ARG interactions with membrane phosphodiester
   region.
 - TRP SC3 bead swapped from TC5e to TC5v. SC1 from TC4 to TC4v.
 - Minor modifications to terminal bead types. Should reflect: charged COO⁻ ≫ amide C-terminus (CT2) ≈ N-methylamide (CT3) ≈ internal backbone > neutral COOH > methyl ester (CT1)

### [0.3.4.0.0] - Alpha release - February 2026 
#### Modified
 - Swapped terminal N-CA-C-O dihedral from an improper potential to a Ryckaert-Bellemans potential for increased stability. Ref angle changed from -30 to 0. Should better reproduce terminal dynamics.

### [0.3.3.0.0] - Alpha release - October 2025 
#### Modified
 - Adjusted N terminal bead types. TQ5p to SQ4p. SP1/2/2a to TP1. Hoping it helps with dipeptide aggregation where terminals play an important role.
 - Added General dihedral potentials for GLY, PRO and X-X-PRO. Note: There might be an edgecase where an l amino precedes a d PRO and vice versa. This will be revisited later.
 - Small changes to C & N-terminals to improve stability (improper dihedral strength and ref angle adjusted).
 - Increased masses of C and N terminal beads from 32 to 36 a.u. for some stability gains. Fixed masses of some terminal modifications which were incorrectly assigned.
 - Slight modification to the cmap bonded potentials to avoid adding them to the 1st and last residue of a protein. Want to avoid overstraining the terminals as they seem to be the weakest link in terms of numerical stability. They should also have a bit more freedom compared to mid residues.
 - CMAP has been turned back on with a beta version of it. Needs more sampling for the final cmap.
 - CA -O tuned from 25k to 20k kJ/mol to increase LINCS stability.
 - N-CA-C was reprofiled from 112.5 @ 275 to 110.5 @ 400 kj/mol/rad. Further increases LINCS stability as it helps avoid two neighboring triangles to be in the same plane and as such LINCS convergence becomes easier.
 - Adjusted terminal charge.

### [0.3.2.0.0] - Alpha release - September 2025 
#### Modified
 - Yet another mega optimization of all dihedrals and backbone bonded parameters.
 - Added coil and helical dihedrals for residues pre Proline.
 - CMAP turned off just while running the validation.
 - Tweaked the C - O distance in C-term residues from 0.12 to 0.13. This helps improve the stability of SC dihedrals tied to the SC1 - CA - O  angle.
   
### [0.3.1.0.0] - Alpha release - August 2025 
#### Modified
###### General
 - Changed order of backbone beads to match AA atom names. Thanks Manel.
 - Expanded the changes to the Daminos.

###### BB bead type changes
- Increased polarity of ALA BB (SP1 -> SP2).
- Increased polarity of GLY BB (SP2 -> SP3).
- PRO BB changed from SP1a to SP1r. Maintains reduced self interaction from lack of HB donor capabilities but maintains same interaction level with water as SP1. Helps with IDP/IDRs.

###### SC bead type changes
 - ARG SC bead types changed from TN4 and 2xTN6q(0.5) to 3xTP1q(0.3,2x0.35). Previous model was too hydrophobic, causing issues with IDPs & Peripheric membrane proteins.
 - HIS TN5a to TN6a. Increase polarity of HIS a bit, as recomended by WW and TPMFs.

###### Bonded setup changes.
 - CA - CA distance adjusted slightly from 0.379 to 0.384.
 - CA - O distance adjusted slightly from 0.238 to 0.240.
 - Further refined alpha helix phi/psi dihedrals.
 - Coil Phi/Psi pairs completely overhauled. Introduced CMAP correction for IDR regions. WARNING: These changes have not yet been applied to DAminoacids.



### [0.2.3.0.0] - Alpha release
#### Modified
 - Fixed issue with terminal partial charges where users could end up with non integer charges. Thanks Fabian. 
 - Add bb_atomname force field variable. Thanks Chris.
 - Added disulfide bridge logging. Thanks Chris.
 - Fixed N-term modifications to work with PRO. Thanks Fabian.
   
### [0.2.2.0.0] - Alpha release, update #6 - February 2025 
#### Added
 - Introduced GLUP, GLH, DGLP; ASPP, ASH, DAPP; LSN, LYN, DLSN charge states of GLU, ASP, LYS. Thanks Tugba.
#### Modified
 - Added M3 Go reference to citations.bib. Thanks Thilo.

### [0.2.1.0.0] - Alpha release, update #5 - February 2025 
#### Modified
 - Introduced exclusion that was missing on TRP ring and was causing instabilities.
 - Further increased strength of coil secundary structure dependent dihedral (+10%, 135% of optimal fit).

### [0.2.0.0.0] - Alpha release, update #4 - February 2025 
#### Added
 - Introduced D-Aminoacids. Nomenclature Dxxx, where xxx is the regular 3 letter aa code.
 - Introduced BB phi/psi dihedrals for D-aminoacids.
 - Further refined the terminals to improve accuracy and stability.
 - Adjusted bonded potentials on LEU, GLU, DLEU, DGLU to improve stability when part of terminal residues.
#### Modified
 - Concatenated all .map files into single files.
 - ILE changed to C1r. ALA changed from TC3 to TC2. Both changes should improve Prot-CHOL interactions. ILE slightly less self interaction compared to LEU. Keeps trend of ALA < VAL < ILE < LEU in terms of hydrophobicity.
 - GLY backbone changed from SP1 to SP2 to reveal naked amide character.
 - e label added to TC5 bead in th
 - e middle of TRP. Should improve cation-pi with TRP without much the drawbacks observed from using e labels on all TRP beads.
 - ARG bead types changed from TN4/TN6 to TN1. Should improve cation-pi & membrane interactions.
 - Strength of secondary structure dependent dihedrals increased by 25% to improve IDP expansion & alpha helix curvature. General dihedrals, beta, turn and bend were kept the same. 

### [0.1.3.0.0] - Alpha release, update #3 - October 2024 
#### Modified
 - Adjusted sidechain bonded terms of ALL residues to increase stability and rotamer accuracy following what was done for the aromatic residues.
 - Changed file organisation, separating links, dihedrals and aminoacid definitions onto different files.
 - Yet another round of implementation cleanup and futureproofing. Greatly reduced .itp file length by cleaning up exclusion syntax.
 - Removed all eh labels.
 - PRO and HYP BB bead type changed from SP1 to SP1a.
 - Removed r labels from LEU and ILE. Should strengthen prot-prot interactions further.
 - Updated coil dihedrals
 - Updated helix psi & phi
 - Corrected the definition of the phi dihedral. This should center both phi and psi correctly around the target residue Calpha. Previously it was defining phi on the i+1 residue and psi on the i residue, now both are correctly applied on top of i. This should not impact most systems, but will slightly improve small peptides that may not have any tertiary bias applied. Terminals will slightly improve, I think.

### [0.1.1.0.0] - Alpha release, update #1 - April 2024 
#### Added
 - Introduced more new protein terminals. 
     - CT1, CT2, CT3.
     - NACE.
 - Introduced new ARG sidechain model.
 - Added cys bridges (Thanks Fabian).
 - Added PHI-PSI dihedrals to be added by default if -dssp and -ss are not called for. These will be the default for OLIVES use (Thanks Fabian & Kasper).
 - Added HIS tautomers (Thanks Kasper).
 - Added Citations.bib (Thanks Fabian).
 - Added extra bonded parameters to certain sidechains to improve rotamer behavior (PHE, TYR, TRP, ARG). (Inspired by Kasper's issue with some TRP rotamers.)
#### Modified
 - Adjusted sidechain bonded terms of aromatic residues to increase stability (PHE, TYR, TRP). They now share the same reasoning behind their bonded terms and have added input for increased stability.
 - Implementation cleanup and futureproofing (Thanks Fabian).
 - C-Terminals now correctly override the last CA - O bond. Fixed after modification fix in vermouth 0.10. Versions under 0.10 will not work correctly. (Thanks Fabian)
 - LEU is now C1r & ILE is now C2r. Should slightly improve lipid & CHOL binding.
 - VAL is now SC2 (previously SC3, now it is better distinguished from ALA. Attempting to keep the hydropathy trend of L > I >> V > A). 

### [0.1.0.0.0] - Alpha release - February 2024 - LBA
#### Modified
 - Adjusted backbone masses to increase stability.
 - Adjusted stiff_fc from 100000 to 30000.
 - Adjusted alpha helix dihedral parameters. Now also using proper dihedrals like the remaining SS motifs.
 - Proline N partial charge moved to C. Mimics loss of amide hydrogen in proline. Reduces HBond estabilishment and reduces dipole.
 - Proline bonded parameters further refined from CHARMM, AMBER & PDB references.
 - Added specific dihedral parameters for proline (Coil & helix for now.)(WIP - Helix has only custom PSI.).
 - Added specific dihedral parameters for glycine.
 - ARG bonded parameters weakened.
#### Added
 - Introduced dFLEXIBLE flag to facilitate minimization.
 - Introduced new protein terminals. 
     - Neutral terminals -COOH -NH2.
     - Charged terminals.
 - Introduced amber forcefield mappings.
 - Introduced hydroxyproline. (WIP - No amber mappings yet. Bonded params same as PRO except for SC1 - BB bond which has been adjusted to account for hydroxyl.)
 - Introduced neutral GLY ASP LYS.
 - Added eh labels to TC5 beads in aromatic residues. Following directions from Ilias' and Liguo's work.
 
### [0.0.3.9.4] - 2023-06-20 - LBA
#### Modified
 - Getting rid of -DSTABILIZE flag. All CA-SC1 bonds are now harmonic without need for flag.
 
### [0.0.3.9.3] - 2023-06-12 - LBA
#### MAJOR
 - Improved COIL, BEND & TURN dihedrals. Should improve IDP & elastic network performance.
 - Fully revamped Martinize implementation. Fully fixes Martinize 2 issues.
#### Modified
 - Changed BB bead names to AA names. Makes most AA tools simply work.
 
### [0.0.3.9.0] - 2023-05-03 - LBA
#### MAJOR
 - Substantially improved the bonded parameters of all sidechain residues.
#### Modified
 - Expanded LEU, ISO SC bond lengths by 15% to reduce stickyness.
 - Kept LEU and ISO as regular beads with r label.
 
### [0.0.3.8.6_f] - 2023-04-26 - LBA
#### Reverted
 - Reverted on ISO, LEU size increase. Back to S SC beads.
#### Modified
 - Expanded LEU, ISO SC bond lengths by 15% to reduce stickyness.
 - Modified bonded parameters.
 
### [0.0.3.8.6_e] - 2023-04-24 - LBA
#### Reverted
 - Reverted on ISO, LEU size increase. Back to S SC beads.
#### Modified
 - Expanded LEU, ISO SC bond lengths by 15% to reduce stickyness.
 
### [0.0.3.8.6_d] - 2023-04-24 - LBA
#### Reverted
 - Reverted on r label on all C  beads. Kept only on SC.
#### Modified
 - Expanded LEU, ISO SC bond lengths by 15% to reduce stickyness. Kept R beads with r label.
 - Partial Charge set to 0.05.

### [0.0.3.8.6_c] - 2023-04-24 - LBA
#### Reverted
 - Reverted on r label on all C  beads. Kept only on SC.
#### Modified
 - Expanded LEU, ISO SC bond lengths by 15% to reduce stickyness. Kept R beads with r label.
 - Partial Charge set to 0.0 (Effectively model #1).
 
### [0.0.3.8.6_a] - 2023-04-20 - LBA
#### Reverted
 - Reverted on r label on all C  beads. Kept only on SC.
#### Modified
 - Expanded LEU, ISO SC bond lengths by 15% to reduce stickyness. Kept R beads with r label.
 
### [0.0.3.8.6_b] - 2023-04-20 - LBA
#### Reverted
 - Reverted on r label on all C  beads. Kept only on SC.
 - Reverted on ISO, LEU size increase. Back to S SC beads.
 
### [0.0.3.8.5] - 2023-04-11 - LBA
#### Modified
 - Adjusted SC1-CA -N  angle from 109.5 to 111.5 to match PDB distros.
 
### [0.0.3.8.4] - 2023-04-11 - LBA
#### Added
 - Added r label to all C  beads to reduce protein-protein interactions.
 
### [0.0.3.8.3] - 2023-04-05 - LBA
#### Added
 - First CHANGELOG for model #3.
 - Added r label to SC beads that had S->R size increase (LEU, ISO). For now, kept ASN without an r label. This can be revisited later.

