# BUSCO-Colab

<div align="center">

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1S_DMl1tFxSiI4w2kKdBHd9pXGdDSK9Rw?usp=sharing)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![BUSCO v6](https://img.shields.io/badge/BUSCO-v6.0.0-green.svg)](https://busco.ezlab.org/)
[![Paper](https://img.shields.io/badge/MBE-10.1093/molbev/msab199-red.svg)](https://doi.org/10.1093/molbev/msab199)

</div>

## Overview

This Google Colab notebook provides an easy-to-use interface for **BUSCO v6.0.0** (Benchmarking Universal Single-Copy Orthologs), a tool for assessing genome assembly and annotation completeness. BUSCO was developed by Manni et al. (2021) and the Zdobnov Lab, using evolutionarily-informed expectations of gene content from OrthoDB v12. This implementation is a user-friendly Colab wrapper that makes BUSCO accessible through a streamlined notebook interface optimized for proteome analysis.

**🚀 [Click here to open the BUSCO-Colab notebook](https://colab.research.google.com/drive/1S_DMl1tFxSiI4w2kKdBHd9pXGdDSK9Rw?usp=sharing)**

> **Note**: This is an unofficial Google Colab implementation. All credit for BUSCO, its methodology, and scientific contributions goes to the original authors. This notebook simply provides a convenient interface for running their published tool.

## Key Features of This Interface

- **🧬 BUSCO v6.0.0**: Access to the latest BUSCO version with OrthoDB v12 datasets
- **⚡ Zero Setup**: Automated conda environment and dependency installation
- **📁 Flexible Input**: Direct upload or Google Drive paths with automatic .gz decompression
- **🤖 Smart Automation**: Auto-detection of CPU cores, proteome names, and FASTA header cleaning
- **☁️ Google Drive Integration**: Automatic backup of results with intelligent folder naming
- **🏷️ Intelligent Naming**: Output folders named as `ProteomeName_BUSCOv6.0.0_DatabaseName`
- **📊 Complete Results**: Summary statistics, full tables, and gene-level results
- **⚙️ Optimized Performance**: Automatic CPU maximization (2-44 threads depending on runtime selection)
- **🔄 Progress Tracking**: Real-time stage indicators and time estimation

---

## Quick Start

### 1. Open the Notebook
Click the "Open in Colab" badge above

### 2. **⚡ CRITICAL: Select TPU Runtime for Best Performance**
Before running any cells:
- Go to **Runtime → Change runtime type**
- Set **Hardware accelerator** to **TPU v5e-1** or **TPU v6e-1**
- This gives you **24-44 CPU threads** instead of 2
- BUSCO runs 12-22x faster on TPU runtime

### 3. Setup Environment (1-2 minutes)
Run **Cell 1** to install conda/mamba environment
- Kernel will restart automatically
- Wait for restart before proceeding

### 4. Install BUSCO (3-5 minutes)
Run **Cell 2** to install BUSCO v6.0.0 and dependencies via bioconda

### 5. Upload Your Proteome
Run **Cell 3** to upload your protein FASTA file
- **Option A**: Direct upload (< 100 MB recommended)
- **Option B**: Google Drive path for large files
- Supports: `.faa`, `.fasta`, `.fa`, `.pep`, `.gz` (auto-decompressed)

### 6. Select BUSCO Lineage
- **Cell 4**: Browse 100+ available OrthoDB v12 lineages
- **Cell 5**: Download your chosen lineage (1-5 minutes)
  - Plants: `solanales_odb12`, `fabales_odb12`, `embryophyta_odb12`
  - Fungi: `fungi_odb12`, `ascomycota_odb12`
  - Animals: `metazoa_odb12`, `vertebrata_odb12`
  - Bacteria: `bacteria_odb12`, `proteobacteria_odb12`
  - And many more...

### 7. Run Analysis
Run **Cell 6** to start BUSCO analysis
- **Optional**: Custom output name (leave empty for auto-naming from proteome file)
- **Automatic**: CPU threads set to maximum available
- **Time**: 1-30 minutes depending on proteome size and runtime
- **Results**: Automatically saved to Google Drive and downloaded as ZIP

---

## System Requirements & Performance

### ⚡ Runtime Selection (CRITICAL FOR PERFORMANCE)

Google Colab provides vastly different CPU resources depending on your runtime selection. **The notebook automatically detects and uses all available CPU threads**, but you must select the right runtime type first.

#### **Recommended: TPU Runtime (Best Performance)**

| TPU Type | CPU Threads | RAM | How to Enable |
|----------|-------------|-----|---------------|
| **TPU v5e-1** | 24 | 48 GB | Runtime → Change runtime type → TPU v5e-1 |
| **TPU v6e-1** | 44 | 176 GB | Runtime → Change runtime type → TPU v6e-1 |

- **Available on**: Free, Pro, and Pro+ tiers
- **Performance**: 12-22x faster than standard GPU/CPU runtime
- **Note**: You're using the TPU's CPU host, not the TPU cores themselves

**Why TPU for BUSCO?** BUSCO is CPU-intensive (HMMER searches, gene prediction). The TPU runtime provides the highest CPU core count on Colab, making it ideal for large proteomes even though we're not using TPU acceleration.

#### **Standard GPU/CPU Runtime (Not Recommended for Large Proteomes)**

| Tier | Standard RAM | High-RAM (Pro+ only) |
|------|--------------|----------------------|
| **Free** | 2 threads, ~13 GB RAM | Not available |
| **Pro** ($9.99/month) | 2 threads, ~13 GB RAM | 2-4 threads, ~25-32 GB RAM |
| **Pro+** ($49.99/month) | 2 threads, ~13 GB RAM | **8 threads, ~52 GB RAM** |

**To enable High-RAM (Pro+)**: Runtime → Change runtime type → Select "High-RAM"

### Performance Comparison

| Runtime Type | CPU Threads | Small Proteome<br>(<10k seqs) | Large Proteome<br>(>50k seqs) |
|-------------|-------------|-------------------------------|-------------------------------|
| **GPU/CPU Standard** | 2 | 10-15 min | 30-60 min |
| **Pro+ High-RAM** | 8 | 3-5 min | 10-20 min |
| **TPU v5e-1** | 24 | 1-3 min | 5-10 min |
| **TPU v6e-1** | 44 | <1 min | 3-8 min |

### Input Requirements
- **Format**: Valid FASTA with `>` headers
- **Type**: Protein sequences (amino acids only)
- **Size**: No hard limit (use Google Drive for files > 100 MB)
- **Compression**: Native `.gz` support with automatic decompression

---

## Output Structure

```
BUSCO_Results/
└── YourProteome_BUSCOv6.0.0_solanales_odb12/
    ├── short_summary.txt              # Quick overview of BUSCO results
    ├── full_table.tsv                 # Gene-by-gene completeness scores
    ├── missing_busco_list.tsv         # List of missing BUSCOs
    ├── fragmented_busco_sequences/    # Partial gene matches
    ├── single_copy_busco_sequences/   # Complete single-copy orthologs
    └── hmmer_output/                  # Raw HMMER search results
```

---

## Technical Implementation

### Automated Features
- **CPU Detection**: `psutil.cpu_count()` for automatic core detection and maximization
- **Header Sanitization**: Removes problematic characters (`/|:;,()[]{}`) that crash BUSCO
- **Name Detection**: Auto-extracts proteome basename and sanitizes for filesystem safety
- **Version Tracking**: Dynamically detects BUSCO version for accurate folder naming
- **Compression Handling**: Automatic `.gz` decompression for both upload and Drive paths

### Dependencies (Auto-installed)
- BUSCO v6.0.0
- HMMER v3.4
- Augustus
- BioPython
- pandas
- prodigal

### Environment
- Python 3.11
- Conda/Mamba package manager
- Bioconda channel for bioinformatics tools

---

## Troubleshooting

### Slow Performance
**Problem**: Analysis takes 30+ minutes for moderate-sized proteome

**Solution**: You're likely using standard GPU/CPU runtime (2 threads)
1. Go to **Runtime → Change runtime type**
2. Select **Hardware accelerator → TPU v5e-1** or **TPU v6e-1**
3. Restart runtime and re-run setup cells
4. You'll now have 24-44 threads instead of 2

### "Command not found: busco"
**Solution**: Ensure you completed the setup sequence:
1. Run Cell 1 (Setup Conda) → Wait for kernel restart
2. Run Cell 2 (Install BUSCO) → Wait for completion
3. Proceed to upload and analysis cells

### "Invalid FASTA format" or Header Errors
**Solution**: The notebook automatically sanitizes headers, but ensure:
- File starts with `>` character
- Sequences are amino acids (not nucleotides)
- No binary data or corrupted content

### "Lineage dataset not found"
**Solution**: Download the lineage first:
1. Run Cell 4 to browse available lineages
2. Copy the exact lineage name (e.g., `solanales_odb12`)
3. Run Cell 5 to download

---

## Citation

If you use this notebook in your research, please cite the BUSCO publications:

### BUSCO v6 (Current Version)
```bibtex
@article{manni2021busco,
  title={BUSCO Update: Novel and Streamlined Workflows along with Broader and Deeper Phylogenetic Coverage for Scoring of Eukaryotic, Prokaryotic, and Viral Genomes},
  author={Manni, Mosè and Berkeley, Matthew R and Seppey, Mathieu and Simão, Felipe A and Zdobnov, Evgeny M},
  journal={Molecular Biology and Evolution},
  volume={38},
  number={10},
  pages={4647--4654},
  year={2021},
  publisher={Oxford University Press},
  doi={10.1093/molbev/msab199}
}
```

### Original BUSCO Publication
```bibtex
@article{simao2015busco,
  title={BUSCO: assessing genome assembly and annotation completeness with single-copy orthologs},
  author={Simão, Felipe A and Waterhouse, Robert M and Ioannidis, Panagiotis and Kriventseva, Evgenia V and Zdobnov, Evgeny M},
  journal={Bioinformatics},
  volume={31},
  number={19},
  pages={3210--3212},
  year={2015},
  publisher={Oxford University Press},
  doi={10.1093/bioinformatics/btv351}
}
```

---

## License

This notebook implementation is released under the **MIT License**. BUSCO itself is distributed under its own license terms (see [BUSCO GitLab](https://gitlab.com/ezlab/busco)).

---

## Acknowledgments

- **BUSCO Development**: Zdobnov Lab, University of Geneva
- **OrthoDB**: Maintained by the Zdobnov Lab and collaborators
- **Bioconda**: Community-maintained bioinformatics software distribution

---

## Links

- **Official BUSCO Documentation**: https://busco.ezlab.org/
- **BUSCO GitLab Repository**: https://gitlab.com/ezlab/busco
- **OrthoDB Database**: https://www.orthodb.org/
- **BUSCO User Guide**: https://busco.ezlab.org/busco_userguide.html

---

## Contributing

Issues and pull requests are welcome! When contributing:
- Test all changes in Google Colab environment
- Follow existing code style (minimal, surgical modifications)
- Update documentation for new features
- Ensure backward compatibility with existing workflows

---

## Support

**For this notebook**: [Open an issue on GitHub](https://github.com/YOUR_USERNAME/BUSCO-Colab/issues)

**For BUSCO itself**: Consult [official documentation](https://busco.ezlab.org/) or [BUSCO GitLab issues](https://gitlab.com/ezlab/busco/issues)
