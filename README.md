# Jupyter to Overleaf to GitHub Workflow

A seamless academic writing workflow that combines computational notebooks, collaborative LaTeX editing, and version control.

## Overview

This workflow enables researchers to:

1. **Develop** - Write code, run experiments, and analyze data in JupyterLab
2. **Document** - Convert notebooks to academic LaTeX papers
3. **Collaborate** - Sync to Overleaf for co-author editing
4. **Archive** - Push final versions to GitHub with full reproducibility

## Workflow Architecture

```
┌─────────────────┐
│  JupyterLab     │  1. Research & Development
│  - Python code  │     - Data analysis
│  - Equations    │     - Experiments
│  - Figures      │     - Documentation
└────────┬────────┘
         │
         │ 2. Convert
         ▼
┌─────────────────┐
│  LaTeX + TikZ   │  3. Academic Format
│  - Acta Acustica│     - 2-column layout
│  - BibTeX refs  │     - Vector figures
│  - Equations    │     - Citations
└────────┬────────┘
         │
         │ 4. Sync
         ▼
┌─────────────────┐
│  Overleaf       │  5. Collaboration
│  - Live editing │     - Supervisor review
│  - Comments     │     - Co-author input
│  - Track changes│     - Final edits
└────────┬────────┘
         │
         │ 6. Commit
         ▼
┌─────────────────┐
│  GitHub         │  7. Version Control
│  - Source code  │     - Full history
│  - Data/results │     - Reproducibility
│  - LaTeX source │     - DOI via Zenodo
└─────────────────┘
```

## Features

- Automated notebook → LaTeX conversion with proper academic formatting
- Integration with Overleaf MCP for seamless sync
- GitHub Actions for continuous paper building
- Template compliance (Acta Acustica, IEEE, etc.)
- Equation extraction with physical/perceptual interpretations
- Figure conversion (matplotlib → TikZ where possible)
- Citation management from notebook to BibTeX
- Reproducible research with frozen environments

## Quick Start

### 1. Start JupyterLab

```bash
# From mcp-pipeline directory
bash up-min.sh

# Access JupyterLab
open http://localhost:8888
```

### 2. Create Research Notebook

Open the template notebook: `notebooks/academic_paper_template.ipynb`

### 3. Run Conversion

```python
from workflow import notebook_to_paper

# Convert notebook to LaTeX
notebook_to_paper(
    notebook="my_research.ipynb",
    output_dir="shared/docs/my_paper",
    template="acta_acustica"
)
```

### 4. Sync to Overleaf

```python
from workflow import sync_to_overleaf

# Upload to Overleaf
sync_to_overleaf(
    latex_dir="shared/docs/my_paper",
    project_name="default"
)
```

### 5. Commit to GitHub

```bash
cd shared/docs/my_paper
git add .
git commit -m "Add paper: My Research Title"
git push origin main
```

## Installation

### Prerequisites

- MCP Pipeline running (see main README)
- Overleaf account with Git token
- GitHub account

### Setup

1. Configure Overleaf credentials:
```bash
# Edit overleaf-mcp/projects.json
{
  "projects": {
    "default": {
      "name": "My Research Paper",
      "projectId": "YOUR_OVERLEAF_PROJECT_ID",
      "gitToken": "YOUR_OVERLEAF_GIT_TOKEN"
    }
  }
}
```

2. Rebuild Overleaf MCP:
```bash
docker compose build overleaf-mcp
docker compose up -d overleaf-mcp
```

3. Install workflow package in JupyterLab:
```bash
docker exec -it mcp-jupyter pip install -e /home/jovyan/shared/workflow
```

## Usage Guide

### Notebook Structure

Organize your research notebook with these sections:

```python
# Cell 1: Title and Metadata
"""
# Paper Title: Your Research Title Here

**Authors:** Your Name
**Institution:** Your University
**Contact:** your.email@university.edu
**Keywords:** keyword1, keyword2, keyword3
**Template:** your_template_choice
"""

# Cell 2: Abstract
"""
ABSTRACT: Your 200-word abstract here...
"""

# Cell 3-5: Introduction
"""
## Introduction

Background and motivation...
"""

# Code cells: Implementation
import numpy as np
import matplotlib.pyplot as plt

# Cell N: Results
"""
## Results

Discussion of findings...
"""

# Cell N+1: Conclusion
"""
## Conclusion

Summary and future work...
"""
```

### Conversion Options

```python
notebook_to_paper(
    notebook="research.ipynb",
    output_dir="shared/docs/paper",

    # Template selection
    template="acta_acustica",  # or "ieee", "aes", "custom"

    # Figure handling
    save_figures=True,
    convert_to_tikz=False,  # Set True for simple plots
    figure_format="pdf",

    # Citation processing
    extract_citations=True,
    bibtex_file="references.bib",

    # Code inclusion
    include_code=False,  # Exclude code from paper
    code_appendix=True,  # Add code to appendix

    # Equation formatting
    extract_equations=True,
    add_interpretations=True,  # Add physical/perceptual notes
)
```

### Overleaf Sync

```python
from workflow import sync_to_overleaf, pull_from_overleaf

# Upload local changes to Overleaf
sync_to_overleaf(
    latex_dir="shared/docs/paper",
    project_name="default",
    commit_message="Update results section"
)

# Pull Overleaf changes back
pull_from_overleaf(
    project_name="default",
    output_dir="shared/docs/paper",
    files=["main.tex", "references.bib"]
)
```

### GitHub Integration

Create `.github/workflows/build-paper.yml`:

```yaml
name: Build LaTeX Paper

on:
  push:
    paths:
      - 'paper/**'
  pull_request:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Compile LaTeX
        uses: xu-cheng/latex-action@v2
        with:
          root_file: paper/main.tex

      - name: Upload PDF
        uses: actions/upload-artifact@v3
        with:
          name: paper-pdf
          path: paper/main.pdf
```

## Templates

### Acta Acustica Template

Features:
- Two-column format
- Physical and perceptual equation interpretations
- TikZ diagrams
- Comprehensive bibliography
- Symbol tables with units

Location: `templates/acta_acustica/`

### IEEE Template

Features:
- IEEE conference/journal format
- Standard figure/table formatting
- IEEE citation style

Location: `templates/ieee/`

## Examples

### Example 1: Data Analysis

See `examples/data_analysis.ipynb` for:
- Statistical analysis
- Figure generation
- Results documentation
- Complete LaTeX paper output

### Example 2: Mathematical Study

See `examples/mathematical_proof.ipynb` for:
- Equation derivation
- Theorem proofs
- Visualization
- Publication-ready formatting

## Best Practices

### 1. Reproducibility

```python
# Always include at top of notebook
import numpy as np
np.random.seed(42)  # Fixed seed

# Document versions
import sys
print(f"Python: {sys.version}")
print(f"NumPy: {np.__version__}")
print(f"Date: {datetime.now()}")
```

### 2. Figure Quality

```python
# High-resolution figures
plt.figure(figsize=(6, 4), dpi=300)
plt.plot(data)
plt.xlabel("Time (ms)")
plt.ylabel("Amplitude")
plt.savefig("figure.pdf", bbox_inches='tight')
```

### 3. Equation Documentation

```python
"""
The interaural time difference is computed as:

$$\Delta t(\varphi) = \frac{a}{c}(\varphi + \sin\varphi)$$

**Physical interpretation:** Path-length difference around spherical head
of radius $a$, where arc term accounts for diffraction.

**Perceptual interpretation:** 10 μs ≈ 1° azimuth discrimination at
frontal positions for frequencies below 1.5 kHz.
"""
```

### 4. Citations

```python
# In notebook markdown:
"""
Recent studies [1], [2] demonstrate real-time HRTF interpolation.

References:
[1] Brown, C. P., & Duda, R. O. (1998). A structural model for binaural sound synthesis.
[2] ...
"""
```

## Workflow API Reference

### Core Functions

```python
# Conversion
notebook_to_paper(notebook, output_dir, **options)
extract_figures(notebook, output_dir, format='pdf')
extract_equations(notebook, add_interpretations=True)
extract_citations(notebook, bibtex_file)

# Overleaf sync
sync_to_overleaf(latex_dir, project_name, message)
pull_from_overleaf(project_name, output_dir, files)
list_overleaf_projects()
get_overleaf_status(project_name)

# GitHub integration
create_github_repo(name, description, private=True)
push_to_github(local_dir, repo_name, message)
create_release(repo_name, version, notes)

# Utilities
validate_latex(tex_file)
compile_latex(tex_file, output_dir)
check_citations(tex_file, bib_file)
```

## Troubleshooting

### Conversion Issues

**Problem:** Figures not appearing in LaTeX
```python
# Solution: Explicitly save figures
plt.savefig("figure1.pdf", bbox_inches='tight')
```

**Problem:** Equations not formatted correctly
```python
# Solution: Use raw LaTeX in markdown cells
r"$$E = mc^2$$"
```

### Overleaf Sync Issues

**Problem:** Authentication failed
```bash
# Check token in projects.json
# Regenerate token in Overleaf settings
```

**Problem:** Merge conflicts
```python
# Pull before pushing
pull_from_overleaf(project_name, output_dir)
# Resolve conflicts
sync_to_overleaf(latex_dir, project_name)
```

### GitHub Issues

**Problem:** Large files rejected
```bash
# Use Git LFS for data files
git lfs track "*.wav"
git lfs track "*.mat"
```

## Advanced Usage

### Custom Templates

Create your own template:

```python
from workflow import Template

template = Template(
    name="custom",
    documentclass="article",
    packages=["amsmath", "tikz", "biblatex"],
    preamble=r"\usepackage{custom}",
    layout="twocolumn"
)

notebook_to_paper(
    notebook="research.ipynb",
    template=template
)
```

### Automated Pipeline

```python
# Complete workflow script
from workflow import Pipeline

pipeline = Pipeline(
    notebook="research.ipynb",
    overleaf_project="default",
    github_repo="username/paper-repo"
)

# Run complete workflow
pipeline.run(
    convert=True,
    compile=True,
    sync_overleaf=True,
    push_github=True,
    create_release="v1.0.0"
)
```

## Citation

If you use this workflow in your research, please cite:

```bibtex
@software{jupyter_overleaf_workflow,
  title = {Jupyter to Overleaf Workflow for Academic Writing},
  year = {2025},
  publisher = {GitHub},
  url = {https://github.com/YOUR_USERNAME/jupyter-overleaf-workflow}
}
```

## License

Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0)

This work is free for educational and research use. Commercial use requires permission.

See LICENSE file for full terms.

## Acknowledgments

This work was developed with assistance from Claude (Anthropic).

## Support

- Issues: GitHub Issues
- Documentation: This README
- Examples: `examples/` directory

---

**For Students and Researchers**
A tool to streamline academic writing from computational notebooks to published papers.
