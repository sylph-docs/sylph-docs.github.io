
# Sylph cookbook

  * [Read sketching options](#read-sketching-options)
      - [Sketching many paired-end reads](#sketching-many-paired-end-reads)
      - [Sketching single-end reads](#sketching-single-end-reads)
  * [Database sketching options](#database-sketching-options)
      - [Creating a database of fasta files](#creating-a-database-of-fasta-files)
      - [Custom databases: to dereplicate or not?](#custom-databases-to-dereplicate-or-not)
      - [Creating a database of contigs or if genomes are all in one fasta file](#creating-a-database-of-contigs-or-if-genomes-are-all-in-one-fasta-file)
      - [Sketching a large database](#sketching-a-large-database)
  * [Profiling/querying](#profiling-and-querying)
      - [Standard profiling and querying](#standard-profiling-and-querying)
      - [Lazy profiling and querying without sketching](#lazy-profiling-and-querying-without-sketching)
      - [Profiling small genomes such as viruses](#profiling-small-genomes-such-as-viruses)
      - [Estimating percentage of unknown reads](#estimating-percentage-of-unknown-reads-in-database)
      - [Important notes for estimating unknown percentage](#important-notes-for-estimating-unknown-percentage)
  * [Taxonomy integration with sylph-tax](#taxonomy-integration-with-sylph-tax)
      - [Standard taxonomic integration (one metagenome, one database)](#standard-taxonomic-integration-one-metagenome-one-database)
      - [Taxonomic integration: more than 1 metagenome, more than 1 database](#taxonomic-integration-more-than-1-metagenome-more-than-1-database)
      - [Custom taxonomies for custom databases](#custom-taxonomies-for-custom-databases)

## Read sketching options

"Sketching" is equivalently to "indexing". Sketching reads gives you a small index that you can efficiently reuse. 

#### Sketching many paired-end reads

```sh
sylph sketch -1 *_1.fastq.gz -2 *_2.fastq.gz -d my_sketches -t 50 
```

- `-1/-2`: Input the first and second paired-end reads; can be gzipped. 
- `-t`: parallelized sketching over the samples. Each sample uses 1 thread: e.g. 5 samples would use 5 threads. 
- `-d`: all read sketches are output to a new folder called `my_sketches` with `.sylsp` suffixes. The folder will be created if it is not present.

#### Renaming (identical) reads with `-S`

!!! warning

    If you have identical read names but different folder names (e.g. `folder1/reads.fq, folder2/reads.fq`) sylph will only output one `reads.fq.sylsp` file. This will cause issues **unless** you use the -S option shown below.

```sh
sylph sketch -1 folder1/reads_1.fq folder2/reads_1.fq -2 folder1/reads_2.fq folder2/reads_2.fq -S sample1 sample2
ls sample1.paired.sylsp sample2.paired.sylsp
``` 

- `-S`: renames the output sketch to `sample{x}.paired.sylsp`. 
- `--lS`: input a newline delimited list of sample names

#### Sketching single-end reads

```sh
sylph sketch -r reads1.fq reads2.fa
sylph sketch reads1.fq 

# DON'T DO: sylph sketch reads.fa
```

- `-r`: you can sketch many single-end reads at once. 
- If  `-r` is not specified, fastq files are assumed to be reads. If fasta reads, *you must use -r*.

## Database sketching options

You can create a custom database with sylph. This requires only fasta files. 

#### Creating a database of fasta files

```sh
sylph sketch genomes/*.fa.gz -o database -t 50 -c 200
# EQUIVALENT
sylph sketch -g genomes/*.fa.gz -o database -t 50
```

- fasta files are assumed to be genomes. All genomes are combined into a new file called `database.syldb`. 
- `-t`: sketching can use many threads
- `-c`: the **compression** parameter. Memory/runtime scale like `1/c`; higher `c` is faster but less sensitive at low coverage.
-  Default `c = 200`. The `-c` for genomes must be than >= the `-c` for reads (strict > is allowed) 

#### Custom databases: to dereplicate or not?

We recommend **dereplicating your database of genomes at the species level (95% ANI)**. 

Sylph handles similar genomes within your database by reassigning k-mers (see Fig. 1 in the paper) to a "best" genome. This works well when the genomes are < 95% ANI to each other. HOWEVER, this is less reliable when the genomes are very similar. 

When k-mer reassignment fails, a single strain can appear as two (or more) similar strains being present --- your abundances will be skewed because the species appear twice as abundant.

Thus, dereplication prevents overestimation of abundances.

#### Creating a database of contigs or if genomes are all in one fasta file 
```sh
sylph sketch all_genomes.fa -i -o contig_database
```

- `-i` considers every record in a fasta (i.e. contig) as a genome
- This is single-threaded currently, so a bit slower

#### Sketching a large database

```sh 
sylph sketch -l genome_list.txt -t 50
```

- `-l` is a newline delimited text file, with each genome on a separate line. 

## Profiling and querying

!!! TIP

    Most of the time, you should use `sylph profile` instead of `sylph query`. See [the tutorial](5‐minute-sylph-tutorial.md) for how `profile` and `query` differ. 

#### Standard profiling and querying

```sh
sylph profile *.syldb *.sylsp  -t 50 -o results.tsv
# sylph query *.syldb *.sylsp  -t 50 -o results.tsv
```

- `*.syldb`: all syldb files (generated by sketching genomes) are aggregated together into one large database. 
- `*.sylsp`: each sylsp file (generated by sketching reads) is queried/profiled against the combined database.
- `-t`: 50 threads used. Even single-sample vs single-database is multi-threaded. 
- `-o`: output results to a file. Can also redirect using `>` instead of `-o`. 


#### Lazy profiling and querying without sketching

```sh
# raw genomes
sylph profile genome1.fa genome2.fa sample.sylsp

# raw reads
sylph profile database.syldb raw_reads.fq 

# raw paired-end reads (since sylph v0.6.0)
sylph profile database.syldb -1 *_1.fq.gz -2 *_2.fq.gz
```

- If you input fastas and fastqs into profile/query, sylph will sketch them and then use them. 
- Parameters can be set in the `sylph profile` command for sketching. We recommend using `sylph sketch` instead, though, as there are more options. 

#### Profiling small genomes such as viruses

```sh
sylph sketch -c 100 virus_genomes.fa -i -o viruses
sylph sketch -c 200 prokaryotic_genomes/* -o proks -t 50
sylph sketch -c 100 read.fq 
sylph profile *.syldb  *.sylsp -t 50 --min-number-kmers 20 -o results.tsv
```

Notes:

- Sketch viruses at `-c 100`, reads at `-c 100`, but genomes at `-c 200`. sylph actually runs without issues if the -c for all genomes is >= the -c for reads, which is a useful technique. 
- Sketching smaller genomes with smaller `-c` is preferable. 
- Be sure to set `--min-num-kmers` to smaller than default (50) if you care about outputting results for small genomes. 

#### Estimating percentage of unknown reads in database

Sylph can estimate the percentage of your reads that are present at the species level using the `-u` or `---estimate-unknown` option. Sylph does not classify reads directly but estimates this percentage from the lengths and coverages of detected genomes.  

```sh
sylph profile -u database.syldb sample.sylsp -o results_with_unknown.tsv

# input read identities. see below for information on the --read-seq-id parameter. 
sylph profile -u --read-seq-id 99.5 database.syldb sample.sylsp -o results_with_unknown.tsv
```

!!! IMPORTANT

    1. The `-u` option multiplies the `Sequence_abundance` column by the percent of classified reads. 
    2. The sum of `Sequence_abundance` column is the percentage of classified reads. 
    3. `-u` changes the `Eff_cov` column to `True_cov`. `-u` estimates the true coverage, not the effective coverage (which is influenced by read length and error rate).


#### Important notes for estimating unknown percentage

`-u` needs the percent identity of your sequences (i.e. 100 - error percent). 

- You can supply a `--read-seq-id`, e.g. 99.5 works fine for Illumina reads. 

- If you do not provide `--read-seq-id`, `-u` will estimate sequence identity. 
This estimate works well for metagenomes that are (1) not too complex, e.g. host-associated metagenomes, and (2) > 1GB sequencing depth. 

For Soil/Ocean, i.e. complex metagenomes, and low sequencing depth this estimate does not work well. You could set `--read-seq-id` to something like 99.5 instead. For short reads, sylph v0.6 automatically sets `--read-seq-id` to 99.5 if median k-mer depth is < 3. 

#### Estimating coverage for small contigs

!!! TIP

    We have developed a new method called [fairy](https://github.com/bluenote-1577/fairy) for calculating contig coverages quickly. Consider using fairy instead of sylph for this task, especially if you want to bin contigs. 

```sh
sylph sketch -i contigs.fa -o contigs -1 reads_1.fq -2 reads_2.fq
sylph profile contigs.syldb reads_1.paired.fq.sylsp -u --min-number-kmers 10 --read-len 150 -o results.tsv 
```

## Taxonomy integration with sylph-tax

See [the documentation on taxonomy integration with sylph-tax](sylph-tax.md) for installation and thorough documentation. 

#### Standard taxonomic integration (one metagenome, one database)

```sh 
sylph profile gtdb-r220-c200-dbv1.syldb -1 read_name1.fq -2 read_name2.fq -o result.tsv
sylph-tax taxprof result.tsv -t GTDB_r220 -o PREFIX_

### new file will be called PREFIX_read_name1.fq.sylphmpa
head PREFIX_read_name1.fq.sylphmpa
```

* `sylph-tax taxprof` takes sylph's result and a taxonomy (`-t`) and outputs `.sylphmpa` files with prefix (`-o`)
* `GTDB_r220` database is used, so we use `GTDB_r220` as the metadata file. See `sylph-tax taxprof -h` for available databases. 
* When using paired-end reads, the "sample name" is the first read (`read_name1.fq`). For single-end reads, it is just the read name. 


#### Taxonomic integration: more than 1 metagenome, more than 1 database
```sh 
sylph profile  gtdb-r220-c200-dbv1.syldb\
               fungi-refseq-2024-07-25-c200-v0.3.syldb \
              -1 A1.fq B1.fq -2 A2.fq B2.fq -o result.tsv
sylph-tax taxprof result.tsv -t GTDB_r220 FungiRefSeq-2024-07-25

# multiple .sylphmpa files are output
head A1.fq.sylphmpa
head B1.fq.sylphmpa
```

* `-t` can take multiple taxonomies. This is needed because `gtdb-r220...` and `fungi-refseq...` are both used (these databases are concatenated by sylph into a big database for profiling). 
* `sylph-tax taxprof` works on multiple samples at once. Both `A1.fq.sylphmpa` and `B1.fq.sylphmpa` are output. 

#### Custom taxonomies for custom databases?

See [this page for more information about building custom taxonomies for sylph-tax](sylph-tax-custom-taxonomies.md).
