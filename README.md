# plasgraph2-datasets

This repository contains training and testing sets for the PlASgraph2 tool. 

Each set is consists of 

* A `.csv` file listing all assemblies (e.g. `eskapee-test.csv`) with columns: path to `gfa.gz` file, path to `gfa.csv` file and an identifier of the sample
* A folder with files for each assembly (more detailed description of both files is below):
  * `gfa.gz` file is a [GFA](http://gfa-spec.github.io/GFA-spec/GFA1.html) file with the assembly graph, compressed by gzip
  * `gfa.csv` is a file with the correct classification of each contig

## GFA assembly graph files

PlASgraph2 was developed with GFA files produced by <a href="https://github.com/rrwick/Unicycler">Unicycler</a> and <a href="https://github.com/ncbi/SKESA">SKESA</a>. In principle, plASgraph2 should be usable with other assemblers that use the GFA format. However, one of the features is the read coverage of a node which is currently obtained from GFA files as follows:

* If nodes have the `dp` tag, containing normalized read depth computed by Unicycler, it is used as read depth of the node.
* If nodes have the `KC` tag contining k-mer count reorted by SKESA, its value is divided by the length of the sequence corresponding to the node.

In both cases, the coverage is normalized by dividing it with weighted mean of coverages of all nodes. As a result, chromosome contigs are expected to have coverage close to 1. However, for Unicycler this step does not change the values much because a similar procedure was already done.

For other assemblies, make sure that the GFA contains a `dp` or `KC` tag with a similar meaning. If the assembler does not provide this information, you can align the source reads to the assembly and label the nodes with read coverage obtained from the alignments.

## Classification CSV files

The CSV file with correct classification should contain the following columns (plus any other optional columns):

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

## Summary tables

* `reference_genomes.csv` contains the list of chromosome references (roughly one per species used in the study) used in our paper to aid golden standard annotation of contigs in hybrid assemblies. The csv file has two columns: accession number of the sequence and description of the sequence
* `all_samples.csv` contains the list of all samples in all our sets. Columns:
  * `dataset` which dataset uses this sample
  * `our_id` our sample id which is used with suffix `-s` or `-u` based on assembler used
  * `sample_id` typically NCBI/ENA/DDBJ accession of the bacterial sample, except for samples from Arredondo-Alonso el al 2018, where their ID is used
  * `short_reads` accession of short reads in SRA. Missing in samples from Boostrom et al. 2022 where short reads provided by the authors
  * `long_reads` accession of long reads in SRA. Missing in samples from Arredondo-Alonso et al. 2018 where long reads provided by the authors at figshare
  * `long_reads_type` Nanopore or PacBio
  * `reference` source of the sample
  * `species` bacterial species 