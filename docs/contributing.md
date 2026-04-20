# Contributing to 16FlYMaze

Thank you for your interest in contributing to **16FlYMaze**! This document outlines how to get involved.

---

## Team

16FlYMaze was developed by a multidisciplinary team led by **Glenn Turner's Lab** at **Janelia Research Campus, Howard Hughes Medical Institute (HHMI)**, with funding from HHMI:

| Name | Affiliation | Role |
|---|---|---|
| **Rishika Mohanta** | Janelia Research Campus, HHMI | Project Lead · Software · Design · Final Assembly |
| **Glenn Turner** | Janelia Research Campus, HHMI | Head of Lab · Original Design |
| **Adithya Rajagopalan** | Janelia Research Campus, HHMI | Original Design |
| **Tzuhsuan Ma** | Janelia Research Campus, HHMI | Experiment Design |
| **Ann Hermundstad** | Janelia Research Campus, HHMI | Experiment Design |
| **Kevin Miller** | Google DeepMind | Experiment Design |
| **Maria Eckstein** | Google DeepMind | Experiment Design |
| **Peter Polidoro** | Janelia Research Campus, HHMI | Pneumatics · Electrical · Assembly |
| **Steven Sawtelle** | Janelia Research Campus, HHMI | Optics |
| **Tobias Goulet** | Janelia Research Campus, HHMI | Pneumatics · Electrical · Assembly |
| **Jeff Talbot** | Janelia Research Campus, HHMI | Pneumatics · Electrical · Assembly |

---

## Reporting Bugs

If you encounter a bug, please [open an issue](https://github.com/neurorishika/16FlYMaze/issues) with:

- A clear description of the problem
- Steps to reproduce it
- Your Python version and OS
- Any relevant error messages or tracebacks

---

## Suggesting Features

Feature requests are welcome! Please [open an issue](https://github.com/neurorishika/16FlYMaze/issues) with the `enhancement` label and describe:

- The use case or motivation
- A description of the proposed feature
- Any relevant prior art or references

---

## Development Setup

```bash
# Clone the repository
git clone https://github.com/neurorishika/16FlYMaze.git
cd 16FlYMaze

# Create a conda environment
conda create -n sixteeny-dev python=3.8
conda activate sixteeny-dev

# Install in editable mode with development dependencies
pip install -e .
```

---

## Code Style

- Follow **PEP 8** for Python code style
- Use descriptive variable names and docstrings
- Keep functions focused on a single responsibility
- Document all public classes and methods with docstrings matching the existing style

---

## Documentation

The documentation is built with [MkDocs Material](https://squidfunk.github.io/mkdocs-material/).

To build and preview locally:

```bash
pip install mkdocs-material mkdocs-minify-plugin
mkdocs serve
```

Then open [http://127.0.0.1:8000](http://127.0.0.1:8000) in your browser.

Documentation source files are in `docs/`. Please update relevant pages when making code changes.

---

## Pull Request Process

1. Fork the repository and create a feature branch
2. Make your changes with clear, atomic commits
3. Ensure existing functionality is not broken
4. Update documentation as needed
5. Open a pull request with a clear description of your changes

---

## License

By contributing, you agree that your contributions will be licensed under the **BSD 3-Clause License**.
