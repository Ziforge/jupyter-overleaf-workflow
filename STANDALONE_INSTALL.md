# Standalone Installation Guide

This guide shows how to use the Jupyter-Overleaf workflow without the full MCP Pipeline infrastructure.

## Quick Start (Standalone)

### Prerequisites

- Python 3.8 or higher
- Jupyter/JupyterLab
- LaTeX distribution (TeX Live, MiKTeX, or MacTeX)
- Git
- Overleaf account (optional, for collaboration)

### Installation

1. **Clone the repository:**

```bash
git clone https://github.com/YOUR_USERNAME/jupyter-overleaf-workflow.git
cd jupyter-overleaf-workflow
```

2. **Install Python package:**

```bash
pip install -e .
```

Or install from PyPI (once published):

```bash
pip install jupyter-overleaf-workflow
```

3. **Verify installation:**

```bash
python -c "import workflow; print(workflow.__version__)"
```

## Basic Usage (No MCP Required)

### Convert Notebook to LaTeX

```python
from workflow import notebook_to_paper

# Convert your notebook
result = notebook_to_paper(
    notebook="my_research.ipynb",
    output_dir="paper_output",
    template="twocolumn",  # or "article", "ieee", "thesis"
    save_figures=True,
    include_code=False
)

print(f"LaTeX file created: {result['main_tex']}")
```

### Compile LaTeX Locally

```bash
cd paper_output
pdflatex main.tex
bibtex main
pdflatex main.tex
pdflatex main.tex
```

Or use the workflow helper:

```python
from workflow.utils import compile_latex

compile_latex("paper_output/main.tex")
```

### Manual Overleaf Sync

Without MCP service, sync manually:

```bash
# Clone your Overleaf project
git clone https://git:YOUR_TOKEN@git.overleaf.com/YOUR_PROJECT_ID

# Copy generated files
cp paper_output/* overleaf-project/

# Commit and push
cd overleaf-project
git add .
git commit -m "Update from Jupyter"
git push
```

## With MCP Pipeline (Advanced)

For full automation with the MCP Pipeline:

1. **Start MCP services:**

```bash
cd /path/to/mcp-pipeline
bash up-min.sh
```

2. **Configure Overleaf MCP:**

Edit `mcp-pipeline/overleaf-mcp/projects.json`:

```json
{
  "projects": {
    "default": {
      "name": "My Paper",
      "projectId": "YOUR_OVERLEAF_PROJECT_ID",
      "gitToken": "YOUR_GIT_TOKEN"
    }
  }
}
```

3. **Use automated sync:**

```python
from workflow import notebook_to_paper, sync_to_overleaf

# Convert
result = notebook_to_paper(
    notebook="research.ipynb",
    output_dir="paper",
    template="twocolumn"
)

# Auto-sync to Overleaf
sync_to_overleaf(
    latex_dir="paper",
    project_name="default"
)
```

## Features Available Without MCP

Even without the MCP infrastructure, you can use:

- Notebook to LaTeX conversion
- Multiple templates (article, IEEE, thesis, etc.)
- Figure extraction and formatting
- Equation extraction
- Citation management
- GitHub integration
- Local LaTeX compilation

## Features Requiring MCP

These features need the MCP Pipeline running:

- Automated Overleaf sync via API
- Real-time Overleaf file reading
- LaTeX compilation via docs-mcp service
- Integration with other MCP services (DSP, metrics, etc.)

## Docker-Free Setup

To use without Docker:

1. **Install LaTeX locally:**

macOS:
```bash
brew install --cask mactex
```

Ubuntu/Debian:
```bash
sudo apt-get install texlive-full
```

Windows:
```
Download MiKTeX from https://miktex.org/
```

2. **Install Python dependencies:**

```bash
pip install jupyter nbformat nbconvert requests
```

3. **Use the workflow:**

```python
from workflow import notebook_to_paper

notebook_to_paper(
    notebook="my_paper.ipynb",
    output_dir="output"
)
```

## Configuration

### Environment Variables (Optional)

```bash
export OVERLEAF_MCP_URL="http://localhost:7105"  # If using MCP
export LATEX_COMPILER="pdflatex"                  # Or xelatex, lualatex
```

### Python Configuration

Create `workflow_config.py`:

```python
config = {
    "default_template": "twocolumn",
    "figure_format": "pdf",
    "include_code": False,
    "latex_compiler": "pdflatex",
    "overleaf_mcp_url": "http://localhost:7105"
}
```

## Examples

See `examples/` directory:

- `simple_paper.ipynb` - Basic example
- `data_analysis.ipynb` - Statistical analysis paper
- `mathematical_proof.ipynb` - Math-heavy document

Run examples:

```bash
cd examples
jupyter notebook simple_paper.ipynb
```

## Troubleshooting

### LaTeX compilation fails

**Problem:** Missing packages

```bash
# Install missing packages
sudo tlmgr install PACKAGE_NAME
```

**Problem:** Path issues

```bash
# Check LaTeX installation
which pdflatex
pdflatex --version
```

### Conversion issues

**Problem:** Figures not appearing

```python
# Explicitly save figures
plt.savefig("figure1.pdf", bbox_inches='tight')
```

**Problem:** Equations malformed

```markdown
# Use raw LaTeX in markdown
$$\alpha = \beta + \gamma$$
```

### Overleaf sync (manual)

**Problem:** Authentication

- Regenerate Git token in Overleaf Account Settings
- Use token in git clone URL: `https://git:TOKEN@git.overleaf.com/PROJECT_ID`

## GitHub Integration

### Setup GitHub CLI (optional but recommended)

```bash
# macOS
brew install gh

# Ubuntu
sudo apt install gh

# Authenticate
gh auth login
```

### Manual Git Push

```bash
cd paper_output
git init
git add .
git commit -m "Initial paper version"
gh repo create my-paper --private --source=. --push
```

### Using workflow helper

```python
from workflow import push_to_github

push_to_github(
    local_dir="paper_output",
    repo_name="username/my-paper",
    commit_message="Add paper draft"
)
```

## Best Practices

1. **Always use version control:**
   ```bash
   git init
   git add .
   git commit -m "Initial commit"
   ```

2. **Keep notebooks clean:**
   - Clear outputs before committing
   - Use meaningful cell structure
   - Document parameters

3. **Reproducibility:**
   ```python
   import numpy as np
   np.random.seed(42)  # Fixed seed

   print(f"Date: {datetime.now()}")
   print(f"Python: {sys.version}")
   ```

4. **Figure quality:**
   ```python
   plt.figure(dpi=300)  # High resolution
   plt.savefig("fig.pdf")  # Vector format
   ```

## Support

- **Issues:** https://github.com/YOUR_USERNAME/jupyter-overleaf-workflow/issues
- **Documentation:** README.md
- **Examples:** `examples/` directory

## License

Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0)

Free for educational and research use.

---

This work was developed with assistance from Claude (Anthropic).
