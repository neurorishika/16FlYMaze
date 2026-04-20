<div align="center">

<img src="documentation/16FlyMaze.png" alt="16-Y Arena" width="320"/>

# 🪰 SixteenY — Turner Lab Opto2AFC 16-Y Arena

**A high-throughput optogenetic 2-Alternative Forced Choice (2AFC) behavioral system for 16 simultaneous *Drosophila* experiments**

[![Python](https://img.shields.io/badge/Python-3.8-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/downloads/release/python-380/)
[![Platform](https://img.shields.io/badge/Platform-Windows-0078D4?style=for-the-badge&logo=windows&logoColor=white)](https://www.microsoft.com/windows)
[![License](https://img.shields.io/badge/License-BSD%203--Clause-blue?style=for-the-badge)](LICENSE)
[![CUDA](https://img.shields.io/badge/CUDA-11.x-76B900?style=for-the-badge&logo=nvidia&logoColor=white)](https://developer.nvidia.com/cuda-toolkit)
[![ROS2](https://img.shields.io/badge/ROS2-Humble-22314E?style=for-the-badge&logo=ros&logoColor=white)](https://docs.ros.org/en/humble/)
[![PyQt5](https://img.shields.io/badge/GUI-PyQt5-41CD52?style=for-the-badge&logo=qt&logoColor=white)](https://riverbankcomputing.com/software/pyqt/)

[📖 Documentation](https://neurorishika.github.io/TurnerLab_Opto2AFC_16Y) · [🚀 Quick Start](#quick-start) · [🐛 Report a Bug](https://github.com/neurorishika/TurnerLab_Opto2AFC_16Y/issues) · [💡 Request Feature](https://github.com/neurorishika/TurnerLab_Opto2AFC_16Y/issues)

</div>

---

## 📋 Overview

**SixteenY** is a complete software stack for running automated optogenetic 2-Alternative Forced Choice (2AFC) behavioral experiments on *Drosophila melanogaster* (fruit flies) in 16 simultaneous Y-shaped arenas. Developed at the Turner Lab, this system integrates hardware control, real-time video tracking, experiment management, and post-hoc data analysis into a cohesive, GUI-driven workflow.

The system uses GPU-accelerated image processing (CUDA/CuPy) to track fly positions across all 16 arenas in real time, delivers precisely controlled odor stimuli and optogenetic light pulses, and records experimental outcomes for downstream analysis.

---

## ✨ Key Features

| Feature | Description |
|---|---|
| 🔬 **16 Simultaneous Arenas** | Run identical or independent experiments on 16 Y-mazes at once |
| 🎥 **GPU-Accelerated Tracking** | Real-time fly position tracking using CuPy & CUCIM on NVIDIA GPUs |
| 💡 **Optogenetic Stimulation** | Precise LED pulse-pattern control via serial Arduino interface |
| 🌬️ **Odor Delivery** | Automated odor valve control via ROS2 / Raspberry Pi interface |
| 🌡️ **Flow Rate Control** | Alicat Mass Flow Controller (MFC) integration |
| 🎮 **PyQt5 GUI Suite** | Full suite of graphical tools — no command line required |
| 🧪 **Multiple Paradigms** | 2AFC, Deterministic Finite-State, and Multi-Level experiment designs |
| 📊 **Built-in Analysis** | Automated data processing and video generation tools |
| 📧 **Email Notifications** | Gmail API integration for remote experiment monitoring |

---

## 🏗️ System Architecture

```
┌──────────────────────────────────────────────────────────────────────┐
│                         SixteenY Software Stack                      │
├───────────────────────┬──────────────────────────────────────────────┤
│   GUI Applications    │  sixteeny/gui/                               │
│                       │  ├─ 16Y_experimenter.py      (main runner)   │
│                       │  ├─ 16Y_project_manager.py   (file manager)  │
│                       │  ├─ 16Y_rig_configurator.py  (hardware cfg)  │
│                       │  ├─ 16Y_mask_designer.py     (arena masks)   │
│                       │  ├─ 16Y_2AFC_designer.py     (2AFC design)   │
│                       │  ├─ 16Y_DFSE_designer.py     (FSE design)    │
│                       │  ├─ 16Y_DMLE_designer.py     (MLE design)    │
│                       │  └─ 16Y_stimulus_designer.py (LED patterns)  │
├───────────────────────┼──────────────────────────────────────────────┤
│  Hardware Controllers │  sixteeny/controller/                        │
│                       │  ├─ camera.py    (Spinnaker FLIR camera)     │
│                       │  ├─ led.py       (LED/optogenetics Arduino)  │
│                       │  ├─ odor.py      (ROS2 odor valve control)   │
│                       │  └─ mfc.py       (Alicat MFC flow rates)     │
├───────────────────────┼──────────────────────────────────────────────┤
│       Utilities       │  sixteeny/utils/                             │
│                       │  ├─ tracker.py   (real-time fly tracker)     │
│                       │  ├─ experimenter/ (experiment state machine) │
│                       │  ├─ emailer.py   (Gmail API notifications)   │
│                       │  └─ printer.py   (logging utility)           │
├───────────────────────┼──────────────────────────────────────────────┤
│      Analysis         │  sixteeny/analysis/                          │
│                       │  ├─ 16Y_data_processor.py  (post-hoc GUI)   │
│                       │  └─ 16Y_video_generator.py (video output)   │
└───────────────────────┴──────────────────────────────────────────────┘
```

---

## 🖥️ GUI Applications

<table>
  <tr>
    <td align="center"><b>🧫 Project Manager</b><br><sub>Organize experiments, stimuli, and data</sub></td>
    <td align="center"><b>⚙️ Rig Configurator</b><br><sub>Configure hardware ports and settings</sub></td>
    <td align="center"><b>🗺️ Mask Designer</b><br><sub>Draw arena region-of-interest masks</sub></td>
  </tr>
  <tr>
    <td align="center"><b>🔬 Experimenter</b><br><sub>Run live experiments with real-time tracking</sub></td>
    <td align="center"><b>📐 2AFC Designer</b><br><sub>Design reward-probability schedules</sub></td>
    <td align="center"><b>💡 Stimulus Designer</b><br><sub>Create LED pulse-pattern stimuli</sub></td>
  </tr>
</table>

---

## 🚀 Quick Start

### Prerequisites

- Windows 10/11 (64-bit)
- [Anaconda](https://www.anaconda.com/) or Miniconda
- NVIDIA GPU with CUDA 11.x support
- FLIR Spinnaker SDK
- FFMPEG (added to PATH)

### Installation

```bash
# 1. Create and activate a conda environment
conda create -n sixteeny python=3.8
conda activate sixteeny

# 2. Install core Python dependencies
pip install pyserial numpy matplotlib scikit-image scikit-video PyQt5 alicat pandas seaborn

# 3. Install CUDA and CuPy
conda install -c conda-forge cupy cudatoolkit=11.0

# 4. Install the Spinnaker Python SDK (download .whl from FLIR)
pip install spinnaker_python-*.whl --force-reinstall --user

# 5. Install Google API client for email notifications
pip install --upgrade google-api-python-client oauth2client

# 6. Install the sixteeny package (from the repository root)
pip install -e .
```

> 📖 For full installation details including CUCIM, ROS2 interface setup, and email configuration, see the [Installation Guide](https://neurorishika.github.io/TurnerLab_Opto2AFC_16Y/getting-started/installation/).

### Running the Applications

Double-click the provided `.bat` shortcuts (or run them from a terminal):

| Script | Application |
|---|---|
| `run_project_manager.bat` | Launch the Project Manager |
| `run_rig_configurator.bat` | Launch the Rig Configurator |
| `run_mask_designer.bat` | Launch the Mask Designer |
| `run_experiment.bat` | Launch the Experimenter |
| `initialize_shortcuts.bat` | Create desktop shortcuts for all tools |

---

## 📁 Project Structure

```
TurnerLab_Opto2AFC_16Y/
├── sixteeny/                  # Main Python package
│   ├── main.py                # Core experiment loop
│   ├── controller/            # Hardware driver classes
│   ├── gui/                   # PyQt5 GUI applications
│   ├── utils/                 # Tracking, experiment logic, email
│   └── analysis/              # Data processing & video tools
├── documentation/             # Diagrams and screenshots
├── example_project/           # Sample project with data structure
├── masks/                     # Saved arena mask files
├── setup.py                   # Package installation
├── *.bat                      # Windows launcher scripts
└── install_instructions.txt   # Detailed setup notes
```

---

## 🧬 Experiment Paradigms

### 2AFC (Two-Alternative Forced Choice)
Flies choose between two odor arms (Odor 1 and Odor 2) with configurable reward probabilities. Supports randomized or pre-sampled left/right assignments.

### DFSE (Deterministic Finite-State Experiment)
A Markov-chain-based paradigm where reward availability transitions between discrete states based on the fly's choices, enabling reversal learning and contingency tasks.

### DMLE (Deterministic Multi-Level Experiment)
Extends DFSE with multiple reward levels (low/mid/high), enabling experiments that test graded value representations.

---

## 📊 Hardware Components

| Component | Device | Interface |
|---|---|---|
| Camera | FLIR Spinnaker (machine vision) | USB3 / GigE |
| Optogenetic LEDs | Custom Arduino-based controller | Serial (COM ports) |
| Odor Valves | Raspberry Pi via ROS2 | Network / USB |
| Mass Flow Controllers | Alicat Scientific MFC | Serial (COM port) |

---

## 📄 License

This project is licensed under the **BSD 3-Clause License** — see the [LICENSE](LICENSE) file for details.

Copyright © 2022 [Rishika Mohanta](https://github.com/neurorishika), Turner Lab.

---

## 🙏 Acknowledgements

Developed in the **Turner Lab** for high-throughput *Drosophila* behavioral neuroscience.  
Special thanks to all contributors and collaborators who helped build and validate this system.

---

<div align="center">
<sub>Built with ❤️ for fly neuroscience</sub>
</div>
