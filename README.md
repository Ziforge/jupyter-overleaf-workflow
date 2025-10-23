# Jupyter-Overleaf Workflow

[![License: CC BY-NC-SA 4.0](https://img.shields.io/badge/License-CC%20BY--NC--SA%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by-nc-sa/4.0/)
[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)

Convert Jupyter notebooks to academic LaTeX papers with Overleaf and GitHub integration. Perfect for students and researchers.

## Quick Start

```bash
# Install
git clone https://github.com/Ziforge/jupyter-overleaf-workflow.git
cd jupyter-overleaf-workflow
pip install -e .

# Try the example
jupyter notebook examples/simple_paper.ipynb
```

## Workflow Overview

```mermaid
flowchart TB
    Start([Research Project]) --> Jupyter[📓 Jupyter Notebook]
    Jupyter --> Convert{Convert to LaTeX}
    Convert --> LaTeX[📄 LaTeX Document]
    LaTeX --> PDF[📕 PDF Output]
    LaTeX --> Overleaf[☁️ Overleaf Sync]
    Overleaf --> Collab[👥 Collaborative Editing]
    Collab --> GitHub[📦 GitHub Archive]
    GitHub --> Done([✅ Published])

    style Jupyter stroke:#f0f,stroke-width:3px
    style LaTeX stroke:#00f,stroke-width:3px
    style Overleaf stroke:#0f0,stroke-width:3px
```

## Features

- ✅ **Notebook to LaTeX** - Automatic conversion with academic formatting
- ✅ **Multiple Templates** - Article, IEEE, two-column, thesis
- ✅ **Overleaf Sync** - Collaborate with advisors and co-authors
- ✅ **GitHub Integration** - Version control and reproducibility
- ✅ **Figure Extraction** - Automatic figure handling
- ✅ **Citation Management** - BibTeX generation
- ✅ **Free for Education** - CC BY-NC-SA 4.0 license

## Installation

### Standalone (Simple)

```bash
pip install -e .
```

Requirements:
- Python 3.8+
- Jupyter/JupyterLab
- LaTeX (TeX Live, MiKTeX, or MacTeX)

### With MCP Pipeline (Advanced)

For automated Overleaf sync, use with the [MCP Pipeline](https://github.com/Ziforge/mcp-pipeline).

## Usage

### 1. Create Notebook

Structure your notebook like this:

```python
# Cell 1: Metadata
"""
# Paper Title: Your Research Title

**Authors:** Your Name
**Institution:** Your University
**Keywords:** research, paper, jupyter
**Template:** twocolumn
"""

# Cell 2: Abstract
"""
## Abstract
Your abstract here...
"""

# Cell 3+: Content with code and analysis
```

### 2. Convert to LaTeX

```python
from workflow import notebook_to_paper

result = notebook_to_paper(
    notebook="my_paper.ipynb",
    output_dir="paper",
    template="twocolumn",
    include_code=False
)
```

### 3. Compile PDF

```bash
cd paper
pdflatex main.tex
bibtex main
pdflatex main.tex
pdflatex main.tex
```

### 4. Sync to Overleaf

**Manual:**
```bash
git clone https://git:TOKEN@git.overleaf.com/PROJECT_ID
cp paper/* overleaf-project/
cd overleaf-project && git add . && git commit -m "Update" && git push
```

**Automated (with MCP):**
```python
from workflow import sync_to_overleaf

sync_to_overleaf(latex_dir="paper", project_name="default")
```

### 5. Push to GitHub

```python
from workflow import push_to_github

push_to_github(
    local_dir="paper",
    repo_name="username/my-paper",
    commit_message="Add paper draft"
)
```

## Available Templates

- `article` - Single column
- `twocolumn` - Conference format
- `ieee` - IEEE style
- `acta_acustica` - Acta Acustica journal
- `thesis` - Dissertation format

## Notebook Structure

```mermaid
graph TD
    Notebook[📓 Jupyter Notebook] --> Meta[📋 Metadata]
    Notebook --> Abstract[📝 Abstract]
    Notebook --> Intro[🎯 Introduction]
    Notebook --> Methods[🔬 Methods & Code]
    Notebook --> Results[📊 Results]
    Notebook --> Discuss[💭 Discussion]
    Notebook --> Conclude[✅ Conclusion]

    Methods --> Code[💻 Python Code]
    Methods --> Figures[📈 Figures]
    Results --> Data[📉 Data Display]

    style Notebook stroke:#f0f,stroke-width:3px
    style Code stroke:#00f,stroke-width:2px
    style Figures stroke:#0f0,stroke-width:2px
```

## API Reference

```python
# Conversion
from workflow import notebook_to_paper

notebook_to_paper(
    notebook="file.ipynb",
    output_dir="output",
    template="twocolumn",      # Template choice
    save_figures=True,          # Save figures
    figure_format="pdf",        # Figure format
    include_code=False          # Exclude code
)

# Overleaf
from workflow import sync_to_overleaf, pull_from_overleaf

sync_to_overleaf(latex_dir="paper", project_name="default")
pull_from_overleaf(project_name="default", output_dir="paper")

# GitHub
from workflow import push_to_github, create_github_repo

push_to_github(local_dir="paper", repo_name="user/repo", commit_message="Update")
create_github_repo(name="my-paper", description="Research paper", private=True)
```

## Examples

See `examples/simple_paper.ipynb` for a complete working example showing:
- Proper notebook structure
- Code and analysis
- Figure generation
- Conversion to LaTeX
- Overleaf sync instructions

## LaTeX Installation

**macOS:**
```bash
brew install --cask mactex
```

**Ubuntu/Debian:**
```bash
sudo apt-get install texlive-full
```

**Windows:**
Download [MiKTeX](https://miktex.org/)

## Troubleshooting

**Conversion fails:**
- Ensure metadata in first cell
- Check all code cells run
- Save figures with `plt.savefig()`

**LaTeX errors:**
- Install missing packages: `tlmgr install PACKAGE`
- Check LaTeX path: `which pdflatex`

**Overleaf sync issues:**
- Verify Git token in Overleaf settings
- Check project ID in URL
- Ensure write access

## Project Structure

```
jupyter-overleaf-workflow/
├── README.md              # This file
├── LICENSE                # CC BY-NC-SA 4.0
├── setup.py               # Package installation
├── requirements.txt       # Dependencies
├── workflow/              # Core package
│   ├── converter.py       # Notebook → LaTeX
│   ├── overleaf.py        # Overleaf integration
│   ├── github.py          # GitHub automation
│   └── templates.py       # LaTeX templates
└── examples/
    └── simple_paper.ipynb # Working example
```

## License

**Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International**

- ✅ Free for educational and research use
- ✅ Share and adapt freely
- ✅ Must credit original work
- ❌ No commercial use without permission

[Full License Text](LICENSE)

## Citation

If you use this workflow in your research:

```bibtex
@software{jupyter_overleaf_workflow,
  title = {Jupyter-Overleaf Workflow for Academic Writing},
  year = {2025},
  publisher = {GitHub},
  url = {https://github.com/Ziforge/jupyter-overleaf-workflow}
}
```

## Contributing

Contributions welcome! Please:
- Report issues on GitHub
- Submit pull requests
- Share your templates
- Improve documentation

## Documentation

- 📖 **Quick Start:** [docs/QUICKSTART.md](docs/QUICKSTART.md) - Get started in 5 minutes
- 🔧 **Standalone Install:** [docs/STANDALONE_INSTALL.md](docs/STANDALONE_INSTALL.md) - Install without MCP
- 📊 **Diagrams:** [docs/DIAGRAMS.md](docs/DIAGRAMS.md) - Visual workflow diagrams

## Support

- 🐛 **Issues:** [GitHub Issues](https://github.com/Ziforge/jupyter-overleaf-workflow/issues)
- 💬 **Discussions:** [GitHub Discussions](https://github.com/Ziforge/jupyter-overleaf-workflow/discussions)
- 📖 **Examples:** See `examples/` directory

## Acknowledgments

This work was developed with assistance from Claude (Anthropic).

---

**Made for students and researchers worldwide** 🌍📚

Version 1.0.0
