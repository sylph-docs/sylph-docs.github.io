> [!WARNING]
> sylph-utils has been replaced with [sylph-tax](https://github.com/bluenote-1577/sylph-tax). sylph-utils still works but will not be updated anymore. 

## [sylph-utils](https://github.com/bluenote-1577/sylph-utils) - scripts for integrating taxonomic information

The [sylph-utils](https://github.com/bluenote-1577/sylph-utils) repository contains scripts/metadata for integrating taxonomy information into sylph's output. Details are given below.

## How to generate profiles with taxonomic information using sylph-utils

By default, sylph's TSV outputs contain no taxonomic information. However, sylph supports integrating a few major genome databases for taxonomic profiles. The pre-indexed versions of a few databases are available [here](https://github.com/bluenote-1577/sylph/wiki/Pre%E2%80%90built-databases).

In the [sylph-utils](https://github.com/bluenote-1577/sylph-utils) repository, the `sylph_to_taxprof.py` script can be used as follows

```sh
git clone https://github.com/bluenote-1577/sylph-utils
sylph profile  gtdb-r220-c200-dbv1.syldb sample.sylsp -o result.tsv
python sylph-utils/sylph_to_taxprof.py -s result.tsv -m sylph-utils/prokaryote/gtdb_r220_metadata.tsv.gz
ls *.sylphmpa
```

The `gtdb_r220_metadata.tsv.gz` file links the GTDB taxonomy to each genome. It groups all the results by sample name and outputs a file called `sample.sylphmpa` with the format shown below.  

## MetaPhlAn-like or CAMI-like outputs

`*.sylphmpa` files look like this: 

```
#SampleID       biofilm_reads/SRR24442552_1.fastq.gz
clade_name     relative_abundance      sequence_abundance      ANI (if strain-level)
d__Bacteria     99.99999999999997       3.4775000000000014      NA
d__Bacteria|p__Actinomycetota   1.7266  0.0414  NA
d__Bacteria|p__Actinomycetota|c__Acidimicrobiia 1.3727  0.0313  NA
d__Bacteria|p__Actinomycetota|c__Acidimicrobiia|o__Acidimicrobiales     1.3727  0.0313  NA
d__Bacteria|p__Actinomycetota|c__Acidimicrobiia|o__Acidimicrobiales|f__Ilumatobacteraceae       1.3727  0.0313  NA
d__Bacteria|p__Actinomycetota|c__Acidimicrobiia|o__Acidimicrobiales|f__Ilumatobacteraceae|g__Casp-actino5       1.0267  0.0172  NA
d__Bacteria|p__Actinomycetota|c__Acidimicrobiia|o__Acidimicrobiales|f__Ilumatobacteraceae|g__Casp-actino5|s__Casp-actino5       1.0267  0.0172  NA
d__Bacteria|p__Actinomycetota|c__Acidimicrobiia|o__Acidimicrobiales|f__Ilumatobacteraceae|g__Casp-actino5|s__Casp-actino5|t__GCA_017859785.1    1.0267  0.0172  98.54
d__Bacteria|p__Actinomycetota|c__Acidimicrobiia|o__Acidimicrobiales|f__Ilumatobacteraceae|g__Ilumatobacter      0.346   0.0141  NA
....
```

> [!TIP]
> This is a valid TSV file, but rows prefixed with `#` are comments.
> You can read `.sylphmpa` files with pandas in python like `pd.read_csv('output.sylphmpa',sep='\t', comment='#')`. 

There are five important columns:

1. `clade_name`: A string like `d__Bacteria|p__Actinomycetota|c__Acidimicrobiia|o__Acidimicrobiales|f__Ilumatobacteraceae` that describes the clade. `t__STRAIN` represents the exact genome identifier. 
2. `relative_abundance`: the taxonomic relative abundance of the clade
3. `sequence_abundance`: the sequence abundance of the clade, i.e. the % of reads assigned
4. `ANI`: this is `NA` except for at the strain level (`t__strain`). Otherwise it is sylph's ANI estimate. 
5. `Coverage`: (new in v0.2 of `sylph_to_taxprof.py`) This is the `Eff_cov` or `True_cov` column of sylph's output.

> [!TIP]
> In v0.2 of `sylph_to_taxprof.py`, viral-host information is available for IMG/VR 4.1. The `-a` option adds a new column in the .sylphmpa files associating viral genomes to their hosts. For example, a row can look like : 
>
> `r__Duplodnaviria|k__Heunggongvirae|p__Uroviricota|c__Caudoviricetes|||||t__IMGVR_UViG_2503982007_000001 ...    d__Bacteria;p__Firmicutes;c__Bacilli;o__Staphylococcales;f__Staphylococcaceae;g__Staphylococcus;s__Staphylococcus epidermidis` 
>
> where IMGVR_UVIG_2503982007's host is Staphylococcus epidermidis.

## Custom taxonomies and how it works

The `sylph_to_taxprof.py` file is quite simple. If you're serious about using a new taxonomy, it should be short enough to read. 

Briefly: the `gtdb_r220_metadata.tsv` file (can be gzipped or not) specified by the `-m` option is a file with two columns. The first column is the name of your fasta file, and the second column is a taxonomy string that looks like `d__Archaea;p__Methanobacteriota_B;c__Thermococci;o__Thermococcales;f__Thermococcaceae;g__Thermococcus_A;s__Thermococcus_A alcaliphilus`.

Let's say you want to add `genome1.fa` and `genome2.fa`, two new MAGs, to the GTDB taxonomy. You can change the `gtdb_r220_metadata.tsv` file, to look like this by appending two new lines:

```
...
...
GCA_945889495.1	d__Bacteria;p__Desulfobacterota_B;c__Binatia;o__UBA9968;f__UBA9968;g__DP-20;s__DP-20 sp945889495
GCA_934500585.1	d__Bacteria;p__Bacillota_A;c__Clostridia;o__UBA1381;f__UBA1381;g__RQCD01;s__RQCD01 sp008668455
genome1.fa d__Archaea;(...);s__My new species name`
genome2.fa d__Bacteria;(...);g__My_genus_name;s__My species name2`
```
and use it for the `-m` option in the python script. Don't add the `t__STRAIN` line; the script does it automatically. 

> [!WARNING] 
> For Genbank/RefSeq assemblies, filenames have to be dealt with carefully.
>
> **If `_genomic` or `_ASM` is in your genome file name, e.g. GCF_002863645.1_ASM286364v1_genomic.fna.gz, use "GCF_002863645.1", instead of the whole file name in the first column.**

## Creating taxonomy metadata from RefSeq?

See [this discussion thread](https://github.com/bluenote-1577/sylph/issues/14#issuecomment-2313259637).