# Odor Valve Controller

**Module:** `sixteeny.controller.odor`

Controls the solenoid odor valves via a ROS2 interface running on a Raspberry Pi.

---

## Class: `OdorValveController`

Publishes odor delivery commands to a ROS2 topic consumed by the Raspberry Pi valve controller.

### Constructor

```python
OdorValveController(minimum_delay: float = 0.1)
```

| Parameter | Type | Description |
|---|---|---|
| `minimum_delay` | `float` | Minimum time in seconds between consecutive valve commands |

### Methods

#### Lifecycle

```python
def init(self, *args, **kwargs) -> None
```
Initialize the ROS2 node and publisher. Accepts optional `rclpy.init()` arguments.

```python
def close(self) -> None
```
Destroy the ROS2 node and shut down the rclpy context.

```python
def __enter__(self) -> OdorValveController
def __exit__(self, ...) -> None
```
Context manager support.

#### Odor Delivery

```python
def publish(self, arena: int, odor_vector: list[int]) -> None
```
Publish an odor command for a specific arena.

| Parameter | Type | Description |
|---|---|---|
| `arena` | `int` | Arena index (0–15) |
| `odor_vector` | `list[int]` | Odor IDs for arms `[start, left, right]` — `0`=air, `1`=Odor 1, `2`=Odor 2 |

```python
def create_msg(self, arena: int, odor_vector: list[int]) -> ArenaOdors
```
Create a ROS2 `ArenaOdors` message without publishing it.

---

## ROS2 Message: `ArenaOdors`

**Package:** `y_arena_interfaces`

| Field | Type | Description |
|---|---|---|
| `arena` | `int32` | Arena index |
| `odors` | `int32[3]` | Odor IDs for start, left, and right arms |

---

## Usage Example

```python
from sixteeny.controller.odor import OdorValveController

with OdorValveController(minimum_delay=0.1) as odor:
    # Set arena 0: air in start arm, Odor 1 in left, Odor 2 in right
    odor.publish(arena=0, odor_vector=[0, 1, 2])

    # Later: reset all arms to air
    odor.publish(arena=0, odor_vector=[0, 0, 0])
```

---

## Dependencies

| Package | Purpose |
|---|---|
| `rclpy` | ROS2 Python client library |
| `y_arena_interfaces` | Custom ROS2 message package (must be built on both host and RPi) |
