# AI for Protein Design Workshop

[![Open Notebook 1 In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/pablo-arantes/spde-workshop-ai-protein-design/blob/main/Notebook1_RFdiffusion3_Binder_Design.ipynb)
[![Open Notebook 2 In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/pablo-arantes/spde-workshop-ai-protein-design/blob/main/Notebook2_PyRosetta_Physics_Filters.ipynb)

Hands-on workshop materials for the **2nd Symposium on Protein Design and Engineering (SPDE)** pre-event workshop **"AI for Protein Design"**, held October 5--6, 2026 at [CNPEM](https://cnpem.br/), Campinas, Brazil.

**Organized by:** CNPEM | **Supported by:** RosettaCommons, Serrapilheira | **Co-organizers:** Fiocruz, EMS

> **Symposium website:** [https://pages.cnpem.br/spde/](https://pages.cnpem.br/spde/)

---

## Overview

This workshop guides participants through a complete **de novo protein binder design** pipeline using state-of-the-art AI and physics-based tools, entirely on Google Colab (free GPU). By the end, you will have designed, validated, and ranked novel protein binders against a target of your choice.

### Pipeline

```
Target PDB
    |
    v
[Notebook 1] RFdiffusion3 --> LigandMPNN/solMPNN --> ColabFold AF2 --> Screening
    |                                                                      |
    |                                                          *_designs.zip
    v                                                                      |
[Notebook 2] FastRelax --> Rosetta Physics Filters --> Score Selection ---->
    |                                                                      |
    v                                                                      v
Relaxed PDBs + Binding Energies                         Best Candidates (3D)
```

---

## Repository Structure

```
spde-workshop-ai-protein-design/
|
|-- README.md                        <-- This file
|-- LICENSE
|
|
|-- Notebook1_RFdiffusion3_Binder_Design.ipynb
|   Design binders with RFdiffusion3 + LigandMPNN/solMPNN + ColabFold.
|   Screens by ipTM, pLDDT, binder RMSD. Produces a ZIP of designs.
|   
|-- Notebook2_PyRosetta_Physics_Filters.ipynb
|   Takes the ZIP from Notebook 1 and applies Rosetta-level physics
|   validation: FastRelax, ddG, SAP, contact molecular surface, SASA,
|   buried unsatisfied H-bonds. Selects best candidates.
|
|-- guides/
|   |-- Workshop_RFD3_Student_Guide.pdf
|   |   14-section reference for Notebook 1 (RFdiffusion3 pipeline).
|   |
|   |-- Workshop_PyRosetta_Filters_Student_Guide.pdf
|       13-section reference for Notebook 2 (PyRosetta filters).
|
|-- example_inputs/
    |-- (optional) Example target PDB files for the workshop
```

---

## Notebooks

### Notebook 1: RFdiffusion3 Binder Design Pipeline

| Stage | Tool | What it does |
|-------|------|--------------|
| 0 | Configuration | Target PDB, contig string, hotspot residues, number of designs |
| 1 | **RFdiffusion3** | Diffusion-based backbone generation for de novo binders |
| 2 | **LigandMPNN / solMPNN** | Inverse folding -- designs sequences for the generated backbones |
| 3 | **ColabFold (AF2 Multimer)** | Structure prediction to validate designs fold correctly |
| 4 | Screening | Filters by ipTM, pLDDT, and binder RMSD thresholds |
| 5 | Quality Metrics Plots | 2x3 panel: histograms + scatter plots with threshold lines |
| 6 | 3D Visualization | Interactive py3Dmol viewers (single, overlay, grid, pLDDT coloring) |
| 7 | Download | Packages everything into a ZIP |

**Key features:**
- Supports both **LigandMPNN** and **solMPNN** (soluble MPNN) models
- Optional **Amber relaxation** via ColabFold (`use_relax` toggle)
- **Binder RMSD** calculation (RFD3 backbone vs ColabFold prediction)
- All parameters exposed as Colab form widgets (sliders, dropdowns)

### Notebook 2: PyRosetta Physics Filters

| Stage | Tool | What it does |
|-------|------|--------------|
| 0 | Setup | Upload ZIP from Notebook 1, extract relaxed PDBs |
| 1 | **FastRelax** | Constrained structural optimization + binding energy |
| 2 | **RosettaScripts** | Physics filters: ddG, SAP, CMS, SASA, BUH |
| 3 | Score Filtering | Apply cutoffs (ddG < -30, SAP < 35, CMS > 300) |
| 4 | 3D Visualization | py3Dmol views of top candidates |
| 5 | Download | Package results ZIP |

**Physics metrics computed:**

| Metric | Measures | Good value |
|--------|----------|------------|
| **ddG** | Binding free energy | < -30 REU |
| **SAP score** | Spatial Aggregation Propensity (solubility) | < 35 |
| **Contact molecular surface** | Interface shape complementarity | > 300 |
| **Interface buried SASA** | Interface area (total, hydrophobic, polar) | Higher = more contact |
| **BUH** | Buried unsatisfied H-bond donors/acceptors | Lower = better |

---

## Quick Start

### Requirements

- A Google account (for Colab)
- A free GPU runtime on Google Colab
- **PyRosetta academic license** (Notebook 2 only) -- register at [RosettaCommons](https://www.rosettacommons.org/software/license-and-download)

### Running the Notebooks

1. **Open Notebook 1** in Colab (click the badge above or upload manually)
2. Select **Runtime > Change runtime type > T4 GPU**
3. Run cells sequentially from top to bottom
4. Download the `*_designs.zip` when prompted
5. **Open Notebook 2** in Colab
6. Upload the ZIP from step 4 when prompted
7. Run cells sequentially -- review filtered candidates

> **Estimated runtimes (T4 GPU):**
> - Notebook 1: ~30--60 min for 8 designs
> - Notebook 2: ~1--3 hours for 8 designs (FastRelax is the bottleneck)

---

## Student Guides

PDF reference guides are provided in the `guides/` folder:

- **Workshop_RFD3_Student_Guide.pdf** -- Covers diffusion-based design, inverse folding, structure prediction, RMSD interpretation, and all configuration parameters.
- **Workshop_PyRosetta_Filters_Student_Guide.pdf** -- Covers FastRelax, binding energy, RosettaScripts XML protocol, filter interpretation, and troubleshooting.

---

## Workshop Context

This workshop is part of the **2nd Symposium on Protein Design and Engineering (SPDE)**, a meeting organized by CNPEM with support from RosettaCommons and Serrapilheira, co-organized with Fiocruz and EMS. The symposium covers the full spectrum of protein design -- AI and computational methods, de novo design of therapeutic binders, self-assembling proteins, biosensors, therapeutic peptides, and vaccines.

### Workshop Instructor

- **Pablo Arantes** (EMS, Brazil) -- Workshop lead, scientific committee


### Organizing Committee

- Helder Ribeiro (CNPEM, Brazil) -- Scientific committee
- Pablo Arantes (EMS, Brazil) -- Workshop lead, scientific committee
- Roberto Lins (Fiocruz, Brazil) -- Scientific committee

### Key Dates

| Milestone | Date |
|-----------|------|
| Pre-event Workshop | October 5--6, 2026 |
| Symposium | October 6--8, 2026 |
| Venue | CNPEM, Campinas, SP, Brazil |

---

## Tools & References

| Tool | Reference |
|------|-----------|
| RFdiffusion3 | Watson et al. (2023). De novo design of protein structure and function with RFdiffusion. *Nature* 620, 1089--1100. |
| LigandMPNN | Dauparas et al. (2023). Atomic context-conditioned protein sequence design using LigandMPNN. *bioRxiv*. |
| ColabFold | Mirdita et al. (2022). ColabFold: making protein folding accessible to all. *Nat Methods* 19, 679--682. |
| AlphaFold2 | Jumper et al. (2021). Highly accurate protein structure prediction with AlphaFold. *Nature* 596, 583--589. |
| PyRosetta | Chaudhury et al. (2010). PyRosetta. *Bioinformatics* 26(5), 689--691. |
| Rosetta Energy | Alford et al. (2017). The Rosetta All-Atom Energy Function. *J. Chem. Theory Comput.* 13(6), 3031--3048. |

---

## License

This workshop material is provided for educational purposes. The notebooks and guides are released under the [MIT License](LICENSE). Third-party tools (RFdiffusion3, PyRosetta, ColabFold) are subject to their own licenses.

---

## Acknowledgments

- **CNPEM** for hosting the symposium and providing infrastructure
- **RosettaCommons** for supporting the workshop and providing PyRosetta academic licenses
- **Serrapilheira Institute** for financial support
- **Fiocruz** and **EMS** for co-organizing
- The developers of RFdiffusion3, LigandMPNN, ColabFold, and PyRosetta

---

<p align="center">
  <em>2nd Symposium on Protein Design and Engineering -- CNPEM, October 2026</em><br>
  <a href="https://pages.cnpem.br/spde/">https://pages.cnpem.br/spde/</a>
</p>
