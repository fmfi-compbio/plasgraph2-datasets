# plasgraph2-datasets

This repository contains training and testing sets for the [PlASgraph2](https://github.com/cchauve/plASgraph2) tool. 

Each set is consists of 
* A `.csv` file listing all assemblies (e.g. `eskapee-test.csv`) with columns: path to `gfa.gz` file, path to `gfa.csv` file and an identifier of the sample
* A folder with files for each assembly:
  * `gfa.gz` file is a [GFA](http://gfa-spec.github.io/GFA-spec/GFA1.html) file with the assembly, compressed by gzip
  * `gfa.csv` is a file with the correct classification of each contig, containing the following columns (plus any other optional columns):
    * `contig`: contig id from the `gfa.gz` file 
    * `label`: one of the strings `chromosome`, `plasmid`, `ambiguous` or `unlabeled`. Label `ambiguous` means that the contig should be correctly classified as both a chromosome and a plasmid (e.g. a transposon present in both molecules of the given sample) and `unlabeled` means that the correct label is unknown, e.g. due to short contig size. 
    * `length`: the contig length (used for evaluation only, not needed for training)
    * `chrom_score`: for golder answer, this should be 1 for chromosome and ambiguous and 0 for plasmid and unknown (if omitted, score will be filled in according to `label`). For predictions of actual tools, the score should be the confidence that the contig belongs to the chromosome class.
    * `plasmid_score`: should be 1 for plasmid and ambiguous and 0 for chromosome and unknown; otherwise analogous to `chrom_score`.
  
## ESKAPEE training and testing sets

ESKAPEE species are Enterococcus faecium, Staphylococcus aureus, Klebsiella pneumoniae, Acinetobacter baumannii, Pseudomonas aeruginosa, Enterobacter spp and Escherichia coli

* `eskapee-train`:  The training set of samples from ESKAPEE species (internally split to training and validation). It contains 70 samples, 10 samples from each species. For each sample there are 2 assemblies (Unicycler and SKESA).
* `eskapee-test`: The testing set of samples from ESKAPEE species, 112 samples in total (E.fae. 2, S.aur. 31, K.pne. 46, A.bau. 5, P.aer. 5, E.spp 15, E.col 8)

## Other testing sets

* `cfre-test`: Citrobacter freundii, 50 samples
* `efer-test`: Escherichia fergusonii, 50 samples
* `koxy-test`: Klebsiella oxytoca, 31 samples
* `myco-test`: mycobacteria (genera Mycobacterium, Mycobacteroides, Mycolicibacterium, Mycolicibacter), 30 samples
* `sent-test`: Salmonella enterica, 29 samples
