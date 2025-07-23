### `download` - download taxonomy metadata 

```
sylph-tax download --download-to /my/folder/sylph_taxonomy_files/
```

* Downloads taxonomic annotation files (~50 MB; [see here](https://zenodo.org/records/14320496)) to `--download-to`.
* The `--download-to` folder must exist. The location can be wherever. Its location is written to `~/.config/sylph-tax/config.json`. 
* If you don't have access to `$HOME`, you can specify a custom location in the `SYLPH_TAXONOMY_CONFIG` environment variable. E.g. `export SYLPH_TAXONOMY_CONFIG=/write_access_folder/sylph-tax-config.json`.
### `taxprof` - taxonomic profiles from sylph's output

```sh
sylph-tax taxprof sylph_results/*.tsv  -o prefix_or_folder/ -t {sylph-tax identifier}
```

* `sylph_results/*.tsv`: outputs from sylph. **The databases used for sylph must be the same as the `-t` option.**
* `-t/--taxonomy-metadata`:  A list of `sylph-tax identifier`s specified [in this table](sylph-tax.md/#taxonomy-integration-available-databases-with-taxonomy-files) (e.g. `GTDB_r220` or `IMGVR_4.1`).  Multiple taxonomy metadata files can be input. [Custom taxonomy files are also possible](sylph-tax-custom-taxonomies.md).
* `-o`: prepends this prefix to all of the output files. One file is output per sample in `sylph_output.tsv`
* `-a/--annotate-virus-hosts`: annotates viral genomes with host information metadata (only works if using a pre-built viral database)
* `--pavian`: outputs a taxonomy file that can be visualized via [pavian](https://fbreitwieser.shinyapps.io/pavian/) but removes some relevant statistics. 
* Output suffix is `.sylphmpa`.  

!!! tip

    In python/pandas, `pd.read_csv('output.sylphmpa',sep='\t', comment='#')` works.

### `merge` - merge multiple taxonomic profiles


```sh
sylph-tax merge *.sylphmpa --column {ANI, relative_abundance, sequence_abundance} -o output_table.tsv
```

* `*.sylphmpa` files are outputs from `sylph-tax taxprof`. 
* `--column` can be ANI, relative abundance, or sequence abundance (see paper for difference between abundances)
* `-o` output file in TSV format.

#### Output format for `merge` (TSV)

```sh
clade_name  sample1.fastq.gz  sample2.fastq.gz
d__Archaea  0.0  1.1
d__Archaea|p__Methanobacteriota 0.0     0.0965
...
```
