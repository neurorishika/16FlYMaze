# Camera Controller

**Module:** `sixteeny.controller.camera`

Controls the FLIR Spinnaker machine-vision camera used to image all 16 arenas.

---

## Classes

### `SpinnakerCamera`

A high-level wrapper around the PySpin SDK for acquiring frames from a FLIR camera.

#### Constructor

```python
SpinnakerCamera(
    camera_index=0,
    exposure_time=10000,
    gain=0,
    frame_rate=30,
    image_width=1920,
    image_height=1200,
    pixel_format="Mono8"
)
```

| Parameter | Type | Description |
|---|---|---|
| `camera_index` | `int` | Index of the camera in the Spinnaker camera list |
| `exposure_time` | `float` | Exposure time in microseconds |
| `gain` | `float` | Analog gain |
| `frame_rate` | `float` | Target frame rate (fps) |
| `image_width` | `int` | Image width in pixels |
| `image_height` | `int` | Image height in pixels |
| `pixel_format` | `str` | Pixel format (e.g., `"Mono8"`) |

#### Methods

```python
def init(self) -> None
```
Initialize the camera: configure acquisition parameters and begin acquisition.

```python
def close(self) -> None
```
Stop acquisition and release the camera.

```python
def get_frame(self) -> np.ndarray
```
Acquire and return a single frame as a NumPy array.

```python
def __enter__(self) -> SpinnakerCamera
def __exit__(self, ...) -> None
```
Context manager support.

---

## Module-level Functions

```python
def list_cameras() -> PySpin.CameraList
```
Return a list of all connected Spinnaker cameras. Initializes the PySpin `System` singleton if not already initialized.

```python
def video_writer(save_queue: queue.Queue, writer) -> None
```
Worker function for asynchronous video writing. Reads frames from `save_queue` and writes them using a `skvideo` writer. Send `None` to stop.

---

## Usage Example

```python
from sixteeny.controller.camera import SpinnakerCamera

with SpinnakerCamera(camera_index=0, exposure_time=5000, frame_rate=20) as cam:
    frame = cam.get_frame()
    # frame is a NumPy array (H × W) in Mono8 format
    print(frame.shape)
```

---

## Dependencies

| Package | Purpose |
|---|---|
| `PySpin` | FLIR Spinnaker Python API |
| `numpy` | Frame data arrays |
| `skvideo` | Video writing (requires FFMPEG) |
| `cupy` | GPU array support (optional) |
