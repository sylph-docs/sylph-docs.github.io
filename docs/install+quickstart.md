## Very quick start

### Profile metagenome sample against [GTDB-R220](https://gtdb.ecogenomic.org/) (113,104 bacterial/archaeal species) 

```sh
conda install -c bioconda sylph

# download GTDB-R220 pre-built database (~13 GB)
wget http://faust.compbio.cs.cmu.edu/sylph-stuff/gtdb-r220-c200-dbv1.syldb

# multi-sample paired-end profiling (sylph version >= 0.6)
sylph profile gtdb-r220-c200-dbv1.syldb -1 *_1.fastq.gz -2 *_2.fastq.gz -t (threads) > profiling.tsv

# multi-sample single-end profiling
sylph profile gtdb-r220-c200-dbv1.syldb *.fastq -t (threads) > profiling.tsv
```


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
