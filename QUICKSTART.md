# Quick Start Guide

Get started with Jupyter-Overleaf Workflow in 5 minutes.

## Installation

```bash
# Clone repository
git clone https://github.com/YOUR_USERNAME/jupyter-overleaf-workflow.git
cd jupyter-overleaf-workflow

# Install package
pip install -e .
```

## Your First Paper

### 1. Create Notebook

Open Jupyter:

```bash
jupyter notebook
```

Create new notebook with this structure:

```python
# Cell 1: Metadata
"""
# Paper Title: My Research Title

**Authors:** Your Name
**Institution:** Your University
**Keywords:** research, paper, jupyter
**Template:** twocolumn
"""

# Cell 2: Abstract
"""
## Abstract
Your 200-word abstract here...
"""

# Cell 3+: Introduction, Methods, Results, etc.
"""
## Introduction
Background and motivation...
"""

# Code cells with your analysis
import numpy as np
import matplotlib.pyplot as plt

# Your research code here

# Figures
plt.figure(dpi=300)
plt.plot(data)
plt.savefig("figure1.pdf")
plt.show()
```

### 2. Convert to LaTeX

At the end of your notebook:

```python
from workflow import notebook_to_paper

result = notebook_to_paper(
    notebook="my_paper.ipynb",
    output_dir="paper_output",
    template="twocolumn",
    include_code=False
)

print(f"Paper created: {result['main_tex']}")
```

### 3. Compile PDF

```bash
cd paper_output
pdflatex main.tex
bibtex main
pdflatex main.tex
pdflatex main.tex
```

Done! You now have a publication-ready PDF.

## Sync to Overleaf (Optional)

### Manual Method

```bash
# Get your Overleaf Git token from Account Settings

# Clone Overleaf project
git clone https://git:YOUR_TOKEN@git.overleaf.com/YOUR_PROJECT_ID

# Copy files
cp paper_output/* overleaf-project/

# Push
cd overleaf-project
git add .
git commit -m "Update from Jupyter"
git push
```

### Automated Method (Requires MCP)

```python
from workflow import sync_to_overleaf

sync_to_overleaf(
    latex_dir="paper_output",
    project_name="default"
)
```

## Push to GitHub

```bash
cd paper_output

# Initialize repo
git init
git add .
git commit -m "Initial paper version"

# Create GitHub repo
gh repo create my-paper --private --source=. --push
```

Or use Python:

```python
from workflow import push_to_github

push_to_github(
    local_dir="paper_output",
    repo_name="username/my-paper",
    commit_message="Initial paper"
)
```

## Available Templates

- `article` - Single column article
- `twocolumn` - Two-column conference format
- `ieee` - IEEE conference style
- `acta_acustica` - Acta Acustica journal format
- `thesis` - Thesis/dissertation format

## Common Commands

### Convert with different template

```python
notebook_to_paper("notebook.ipynb", "output", template="ieee")
```

### Include code in paper

```python
notebook_to_paper("notebook.ipynb", "output", include_code=True)
```

### Save figures as PNG

```python
notebook_to_paper("notebook.ipynb", "output", figure_format="png")
```

## Troubleshooting

### LaTeX not found

```bash
# macOS
brew install --cask mactex

# Ubuntu
sudo apt-get install texlive-full
```

### Missing Python packages

```bash
pip install jupyter nbformat nbconvert requests matplotlib
```

### Conversion fails

- Check notebook has proper metadata in first cell
- Ensure all code cells run without errors
- Save figures explicitly with `plt.savefig()`

### Overleaf sync issues

- Verify Git token is correct
- Check Project ID in URL
- Ensure you have write access to project

## Next Steps

- Read full documentation in [README.md](README.md)
- Try the example: `examples/simple_paper.ipynb`
- Customize templates in `workflow/templates.py`
- Share your papers!

## Getting Help

- **Issues:** GitHub Issues
- **Questions:** GitHub Discussions
- **Examples:** `examples/` directory
- **Documentation:** README.md, STANDALONE_INSTALL.md

## License

Free for educational and research use under CC BY-NC-SA 4.0.

Commercial use requires permission.

---

**Happy writing!**

This work was developed with assistance from Claude (Anthropic).
