# pyEAS — Tutorial and Quick Start

pyEAS is a small Python toolkit for generating and broadcasting emergency alert-style audio (SAME/OAME) and for experimenting with TTS-driven alert audio. This README is a step-by-step tutorial to get you from a fresh Linux system to running the graphical tools included in this repository.

This tutorial assumes you are running Linux (desktop or server). The project has been tested on Debian/Ubuntu, Fedora/RHEL, and Arch-based distributions. macOS and Windows are not officially supported.

Prerequisites

- Python 3.8 or newer (3.11 recommended)
- A working audio backend: ALSA, PulseAudio, or PipeWire
- System build tools for compiling Python extension dependencies (package names vary by distro)

1 — Set up a virtual environment (recommended)

Use a virtual environment to isolate project dependencies from system Python packages:

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
```

2 — Install system packages (select one)

Debian / Ubuntu / Debian-based

```bash
sudo apt-get update
sudo apt-get install -y python3-tk espeak-ng python3-pip python3-dev build-essential \
  libasound2-dev libjack-jackd2-dev libsndfile1
```

Fedora / RHEL-based

```bash
sudo dnf install -y python3-tkinter espeak-ng python3-pip python3-devel \
  development-tools alsa-lib-devel libsndfile
```

Arch-based

```bash
sudo pacman -Syu --noconfirm
sudo pacman -S --noconfirm --needed tk espeak-ng python-pip alsa-lib libsndfile base-devel
```

Notes:
- libsndfile (libsndfile1 on Debian/Ubuntu) is required by the soundfile Python package.
- If you run into permission or protected-system pip errors, continue inside the virtualenv rather than using the system pip.

3 — Install Python dependencies

Create a requirements.txt (optional) with these lines:

```
numpy
soundfile
sounddevice
requests
```

Install dependencies:

```bash
pip install -r requirements.txt
```

or install them directly:

```bash
pip install numpy soundfile sounddevice requests
```

4 — Configuration

The repository ships a sample config file at config.toml. Copy and edit it to set your preferred defaults (callsign, event codes, county codes, siren settings, external WAV file path, etc.):

```bash
cp config.toml.example config.toml || true
# edit config.toml with your preferred editor
```

If you already have a config.toml in the repository root, the applications will load it automatically.

5 — Running the GUIs and tools

This project contains several entry scripts that provide different functionality. From the repository root inside your activated virtualenv you can run these directly with Python.

- Full broadcast GUI (main application):

```bash
python proBugEAS.py
```

This opens the main GUI which includes SAME header generation, auto-config options, audio focus controls, and the EAS broadcast launcher.

- OAME SAME-like generator (small GUI):

```bash
python OAME.py
```

OAME provides a compact GUI focused on composing OAME-style packet bursts and selecting target zones.

- SAME header generator (standalone):

```bash
python SAME.py
```

SAME.py includes a small GUI and an encode_same_string() function you can import from other scripts.

Running headless / scripting

You can import modules from this repository into your own scripts. For example, to generate a SAME header string from Python:

```python
from SAME import encode_same_string
header = encode_same_string("TOR", ["allegheny_pa"], duration_hhmm="0100", originator="WXR", station_id="BUG/HTTP")
print(header)
```

6 — Quick example: generate a SAME header from the GUI

1. Launch proBugEAS.py or SAME.py.
2. In the SAME tab choose an event code (e.g. TOR - Tornado Warning).
3. Enter county keys (e.g. allegheny_pa or 042003) separated by commas.
4. Set duration (HHMM), originator (3 characters), and station ID.
5. Click Generate SAME Header. The generated header will appear in the output field and can be copied or saved.

7 — Troubleshooting

- PortAudio / sounddevice errors: ensure system audio libraries are installed (alsa, pulseaudio, or pipewire) and that the user running the program has access to the audio devices. On many distros adding your user to the "audio" group helps.
- soundfile installation fails: confirm libsndfile is present (libsndfile1 on Debian/Ubuntu).
- GUI issues with tkinter: install the tkinter package for your distribution (python3-tk or python3-tkinter).
- PulseAudio / PipeWire control: proBugEAS attempts to use pactl for audio focus. Ensure pactl is available (install pulseaudio-utils or pipewire-pulse packages) if you want automatic audio focus control.

8 — Development notes and tests

- The codebase uses conventional tkinter-based GUIs. You can run individual modules to test components (SAME.py, OAME.py).
- To run unit-style tests or to script header generation, import functions from SAME.py and call encode_same_string() with appropriate parameters.

9 — Contributing

Contributions are welcome. Please open issues or PRs with reproducible steps and logs. When submitting code changes, include a short description of what the change fixes or adds and provide minimal reproduction steps.

10 — License

This repository includes a LICENSE file. Review it for project licensing details.

If you want any section expanded (examples for scripting, audio debugging steps, or more detailed config guidance), tell me which area to expand and I will update the README accordingly.
