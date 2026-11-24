# High-Resolution Martini 3 Protein Model

**Open beta** · Current version: `beta_1.0.0` (Oct 2026) · Force field ID: `M3HighResBeta` · License: Apache 2.0

This repository is the public open beta of the high-resolution Martini 3 protein backbone and sidechain model developed within the Martini Protein Task Force (PTF). It distributes the force field, mapping and topology files needed to coarse-grain proteins with `martinize2`.

> **This is a beta.** Parameters may still change between releases, and things can and will blow up. Please report problems (see [Reporting issues](#reporting-issues)).

## Citation

If you use this model, please cite:

- L. Borges-Araujo, et al., 2026 (in preparation)
- P.C.T. Souza, et al., *Martini 3: a general purpose force field for coarse-grained molecular dynamics*, *Nat. Methods*, 2021. DOI: [10.1038/s41592-021-01098-3](https://doi.org/10.1038/s41592-021-01098-3)

Full references are in [citations.bib](Models/Beta_1.0.0/force_fields/M3HighResBeta/citations.bib). The force field files also carry `[ citations ]` sections, so `martinize2` lists these papers in generated topologies.

## Requirements

1. **GROMACS 2023.3 or newer.** Earlier versions have constraint and virtual-site convergence issues that lead to crashes.
2. **Martinize2 compatibility.** We recommend installing the stable PyPI release of Martinize2 (provided by the `vermouth` package), rather than the development version directly from GitHub:

   ```bash
   pip install vermouth==0.15.0
   ```

   Versions older than 0.14.0 may produce incorrect `.itp` parameters. The GitHub development version currently has known compatibility issues with terminal residue assignment.

3. **The supplied `martini_v3.0.0_HighResBeta.itp`.** It leaves the default Martini 3 interactions unchanged, but adds the dummy `Up` bead type and the `CMAP` definitions. Simulations will not run with the stock `martini_v3.0.0.itp`; `grompp` will error out on the missing `CMAP` types.

### Known limitations — terminal residues

The current open-beta release has some known compatibility issues with terminal modifications in Martinize2.

- **Standard L-amino-acid termini:** Expected to work correctly with Martinize2 versions `0.14.x` and `0.15.0`.
- **D-amino acids and hydroxyproline (HYP) at terminal positions:** Terminal modifications may not be correctly applied.
- **Martinize2 development version (GitHub):** Currently has compatibility issues that may result in missing terminal modifications, even for standard amino acids.

Users should carefully check Martinize2 warnings and verify that the expected terminal modifications are present in the generated `.itp` files.

Please report unexpected behavior through GitHub Issues. These limitations are being investigated and will be addressed in future updates.

## Martinizing a protein

```bash
martinize2 -v \
    -f protein.pdb -x cg.pdb -o topol.top \
    -dssp -elastic -ef 700 -eu 0.85 -eb CA -noscfix \
    -ff M3HighResBeta \
    -ff-dir Models/Beta_1.0.0/force_fields \
    -map-dir Models/Beta_1.0.0/mappings
```

`-noscfix` is needed because the force field does not define side-chain angle/torsion corrections; without it martinize2 stops on a `missing-feature` warning.

The generated `topol.top` includes `martini.itp`. Replace that line with `#include "martini_v3.0.0_HighResBeta.itp"` (or copy the supplied file next to `topol.top` under the name your topology expects).

### Structure biases

- **Peptides:** no tertiary bias is needed. Use `-dssp` or `-ss`.
- **Elastic network:** `-elastic -ef 700 -eu 0.85 -eb CA`. `-dssp` or `-ss` is not necessary. These defaults are still being optimized.
- **Go model:** `-go -go-atomname VS -go-eps 6 -go-backbone CA`. `-dssp` or `-ss` is not necessary. Defaults are still being optimized.
- **OLIVES network:** see [OLIVES](#olives-network) below.
- **IDPs / IDRs:** define the structure as coil with `-ss CCCC...` and add `-id-regions -idr-tune`. This applies `CMAP` torsion corrections that improve backbone conformations, IDP size and dynamics.

### OLIVES network

[OLIVES](OLIVES/OLIVES_v2.9.1_alpha_0.2.2.py) (Pedersen et al., 2024, DOI: [10.1021/acs.jctc.4c00553](https://doi.org/10.1021/acs.jctc.4c00553)) adds a Go-like network of hydrogen-bond-based contacts to a martinized protein. It needs `numpy`, `mdtraj` and `networkx`.

1. Run `martinize2` **without** `-dssp` or `-ss` (and without `-elastic`). OLIVES does not use the secondary-structure-biased bonded potentials. martinize2 will warn about the missing flag, so add `-maxwarn 1` (together with `-noscfix`).
2. Run the script on the resulting CG structure and topology:

   ```bash
   python3 OLIVES/OLIVES_v2.9.1_alpha_0.2.2.py -c cg.pdb -i molecule_0.itp \
       --ss_scaling 0.3 --ts_scaling 0.8
   ```

The recommended scalings for the secondary and tertiary networks are `--ss_scaling 0.3` (range 0.2-0.4) and `--ts_scaling 0.8` (range 0.7-1.0). The script also supports multistate input, quaternary (inter-chain) scaling and separate `.itp` output; run it with `-h` for all options. Default values are still being optimized.

## MDP recommendations

- Use `-DFLEXIBLE` during minimization.
- Slightly stricter settings than usual:
  ```
  dt                      = 0.01
  verlet-buffer-tolerance = -1
  lincs_order             = 12
  lincs_iter              = 2
  ```

## Reporting issues

Please open a GitHub issue and include:

- **Martinize failures:** your vermouth/martinize2 version, the non-working `.pdb`, and the *quick fix* working `.pdb` if you have one.
- **Numerical crashes:** your vermouth/martinize2 and GROMACS versions, plus a folder with all `.mdp`, `.itp`, `.top`, `.ndx`, `.gro` and `.pdb` files needed to reproduce.

### Martinize/vermouth failures

In preliminary tests, Martinize2 occasionally failed to introduce all necessary bonded parameters (in approximately 10% of the proteins examined), potentially leading to numerical instabilities or crashes. Some of these issues appeared to be associated with the quality of the starting protein structures.

GROMACS will warn you:

```
NOTE 2 [file topol.top, line 13]:
In moleculetype 'molecule_0' 1 atoms are not bound by a potential or constraint to any other atom in the same moleculetype. Although technically this might not cause issues in a simulation, this often means that the user forgot to add a bond/potential/constraint or put multiple molecules in the same moleculetype definition by mistake. Run with -v to get information for each atom.
```

A quick fix:

1. Quickly minimize/equilibrate your protein using CHARMM (no need for production). Often minimizing is enough.
2. If you designed your protein/peptide (for example with VMD), try obtaining a new design. The issue is caused by very minor differences in the initial input structure.

### Numerical crashes

Occasional numerical stability issues are still expected in this beta.

## License

Released under the [Apache License 2.0](LICENSE).
