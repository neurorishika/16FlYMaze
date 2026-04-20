# LED Controller

**Module:** `sixteeny.controller.led`

Controls the custom Arduino-based LED controllers for optogenetic stimulation and IR backlight.

---

## Helper Functions

```python
def hex_to_rgb_intensity(hex_value: bytes) -> tuple[int, int, int]
```
Convert a hex color string (e.g., `b"#FF8000"`) to RGB intensity percentages (0–100).

---

## Class: `LEDController`

Controls all 4 LED modules (16 arenas) via serial connections.

### Constructor

```python
LEDController(
    ports=["COM3", "COM5", "COM6", "COM4"],
    baudrate=115200,
    arena_panel_ids=["0001", "0010", "1000", "0100"],
    irgb_scaling_factors=None   # (16, 4) array of scaling factors per arena per channel
)
```

| Parameter | Type | Description |
|---|---|---|
| `ports` | `list[str]` | COM port names for each of the 4 LED modules |
| `baudrate` | `int` | Serial baud rate (default: 115,200) |
| `arena_panel_ids` | `list[str]` | 4-character binary panel IDs |
| `irgb_scaling_factors` | `np.ndarray` | `(16, 4)` scaling factors (IR, R, G, B) per arena |

### Methods

#### Lifecycle

```python
def setup_connections(self) -> None
```
Open serial connections to all LED modules.

```python
def close_connections(self) -> None
```
Close all serial connections.

```python
def init(self) -> None
```
Initialize the LED controller (calls `setup_connections`).

```python
def close(self) -> None
```
Close the LED controller (calls `close_connections`).

```python
def __enter__(self) -> LEDController
def __exit__(self, ...) -> None
```
Context manager support.

#### IR Backlight

```python
def initialize_IR(self, ir_intensity: int) -> None
```
Initialize the IR backlight at the given intensity (0–100) for all arenas.

```python
def turn_on_backlight(self) -> None
```
Turn on the IR backlight for all arenas at the last set intensity.

```python
def turn_off_backlight(self) -> None
```
Turn off the IR backlight for all arenas.

#### Stimulus Control

```python
def set_led_pulse_pattern(
    self,
    arena: int,
    pulse_width: float,
    pulse_period: float,
    pulse_count: int,
    pulse_delay: float,
    pulse_repeat: int,
    pulse_deadtime: float
) -> None
```
Program the pulse-train pattern for a given arena.

```python
def set_led_stimulus_intensity(
    self,
    arena: int,
    ir: int,
    r: int,
    g: int,
    b: int
) -> None
```
Set the IRGB intensity (0–100) for a given arena, applying per-arena scaling factors.

```python
def run_led_stimulus(self, arena: int) -> None
```
Trigger the pre-programmed stimulus on the given arena.

```python
def reset_arena_led_stimuli(self, arena: int) -> None
```
Reset all LED parameters for the given arena to zero.

```python
def reset_module_led_stimuli(self, module: int) -> None
```
Reset all arenas in the given LED module.

```python
def led_stimulation(
    self,
    arena: int,
    stim_file: str
) -> None
```
Load a `.stim` JSON file and execute the full stimulation sequence on the given arena.

---

## Usage Example

```python
from sixteeny.controller.led import LEDController

with LEDController(ports=["COM3", "COM5", "COM6", "COM4"]) as led:
    led.initialize_IR(ir_intensity=100)
    led.turn_on_backlight()

    # Deliver a stimulus on arena 0
    led.led_stimulation(arena=0, stim_file="stimulus_zoo/my_stim.stim")

    led.turn_off_backlight()
```

---

## Dependencies

| Package | Purpose |
|---|---|
| `serial` | PySerial for COM port communication |
| `numpy` | Intensity scaling calculations |
