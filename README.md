# pyEAS

A small Python project for audio / TTS interactions. This README provides clearer installation instructions, a recommended Python workflow, and troubleshooting notes.

## Requirements

- Linux (desktop/server). This project has been tested on Debian/Ubuntu, Fedora/RHEL, and Arch-based distributions. macOS and Windows are not officially supported.
- Python 3.8+ (3.11 recommended)
- A working audio backend (ALSA / PulseAudio / PipeWire)

## Recommendation

Use a Python virtual environment to avoid modifying system packages:

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
```

## System dependencies

Install the system packages for your distribution, then install the Python dependencies.

### Debian / Ubuntu / Debian-based

```bash
#!/bin/bash
set -e
sudo apt-get update
sudo apt-get install -y \
  python3-tk espeak-ng python3-pip python3-dev build-essential \
  libasound2-dev libjack-jackd2-dev libsndfile1

# inside an activated venv (or omit venv if you know what you're doing):
python -m pip install --upgrade pip
python -m pip install numpy soundfile sounddevice requests
```

Notes:
- `libsndfile1` is required by the `soundfile` Python package.
- If you encounter pip's system package protections, use a virtualenv instead of forcing system pip.

### Fedora / RHEL-based

```bash
#!/bin/bash
set -e
sudo dnf install -y python3-tkinter espeak-ng python3-pip python3-devel \
  development-tools alsa-lib-devel libsndfile

python -m pip install --upgrade pip
python -m pip install numpy soundfile sounddevice requests
```

### Arch-based

```bash
#!/bin/bash
set -e
sudo pacman -Syu --noconfirm
sudo pacman -S --noconfirm --needed tk espeak-ng python-pip alsa-lib libsndfile base-devel

python -m pip install --upgrade pip
python -m pip install numpy soundfile sounddevice requests
```

## Python dependencies

If you prefer, create a `requirements.txt` with:

```
numpy
soundfile
sounddevice
requests
```

and install with:

```bash
pip install -r requirements.txt
```

## Running

Run the project's Python entrypoint. Example:

```bash
python main.py
```

(Replace `main.py` with the actual entrypoint in this repository.)

## Troubleshooting

- If you see errors related to PortAudio / sounddevice, ensure your system audio libraries are installed (`alsa`, `pulseaudio` or `pipewire`) and you are not running in a container without audio access.
- For permission errors with audio devices, make sure your user is in the `audio` group (if your distro uses one) or run the program without sudo.
- If `soundfile` fails to install, confirm `libsndfile` is installed (see system deps above).

## Contributing

Contributions welcome. Please open issues or PRs with reproducible steps and log output.

## License

Add a LICENSE file (for example: MIT) and include the license name here.
