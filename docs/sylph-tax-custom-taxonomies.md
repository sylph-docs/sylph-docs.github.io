## Creating custom taxonomies

If you're working with custom sylph databases, you can easily create your own taxonomy metadata file. You can look at our pre-built taxonomy files (https://zenodo.org/records/14320496) for examples. 

A taxonomic metadata file is simply a two-column TSV file:

- Column 1: the name of your genome's FASTA file: 
	- `my_mag.fa`
- Column 2: a semicolon-delimited taxonomy string. 
	- `d__Archaea;p__Methanobacteriota_B;c__Thermococci;o__Thermococcales;f__Thermococcaceae;g__Thermococcus_A;s__Thermococcus_A alcaliphilus`

Note: do not add the `t__STRAIN` line.

### Custom taxonomy example usage case

You obtained two new MAGs: `genome1.fa` and `genome2.fa` and you ran GTDB-tk to get their taxonomic annotation. You want to to profile against the new MAGs and the GTDB database.

1. Create a file called `taxonomy.tsv` as follows:

    ```
    genome1.fa d__Archaea;(...);s__My new species name`
    genome2.fa d__Bacteria;(...);g__My genus name;s__My species name2`
    ```

2. Use `taxonomy.tsv` as an argument to `sylph-tax taxprof`.

    ```sh
    ## profile against gtdb_r220 and your new MAGs
    sylph profile gtdb_r220.syldb my_custom_mags.syldb ... -o gtdb+mags_output.tsv

    ## use your new taxonomy.tsv file and GTDB_r220
    sylph-tax taxprof gtdb+mags_output.tsv -t GTDB_r220 taxonomy.tsv
    ```

!!! note

    The parsing of the taxonomic metadata file is done in the script https://github.com/bluenote-1577/sylph-tax/blob/main/sylph_tax/sylph_to_taxprof.py. Refer to this reference implementation if needed. 


!!! warning

    For Genbank/RefSeq genomes, filenames have to be dealt with carefully.

    - If `_genomic` or `_ASM` is in your genome file name, use the part before `_genomic` or `_ASM`.

    So for `GCF_002863645.1_ASM286364v1_genomic.fna.gz`, use `GCF_002863645.1` in column 1. 

## Creating taxonomy metadata from RefSeq?

See [this discussion thread](https://github.com/bluenote-1577/sylph/issues/14#issuecomment-2313259637).
