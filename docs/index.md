# sylph - fast and precise species-level metagenomic profiling with ANIs 

**Sylph** is a program that performs (1) **metagenomic profiling** or (2) **containment average nucleotide identity querying** for metagenomic shotgun sequencing samples. 

- **Metagenomic profiling**: sylph can determine the species/taxa in your sample and their abundances, just like [Kraken](https://ccb.jhu.edu/software/kraken/) or [MetaPhlAn](https://github.com/biobakery/MetaPhlAn).

- **Containment ANI querying**: sylph can search a genome, e.g. E. coli, against your sample. If sylph outputs an estimate of 97% ANI, your sample contains an E. coli with 97% ANI to the queried genome.

<p align="center"><img src="assets/sylph.gif?raw=true"/></p>
<p align="center">
   <i>
   Profiling 1 Gbp of mouse gut reads against 85,205 genomes in a few seconds 
   </i>
</p>


### Why sylph?

1. **Precise species-level profiling**: sylph has less false positives than Kraken and is about as precise and sensitive as marker gene methods (MetaPhlAn, mOTUs). 

2. **Ultrafast, multithreaded, multi-sample**: sylph can be > 50x faster than other methods. Sylph only takes ~15GB of RAM for profiling against the entire GTDB-R220 database (110k genomes).

3. **Accurate (containment) ANI information**: sylph can give accurate **ANI estimates** between reference genomes and your metagenome sample down to 0.1x coverage.

4. **Customizable databases and pre-built databases**: We offer pre-built databases of [prokaryotes, viruses, eukaryotes](pre‐built-databases.md). Custom databases (e.g. using your own MAGs) are easy to build.  

5. **Short or long reads**: Sylph was also the most accurate method [on Oxford Nanopore's independent benchmarks](https://nanoporetech.com/resource-centre/genomic-and-epigenomic-insights-into-microbial-biology-with-nanopore-metagenomic-and-isolate-sequencing).

### How does sylph work?

sylph uses a k-mer containment method. sylph's novelty lies in **using a statistical technique to estimate k-mer containment for low coverage genomes** , giving accurate results for low abundance organisms. See [here for more information on what sylph can and can not do](sylph-can-cannot.md). 

## How do I use sylph?

See the left side bar for more information. See [the quick start and installation instructions](install+quickstart.md) for a quick start. 


## Changelog

See the [CHANGELOG](https://github.com/bluenote-1577/sylph/blob/main/CHANGELOG.md) for complete details.

## Citing sylph

Jim Shaw and Yun William Yu. Rapid species-level metagenome profiling and containment estimation with sylph (2024). Nature Biotechnology.



