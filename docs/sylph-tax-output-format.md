## `.sylphmpa` taxonomic profiling output format

`*.sylphmpa` files look like this: 

```
#SampleID       /home/jshaw/projects/temp/amr/short_reads/SRR14739086_1.fastq.gz        Taxonomies_used:['GTDB_r220']
clade_name      relative_abundance      sequence_abundance      ANI (if strain-level)    Coverage (if strain-level)
d__Bacteria     100.00010000000003      100.00019999999996      NA      NA
d__Bacteria|p__Pseudomonadota   100.00010000000003      100.00019999999996      NA      NA
d__Bacteria|p__Pseudomonadota|c__Gammaproteobacteria    100.00010000000003      100.00019999999996      NA      NA
d__Bacteria|p__Pseudomonadota|c__Gammaproteobacteria|o__Enterobacterales        35.6384 36.0603 NA      NA
....
```


!!! tip

    This is a valid TSV file, but rows prefixed with `#` are comments.
    You can read `.sylphmpa` files with pandas in python like `pd.read_csv('output.sylphmpa',sep='\t', comment='#')`. 

There are five important columns:

1. `clade_name`: A string like `d__Bacteria|p__Actinomycetota|c__Acidimicrobiia|o__Acidimicrobiales|f__Ilumatobacteraceae` that describes the clade. `t__STRAIN` represents the exact genome identifier. 
2. `relative_abundance`: the taxonomic relative abundance of the clade
3. `sequence_abundance`: the sequence abundance of the clade, i.e. the % of reads assigned
4. `ANI`: this is `NA` except for at the strain level (`t__strain`). Otherwise it is sylph's ANI estimate. 
5. `Coverage`: This is the `Eff_cov` or `True_cov` column of sylph's output.

!!! tip

     Viral-host information is available for IMG/VR 4.1. The `-a` option adds a new column in the `.sylphmpa` files associating viral genomes to their hosts. For example:

    - `r__Duplodnaviria|k__Heunggongvirae|p__Uroviricota|c__Caudoviricetes|||||t__IMGVR_UViG_2503982007_000001 ...    d__Bacteria;p__Firmicutes;c__Bacilli;o__Staphylococcales;f__Staphylococcaceae;g__Staphylococcus;s__Staphylococcus epidermidis` 

    indicates that IMGVR_UVIG_2503982007's host is Staphylococcus epidermidis.
