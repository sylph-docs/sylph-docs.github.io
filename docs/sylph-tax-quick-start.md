## Install

#### Install option 1 - Conda

```sh
conda install -c bioconda sylph-tax
```

#### Install option 2 - Python

```
git clone https://github.com/bluenote-1577/sylph-tax
cd sylph-tax
pip install .
```

## Quick start

#### Step 1: Profile using one or more of the available databases with sylph

```sh
# profile something with pre-built GTDB_r220 / IMGVR_4.1 databases
sylph profile .... gtdb-r220-c200-dbv1.syldb imgvr_c200_v0.3.0.syldb  > sylph_results/my_result.tsv
```

#### Step 2: use `sylph-tax` to get taxonomic profile

- Download the taxonomy files (only needs to be done once)

```sh
# download all taxonomy files (~50 MB)
sylph-tax download --download-to /any/folder
```

- Use `sylph-tax taxprof` and specify the `sylph-tax identifiers` [in this table](sylph-tax.md/#taxonomy-integration-available-databases-with-taxonomy-files) (the first column) corresponding to your database
- The option `-a` or `--annotate-virus-hosts` outputs virus host predictions if using a valid viral database

```sh
# incorporate GTDB-r220 and IMGVR-4.1 taxonomies into sylph's results
sylph-tax taxprof sylph_results/*.tsv -t GTDB_r220 IMGVR_4.1 -o output_prefix-

ls output_prefix-sample1.sylphmpa
ls output_prefix-sample2.sylphmpa
...
```

- Merge results (optional)

```sh
# merge multiple results
sylph-tax merge *.sylphmpa --column relative_abundance -o merged_abundance_file.tsv
```

