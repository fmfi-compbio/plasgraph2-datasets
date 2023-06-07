# plasgraph2-datasets

This repository contains training and testing sets for the PlASgraph2 tool. 

Each set is consists of 
* A `.csv` file listing all assemblies (e.g. `eskapee-test.csv`) with columns: path to `gfa.gz` file, path to `gfa.csv` file and an identifier of the sample
* A folder with files for each assembly:
  * `gfa.gz` file is a [GFA](http://gfa-spec.github.io/GFA-spec/GFA1.html) file with the assembly, compressed by gzip
  * `gfa.csv` is a file with correct classification of each contig, containing the following columns (plus any other optinal columns):
     * `contig`: contig id from the `gfa.gz` file 
     * `label`: one of the strings `chromosome`, `plasmid`, `ambiguous` or `unlabeled`. Label `ambiguous` means that the contig should be correctly classified as both a chromosome and a plasmid (e.g. a transposon present in both molecules of the given sample) and `unlabeled` means that the correct label is unknown, e.g. due to short contig size. 
     * `length`, giving the contig length (used for evaluation only, not needed for training)
     * `chrom_score`: for golder answer, this should be 1 for chromosome and ambiguous and 0 for plasmid and unknown (if omitted, score will be filled in according to `label`). For predictions of actual tools, score should by a confidence that the contig belongs to the chromosome class.
     * `plasmid_score`: should be 1 for plasmid and ambiguous and 0 for chromosome and unknown, othewise analogous to `chrom_score`.
  
