# Data Processor

**Module:** `sixteeny.analysis.16Y_data_processor`

A PyQt5 GUI application for post-hoc processing of 16FlYMaze experimental data.

---

## Overview

The Data Processor loads raw tracking arrays and experiment logs from completed experiment folders and applies cleaning, alignment, and metric computation to produce analysis-ready output files.

---

## Key Functions

### Coordinate Realignment

```python
def euclidean_distance(point1: np.ndarray, point2: np.ndarray) -> float
```
Compute Euclidean distance between two 2D points.

```python
def realign_lr(
    start_arm: float,
    point: np.ndarray,
    arena_index: int
) -> np.ndarray
```
Rotate fly position coordinates into a canonical **Left–Start–Right** frame.

| `start_arm` | Rotation applied |
|---|---|
| `0.0` | +120° |
| `1.0` | 0° (no rotation) |
| `2.0` | −120° |

Clockwise arenas additionally mirror the x-axis (`× [−1, 1]`).

```python
def realign_odor(
    odor_vector: np.ndarray,
    point: np.ndarray
) -> np.ndarray
```
Re-align fly position into an **Odor 1–Start–Odor 2** frame by mirroring the x-axis when odors are swapped.

---

## Computed Metrics

| Metric | Description |
|---|---|
| **Choice fraction** | Fraction of trials in which Odor 1 (or left) was chosen |
| **Response time** | Time from trial start to first arm entry |
| **Dwell time** | Time spent in the chosen arm before reward evaluation |
| **Reward rate** | Fraction of trials with reward delivered |
| **Trial length** | Total duration from trial start to next trial start |

---

## Output Files

Processed data is written to CSV files in the experiment directory:

| File | Contents |
|---|---|
| `processed_data.csv` | Per-trial metrics for all arenas |
| `trajectories.npy` | Realigned trajectory arrays |

---

## Dependencies

| Package | Purpose |
|---|---|
| `numpy` | Array operations |
| `scikit-image` | Affine transforms for coordinate alignment |
| `joblib` | Parallel processing across arenas |
| `PyQt5` | GUI application framework |
