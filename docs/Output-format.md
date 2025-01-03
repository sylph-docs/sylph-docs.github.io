## TSV Output

Sylph outputs a TSV (tab-separated values) file. Each row is one genome detected in the metagenome sample. 

```sh
Sample_file   Genome_file   Taxonomic_abundance   Sequence_abundance   Adjusted_ANI   Eff_cov   ANI_5-95_percentile   Eff_lambda   Lambda_5-95_percentile   Median_cov   Mean_cov_geq1   Containment_ind   Naive_ANI   Contig_name
reads.fq   genome.fa   78.1242   81.8234   97.53   264.000   NA-NA   HIGH   NA-NA   264   264.143   10281/22299   97.53   NC_016901.1 Shewanella baltica OS678, complete genome
```

- **Sample_file**: the filename of the reads/sample.
- **Genome_file**: the filename of the detected genome.
- **Taxonomic_abundance**: normalized taxonomic abundance as a percentage. Coverage-normalized - same as MetaPhlAn abundance
     * *Not present for `sylph query`*
- **Sequence_abundance**: normalized sequence abundance as a percentage. The "percentage of reads" assigned to each genome - same as Kraken abundance
     * *Not present for `sylph query`*
- **Adjusted_ANI**: adjusted containment ANI estimate.
    * If coverage adjustment is possible (cov is < 3x cov): returns coverage-adjusted ANI
    * If coverage is too low/high: returns Naive_ANI (see below)
- **Eff_cov/True_cov**: an estimate of the effective coverage (Eff_cov). If `-u` specified, the true coverage (True_cov). **Always a decimal number.** 
    * "Effective" coverage is a k-mer based estimate of the sequencing depth-of-coverage. It slightly underestimates the true sequencing depth. See the paper for more information. 
- **ANI_5-95_percentile**: [5%,95%] confidence intervals. **Not always a decimal number**.
    * If coverage adjustment is possible: `float-float` e.g. `98.52-99.55`
    * If coverage is too low/high: `NA-NA` is given. 
- **Eff_lambda**: estimate of the effective coverage parameter. **Not always a decimal number**. 
    * If coverage adjustment is possible: lambda estimate is given
    * If coverage is too low/high: `LOW` or `HIGH` is output
- **Lambda_5-95_percentile:** [5%, 95%] confidence intervals for lambda. Same format rules as ANI_5-95_percentile.
- **Median_cov**: median k-mer multiplicity for k-mers with >= 1 multiplicity.
- **Mean_cov_geq1**: mean k-mer multiplicity for k-mers with >= 1 multiplicity.
- **Containment_ind**: `int/int` showing the containment index (number of k-mers found in sample divided by total k-mers), e.g. `959/1053`.
- **Naive_ANI**: containment ANI without coverage adjustment.
- **kmers_reassigned**: the number of k-mers reassigned away from the genome. 
     * *Not present for `sylph query`*
- **Contig_name**: name of the first contig in the genome (or just the contig name for the -i option).

## How do I get a taxonomic profile like MetaPhlAn?

See the [manual outlined here](sylph-tax.md).

## Note on containment ANI

In sylph, ANI implicitly means **containment ANI** between a genome and a metagenome. 

Containment ANI is calculated from the number of k-mers in a reference genome contained in a metagenome. 

1. If the "metagenome" is a _single genome_, the containment ANI approximates the standard ANI. 
2. If the "metagenome" is a _collection of genomes_, the containment ANI can be interpreted as a "nearest neighbour ANI". 
3. **(profiling)**: If the metagenome is _reads from a collection of genomes_, sylph estimates an adjusted (containment) ANI that is the same as case 2 _with a statistical model_. 

Note: containment ANI slightly overestimates the true ANI. See Supplementary Figures in our paper. 
