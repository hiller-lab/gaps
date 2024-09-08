# GAPS: Gene Annotation Pipeline from sequence and structure Similarity

---

## Installation

### Software requirements

#### Python

##### Install miniconda

Miniconda is a free minimal installer for conda. It is a small bootstrap version of Anaconda that includes only conda, Python, the packages they both depend on, and a small number of other useful packages.

To check if miniconda is installed on your system, open the terminal and type:

```
conda
```

If miniconda is installed, this command should display relevant information. If it's not installed, you'll likely get a "command not found" error. In the latter case, you may need to contact your cluster administrator or follow your cluster's specific installation instructions to set up miniconda (installation guide here: https://docs.conda.io/projects/miniconda/en/latest/miniconda-install.html)

##### Build the required miniconda environment

A Conda environment is a self-contained and isolated environment within which you can install specific versions of packages, libraries, and dependencies required to run code. Environments allow you to manage and control the software configuration for a particular project, ensuring compatibility and reproducibility, and avoiding conflicts between different software requirements.

To run the GAPS pipeline, you need to build a specific environment. Use the terminal or an Anaconda Prompt for the following steps:

1. Navigate to the folder that contains the `gaps_environment.yml` file.

2. Create the environment from the `gaps_environment.yml` file:
```
conda env create -f gaps_environment.yml
```

> The first line of the `yml` file sets the new environment's name. In the default case it is: `gaps_env`.

3. Verify that the new environment was installed correctly:
```
conda env list
```

> `gaps_env` should appear in the list.

4. Activate the new environment:
```
conda activate gaps_env
```

##### Use a different miniconda environment

Advanced users may want to use/build their own environment to run the pipeline. Make sure that the following modules are installed:

- python>=3.8
- numpy
- pandas
- openpyxl
- biopython
- matplotlib
- json
- csv


#### SLURM

SLURM, which stands for "Simple Linux Utility for Resource Management," is an open-source, highly configurable workload management system used in high-performance computing (HPC) environments. It is designed to efficiently manage and allocate computing resources such as processors (CPU cores), memory, and GPUs in a cluster or supercomputing environment. SLURM is widely used in academic, research, and scientific computing settings for scheduling and managing batch jobs and parallel computing tasks. More about SLURM: https://slurm.schedmd.com/quickstart.html

To check if SLURM (Simple Linux Utility for Resource Management) is installed on your cluster, open the terminal and type:

```
sinfo
```

If SLURM is installed, this command should display information about the nodes in your cluster. If it's not installed, you'll likely get a "command not found" error. In the latter case, you may need to contact your cluster administrator or follow your cluster's specific installation instructions to set up SLURM.

#### Modules and module systems

A 'module' in HPC computing is a self-contained unit that encapsulates a specific software package, library, or tool along with its associated environment settings. It allows users to create isolated and controlled software environments within the HPC cluster. These modules can be loaded or unloaded as needed, enabling users to access and use different software packages without conflicts or interference with other installations. These modules are part of a 'module system', which is a software tool used to organize and control the installation and usage of various software applications, libraries, compilers, and tools on an HPC cluster.

By means of the module system, all software currently available on your cluster can be listed, loaded, and unloaded, by using the command module. The specific name or command to load modules can vary depending on the module system being used on a particular cluster; however, most popular module systems use the following command:

```
module load
```

To display a list of all available modules (or specific module):

```
module spider
module spider MODULENAME
```

Make sure that the module system installed on your system use the `module load` command; otherwise, the python code of the pipeline will need changes.


##### HHblits

HHblits is a bioinformatics tool and software package that comes with the HH-suite module, used for the sensitive detection of remote homologs in protein sequences. It is an advanced method for searching sequence databases and identifying relationships between proteins that might not be readily apparent through traditional sequence alignment methods.

Make sure that HH-suite is installed by typing the following command in the terminal:

```
module spider HH-suite
```

If it's not installed, you'll likely get a "module not found" error. In this case, make sure that your local module system does not use a different name than `HH-suite` (if this is the case, the python code of the pipeline will need changes). If the module is really missing, you may need to contact your cluster administrator or follow your cluster's specific installation instructions to set up this module.

##### AlphaFold

AlphaFold is a groundbreaking artificial intelligence (AI) system developed by DeepMind, a subsidiary of Alphabet Inc. (Google's parent company), that is used for predicting the 3D structures of proteins with remarkable accuracy. Understanding the 3D structures of proteins is crucial in biology and biochemistry because a protein's function is often closely tied to its shape.

Make sure that AlphaFold is installed by typing the following command in the terminal:

```
module spider AlphaFold
```

If it's not installed, you'll likely get a "module not found" error. In this case, make sure that your local module system does not use a different name than `AlphaFold` (if this is the case, the python code of the pipeline will need changes). If the module is really missing, you may need to contact your cluster administrator or follow your cluster's specific installation instructions to set up this module.

##### Foldseek and MMseq2

Foldseek is a a bioinformatic tool that enables fast and sensitive comparisons of large protein structure sets. It is a method for searching protein structure databases and identifying relationships between proteins that might differ in amino acid sequence but have a very similar fold.

Make sure that Foldseek is installed by typing the following command in the terminal:

```
module spider Foldseek
```

As Foldseek depends on MMseq2, the latter also needs to be installed. Check its presence with the command:

```
module spider MMseq2
```

If these are not installed, you'll likely get a "module not found" error. In this case, make sure that your local module system does not use a different name than `Foldseek` or `MMseq2` (if this is the case, the python code of the pipeline will need changes). If the modules are really missing, you may need to contact your cluster administrator or follow your cluster's specific installation instructions to set up these modules.

### Database requirements

By default, the GAPS pipeline makes use of the databases:

- AlphaFold-proteome (Foldseek)
- AlphaFold-Uniref50 (Foldseek)
- CATH (HHblits)
- NCBI_CD (HHblits)
- Pfam (HHblits)
- PDB (AlphaFold, Foldseek)
- PDB70 (HHblits)
- Uniclust30/Uniref30 (HHblits)
- PHROGs (HHblits)

These databases are mostly software specific, i.e. AlphaFold, HHblits and Foldseek all require databases of different types. Please check the documentation of each of these softwares to learn more about where and how to download the correct databases. E.g.:

- https://github.com/soedinglab/hh-suite
- https://github.com/steineggerlab/foldseek

These databases are required locally, i.e. they need to be downloaded and installed for the pipeline to work.

The full filepaths to these databases will need to be entered in the GAPS source code, in the appropriate code blocks of the Jupyter nodebook. Additional databases can also be added by modifying the source code (Jupyter notebook).

### Other requirements

These databases in the form of multisequence FASTA files are also used by GAPS:

- PDB
- RefSeq
- SwissProt

These files are the same that are required by BLAST to perform a search, although BLAST itself is (currently) not used in GAPS.
These files are required locally, i.e. they need to be downloaded and installed for the pipeline to work.

Additionally, the pdb_seqres.txt file must be downloaded, unzipped and moved to the other_files folder. Download from: https://www.rcsb.org/docs/programmatic-access/file-download-services

<br />

---

## Execution

GAPS is written as a Jupyter notebook because it allows to show the entire process in one file. It also allows readers and users to interpret the code easily, modify it, and execute it one step at a time (if needed, all in one go). The results are stored and displayed together with the code that was used to generate them, enabling logging and transparency.

The notebook `GAPS_Escherichia_virus_HeidiAbel.ipynb` serves as an example and as a starting point for using GAPS in your own project. Copy and modify it according to your needs.

Instructions on how to run GAPS is provided within the GAPS notebook.

Test input files are provided in the folder `test_input_files`.

<br />

---

## References

To be updated.

