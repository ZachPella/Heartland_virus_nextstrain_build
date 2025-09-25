# Heartland Virus Nextstrain Build

This repository contains a Nextstrain build for phylogenetic analysis of Heartland virus L segment sequences. The pipeline processes viral genome sequences to create interactive phylogenetic visualizations showing the evolution and geographic distribution of Heartland virus.

<img width="1207" height="615" alt="image" src="https://github.com/user-attachments/assets/4ec14475-ba27-4b6e-b9d8-6ef4a1bb63a6" />
<img width="1199" height="566" alt="image" src="https://github.com/user-attachments/assets/1701735a-2ca3-4d9b-abe2-2ec00b88333d" />

## Overview

Heartland virus is a phlebovirus (family Phenuiviridae) first discovered in Missouri in 2009. This build analyzes the L (large) segment sequences to understand the phylogenetic relationships and spatiotemporal patterns of the virus.

## Data

The analysis includes:
- **29 Heartland virus L segment sequences** from various US states and one from China
- **Geographic coverage**: Missouri, Georgia, New York, Tennessee, Kentucky, Kansas, Nebraska, and China
- **Temporal range**: 2009-2023
- **Reference sequence**: JX005847.1 (2009 Missouri isolate)

### Sample Distribution by State
- Georgia: 6 sequences
- New York: 8 sequences  
- Missouri: 5 sequences
- Kentucky: 3 sequences
- Tennessee: 1 sequence
- Kansas: 1 sequence
- Nebraska: 1 sequence
- China: 1 sequence

## Pipeline

The Nextstrain build performs the following steps:

1. **Sequence indexing** - Create composition index for filtering
2. **Filtering** - Select representative sequences (max 20 per state/year, from 2009 onwards)
3. **Alignment** - Align sequences to reference genome, filling gaps with N
4. **Tree building** - Construct maximum likelihood phylogenetic tree using 14 threads
5. **Tree refinement** - Estimate timetree with molecular clock, filter outliers
6. **Ancestral reconstruction** - Infer ancestral sequences and mutations
7. **Translation** - Translate to amino acid sequences using GenBank reference
8. **Trait inference** - Reconstruct ancestral geographic states (state and country)
9. **Export** - Generate JSON file for Auspice visualization

## Configuration

Key parameters:
- **Coalescent model**: Optimized
- **Date inference**: Marginal 
- **Clock filter**: 4 IQDs from expectation
- **Root**: JX005847.1 (2009 Missouri sequence)
- **Geographic levels**: Country and State
- **Grouping**: By state and year (max 20 sequences per group)

## Requirements

- [Nextstrain CLI](https://docs.nextstrain.org/projects/cli/en/stable/)
- [Augur](https://docs.nextstrain.org/projects/augur/en/stable/) (phylogenetic analysis)
- [Auspice](https://docs.nextstrain.org/projects/auspice/en/stable/) (visualization)

## Usage

### Running the complete pipeline:
```bash
nextstrain build .
```

### Running specific steps:
```bash
# Filter sequences
snakemake results/filtered.fasta

# Build tree  
snakemake results/tree.nwk

# Generate final visualization
snakemake auspice/heartland.json
```

### View results:
```bash
nextstrain view .
```

## Output

The pipeline generates:
- **Phylogenetic tree** with temporal calibration
- **Geographic visualization** showing virus spread patterns  
- **Mutation analysis** identifying key genetic changes
- **Interactive Auspice visualization** at `auspice/heartland.json`

## Files Structure

```
├── Snakefile              # Main pipeline workflow
├── data/
│   ├── sequences.fasta     # Input sequences  
│   └── metadata.tsv        # Sample metadata
├── config/
│   ├── reference.fasta     # Reference sequence
│   ├── heartland_segments.gb # GenBank annotation
│   ├── colors.tsv         # Color scheme
│   ├── lat_longs.tsv      # Geographic coordinates
│   └── auspice_config.json # Visualization config
├── results/               # Intermediate analysis files
└── auspice/              # Final visualization JSON
    └── heartland.json
```

## Cleaning

Remove all generated files:
```bash
snakemake clean
```

## Citation

If you use this build in your research, please cite:
- [Nextstrain](https://nextstrain.org)
- Original Heartland virus discovery: McMullan et al. N Engl J Med 2012

## License

This analysis pipeline is provided under open source principles. Sequence data sources and citations are included in the metadata.

---

For questions or issues, please refer to the [Nextstrain documentation](https://docs.nextstrain.org) or open an issue in this repository.
