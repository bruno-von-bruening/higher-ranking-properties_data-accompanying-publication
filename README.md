# Data for *Benchmarking higher-ranking multipoles and polarizability tensors for small molecular systems*

## Accompanying Publication

This repository hosts the data for the publication *Benchmarking higher-ranking multipoles and polarizability tensors for small molecular systems*.

**TODO: Add DOI, full citation (authors, journal, year) and ideally a BibTeX entry.**

## Installation

Set up a Python environment that contains the necessary packages. These are listed in `conda_env.yaml`, which can also be used to create a conda environment directly:

```bash
conda env create -f conda_env.yaml
conda activate conda_env_to_view_paper # Or choose this environment in jupyter notebook
```

## Data

### Data Files

The following datasets can be found in the `data/` directory. Each file contains either properties for a specific combination of method and basis set, or the corresponding errors between a trial and a reference property.

#### Properties

Basis set convergence at the correlated level:

* `props_correlated_level_basis_set_convergence.json`

Main dataset (73 compounds), evaluated with the production-level basis set (see paper):

* `props_cc_reference_main_dataset.json`: CCSD(T) reference values
* `props_correlated_methods_main_dataset.json`: CCSD(T), CCSD, MP2, and HF
* `props_all_methods_main_dataset.json`: all evaluated methods, i.e. CCSD(T), CCSD, MP2, HF, and all DFAs

Basis set study at the DFA level:

* `props_dfas_with_smaller_basis.json`: properties of selected DFAs evaluated with various basis sets

#### Errors

* `errors_full_dataset.json`: errors of CCSD, MP2, HF, and DFAs (main basis) with respect to the CCSD(T)/main-basis references
* `errors_basis_set_convergence_dataset.json`: errors of CCSD(T), CCSD, MP2, HF, and various DFAs with respect to the same method paired with aug-cc-pwCV5Z (multipoles) and t-aug-cc-pV5Z (polarizabilities)
* `errors_dfas_with_smaller_basis.json`: errors of selected DFAs evaluated with different basis sets with respect to the CCSD(T) references. This file also includes methods that having scaling to the CCSD(T) reference applied (see Table S6 and Figure S13 in Supporting Information), which are marked by the suffix `_scale-cc`.


### Data Structure

Each data point is indexed by the specific compound and level of theory:

* `method`: correlated method or DFA used for the calculation
* `basis`: basis set used for the calculation. Note that `basis='main_basis'` refers to d-aug-cc-pVQZ with cW core polarization functions for Si, P, and S, which is used as the main basis in the corresponding paper.
* `InChI`: unique identifier for each compound. Search PubChem (`https://pubchem.ncbi.nlm.nih.gov/`) for `InChI=<the-InChi>` to find the compound.
* `molecular_formula`: molecular formula, see listing of all avaiable ones Table S1 in the Supporting Information. The
  molecular structure are named in accordance with Hait and Head-Gordon's work,<sup>1,2</sup> from which the compounds are sampled.

#### Property Columns

Datasets containing properties provide them in the following columns (all in traceless Cartesian form and atomic units):

* `D`: dipole moment (ordered: x, y, z)
* `Q`: quadrupole moment (ordered: xx, xy, yy, xz, yz, zz)
* `DDP`: dipole–dipole polarizability tensor diagonal (ordered: x, y, z)
* `QQP`: quadrupole–quadrupole polarizability tensor diagonal (ordered: xx, xy, yy, xz, yz, zz)

#### Error Columns

Datasets containing errors 20 columns: for each property (tagged `D`, `Q`, `DDP`, `QQP`, as above) the following five indicators are provided:

* `rel-prc-aniso`: relative anisotropic error in percent
* `rel-prc-iso`: relative isotropic error in percent
* `mgn-ref-au`: magnitude of the reference in atomic units
* `mgn-try-au`: magnitude of the trial property in atomic units
* `mgn-diff-au`: magnitude of the difference tensor (element-wise subtraction trial − reference) in atomic units

Errors and magnitudes are defined in Section 4.1 of the paper.


## Loading the Data and Conversion to Spherical Harmonics

The data can loaded easily into python through, e.g.:
```python
  import pandas as pd
  pd.read_json(<path-to-the-relevant-file>)
```
Have a look at the Jupyter notebook `view_data.ipynb` to see how to load the data and convert it to spherical harmonic (spherical tensor) representation as by Stone defined in [3] (e.g. Appendix E).

## Example Finite-Difference Calculations

Example input files can be found in `example_psi4_input_files/`. The provided `.psi4inp` files can be run with:

```bash
psi4 <input>.psi4inp
```

All calculations were performed with Psi4 version 1.10.

The following calculations are provided (each named `<method-name>_<basis-set>_<compound>`):

* `B97-3_aug-pcseg-2_H2O`
* `B97-3-GRAC_aug-pcseg-2_H2O`
* `CCSD(T)_main_basis_H2`
* `CCSD(T)_main_basis_H2S`: note that this calculation takes considerable time to run (around 4 hours)
* `CCSD(T)-conv_main_basis_H2`: conventional CCSD(T) calculation (without density fitting)

<!-- COMMENT: Consider giving an approximate runtime/hardware for the H2S CCSD(T) example. -->

# References
[1] Hait, Diptarka, and Martin Head-Gordon. "How accurate is density functional theory at predicting dipole moments? An assessment using a new database of 200 benchmark values." *Journal of chemical theory and computation* 14.4 (2018): 1969-1981.

[2] Hait, Diptarka, and Martin Head-Gordon. "How accurate are static polarizability predictions from density functional theory? An assessment over 132 species at equilibrium geometry." *Physical Chemistry Chemical Physics* 20.30 (2018): 19800-19810.

[3] Stone, Anthony J., and A. J. Stone. *The theory of intermolecular forces.* Vol. 2. Oxford: Oxford University Press, 2013.
