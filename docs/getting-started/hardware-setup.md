# Hardware Setup

This page describes the hardware components of the 16-Y Arena system, their connections, and configuration requirements.

---

## System Diagram

```
┌─────────────────────────────────────────────────────────────────────┐
│                      Windows Workstation (GPU)                       │
│                                                                     │
│  ┌─────────────┐  ┌──────────────────┐  ┌───────────────────────┐  │
│  │  Spinnaker  │  │  LED Controller  │  │  MFC Controller       │  │
│  │  Camera     │  │  (Arduino-based) │  │  (Alicat via COM)     │  │
│  │  USB3/GigE  │  │  COM3/4/5/6      │  │  COM port             │  │
│  └──────┬──────┘  └────────┬─────────┘  └───────────┬───────────┘  │
│         │                  │                        │               │
└─────────┼──────────────────┼────────────────────────┼───────────────┘
          │                  │                        │
          ▼                  ▼                        ▼
   ┌─────────────┐   ┌──────────────┐        ┌──────────────┐
   │  16-Y Arena │   │  LED Panels  │        │  Alicat MFCs │
   │  (Camera    │   │  (4 modules, │        │  (flow rate  │
   │   field)    │   │   16 arenas) │        │   control)   │
   └─────────────┘   └──────────────┘        └──────────────┘
                                                      │
                                              ┌───────┴───────┐
                                              │  Odor Valves  │
                                              │  (Raspberry Pi│
                                              │   via ROS2)   │
                                              └───────────────┘
```

---

## Hardware Components

### Camera — FLIR Spinnaker

| Parameter | Value |
|---|---|
| SDK | FLIR Spinnaker (latest) |
| Interface | USB 3.0 or GigE |
| Driver | PySpin Python API |
| Frame rate | Configurable (typically 10–30 fps) |
| Resolution | Camera-dependent |

**Setup notes:**

- Install the Spinnaker Full SDK from [FLIR's Box](https://flir.app.boxcn.net/v/SpinnakerSDK)
- Install the matching Python wheel (`spinnaker_python-*.whl`)
- Use FLIR SpinView to verify camera detection before running experiments

---

### LED Controller (Optogenetic Stimulation)

The system uses a custom Arduino-based LED controller with 4 modules, each controlling 4 arenas (16 total).

| Parameter | Value |
|---|---|
| Interface | Serial (RS-232 / USB-Serial) |
| Default COM ports | COM3, COM4, COM5, COM6 |
| Baud rate | 115,200 |
| Panel IDs | `0001`, `0010`, `1000`, `0100` |

**LED channels per arena:**

- **IR** — Infrared backlight for camera imaging (always on during experiment)
- **R, G, B** — Visible wavelength channels for optogenetic stimulation (independently controllable)

**Intensity scaling:** Each channel has an independent scaling factor (0.0–1.0) applied on top of the programmed intensity to account for LED-to-LED variability.

**Serial protocol:** The controller accepts hex-encoded RGB intensity commands and pulse-pattern parameters.

---

### Odor Delivery System

Odor delivery is controlled via a Raspberry Pi running ROS2, which drives the solenoid odor valves.

| Parameter | Value |
|---|---|
| Interface | ROS2 over network (or USB) |
| ROS2 topic | `arena_odors` |
| Message type | `y_arena_interfaces/msg/ArenaOdors` |
| Minimum command delay | 100 ms (configurable) |

**Odor arms per Y-maze:**

Each Y-maze has 3 arms:
- **Arm 0 (Start)** — Air only; fly starts here at the beginning of each trial
- **Arm 1 (Left)** — Odor 1 or Odor 2 (randomized per trial)
- **Arm 2 (Right)** — The other odor

**Odor identifiers:**

| ID | Meaning |
|---|---|
| `0` | Clean air |
| `1` | Odor 1 |
| `2` | Odor 2 |

---

### Mass Flow Controllers (Alicat)

Alicat Scientific mass flow controllers regulate the airflow through the odor delivery system.

| Parameter | Value |
|---|---|
| Interface | Serial (via COM port) |
| Python library | `alicat` |
| Default flow rate | Configured per experiment |

**Setup notes:**

- Assign unique device IDs to each MFC unit
- Set the COM port and device IDs in the Rig Configurator
- The system initializes each MFC to the default flow rate at startup

---

## Rig Wiring Guide

### LED Controller Serial Connections

Connect the four LED controller modules to USB-to-serial adapters on the Windows workstation:

```
Module 1 (Panel ID: 0001) → COM3
Module 2 (Panel ID: 0010) → COM5
Module 3 (Panel ID: 1000) → COM6
Module 4 (Panel ID: 0100) → COM4
```

!!! note "COM Port Assignment"
    COM port numbers may differ on your machine. Use **Device Manager** to identify the correct COM ports and update them in the Rig Configurator.

### Arena Numbering

The 16 arenas are arranged in a 4×4 grid. Arenas are numbered 0–15 left-to-right, top-to-bottom. Arenas at even indices (0, 2, 4, 6, 8, 10, 12, 14) are **clockwise** and odd arenas are **counter-clockwise** — this affects how left/right positions are interpreted.

### MFC Connections

Connect the Alicat MFC to a serial port on the workstation. Set the COM port and device IDs in the Rig Configurator.

### ROS2 / Raspberry Pi Network

The Raspberry Pi running the odor valve controller must be reachable from the Windows workstation over the local network (or USB network adapter). Ensure:

- Both devices are on the same network segment
- The ROS2 domain ID matches between host and RPi
- The `y_arena_interfaces` message package is installed on both machines

---

## Pre-experiment Hardware Checklist

- [ ] Camera detected by SpinView
- [ ] All 4 LED controller serial ports visible in Device Manager
- [ ] LED backlight powers on (test with `test_loop_valves.bat`)
- [ ] MFC flow rates respond (test with MFC viewer in GUI)
- [ ] Raspberry Pi reachable and ROS2 nodes running
- [ ] FFMPEG accessible from terminal (`ffmpeg -version`)
- [ ] Arena masks created and saved for current camera position
