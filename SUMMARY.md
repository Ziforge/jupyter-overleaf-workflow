# Jupyter-Overleaf Workflow - Project Summary

## What Is This?

A complete workflow system that connects:
- **Jupyter notebooks** (research & analysis)
- **LaTeX** (academic formatting)
- **Overleaf** (collaborative editing)
- **GitHub** (version control)

Perfect for students and researchers who want to:
1. Write code and analyze data in Jupyter
2. Automatically convert to publication-ready LaTeX
3. Collaborate with advisors/co-authors on Overleaf
4. Archive everything on GitHub with full reproducibility

## What's Included

### Core Python Package (`workflow/`)

- `converter.py` - Notebook to LaTeX conversion
- `overleaf.py` - Overleaf integration (via MCP or manual)
- `github.py` - GitHub automation
- `templates.py` - Academic templates (IEEE, two-column, thesis, etc.)

### Templates

- Article (single column)
- Two-column (conference style)
- IEEE format
- Acta Acustica (specialized)
- Thesis/dissertation

### Examples

- `simple_paper.ipynb` - Complete working example
- Shows full workflow from notebook to paper

### Documentation

- `README.md` - Complete feature documentation
- `STANDALONE_INSTALL.md` - How to use without MCP Pipeline
- `LICENSE` - CC BY-NC-SA 4.0 (free for education, not commercial)

## Two Ways to Use It

### 1. Standalone (Simpler)

Just need:
- Python + Jupyter
- LaTeX installation
- This package

```bash
pip install -e .

# Convert notebook
from workflow import notebook_to_paper
notebook_to_paper("research.ipynb", "output")

# Compile
cd output && pdflatex main.tex
```

### 2. With MCP Pipeline (Full Automation)

Adds:
- Automated Overleaf sync
- Docker services for builds
- Complete pipeline integration

Requires the full MCP Pipeline running.

## Key Features

- **Reproducible**: All code stays with the paper
- **Automated**: One command to convert
- **Flexible**: Multiple templates, customizable
- **Collaborative**: Sync to Overleaf for co-authors
- **Professional**: Publication-ready formatting

## File Structure

```
jupyter-overleaf-workflow/
├── README.md                    # Main documentation
├── STANDALONE_INSTALL.md        # Install without MCP
├── LICENSE                      # CC BY-NC-SA 4.0
├── setup.py                     # Python package setup
├── requirements.txt             # Dependencies
├── workflow/                    # Core package
│   ├── __init__.py
│   ├── converter.py            # Notebook → LaTeX
│   ├── overleaf.py             # Overleaf integration
│   ├── github.py               # GitHub automation
│   └── templates.py            # LaTeX templates
└── examples/
    └── simple_paper.ipynb      # Working example
```

## Who Should Use This?

- **Students** writing thesis/dissertation
- **Researchers** publishing papers
- **Academics** needing reproducible workflows
- **Anyone** who wants Jupyter + LaTeX integration

## License

**Creative Commons Attribution-NonCommercial-ShareAlike 4.0**

- Free for education and research
- Share and adapt freely
- Must credit and use same license
- No commercial use without permission

## Getting Started

1. **Clone the repository:**
   ```bash
   git clone https://github.com/YOUR_USERNAME/jupyter-overleaf-workflow.git
   ```

2. **Install:**
   ```bash
   cd jupyter-overleaf-workflow
   pip install -e .
   ```

3. **Try the example:**
   ```bash
   cd examples
   jupyter notebook simple_paper.ipynb
   # Run all cells
   ```

4. **Create your own paper:**
   - Copy `simple_paper.ipynb`
   - Add your research
   - Run conversion
   - Compile LaTeX

## Future Plans

Potential enhancements:
- PyPI package publication
- More templates (APA, MLA, Nature, Science)
- Automated citation extraction from DOIs
- Better figure handling (matplotlib → TikZ conversion)
- Web UI for non-coders
- Pandoc integration for Word/HTML output

## Contributing

This is open for educational use. Contributions welcome:
- Report issues on GitHub
- Submit pull requests
- Share your templates
- Improve documentation

## Acknowledgments

This work was developed with assistance from Claude (Anthropic).

## Contact

For questions, feature requests, or contributions:
- GitHub Issues: (repository URL)
- Documentation: See README.md
- Examples: See examples/ directory

---

**Made for students and researchers worldwide**

Free to use for educational purposes. Not for commercial use.

Version 1.0.0 - 2025
