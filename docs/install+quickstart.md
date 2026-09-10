!!! tip
    Click [here to view an infographic summarizing all of the below information.](../assets/sylph_workflow.html)

## Very quick start

### Profile metagenome against a [GTDB database](https://gtdb.ecogenomic.org/) with ~ 200,000 species

!!! note
    See the [prebuilt databases](pre‐built-databases.md) for available versions of GTDB.

```sh
conda install -c bioconda sylph

# download GTDB pre-built database (~19 GB)
wget http://faust.compbio.cs.cmu.edu/sylph-stuff/gtdb-r232-c200-dbv2.syl2db

# multi-sample paired-end profiling 
sylph profile -d gtdb-r232-c200-dbv2.syl2db -1 *_1.fastq.gz -2 *_2.fastq.gz -t (threads) > profiling.tsv

# multi-sample single-end profiling
sylph profile -d gtdb-r232-c200-dbv2.syl2db -r *.fastq -t (threads) > profiling.tsv
```

The output file `profiling.tsv` shows what database genomes are present. This does not have taxonomic information (e.g. species/genus/family). If you want a taxonomic annotations, use [sylph-tax](sylph-tax.md). 

## Install options

#### Option 1: conda install 
[![Anaconda-Server Badge](https://anaconda.org/bioconda/sylph/badges/version.svg)](https://anaconda.org/bioconda/sylph)
[![Anaconda-Server Badge](https://anaconda.org/bioconda/sylph/badges/latest_release_date.svg)](https://anaconda.org/bioconda/sylph)

```sh
conda install -c bioconda sylph
```

#### Option 2: Build from source

Requirements:

1. [rust](https://www.rust-lang.org/tools/install) (version > 1.63) programming language and associated tools such as cargo are required and assumed to be in PATH.
2. A c compiler (e.g. GCC)
3. make
4. cmake

Building takes a few minutes (depending on # of cores).

```sh
git clone https://github.com/bluenote-1577/sylph
cd sylph

# If default rust install directory is ~/.cargo
cargo install --path . --root ~/.cargo
sylph profile test_files/*
```
#### Option 3: Pre-built x86-64 linux statically compiled executable

If you're on an x86-64 system, you can download the binary and use it without any installation. 

```sh
wget https://github.com/bluenote-1577/sylph/releases/download/latest/sylph
chmod +x sylph
./sylph -h
```

Note: the binary is compiled with a different set of libraries (musl instead of glibc), probably impacting performance. 
