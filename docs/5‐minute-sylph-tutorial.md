## 5 minute tutorial

### Sketching 

After installation, clone this repository if you have not done so and run the following.

```sh 
git clone https://github.com/bluenote-1577/sylph
cd sylph

## install sylph. see installation instructions
# cargo install --path . --root ~/.cargo
## OR
# conda install -c bioconda sylph

# sketch reads and genomes. fastq -> samples, fasta -> queries
sylph sketch test_files/o157_reads.fastq test_files/e.coli*.fa -o database
```

There are two types of files: `*.syldb` and `*.sylsp`.

- **FASTQ files** are treated as **samples** and turn into `*.sylsp` 
- **FASTA files** are treated as **genomes** and turn into `*.syldb`
- For other options, such as paired-end reads, see the [cookbook](sylph-cookbook.md)

Genomes are aggregated into one `syldb` file, and each genome is queried against all `sylsp`s. 

### Querying

```sh
# query for ANI 
sylph query o157_reads.fastq.sylsp database.syldb

# ALTERNATIVE: lazy containment without pre-sketching also works 
# sylph query test_files/*
```

The o157_reads.fastq is a "metagenomic sample" containing only E. coli O157 with 1x coverage and 95% identity reads (i.e. 5% error). We query the reads and the database consisting of multiple E. coli genomes. 

You'll see something like the following

```sh
Sample_file	Genome_file	Adjusted_ANI	Eff_cov	ANI_5-95_percentile	Eff_lambda	Lambda_5-95_percentile	Median_cov	Mean_cov_geq1	Containment_ind	Naive_ANI	Contig_name
../test_files/o157_reads.fastq	../test_files/e.coli-o157.fasta	99.73	0.360	99.61-99.91	0.360	0.33-0.38	1	1.187	6208/21899	96.02	NZ_CP017438.1 Escherichia coli O157:H7 strain 2159 chromosome, complete genome
../test_files/o157_reads.fastq	../test_files/e.coli-EC590.fasta	98.25	0.319	98.06-98.55	0.319	0.29-0.34	1	1.172	3122/19330	94.29	NZ_CP016182.2 Escherichia coli strain EC590 chromosome, complete genome
../test_files/o157_reads.fastq	../test_files/e.coli-K12.fasta	98.16	0.327	97.96-98.47	0.327	0.29-0.35	1	1.171	3114/19485	94.26	NC_007779.1 Escherichia coli str. K-12 substr. W3110, complete sequence

```

These are statistics for each genome against our reads. The ANI can be interpreted as nearest-neighbour ANI searching, i.e., "what is the highest ANI between the genomes in my sample and the reference genome?". See [here](Output-format.md) for a detailed output explanation. 

1. The 3rd column gives the coverage adjusted ANI between the genome and this sample. 
2. The second last column is the Naive ANI -- what you would approximately get without sylph's statistical adjustment (i.e. if you used Mash or Sourmash).

Notice the big difference between 1. and 2. This is because the reads are only 1x coverage: **methods like mash screen and sourmash give biased ANI when coverage is low**. 

However, the Eff_cov gives smaller than 1x: **this is because Eff_cov takes into account sequencing error**. Sequencing error reduces the k-mer based coverages (sequencing errors invalidate k-mers). You can estimate the true coverage if you provide some more information. See the [cookbook](sylph-cookbook.md).

Here are the ANIs computed by [skani](https://github.com/bluenote-1577/skani) between the three genomes:

```sh
test_files/e.coli-EC590.fasta	100.00	99.39	98.14
test_files/e.coli-K12.fasta	99.39	100.00	98.09
**test_files/e.coli-o157.fasta	98.14	98.09	100.00**
```

So the ANIs should be 98.14, 98.09, and 100.0 for EC590, K12, and O157 respectively against the sample. As you can see, Sylph's estimates are quite good and much more reasonable than the Naive ANI.  

### Profiling vs ANI querying. What's the difference?

In the above example, notice that querying each E. coli genome gave a high ANI value. However, only one E. coli genome is present in the sample, not all three.

Thus `sylph query` is not a **profiler**. It does not tell you **the abundance** of the genomes in your sample, just **how similar** your reference genome is to your metagenome. 

To remove this redundancy, we can use the `sylph profile` instead. 

```sh
> sylph profile test_files/*  # or use 'profile' on the syldb and sylsp.
...
...
Sample_file	Genome_file	Taxonomic_abundance	Sequence_abundance	Adjusted_ANI	Eff_cov	ANI_5-95_percentile	Eff_lambda	Lambda_5-95_percentile	Median_cov	Mean_cov_geq1	Containment_ind	Naive_ANI	Contig_name
../test_files/o157_reads.fastq	../test_files/e.coli-o157.fasta	100.0000	100.0000	99.73	0.360	99.61-99.91	0.360	0.33-0.38	1	1.187	6208/21899	96.02	NZ_CP017438.1 Escherichia coli O157:H7 strain 2159 chromosome, complete genome
```

Notice the new `Sequence_abundance` and `Taxonomic_abundance` columns, which give the relative abundances as a percentage. See https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8184642/ for how taxonomic and sequence abundance differ.

There is only 1 genome in the sample, so it has 100% abundance. 

!!! note 
    This is a simple example with multiple *strains* of a single *species*. For real metagenomes, we advise **using databases that are dereplicated at the species level**. 

    Our [pre-built databases](pre‐built-databases.md) are all dereplicated at the species level. 

### Real profiling against a database

See the [tutorial here](taxonomic-profiling-tutorial.md) to learn how to profile against the GTDB database, an actual database of > 50,000 genomes prokaryotic genomes. 
