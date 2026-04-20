---
hide:
  - navigation
---

<div class="hero" markdown>

# 🪰 SixteenY

**High-throughput optogenetic 2AFC behavioral system for *Drosophila***

Run 16 simultaneous Y-maze experiments with real-time GPU-accelerated fly tracking, optogenetic stimulation, odor delivery, and automated data collection — all from a unified GUI suite.

[🚀 Get Started](getting-started/installation.md){ .md-button .md-button--primary }
[📖 User Guide](user-guide/overview.md){ .md-button }

</div>

---

## What is SixteenY?

**SixteenY** (also known as the Turner Lab Opto2AFC 16-Y system) is a complete hardware-software platform for running automated optogenetic Two-Alternative Forced Choice (2AFC) behavioral experiments on *Drosophila melanogaster* in 16 simultaneous Y-shaped arenas.

The system was developed in the **Turner Lab** to enable high-throughput, reproducible behavioral neuroscience experiments that combine:

- **Odor discrimination** — two distinct odors delivered to the left and right arms of each Y-maze
- **Optogenetic reinforcement** — precisely timed LED light pulses delivered as conditioned stimuli
- **Automated tracking** — GPU-accelerated real-time detection of fly position and choice

---

## Feature Highlights

<div class="grid cards" markdown>

-   :fontawesome-solid-microscope: **16 Simultaneous Arenas**

    ---

    Run the same or independent experiments on 16 Y-shaped arenas at once, maximizing throughput and enabling within-session comparisons.

-   :fontawesome-solid-bolt: **GPU-Accelerated Tracking**

    ---

    Real-time fly detection and position tracking using NVIDIA CUDA (CuPy & CUCIM), processing all 16 arenas at camera frame rate.

-   :fontawesome-solid-lightbulb: **Optogenetic Stimulation**

    ---

    Programmable LED pulse patterns (pulse width, period, count, delay, intensity) delivered via serial Arduino interface.

-   :fontawesome-solid-wind: **Automated Odor Delivery**

    ---

    Odor valve control over ROS2 / Raspberry Pi interface, with Alicat MFC flow rate management.

-   :fontawesome-solid-desktop: **Full GUI Suite**

    ---

    Seven dedicated PyQt5 applications — from hardware configuration and mask design to live experiment control and post-hoc analysis.

-   :fontawesome-solid-flask: **Multiple Paradigms**

    ---

    Support for 2AFC, Deterministic Finite-State (DFSE), and Multi-Level (DMLE) experiment designs, enabling diverse behavioral tasks.

-   :fontawesome-solid-envelope: **Email Notifications**

    ---

    Gmail API integration for remote experiment monitoring — receive alerts, progress reports, and completion notifications.

-   :fontawesome-solid-chart-bar: **Built-in Analysis**

    ---

    Automated post-hoc data processing, trajectory analysis, and video generation tools included out of the box.

</div>

---

## System Overview

```mermaid
graph TD
    A[👩‍🔬 Researcher] --> B[16Y GUI Suite]
    B --> C[Experimenter]
    B --> D[Project Manager]
    B --> E[Rig Configurator]
    B --> F[Mask Designer]
    B --> G[Experiment Designers]
    B --> H[Stimulus Designer]

    C --> I[sixteeny Core]
    I --> J[Camera Controller\nFLIR Spinnaker]
    I --> K[LED Controller\nArduino Serial]
    I --> L[Odor Controller\nROS2 / RPi]
    I --> M[MFC Controller\nAlicat]

    I --> N[GPU Tracker\nCuPy / CUCIM]
    N --> O[Arena Tracker × 16]
    O --> P[Experiment State Machine]
    P --> K
    P --> L

    I --> Q[Data Logger\nNumPy + JSON]
    Q --> R[Analysis Tools]
```

---

## Quick Navigation

| I want to… | Go to… |
|---|---|
| Install the system for the first time | [Installation Guide](getting-started/installation.md) |
| Understand the typical workflow | [Quick Start](getting-started/quickstart.md) |
| Set up the hardware | [Hardware Setup](getting-started/hardware-setup.md) |
| Configure the rig | [Rig Configurator](user-guide/rig-configurator.md) |
| Design a new experiment | [Experiment Designers](user-guide/experiment-designers.md) |
| Run an experiment | [Experimenter](user-guide/experimenter.md) |
| Analyse collected data | [Analysis Tools](user-guide/analysis.md) |
| Browse the Python API | [API Reference](api-reference/controllers/camera.md) |

---

## License

SixteenY is released under the **BSD 3-Clause License**.  
Copyright © 2022 Rishika Mohanta, Turner Lab.
