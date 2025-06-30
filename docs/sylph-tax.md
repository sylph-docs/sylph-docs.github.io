# sylph-tax - incorporating taxonomy into [sylph](https://github.com/bluenote-1577/sylph)

!!! note
    The [sylph-tax](https://github.com/bluenote-1577/sylph-tax) repository replaces the old [sylph-utils](https://github.com/bluenote-1577/sylph-utils) repository. `sylph-tax` is easier to download/install and use than `sylph-utils`.  

Sylph's TSV outputs do not have taxonomic information. E.g., `GCA_000011.fasta` is a database genome which is found within your metagenome, but this contains no information about the species, genus, order, etc. 

`sylph-tax` can turn `sylph`'s TSV output into a **taxonomic profile** like Kraken or MetaPhlAn. `sylph-tax` does this by using custom taxonomy files to annotate sylph's output. 

## Taxonomy integration - available databases with taxonomy files

The following [pre-built sylph databases](pre‐built-databases.md) have available taxonomic annotations. Custom taxonomies can also be incorporated.


| sylph-tax identifier (used in `taxprof` command)  | Database description                                                                                             | Clades     |
| ---------------------- | ---------------------------------------------------------------------------------------------------------------- | ---------- |
| GTDB_r220              | [GTDB-r220 (April 2024)](https://gtdb.ecogenomic.org/stats/r220)                                                 | Prokaryote |
| GTDB_r214              | [GTDB-r214 (April 2023)](https://gtdb.ecogenomic.org/stats/r214)                                                 | Prokaryote |
| OceanDNA               | [OceanDNA - ocean MAGs from Nishimura & Yoshizawa](https://doi.org/10.1038/s41597-022-01392-5)                   | Prokaryote |
| SoilSMAG               | [Soil MAGs (SMAG) from Ma et al.](https://www.nature.com/articles/s41467-023-43000-z)                            | Prokaryote |
| FungiRefSeq-2024-07-25 | Refseq fungi representative genomes collected on 2024-07-25                                                      | Eukaryote  |
| TaraEukaryoticSMAG     | [TARA eukaryotic SMAGs from Delmont et al.](https://www.sciencedirect.com/science/article/pii/S2666979X22000477) | Eukaryote  |
| IMGVR_4.1              | [IMG/VR 4.1 high-confidence viral OTU genomes](https://genome.jgi.doe.gov/portal/IMG_VR/IMG_VR.home.html)        | Virus      |
| UHGV_ictv | [Unified Human Gut Virus Catalog](https://github.com/snayfach/UHGV) - [ICTV](https://ictv.global/)-like taxonomy        | Virus      |
| UHGV_default              | [Unified Human Gut Virus Catalog](https://github.com/snayfach/UHGV) - UHGV taxonomy      | Virus      |

## How do I use and install sylph-tax?

See the [sylph-tax install and quick start guide here](sylph-tax-quick-start.md) or the navigation side bar. 
