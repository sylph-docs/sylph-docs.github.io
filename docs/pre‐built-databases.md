Pre-sketched databases available for download below. All databases work from sylph version 0.3.x onwards. 

* Use the **Primary** links hosted at `http://faust.compbio.cs.cmu.edu` if possible. We provide mirrors on google cloud, but this costs us more money.

#### Example usage:

```sh
# download database
wget http://faust.compbio.cs.cmu.edu/sylph-stuff/gtdb-r220-c200-dbv1.syldb

# profile against database
sylph profile gtdb-r226-c200-dbv1.syldb -1 sample_R1.fq -2 sample_R2.fq  -t 30 > results.tsv
```

### Note on taxonomy usage:

Most the databases have associated taxonomies that sylph can utilize. See [here for more information on taxonomy integration](sylph-tax.md).



## Database Summary

| Database Type | Database Name | Genomes/vOTUs | c-parameter | Size | Primary Download Link | Mirror | Notes |
|---------------|---------------|---------------|-------------|------|----------------------|--------|-------|
| **Prokaryotic (GTDB)** | GTDB r226 | 143,614 species | c200 | 18.4 GB | [gtdb-r226-c200-dbv1.syldb](http://faust.compbio.cs.cmu.edu/sylph-stuff/gtdb-r226-c200-dbv1.syldb) | [mirror](https://storage.googleapis.com/sylph-stuff/gtdb-r226-c200-dbv1.syldb) | |
| | GTDB r226 | 143,614 species | c1000 | 3.7 GB | [gtdb-r226-c1000-dbv1.syldb](http://faust.compbio.cs.cmu.edu/sylph-stuff/gtdb-r226-c1000-dbv1.syldb) | [mirror](https://storage.googleapis.com/sylph-stuff/gtdb-r226-c1000-dbv1.syldb) | |
| | GTDB r220 | 113,104 species | c200 | 13.1 GB | [gtdb-r220-c200-dbv1.syldb](http://faust.compbio.cs.cmu.edu/sylph-stuff/gtdb-r220-c200-dbv1.syldb) | [mirror](https://storage.googleapis.com/sylph-stuff/gtdb-r220-c200-dbv1.syldb) | |
| | GTDB r220 | 113,104 species | c1000 | 2.6 GB | [gtdb-r220-c1000-dbv1.syldb](http://faust.compbio.cs.cmu.edu/sylph-stuff/gtdb-r220-c1000-dbv1.syldb) | [mirror](https://storage.googleapis.com/sylph-stuff/gtdb-r220-c1000-dbv1.syldb) | |
| | GTDB r214 | 85,202 species | c200 | 10 GB | [v0.3-c200-gtdb-r214.syldb](http://faust.compbio.cs.cmu.edu/sylph-stuff/v0.3-c200-gtdb-r214.syldb) | [mirror](https://storage.googleapis.com/sylph-stuff/v0.3-c200-gtdb-r214.syldb) | |
| | GTDB r214 | 85,202 species | c1000 | 2 GB | [v0.3-c1000-gtdb-r214.syldb](http://faust.compbio.cs.cmu.edu/sylph-stuff/v0.3-c1000-gtdb-r214.syldb) | [mirror](https://storage.googleapis.com/sylph-stuff/v0.3-c1000-gtdb-r214.syldb) | |
| **Prokaryotic (Other)** | OceanDNA | 8,466 ocean MAGs | c200 | 800 MB | [OceanDNA-c200-v0.3.syldb](http://faust.compbio.cs.cmu.edu/sylph-stuff/OceanDNA-c200-v0.3.syldb) | [mirror](https://storage.googleapis.com/sylph-stuff/OceanDNA-c200-v0.3.syldb) | |
| | SMAG | 21,077 soil MAGs | c200 | 2.5 GB | [SMAG-c200-v0.3.syldb](http://faust.compbio.cs.cmu.edu/sylph-stuff/SMAG-c200-v0.3.syldb) | [mirror](https://storage.googleapis.com/sylph-stuff/SMAG-c200-v0.3.syldb) | |
| | UHGG v2.0.1 **(not dereplicated)**| 289,232 gut genomes | c200 | 26 GB | [uhgg_all_c200_v0.3.0.syldb](http://faust.compbio.cs.cmu.edu/sylph-stuff/uhgg_all_c200_v0.3.0.syldb) | [mirror](https://storage.googleapis.com/sylph-stuff/uhgg_all_c200_v0.3.0.syldb) | Not dereplicated - do not use for profiling |
| **Viral** | UHGV | 171,338 gut vOTUs | c100 | 0.4 GB | [uhgv_c100_dbv1.syldb](http://faust.compbio.cs.cmu.edu/sylph-stuff/uhgv_c100_dbv1.syldb) | [mirror](https://storage.googleapis.com/sylph-stuff/uhgv_c100_dbv1.syldb) | |
| | UHGV | 171,338 gut vOTUs | c200 | 0.2 GB | [uhgv_c200_dbv1.syldb](http://faust.compbio.cs.cmu.edu/sylph-stuff/uhgv_c200_dbv1.syldb) | [mirror](https://storage.googleapis.com/sylph-stuff/uhgv_c200_dbv1.syldb) | |
| | IMG/VR4.1 | 2,917,516 viral genomes | c200 | 2 GB | [imgvr_c200_v0.3.0.syldb](http://faust.compbio.cs.cmu.edu/sylph-stuff/imgvr_c200_v0.3.0.syldb) | [mirror](https://storage.googleapis.com/sylph-stuff/imgvr_c200_v0.3.0.syldb) | |
| **Eukaryotic** | RefSeq Fungi | 595 genomes | c200 | 700 MB | [fungi-refseq-2024-07-25-c200-v0.3.syldb](http://faust.compbio.cs.cmu.edu/sylph-stuff/fungi-refseq-2024-07-25-c200-v0.3.syldb) | [mirror](https://storage.googleapis.com/sylph-stuff/fungi-refseq-2024-07-25-c200-v0.3.syldb) | |
| | TARA Oceans | 713 eukaryotic MAGs/SAGs | c200 | 900 MB | [tara-eukmags-c200-v0.3.syldb](http://faust.compbio.cs.cmu.edu/sylph-stuff/tara-eukmags-c200-v0.3.syldb) | [mirror](https://storage.googleapis.com/sylph-stuff/tara-eukmags-c200-v0.3.syldb) | |

## Parameter Guide
- **c200**: More sensitive, larger file size
- **c1000**: More efficient, smaller file size, less sensitive
- **c100**: More sensitive for smaller genomes. Note: `-c 200` is used by default, so `-c 100` has to be manually specified. `sylph profile c100_database c1000_database -c100 -1 read1.fq -2 read2.fq` will work. 


# Database descriptions 

## GTDB Databases

The [GTDB](https://gtdb.ecogenomic.org/) database is a high-quality, curated taxonomy and database for prokaryotes (archaea and bacteria). We take the dereplicated, species-representative genomes (one genome per species). 

**Available databases**:

- GTDB r226 database (143,614 species representative genomes) - April 16, 2025

- GTDB r220 database (113,104 species representative genomes) - April 24, 2024

- GTDB r214 database (85,202 species representative genomes) - April 28, 2023

## Other prokaryotic databases

1. [OceanDNA](https://doi.org/10.1038/s41597-022-01392-5) catalogue of 8,466 ocean prokaryotic MAGs, `-c 200` (800 MB) 
2. [SMAG](https://www.nature.com/articles/s41467-023-43000-z) catalogue of soil 21,077 soil MAGs, `-c 200` (2.5 GB):
3. [UHGG v2.0.1](https://www.ebi.ac.uk/metagenomics/genome-catalogues/human-gut-v2-0-1) catalogue of 289,232 gut genomes. **Not dereplicated. Do not use for profiling.** `-c 200` (26 GB): 

## Viral databases

1. [Unified Human Gut Virome (UHGV) catalog](https://github.com/snayfach/UHGV) - (171,338 vOTUs from human gut)
    - Note: is more refined and has better taxonomic annotations than IMG/VR for human gut
2.  Pre-sketched IMG/VR4.1 database for high-confidence vOTU representatives (2,917,516 viral genomes). 

## Eukaryotic databases. 
1. 595 representative RefSeq fungi genomes (downloaded 2024-07-25), `-c 200` (700 MB) 
2. 713 TARA Oceans eukaryotic MAGs/SAGs from [Delmont et al.](https://doi.org/10.1016/j.xgen.2022.100123), `-c 200` (900 MB)
