# Experimenter

**Module:** `sixteeny.utils.experimenter`

Experiment state machines that define the sequence of trials for each arena.

---

## Base Class: `Experimenter`

**`sixteeny.utils.experimenter.base`**

Abstract base class for all experiment types.

```python
class Experimenter:
    def __init__(self, experiment_config_file: str)
    def get_next_trial(self, history: dict) -> dict
    def get_all_states(self) -> list[dict]
```

### `get_next_trial(history)`

Returns a dict describing the next trial:

```python
{
    "reward_probability": [float, float, float],   # P(reward) per arm
    "relative_odor_vector": [int, int, int],        # odor IDs for [start, left, right]
    "time_needed_in_reward_zone": [float, ...],     # dwell time per arm (s or "inf")
    "reward_stimulus": [str, str, str],             # .stim files per arm
    "timed": bool,                                  # timed trial flag
    "odor_delay": float,                            # delay before odor delivery (s)
    "unconditioned_stimulus": str,                  # .stim file for US
    "baited": bool                                  # pre-baited trial flag
}
```

---

## Class: `CSVExperimenter`

**`sixteeny.utils.experimenter.openloop`**

Loads a trial schedule from a `.csv` file (used for 2AFC experiments).

```python
CSVExperimenter(experiment_config_file: str)
```

The CSV must have columns:
`P(R|Air)`, `P(R|O1)`, `P(R|O2)`, `Odor(Start)`, `Odor(Left)`, `Odor(Right)`,
`StayTime(Air)`, `StayTime(O1)`, `StayTime(O2)`, `CStim(Air)`, `CStim(O1)`, `CStim(O2)`,
`Timed`, `OdorDelay`, `UStim`, `Baited`

Supports randomized left/right assignment via `1.5` placeholders in the odor columns.

---

## Class: `DeterministicFiniteStateExperimenter`

**`sixteeny.utils.experimenter.openloop`**

Loads a Markov-chain DFSE experiment from a `.ymaze` JSON file.

```python
DeterministicFiniteStateExperimenter(experiment_config_file: str)
```

Key attributes:

| Attribute | Description |
|---|---|
| `n_states` | Number of Markov states |
| `state_labels` | Reward label per state (0=unrewarded, 1=rewarded) |
| `transition_matrix` | `(n_states, 2)` — next state on Odor1/Odor2 choice |
| `current_state` | Current Markov state |
| `naive_trials` | Number of pre-task unrewarded trials |

---

## Class: `DeterministicMultilevelExperimenter`

**`sixteeny.utils.experimenter.closedloop`**

Loads a multi-level DMLE experiment from a `.ymle` JSON file.

```python
DeterministicMultilevelExperimenter(experiment_config_file: str)
```

Extends `DeterministicFiniteStateExperimenter` with three reward levels:

| State Label | Reward Level |
|---|---|
| `0` | Unrewarded |
| `1` | Low |
| `2` | Mid |
| `3` | High |

Each level maps to a separate `.stim` file (`rewarded_low_stimulus`, `rewarded_mid_stimulus`, `rewarded_high_stimulus`).

---

## Usage Example

```python
from sixteeny.utils.experimenter import (
    CSVExperimenter,
    DeterministicFiniteStateExperimenter,
    DeterministicMultilevelExperimenter
)

# 2AFC
exp = CSVExperimenter("experiment_zoo/my_2afc.csv")

# DFSE
exp = DeterministicFiniteStateExperimenter("experiment_zoo/reversal.ymaze")

# DMLE
exp = DeterministicMultilevelExperimenter("experiment_zoo/multilevel.ymle")

# Get first trial
trial = exp.get_next_trial(history={"chosen_odor": []})
print(trial["relative_odor_vector"])  # e.g. [0, 1, 2]
```
