# MetaNextReo

**Metagenomic Nextflow Pipeline for Tracking Reovirus and Orthoreoviruses**

A comprehensive Nextflow pipeline for analyzing Avian Orthoreovirus (ARV) and related reovirus sequences from metagenomic sequencing data.

## Overview

MetaNextReo is designed to process paired-end Illumina sequencing data through a complete viral metagenomics workflow, from raw reads to annotated consensus genomes. The pipeline is optimized for detecting and characterizing orthoreoviruses in avian samples.

## Features

- Quality control with FastQC
- Flexible read trimming (Flexbar, Trim Galore, Trimmomatic)
- Taxonomic classification with Kraken2
- De novo assembly (MEGAHIT or metaSPAdes)
- BLAST-based viral identification
- Reference-based consensus genome generation
- Gene annotation with Prodigal

## Pipeline Workflow

```
Input (FASTQ) --> FastQC --> Trimming --> Assembly --> BLAST Annotation
                                                            |
                                                            v
                              Annotation <-- Consensus <-- Reference Mapping
```

## Requirements

- [Nextflow](https://www.nextflow.io/) (>=20.00.0)
- [Conda](https://docs.conda.io/) or container runtime (Docker/Singularity)
- Required tools:
  - FastQC
  - Flexbar / Trim Galore / Trimmomatic
  - Kraken2
  - MEGAHIT / metaSPAdes
  - BLAST+
  - Snippy
  - Prodigal

## Installation

```bash
# Clone the repository
git clone https://github.com/navduhan/metanextreo.git
cd metanextreo

# Run with Nextflow
nextflow run metanextreo.nf --help
```

## Usage

### Basic Usage

```bash
nextflow run metanextreo.nf --input samplesheet.csv --outdir results
```

### With SLURM

```bash
nextflow run metanextreo.nf --input samplesheet.csv --outdir results -profile slurm
```

### With Local Execution

```bash
nextflow run metanextreo.nf --input samplesheet.csv --outdir results -profile local
```

## Input Format

The pipeline requires a CSV samplesheet with the following format:

```csv
id,reads1,reads2
sample1,/path/to/sample1_R1.fastq.gz,/path/to/sample1_R2.fastq.gz
sample2,/path/to/sample2_R1.fastq.gz,/path/to/sample2_R2.fastq.gz
```

## Parameters

| Parameter | Default | Description |
|-----------|---------|-------------|
| `--input` | - | Path to input samplesheet (required) |
| `--outdir` | `results` | Output directory |
| `--adapters` | `assets/illumina_adapter.fa` | Path to adapter sequences |
| `--trimming_tool` | `flexbar` | Trimming tool (`flexbar`, `trim_galore`, `trimmomatic`) |
| `--quality` | `30` | Quality threshold for trimming |
| `--assembler` | `megahit` | Assembler to use (`megahit`, `metaspades`) |
| `--min_contig_length` | `200` | Minimum contig length |
| `--blast_options` | `['viruses']` | BLAST databases to use (`viruses`, `nt`, `nr`, `all`) |
| `--help` | `false` | Display help message |

### Database Parameters

| Parameter | Description |
|-----------|-------------|
| `--kraken2_db` | Path to Kraken2 database |
| `--blastdb_viruses` | Path to viral BLAST database |
| `--blastdb_nt` | Path to nt BLAST database |
| `--blastdb_nr` | Path to nr BLAST database |
| `--diamonddb` | Path to DIAMOND database |

## Output Structure

```
results/
├── fastqc/              # Raw read quality reports
├── trimming/            # Trimmed reads and reports
├── assembly/            # Assembled contigs
├── blast/               # BLAST annotation results
├── consensus/           # Consensus genome sequences
└── annotation/          # Gene predictions and annotations
```

## Profiles

- **local**: Run on local machine using available cores
- **slurm**: Run on SLURM cluster with configurable queue settings

## Citation

If you use MetaNextReo in your research, please cite:

```
MetaNextReo: Metagenomic Nextflow Pipeline for Tracking Reovirus and Orthoreoviruses
Naveen Duhan
https://github.com/navduhan/metanextreo
```

## Author

**Naveen Duhan**  
Email: naveen.duhan@outlook.com

## License

This project is available under the MIT License.
