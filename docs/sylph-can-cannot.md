## What can sylph do?

- **Profile metagenomes**: sylph can calculate the abundances of genomes in a metagenomic sample by using a reference database. This is the same type of output as Kraken or MetaPhlAn. 
- **Search genomes against metagenomes**: sylph can check if a genome is contained in your sample (e.g. is this _E. coli_ genome in my sample?). 
- **ANI querying**: sylph can estimate the **containment average nucleotide identity** (ANI) of a reference genome to the genomes in your sample.
- **Use custom reference databases**: Eukaryotes, viruses, and any collections of fasta files are ok. 
- **Long-reads are usable**: sylph can utilize nanopore or PacBio reads with high precision. A recent study from [Oxford Nanopore found that sylph is the most accurate profiling method](https://nanoporetech.com/resource-centre/genomic-and-epigenomic-insights-into-microbial-biology-with-nanopore-metagenomic-and-isolate-sequencing) on their data
- **Calculate coverage**: sylph can estimate the coverage (not just the abundance) of genomes in your database. 
- **Calculate the percentage of reads detected in your database at species level**: sylph can check how much of your metagenome is "captured" by the database 

## What can sylph **NOT** do?

Sylph can **not**:

- Map reads. Unlike Kraken, sylph does not classify every read. 
- Find super low abundance genomes. Sylph requires > 0.01-0.05x coverage **at minimum** for bacterial genomes. All bacterial genomes need at least a few hundred short-reads.
- Reliably find genomes at genus level or higher (if it is not present at species level). If your sample is not well-characterized by the database, sylph may struggle. **Note**: this also applies to most profilers. 
- Compare genomes to genomes / metagenomes to metagenomes / contigs to genomes 
- Work with 16S / ITS data 

## How does sylph work?

The below figure summarizes sylph's main steps. 

<img src='https://github.com/bluenote-1577/sylph/assets/12787948/04ff3385-a060-443d-940b-665a2a80a1ca' width=80%/>

1. (Panel 1) Reads and reference genomes are broken into **k-mers** using the `sylph sketch` option. k-mers are downsampled by a fraction of `c`, default = 200. 
2. (Panel 1) Using `sylph query` or `sylph profile`, the k-mers in each reference genome are checked against the k-mers in the reads. 
3. (Panel 2) Sylph uses statistics to estimate the **containment ANI** between each reference genome and the metagenomes. 
4. (Panel 3) `sylph query`: all genomes with high ANI (> 90% default) from the previous step are reported. No abundances.
5. (Panel 3) `sylph profile`: calculates **abundances** and reports the present genomes **at species-level** using a k-mer remapping algorithm if **ANI > 95%**. 
