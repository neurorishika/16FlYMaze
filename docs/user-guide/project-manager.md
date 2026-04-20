# Project Manager

The **16Y Project Manager** is the central hub for organizing all files associated with your experiments.

---

## Launching

```bat
run_project_manager.bat
```

Or double-click the **16Y Project Manager** desktop shortcut (after running `initialize_shortcuts.bat`).

---

## Overview

The Project Manager displays three panels side-by-side:

| Panel | Contents |
|---|---|
| **Experimental Data** | Completed experiment folders in `data/` |
| **Stimulus Zoo** | Available `.stim` files in `stimulus_zoo/` |
| **Experiment Zoo** | Available experiment files (`.csv`, `.ymaze`, `.ymle`) in `experiment_zoo/` |

---

## Project Directory Structure

A SixteenY project follows this directory structure:

```
my_project/
├── data/                     # Completed experiment outputs
│   └── experiment_name/
│       ├── arena_0/          # Per-arena data
│       │   ├── tracking.npy
│       │   └── experiment_log.json
│       ├── ...
│       ├── background.png
│       └── experiment_config.json
├── stimulus_zoo/             # Stimulus files
│   ├── my_stimulus.stim
│   └── ...
├── experiment_zoo/           # Experiment design files
│   ├── my_2afc.csv
│   ├── my_dfse.ymaze
│   └── ...
└── genotypes.log             # Registry of fly genotypes used
```

---

## Key Actions

### Opening a Project

1. Click **Browse** to select an existing project directory
2. Click **Create New** to create a new project folder

The three panels automatically populate with files found in the project directory.

### Launching Designers

- **New Stimulus** — Opens the Stimulus Designer pre-configured for the current project
- **New Experiment** — Opens the appropriate experiment designer
- **Edit Stimulus / Experiment** — Opens the selected file in its designer

### Processing Experimental Data

Select one or more completed experiment folders in the **Experimental Data** panel, then click **Process Data** to launch the Data Processor for batch post-hoc analysis.

### Generating Videos

Select experiment folders and click **Generate Video** to create annotated trajectory video overlays.

---

## Genotypes Log

The `genotypes.log` file tracks fly genotypes used across all experiments in the project. The Experimenter reads this file to populate the genotype dropdown with previously used genotype strings, ensuring consistency.
