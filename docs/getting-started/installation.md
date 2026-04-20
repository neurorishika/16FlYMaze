# Installation

This guide covers the complete installation of the SixteenY system on a Windows workstation connected to the behavioral rig.

---

## Prerequisites

Before installing the software, ensure you have the following:

### Operating System
- Windows 10 or Windows 11 (64-bit)

### Hardware Drivers & SDKs
- **NVIDIA GPU** with CUDA 11.x support (required for real-time tracking)
- **[FLIR Spinnaker SDK](https://www.flir.com/products/spinnaker-sdk/)** — Full SDK + Python Spinnaker wheel (`.whl`)
- **[FFMPEG](https://www.gyan.dev/ffmpeg/builds/)** — Added to system `PATH`
- **[CUDA Toolkit 11.6](https://developer.nvidia.com/cuda-toolkit-archive)** — Ensure `CUDA_PATH` environment variable is set

### Software
- **[Anaconda](https://www.anaconda.com/)** or **[Miniconda](https://docs.conda.io/en/latest/miniconda.html)**
- **Git** (optional, for cloning the repository)

---

## Step-by-Step Installation

### 1. Create a Conda Environment

Open Anaconda Prompt and run:

```bash
conda create -n sixteeny python=3.8
conda activate sixteeny
```

Register the environment as a Jupyter kernel (optional):

```bash
conda install -c anaconda ipykernel
python -m ipykernel install --user --name=sixteeny
```

### 2. Install Core Python Packages

```bash
pip install pyserial numpy matplotlib scikit-image scikit-video PyQt5 alicat
pip install pandas seaborn
```

### 3. Install CUDA-Accelerated Packages

```bash
conda install -c conda-forge cupy cudatoolkit=11.0
```

### 4. Install CUCIM (Windows)

CUCIM is used for GPU-accelerated image morphology. Install from source:

```bash
# Clone the cucim repository
git clone https://github.com/rapidsai/cucim
cd cucim/python/cucim
pip install . -v
```

### 5. Install the Spinnaker Python SDK

1. Download the **Spinnaker SDK** from [FLIR's Box](https://flir.app.boxcn.net/v/SpinnakerSDK)
2. Install the **Full Spinnaker SDK** (the .exe installer)
3. Unzip the **Python Spinnaker SDK** package
4. Install the wheel file:

```bash
cd <download_directory>
pip install spinnaker_python-*.whl --force-reinstall --user
```

### 6. Install FFMPEG

1. Download a build from [gyan.dev/ffmpeg/builds](https://www.gyan.dev/ffmpeg/builds/)
2. Extract to a directory (e.g., `C:\ffmpeg\`)
3. Add `C:\ffmpeg\bin` to your system `PATH`

### 7. Install Google API Client (Email Notifications)

```bash
pip install --upgrade google-api-python-client oauth2client
```

### 8. Configure Email Notifications

The system can send automated email notifications via the Gmail API.

1. Go to the [Google API Console](https://console.developers.google.com/apis/credentials)
2. Create an OAuth 2.0 client ID for a **Desktop application**
3. Download the `client_secret.json` file
4. Save it to `sixteeny/utils/client_secret.json`
5. Run the email authorization script from the repository root:

```bash
python test_and_authorize_email.bat
```

6. Log in to the lab Gmail account and approve the authentication flow

!!! warning "Credentials"
    Never commit your `client_secret.json` or OAuth token to version control.

### 9. Install the SixteenY Package

From the repository root directory:

```bash
pip install -e .
```

The `-e` flag installs in editable (development) mode, so changes to the source code take effect immediately.

---

## Setting Up Desktop Shortcuts

Run the provided batch script to create desktop shortcuts for all GUI applications:

```bat
initialize_shortcuts.bat
```

This creates shortcuts for:

- **16Y Project Manager**
- **16Y Rig Configurator**
- **16Y Mask Designer**
- **16Y Experimenter**

---

## Verifying the Installation

To confirm the installation is complete, activate the environment and check key imports:

```python
import sixteeny
import cupy as cp
import PySpin
import PyQt5
print("Installation OK")
```

If all imports succeed without errors, the installation is complete.

---

## Troubleshooting

??? question "CuPy import fails with CUDA error"
    Ensure the CUDA Toolkit version matches your CuPy build. Check with:
    ```bash
    nvcc --version
    python -c "import cupy; print(cupy.cuda.runtime.runtimeGetVersion())"
    ```
    Reinstall CuPy for your specific CUDA version if needed.

??? question "PySpin import fails"
    Make sure you installed the Spinnaker **Full SDK** (not just the camera viewer) and that the Python wheel was installed with `--user` flag.

??? question "FFMPEG not found"
    Ensure `C:\ffmpeg\bin` (or your FFMPEG path) is added to the system `PATH` environment variable, not just the user `PATH`.

??? question "Google authentication loop"
    Delete the cached credentials file (`~/.credentials/gmail-python-email-send.json`) and re-run the authorization script.
