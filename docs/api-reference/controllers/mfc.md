# Mass Flow Controller

**Module:** `sixteeny.controller.mfc`

Controls Alicat Scientific mass flow controllers (MFCs) that regulate airflow through the odor delivery system.

---

## Class: `MFCController`

Manages one or more Alicat MFC units connected via a single serial port.

### Constructor

```python
MFCController(
    com_port: str,
    device_ids: list[str],
    default_flow_rate: float
)
```

| Parameter | Type | Description |
|---|---|---|
| `com_port` | `str` | Serial COM port (e.g., `"COM7"`) |
| `device_ids` | `list[str]` | List of Alicat device address IDs |
| `default_flow_rate` | `float` | Default flow rate in sccm applied at initialization |

### Methods

#### Lifecycle

```python
def init(self) -> None
```
Initialize all MFCs: test each connection and set to the default flow rate.

```python
def close(self) -> None
```
Set all MFCs to zero flow and close serial connections. Retries up to 10 times on failure.

```python
def __enter__(self) -> MFCController
def __exit__(self, ...) -> None
```
Context manager support.

#### Flow Rate Control

```python
def set_flow_rate(self, mfc_index: int, flow_rate: float) -> None
```
Set the flow rate for the MFC at the given index.

| Parameter | Type | Description |
|---|---|---|
| `mfc_index` | `int` | Index into `device_ids` list |
| `flow_rate` | `float` | Target flow rate in sccm |

```python
def get_flow_rate(self, mfc_index: int) -> float
```
Read the current flow rate from the MFC at the given index.

```python
def test_connection(self, mfc_index: int) -> bool
```
Test whether the MFC at the given index is responsive.

---

## Usage Example

```python
from sixteeny.controller.mfc import MFCController

with MFCController(
    com_port="COM7",
    device_ids=["A", "B"],
    default_flow_rate=100.0
) as mfc:
    # Read current flow from MFC 0
    flow = mfc.get_flow_rate(0)
    print(f"Current flow: {flow} sccm")

    # Set MFC 1 to 50 sccm
    mfc.set_flow_rate(1, 50.0)
```

---

## Dependencies

| Package | Purpose |
|---|---|
| `alicat` | Alicat MFC Python driver |
