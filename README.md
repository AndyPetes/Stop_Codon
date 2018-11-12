# Selection shapes synonymous stop codon use in mammals

One Paragraph of project description goes here

***

## Getting Started

**Command:** Rscript stopcodon.rscript <treefile.ph> <seqfile.fasta>

**Info:** The tree file should include branch lengths. Note that relative branch lengths are treated as fixed.
The sequence file should contain a codon-aware alignment in fasta format and must include the 
stop codon as the last position of the alignment. Sequences that include a gap or a codon other than
a stop codon (i.e. sequences for which the stop codon is not positionally homologous with the last 
position in the alignment) are excluded.

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes. See deployment for notes on how to deploy the project on a live system.

***

### Prerequisites

* Require tree file (".ph") and a codon-aware sequence alignment (".fasta"). Sample files can be simultaneously obtained from the "OrthoMam" database ("http://www.orthomam.univ-montp2.fr/orthomam/html/") which is a database of orthologous mammalian markers. Using the “Browse” tab, the “CDS” option at the top of the page was highlighted. Then the “submit” button at the base of the page was pressed. It is possible to limit the tree to specific species based on clicking the icon that accompanies each image, and chromosome by selecting the correct chromosome. Marker’s can be selected by ticking the requisite box in thefar left column. After selection, the accompanying maximun likelihood tree and nucleotide alignment file can be downloaded together by clicking the download icon at the base of the page.  Although it requires a longer computational time, it is advised to use a greater number of taxa in the alignment as the power to detect purifying selection acting on stop codon use is limited by the number of taxa.
* Require "R" packages "ape" and "expm":

These are then called usng the "require" function in the R script:

```
require(ape)
require(expm)
```

***

### Installing

The "ape" and "expm" packages are installed in the "R" environment using the "install.packages()" command.

```
install.packages("ape")
install.packaages("expm")
```

***

## Running the tests

The code is ran using the following command:

**Rscript stopcodon.rscript <treefile.ph> <seqfile.fasta>**

Where "seqfile.fasta" is a codon-aware alignment of the sequence data and the "treefile.ph" is a maximum likelihood estimate of the tree given the codon-aware alignment.

***


### Break down into end to end tests

Ensure all data is in the required directory

```
cd /home/YourDataDirectory
```
Run the R script

```
Rscript stopcodon.rscript <treefile.ph> <seqfile.fasta>
```

**First Optimisation:** Four numbers will then appear in the terminal window. These iterively optimise the parameters: kappa, omega and treescale factor respectively, with initial starting values of 2 (kappa), 0.2 (omega) and 1 (tresscale). Kappa and omega are rate heterogeneity parameters that model transitions and non-synonymous substitutions respectively. The treescale scaling factor parameter was required to scale the branch lengths of the tree up or down, to account for the use of a different rate matrix in the 64x64 stop-codon extended model when compared with the general 61x61 codon model. The fourth and final value is the maximum likelihood value. These values are calculated letting phi, or the parameter which models mutation rate between alternative stop codons relative to the rate of synonymous substitutions between sense codons, equal to 1.

```
m0.out = optim(c(2,0.2,1),lik_fun,treefile=tree_file,seqfile=seq_file,phifixed=1,control=list(fnscale=-1))
```

**Second Optimisation:** After this optimisation procedure has converged, another optimisation procedure follows, this time allowing the phi variable to be calculated. Five numbers are optimised that appear iteravely in the terminal window:  kappa, omega, treescale, phi with initial values to be optimised: 2, 0.2, 1 and 1 respectively. The fifth and final value to appear is the maximum likelihood value.

```
m1.out = optim(c(2,0.2,1,1),lik_fun,treefile=tree_file,seqfile=seq_file,control=list(fnscale=-1))
```

**Output:** A file called stopcodon.rscript.out, which contains the following: The maximum log likelihood of the second optimisation; ML estimate of kappa of the second optimisation; ML estimate of omega of the second optimisation; ML estimate of the treescaling parameterof the second optimisation; ML estimate of phi delta_lnL (difference in log likelihood compared to a model with phi=1 calculated by getting the difference in maximum likelihood valies between the 2 optimisation procedures; convergence of optimizer (0 = success,1 = failure). 

```
write(c(m1.out$value,m1.out$par,diff,m1.out$convergence),out_file,ncol=10)
```
***

## Authors

* **Cathal Seoighe** - *Initial work* - [StopCodon](https://github.com/CathalSeoighe/StopCodon)
* **Stephen J. Kiniry**
* **Pavel V. Baranov**
* **Haixuan Yang**

***

## Acknowledgments
