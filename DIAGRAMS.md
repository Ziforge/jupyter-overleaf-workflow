# Workflow Diagrams

Visual representations of the Jupyter-Overleaf workflow.

## Complete Workflow

```mermaid
flowchart TB
    Start([Research Project]) --> Jupyter[Jupyter Notebook]
    Jupyter --> |Write code & analysis| Data[Data Analysis]
    Data --> |Generate| Figures[Figures & Results]
    Jupyter --> |Document| Text[Text & Equations]

    Figures --> Convert{Convert to LaTeX}
    Text --> Convert

    Convert --> LaTeX[LaTeX Document]
    LaTeX --> |Compile| PDF[PDF Output]

    LaTeX --> Overleaf{Sync to Overleaf?}
    Overleaf --> |Yes| OL[Overleaf Project]
    Overleaf --> |No| Local[Local Only]

    OL --> Collab[Collaborative Editing]
    Collab --> Review[Supervisor Review]
    Review --> Final[Final Version]

    Local --> Final

    Final --> GitHub{Push to GitHub?}
    GitHub --> |Yes| Repo[GitHub Repository]
    GitHub --> |No| Done([Complete])

    Repo --> Archive[Archived & Reproducible]
    Archive --> Done

    style Jupyter fill:#f9f,stroke:#333,stroke-width:2px
    style LaTeX fill:#bbf,stroke:#333,stroke-width:2px
    style Overleaf fill:#bfb,stroke:#333,stroke-width:2px
    style GitHub fill:#fbb,stroke:#333,stroke-width:2px
```

## Notebook Structure

```mermaid
graph TD
    Notebook[Jupyter Notebook] --> Meta[Cell 1: Metadata]
    Notebook --> Abstract[Cell 2: Abstract]
    Notebook --> Intro[Cell 3: Introduction]
    Notebook --> Methods[Cell 4-N: Methods & Code]
    Notebook --> Results[Cell N+1: Results]
    Notebook --> Discuss[Cell N+2: Discussion]
    Notebook --> Conclude[Cell N+3: Conclusion]

    Meta --> |Title, Authors| Info[Paper Info]
    Abstract --> |200 words| Summary[Summary]
    Methods --> |Python Code| Analysis[Analysis Code]
    Methods --> |Plots| Figures[Figure Generation]
    Results --> |Output| Data[Data Display]

    style Notebook fill:#f9f,stroke:#333,stroke-width:2px
    style Meta fill:#ffd,stroke:#333
    style Methods fill:#dff,stroke:#333
    style Figures fill:#dfd,stroke:#333
```

## Conversion Process

```mermaid
sequenceDiagram
    participant User
    participant Notebook as Jupyter Notebook
    participant Workflow as Workflow Package
    participant Converter as LaTeX Converter
    participant Template as Template Engine
    participant Output as LaTeX Files

    User->>Notebook: Write research & code
    User->>Workflow: notebook_to_paper()
    Workflow->>Notebook: Read .ipynb file
    Notebook-->>Workflow: Notebook data

    Workflow->>Converter: Extract metadata
    Converter-->>Workflow: Title, authors, keywords

    Workflow->>Converter: Extract equations
    Converter-->>Workflow: Math expressions

    Workflow->>Converter: Extract figures
    Converter-->>Workflow: Image files

    Workflow->>Template: Apply template
    Template-->>Workflow: Formatted LaTeX

    Workflow->>Output: Write main.tex
    Workflow->>Output: Write references.bib
    Workflow->>Output: Save figures/

    Output-->>User: LaTeX project ready
```

## Overleaf Integration

```mermaid
flowchart LR
    subgraph Local
        Notebook[Jupyter Notebook]
        Convert[Conversion]
        LaTeX[LaTeX Files]
    end

    subgraph Overleaf
        Git[Git Repository]
        OLEditor[Overleaf Editor]
        Compile[Cloud Compile]
    end

    subgraph Collaboration
        Advisor[Advisor Review]
        CoAuthor[Co-author Edits]
        Comments[Comments & Suggestions]
    end

    Notebook --> Convert
    Convert --> LaTeX
    LaTeX --> |Git Push| Git
    Git --> OLEditor
    OLEditor --> Compile

    OLEditor <--> Advisor
    OLEditor <--> CoAuthor
    OLEditor <--> Comments

    Advisor --> |Final Approval| Final[Final Version]
    CoAuthor --> Final

    style Notebook fill:#f9f,stroke:#333,stroke-width:2px
    style OLEditor fill:#bfb,stroke:#333,stroke-width:2px
    style Final fill:#ffd,stroke:#333,stroke-width:2px
```

## GitHub Workflow

```mermaid
gitGraph
    commit id: "Initial notebook"
    commit id: "Add analysis code"
    commit id: "Generate figures"
    branch paper
    checkout paper
    commit id: "Convert to LaTeX"
    commit id: "First draft"
    commit id: "Add citations"
    checkout main
    commit id: "Update analysis"
    checkout paper
    merge main
    commit id: "Incorporate new results"
    commit id: "Final edits"
    commit id: "Ready for submission" tag: "v1.0"
```

## Template Selection

```mermaid
graph LR
    Start([Choose Template]) --> Type{Paper Type?}

    Type --> |Journal| Journal{Which Journal?}
    Type --> |Conference| Conf{Which Conference?}
    Type --> |Thesis| Thesis[Thesis Template]
    Type --> |General| Article[Article Template]

    Journal --> |IEEE| IEEE[IEEE Template]
    Journal --> |Acta Acustica| Acta[Acta Acustica Template]
    Journal --> |Other| Custom[Custom Template]

    Conf --> |IEEE| IEEEConf[IEEE Conference]
    Conf --> |ACM| ACM[ACM Template]
    Conf --> |Two-column| TwoCol[Two-column Template]

    IEEE --> Format[LaTeX Document]
    Acta --> Format
    Custom --> Format
    IEEEConf --> Format
    ACM --> Format
    TwoCol --> Format
    Thesis --> Format
    Article --> Format

    style Start fill:#ffd,stroke:#333,stroke-width:2px
    style Format fill:#bfb,stroke:#333,stroke-width:2px
```

## Data Flow

```mermaid
graph TB
    subgraph Research
        Experiment[Experiments]
        Data[Raw Data]
        Analysis[Statistical Analysis]
    end

    subgraph Notebook
        Code[Python Code]
        Viz[Visualizations]
        Text[Narrative Text]
    end

    subgraph Output
        LaTeX[LaTeX Source]
        PDF[PDF Document]
        Figures[Figure Files]
        BibTeX[Bibliography]
    end

    subgraph Distribution
        Overleaf[Overleaf Project]
        GitHub[GitHub Repository]
        Journal[Journal Submission]
    end

    Experiment --> Data
    Data --> Analysis
    Analysis --> Code
    Code --> Viz
    Code --> Text

    Viz --> LaTeX
    Text --> LaTeX
    LaTeX --> PDF
    LaTeX --> Figures
    LaTeX --> BibTeX

    PDF --> Overleaf
    LaTeX --> Overleaf
    Figures --> Overleaf
    BibTeX --> Overleaf

    Overleaf --> GitHub
    PDF --> GitHub
    Code --> GitHub

    PDF --> Journal

    style Research fill:#fdd,stroke:#333
    style Notebook fill:#dfd,stroke:#333
    style Output fill:#ddf,stroke:#333
    style Distribution fill:#ffd,stroke:#333
```

## Component Architecture

```mermaid
graph TD
    subgraph User Interface
        CLI[Command Line]
        Jupyter[Jupyter Cells]
        Script[Python Script]
    end

    subgraph Workflow Package
        API[API Layer]
        Converter[Converter Module]
        Overleaf[Overleaf Module]
        GitHub[GitHub Module]
        Templates[Template Engine]
    end

    subgraph External Services
        OLMCP[Overleaf MCP]
        GH[GitHub API]
        LaTeXComp[LaTeX Compiler]
    end

    CLI --> API
    Jupyter --> API
    Script --> API

    API --> Converter
    API --> Overleaf
    API --> GitHub

    Converter --> Templates
    Overleaf --> OLMCP
    GitHub --> GH

    Converter --> LaTeXComp

    style API fill:#f9f,stroke:#333,stroke-width:2px
    style Converter fill:#bbf,stroke:#333,stroke-width:2px
    style OLMCP fill:#bfb,stroke:#333,stroke-width:2px
```

## User Journey

```mermaid
journey
    title Academic Paper Writing Journey
    section Research Phase
      Design experiment: 5: Researcher
      Collect data: 4: Researcher
      Initial analysis: 3: Researcher
    section Development Phase
      Write Jupyter notebook: 5: Researcher
      Create visualizations: 5: Researcher
      Document findings: 4: Researcher
    section Conversion Phase
      Convert to LaTeX: 5: Researcher
      Review formatting: 4: Researcher
      Fix citations: 3: Researcher
    section Collaboration Phase
      Upload to Overleaf: 5: Researcher
      Advisor reviews: 4: Advisor
      Incorporate feedback: 3: Researcher
      Co-author edits: 4: Co-author
    section Publication Phase
      Final revisions: 4: Researcher
      Archive on GitHub: 5: Researcher
      Submit to journal: 5: Researcher
```

## Installation Options

```mermaid
graph TD
    Start([Install Workflow]) --> Mode{Installation Mode?}

    Mode --> |Standalone| Standalone[Standalone Installation]
    Mode --> |MCP Pipeline| MCP[MCP Pipeline Installation]

    Standalone --> Python[Install Python Package]
    Python --> LaTeX[Install LaTeX]
    LaTeX --> Done1([Ready to Use])

    MCP --> Docker[Start Docker Services]
    Docker --> OverleafMCP[Configure Overleaf MCP]
    OverleafMCP --> Services[All Services Running]
    Services --> Done2([Ready to Use])

    Done1 --> Manual[Manual Overleaf Sync]
    Done2 --> Auto[Automated Overleaf Sync]

    style Standalone fill:#bfb,stroke:#333,stroke-width:2px
    style MCP fill:#bbf,stroke:#333,stroke-width:2px
```

---

## Using These Diagrams

These Mermaid diagrams can be:
- Rendered on GitHub (automatic in .md files)
- Used in documentation
- Exported to PNG/SVG with Mermaid CLI
- Embedded in presentations

To export as images:
```bash
npm install -g @mermaid-js/mermaid-cli
mmdc -i DIAGRAMS.md -o diagrams.pdf
```

---

This work was developed with assistance from Claude (Anthropic).
