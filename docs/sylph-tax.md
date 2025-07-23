# sylph-tax - incorporating taxonomy into [sylph](https://github.com/bluenote-1577/sylph)

Sylph's TSV outputs do not have taxonomic information. For example, the genome `GCA_000011.fasta` may be found within your metagenome, but this contains no information about the species, genus, order, etc. 

`sylph-tax` can turn `sylph`'s TSV output into a **taxonomic profile** like Kraken or MetaPhlAn. `sylph-tax` does this by using custom taxonomy files to annotate sylph's output. 

## How do I use and install sylph-tax?

See the [sylph-tax install and quick start guide here](sylph-tax-quick-start.md) or the navigation side bar. 

## Taxonomy integration - available databases with taxonomy files

The following [pre-built sylph databases](pre‐built-databases.md) have available taxonomic annotations in the latest version of sylph-tax. Custom taxonomies can also be incorporated.

| sylph-tax identifier (used in `taxprof` command)  | Database description                                                                  | Clades     |
| ---------------------- | ---------------------------------------------------------------------------------------------------------------- | ---------- |
| GTDB_r226              | [GTDB-r226 (April 2025)](https://gtdb.ecogenomic.org/stats/r226)                                                 | Prokaryote |
| GTDB_r220              | [GTDB-r220 (April 2024)](https://gtdb.ecogenomic.org/stats/r220)                                                 | Prokaryote |
| GTDB_r214              | [GTDB-r214 (April 2023)](https://gtdb.ecogenomic.org/stats/r214)                                                 | Prokaryote |
| GlobDB_r226            | [GlobDB-r226 - a massive prokaryotic genome/MAG catalog](https://globdb.org/)                                    | Prokaryote |
| OceanDNA               | [OceanDNA - ocean MAGs from Nishimura & Yoshizawa](https://doi.org/10.1038/s41597-022-01392-5)                   | Prokaryote |
| SoilSMAG               | [Soil MAGs (SMAG) from Ma et al.](https://www.nature.com/articles/s41467-023-43000-z)                            | Prokaryote |
| FungiRefSeq-2024-07-25 | Refseq fungi representative genomes collected on 2024-07-25                                                      | Eukaryote  |
| TaraEukaryoticSMAG     | [TARA eukaryotic SMAGs from Delmont et al.](https://www.sciencedirect.com/science/article/pii/S2666979X22000477) | Eukaryote  |
| IMGVR_4.1              | [IMG/VR 4.1 high-confidence viral OTU genomes](https://genome.jgi.doe.gov/portal/IMG_VR/IMG_VR.home.html)        | Virus      |
| UHGV_ictv | [Unified Human Gut Virus Catalog](https://github.com/snayfach/UHGV) - [ICTV](https://ictv.global/)-like taxonomy        | Virus      |
| UHGV_default          | [Unified Human Gut Virus Catalog](https://github.com/snayfach/UHGV) - UHGV taxonomy      | Virus      |

