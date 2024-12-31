Pre-sketched databases available for download below. All databases work from sylph version 0.3.x onwards. 

* Use the `http://faust.compbio.cs.cmu.edu` links if possible. We provide mirrors on google cloud, but this costs us more money.

#### Example usage:

```sh
# download database
wget http://faust.compbio.cs.cmu.edu/sylph-stuff/gtdb-r220-c200-dbv1.syldb

# profile against database
sylph profile gtdb-r220-c200-dbv1.syldb -1 sample_R1.fq -2 sample_R2.fq  -t 30 > results.tsv
```

### Note on taxonomy usage:

Most the databases have associated taxonomies that sylph can utilize. See [here for more information on taxonomy integration](sylph-tax.md).

## GTDB Databases

### GTDB r220 database (113,104 species representative genomes) -  24th April, 2024 

1. `-c 200`, more sensitive database (13.1 GB)
    * http://faust.compbio.cs.cmu.edu/sylph-stuff/gtdb-r220-c200-dbv1.syldb (**primary + preferred**)
    * https://storage.googleapis.com/sylph-stuff/gtdb-r220-c200-dbv1.syldb (mirror)
2. `-c 1000` more efficient, less sensitive database (2.6 GB)
    * http://faust.compbio.cs.cmu.edu/sylph-stuff/gtdb-r220-c1000-dbv1.syldb (**primary + preferred**)
    * https://storage.googleapis.com/sylph-stuff/gtdb-r220-c1000-dbv1.syldb (mirror)

### GTDB r214 database (85,202 species representative genomes) - 28th April, 2023

1. `-c 200`, more sensitive database (10 GB)
    * http://faust.compbio.cs.cmu.edu/sylph-stuff/v0.3-c200-gtdb-r214.syldb (**primary + preferred**)
    * https://storage.googleapis.com/sylph-stuff/v0.3-c200-gtdb-r214.syldb (mirror)
2. `-c 1000` more efficient, less sensitive database (2 GB) 
    * http://faust.compbio.cs.cmu.edu/sylph-stuff/v0.3-c1000-gtdb-r214.syldb (**primary + preferred**)
    * https://storage.googleapis.com/sylph-stuff/v0.3-c1000-gtdb-r214.syldb (mirror)


## Other prokaryotic databases

1. [OceanDNA](https://doi.org/10.1038/s41597-022-01392-5) catalogue of 8,466 ocean prokaryotic MAGs, `-c 200` (800 MB) 
    * http://faust.compbio.cs.cmu.edu/sylph-stuff/OceanDNA-c200-v0.3.syldb (**primary + preferred**)
    * https://storage.googleapis.com/sylph-stuff/OceanDNA-c200-v0.3.syldb (mirror)
2. [SMAG](https://www.nature.com/articles/s41467-023-43000-z) catalogue of soil 21,077 soil MAGs, `-c 200` (2.5 GB):
    *  http://faust.compbio.cs.cmu.edu/sylph-stuff/SMAG-c200-v0.3.syldb (**primary + preferred**)
    * https://storage.googleapis.com/sylph-stuff/SMAG-c200-v0.3.syldb (mirror)
3. [UHGG v2.0.1](https://www.ebi.ac.uk/metagenomics/genome-catalogues/human-gut-v2-0-1) catalogue of 289,232 gut genomes. **Not dereplicated. Do not use for profiling.** `-c 200` (26 GB): 
    * http://faust.compbio.cs.cmu.edu/sylph-stuff/uhgg_all_c200_v0.3.0.syldb (**primary + preferred**)
    * https://storage.googleapis.com/sylph-stuff/uhgg_all_c200_v0.3.0.syldb (mirror)

## Viral databases

### Pre-sketched IMG/VR4.1 database for high-confidence vOTU representatives (2,917,516 viral genomes). 
1. `-c 200` (2GB)
    * http://faust.compbio.cs.cmu.edu/sylph-stuff/imgvr_c200_v0.3.0.syldb (**primary + preferred**)
    * https://storage.googleapis.com/sylph-stuff/imgvr_c200_v0.3.0.syldb (mirror)

## Eukaryotic databases. 
1. 595 representative RefSeq fungi genomes (downloaded 2024-07-25), `-c 200` (700 MB) 
    * http://faust.compbio.cs.cmu.edu/sylph-stuff/fungi-refseq-2024-07-25-c200-v0.3.syldb (**primary + preferred**)
    * https://storage.googleapis.com/sylph-stuff/fungi-refseq-2024-07-25-c200-v0.3.syldb (mirror) 

2. 713 TARA Oceans eukaryotic MAGs/SAGs from [Delmont et al.](https://doi.org/10.1016/j.xgen.2022.100123), `-c 200` (900 MB)
    * http://faust.compbio.cs.cmu.edu/sylph-stuff/tara-eukmags-c200-v0.3.syldb (**primary + preferred**)
    * https://storage.googleapis.com/sylph-stuff/tara-eukmags-c200-v0.3.syldb (mirror)
