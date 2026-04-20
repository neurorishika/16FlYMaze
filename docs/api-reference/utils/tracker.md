# Arena Tracker

**Module:** `sixteeny.utils.tracker`

Tracks the state of a single fly in one Y-maze arena across an entire experiment session.

---

## Class: `ArenaTracker`

Maintains per-arena state including fly position, current arm, trial progress, and all logged data matrices.

### Constructor

```python
ArenaTracker(
    arena_index: int,
    experimenter: Experimenter,
    controllers: dict,
    printer: Printer = None
)
```

| Parameter | Type | Description |
|---|---|---|
| `arena_index` | `int` | Arena index (0–15) |
| `experimenter` | `Experimenter` | Experiment state machine for this arena |
| `controllers` | `dict` | Dict of hardware controllers (`"led"`, `"odor"`, `"mfc"`) |
| `printer` | `Printer` | Optional logging printer |

### Key Attributes

| Attribute | Type | Description |
|---|---|---|
| `arena_index` | `int` | This arena's index |
| `trial_count` | `int` | Current trial number |
| `completed` | `bool` | Whether all trials are finished |
| `fly_position` | `np.ndarray` | Current `[x, y]` centroid |
| `current_arm` | `int` | Current arm index (0=Start, 1=Left, 2=Right) |
| `in_reward_zone` | `bool` | Whether fly is in the reward zone |
| `fly_positions` | `np.ndarray` | `(max_frames, 2)` position history |
| `chosen_arms` | `np.ndarray` | `(n_trials,)` chosen arm per trial |
| `chosen_odor` | `np.ndarray` | `(n_trials,)` chosen odor per trial |
| `reward_delivered` | `np.ndarray` | `(n_trials,)` reward delivery flag |

### Methods

```python
def relative_to_absolute_arm(
    self,
    relative_position: int,
    start_arm: int,
    arena_index: int
) -> int
```
Convert a relative arm position (0=Start, 1=Left, 2=Right) to an absolute arm index, accounting for clockwise/counter-clockwise arena orientation.

```python
def update(self, fly_position: np.ndarray, frame_time: float) -> None
```
Process a new frame: update fly position, check arm transitions, evaluate reward zone, and trigger stimulus delivery if needed.

```python
def start_new_trial(self) -> None
```
Initialize a new trial: query the experimenter for the next trial configuration, deliver odors, and reset trial state.

```python
def end_trial(self) -> None
```
Finalize the current trial: log outcomes and check for experiment completion.

---

## Arm Coordinate System

Each Y-maze has 3 arms referenced in two coordinate systems:

| Coordinate | System | Values |
|---|---|---|
| **Absolute** | Physical arm position | 0, 1, 2 |
| **Relative** | Trial-start-relative | 0=Start, 1=Left, 2=Right |

Clockwise arenas (indices 0, 2, 4, 6, 8, 10, 12, 14) have mirrored left/right assignments compared to counter-clockwise arenas.

---

## Usage Example

```python
from sixteeny.utils.tracker import ArenaTracker
from sixteeny.utils.experimenter import CSVExperimenter

experimenter = CSVExperimenter("experiment_zoo/my_2afc.csv")
tracker = ArenaTracker(
    arena_index=0,
    experimenter=experimenter,
    controllers={"led": led_ctrl, "odor": odor_ctrl}
)

# In the main experiment loop:
tracker.update(fly_position=np.array([320, 240]), frame_time=1.23)
```
