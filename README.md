#### READ.ME File Description

***

**Usage:** Rscript stopcodon.rscript <treefile.ph> <seqfile.fasta>

**Info:** The tree file should include branch lengths. Note that relative branch lengths are treated as fixed.
newline The sequence file should contain a codon-aware alignment in fasta format and must include the 
stop codon as the last position of the alignment. Sequences that include a gap or a codon other than
a stop codon (i.e. sequences for which the stop codon is not positionally homologous with the last 
position in the alignment) are excluded.

**Output:** a file called stopcodon.rscript.out, which contains the following: The maximum log likelihood;
ML estimate of kappa; ML estimate of omega; ML estimate of the treescaling parameter; ML estimate of phi
delta_lnL (difference in log likelihood compared to a model with phi=1; convergence of optimizer (0 = success,
1 = failure)
