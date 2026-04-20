# Experiment Designers

SixteenY includes three experiment design tools, each implementing a different behavioral paradigm. All designers save experiment files to the project's `experiment_zoo/` directory.

---

## 2AFC Designer

The **2AFC Designer** creates standard Two-Alternative Forced Choice experiments where flies must choose between two odors with configurable reward probabilities.

### Launching

From the Project Manager, click **New 2AFC Experiment**, or run:

```python
# From the command line
python -m sixteeny.gui.16Y_2AFC_designer
```

### Design Parameters

A 2AFC experiment is organized into **blocks of trials**, each with:

| Parameter | Description |
|---|---|
| **Number of Trials** | How many trials in this block |
| **Reward Probability (Odor 1)** | P(reward \| fly chooses Odor 1 arm), 0.0–1.0 |
| **Reward Probability (Odor 2)** | P(reward \| fly chooses Odor 2 arm), 0.0–1.0 |

### Options

| Option | Description |
|---|---|
| **Randomize L/R** | Randomly assign Odor 1 and Odor 2 to left/right arm each trial |
| **Presampled L/R** | Pre-generate L/R assignments (reproducible randomness) |
| **Expectation Sampling** | Oversample L/R assignments to enforce exact empirical ratios |
| **Odor Delay** | Delay (seconds) between fly entering arm and odor delivery |
| **Dwell Time** | Minimum time fly must remain in arm before reward is evaluated |
| **Timed Trials** | If enabled, trials end after a fixed duration regardless of fly choice |
| **Baited Trials** | Whether the reward arm is pre-baited at trial start |
| **Stimulus Files** | Assign `.stim` LED patterns for rewarded and unrewarded arms |
| **Unconditioned Stimulus** | Optional additional stimulus delivered independently of choice |

### Output Format

Saves as a `.csv` file with one row per trial:

```
P(R|Air),P(R|O1),P(R|O2),Odor(Start),Odor(Left),Odor(Right),
StayTime(Air),StayTime(O1),StayTime(O2),CStim(Air),CStim(O1),CStim(O2),
Timed,OdorDelay,UStim,Baited
```

---

## DFSE Designer (Deterministic Finite-State Experiment)

The **DFSE Designer** implements a reversal-learning / contingency task where reward availability is governed by a Markov chain that transitions based on the fly's choices.

### Launching

From the Project Manager, click **New DFSE Experiment**.

### Design Parameters

| Parameter | Description |
|---|---|
| **Number of States** | Total states in the state machine |
| **State Labels** | Reward level per state: `0` = unrewarded, `1` = rewarded |
| **Transition Matrix** | `[n_states × 2]` — `T[i, 0]` = next state if fly chooses Odor 1; `T[i, 1]` = next state if fly chooses Odor 2 |
| **Naive Trials** | Number of initial trials with no reward (state machine inactive) |
| **Total Trials** | Total number of trials (naive + task) |
| **Rewarded Stimulus** | `.stim` file used when reward is given |
| **Unrewarded Stimulus** | `.stim` file used when reward is withheld |
| **L/R Randomization** | Whether to randomize left/right assignment |

### Example: Simple Reversal

A simple 2-state reversal (Odor 1 rewarded → Odor 2 rewarded):

```
States:      S0 (Odor 1 rewarded)    S1 (Odor 2 rewarded)
Transitions: S0 →[O1]→ S0            S0 →[O2]→ S1
             S1 →[O1]→ S0            S1 →[O2]→ S1
```

### Output Format

Saves as a `.ymaze` JSON file containing state labels, transition matrix, stimulus assignments, and trial counts.

---

## DMLE Designer (Deterministic Multi-Level Experiment)

The **DMLE Designer** extends DFSE with **three reward levels** — low, mid, and high — enabling experiments that test graded value representations and multi-level contingency learning.

### Launching

From the Project Manager, click **New DMLE Experiment**.

### Design Parameters

| Parameter | Description |
|---|---|
| **Number of States** | Total states in the state machine |
| **State Labels** | Reward level per state: `0` = unrewarded, `1` = low, `2` = mid, `3` = high |
| **Transition Matrix** | Same as DFSE: `[n_states × 2]` |
| **Naive Trials** | Number of unrewarded naive trials |
| **Total Trials** | Total number of trials |
| **Rewarded Low/Mid/High Stimuli** | Three `.stim` files for each reward magnitude |
| **Unrewarded Stimulus** | `.stim` file for unrewarded trials |
| **L/R Randomization** | Whether to randomize left/right assignment |

### Output Format

Saves as a `.ymle` JSON file, structurally similar to `.ymaze` but with additional reward-level fields.

---

## Choosing a Paradigm

| Paradigm | Use When |
|---|---|
| **2AFC** | Fixed reward probabilities; studying odor preference or bias |
| **DFSE** | Reversal learning, contingency tasks, reward tracking |
| **DMLE** | Value-based choice, multi-level reward coding |
