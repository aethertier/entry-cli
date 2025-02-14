Entryway analysis
=================

The eNTRy rules are a series of guidelines that can increase small-molecule
accumulation in gram-negative bacteria. A molecule is likely to accumulate if it
contains few rotatable bonds, low three dimensionality, and an ionizable
nitrogen.

The webapp [entry-way](http://www.entry-way.org) was created to aid in the application of these guidelines
by performing the necessary predictions of physiochemical properties. Although
freely available at entry-way.org, these same calculations can be performed
locally.


Installing the pakage
---------------------

Entryway relies on [OpenBabel](http://openbabel.org) and [RDKit](https://www.rdkit.org/) for handling chemical 
structures and conformer generation and NumPy for globularity calculations. These dependencies are most conveniently 
installed via [Conda](https://conda.io/docs/user-guide/install/index.html). The included `environment.yml` 
file makes this straightforward:

```sh
# Download source
git clone https://github.com/aethertier/entry-cli.git
cd entry-cli

# Prepare conda environment
conda env create -f environment.yml
conda activate entry-cli-env

# Install module 'entry_cli'
make install
# To uninstall the package later, run 'make uninstall'
```


Running calculations
--------------------

Although other file formats will be implemented shortly, molecules can be submitted as SMILES strings for now.
This can be achieved through command line arguments:

```sh
# Ampicillin
entry-cli -s "O=C(O)[C@@H]2N3C(=O)[C@@H](NC(=O)[C@@H](c1ccccc1)N)[C@H]3SC2(C)C"

# Deoxynybomycin
entry-cli -s "CC(C1=CC(C(C)=CC(N2C)=O)=C2C3=C1N4CO3)=CC4=O"
```

Alternatively, a batch file can be provided that contains several molecules with SMILES and names. Results are reported 
as a csv file. The name of the output file is optional. If no output file is specified, then the base name of the batch
file is used:

```sh
# The following will provide the same result
entry-cli -b tests/b-lactams.smi -o tests/b-lactam.csv
entry-cli -b tests/b-lactams.smi
```

Additional commnd line commands can be shown using the `--help` option:

```
usage: entry-cli [-h] [-s SMILES string | -b Batch file] [-o Output file]
                 [--conf-cutoff <int>] [--rmsd-cutoff <int>]
                 [--energy-cutoff <int>]

Performs calculation of physiochemical properties of potential antibiotics.
SMILES strings are parsed, conformers are generated, and properties
calculated. Properties include: chemical formula, molecular weight, rotatable
bonds, globularity, and PBF.

optional arguments:
  -h, --help            show this help message and exit

Input & Output Options:
  -s SMILES string, --smiles SMILES string
  -b Batch file, --batch Batch file
  -o Output file, --output Output file
                        Defaults to csv file with same name as input

Conformer Generation Options:
  --conf-cutoff <int>   Max number of conformers to generate, default: 100,000
  --rmsd-cutoff <int>   Similarity threshold for conformers, default: 0.5
  --energy-cutoff <int>
                        Max relative energy between conformers, default: 50
```

Alternatively, the functions can be imported as a python module.

```python 
from entry_cli.calc_props import smiles_to_ob, average_properties
```


Citing
------

Please cite our paper on the eNTRy rules:

[Richter, M. F.; Drown, B. S.; Riley, A. P.; Garcia, A.; Shirai, T.; Svec, R. L.; Hergenrother, P. J. *Nature* __2017__,
*545*, 299-304.](https://doi.org/10.1038/nature22308)
