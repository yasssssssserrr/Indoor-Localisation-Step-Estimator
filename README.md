# Indoor Localisation (Step Estimator)
A compact, reproducible pipeline to estimate pedestrian steps and 2D tracks from smartphone accelerometer and gyroscope data.
This repo implements the minimal “Step Estimator” described in the course material and plots the resulting trajectory. It is designed to run either in Google Colab or locally with Python.

## Quick start
### Option A — Google Colab (recommended)
- Open 03_IndoorLocalisation/StepEstimator.ipynb in Colab.
- Mount your Drive (cell 1 does this) and set the base path to where PhyphoxData/ lives.
- Run all cells top-to-bottom. The notebook will:
- load paired acc*.csv / gyro*.csv files,
- detect steps from accelerometer magnitude,
- estimate heading from the gyroscope,
- integrate a constant step length to draw the 2D track.

### Option B — Local Python
- Create a virtual environment and install dependencies:
python -m venv .venv
source .venv/bin/activate  # Windows: .venv\Scripts\activate
pip install -r requirements.txt
- Open StepEstimator.ipynb in JupyterLab / VS Code and edit the base path in the first cell to point to PhyphoxData/.
- Run all cells.
> Note: The notebook expects Phyphox CSV headers such as
Time (s), Acceleration x (m/s^2), Acceleration y (m/s^2), … and
Time (s), Gyroscope x (rad/s), Gyroscope y (rad/s), …

## Requirements
- Python 3.9+
- Packages: numpy, pandas, matplotlib, scipy

## Data
- Files come from the Phyphox app (Android/iOS). Each experiment has one accN.csv and one gyroN.csv recorded over the same time window.
- Example files are under 03_IndoorLocalisation/PhyphoxData/.
- The notebook trims an initial idle segment and an end margin to reduce boundary effects.
### Expected CSV columns
- Accelerometer: Time (s), Acceleration x/y/z (m/s^2), Absolute acceleration (m/s^2)
- Gyroscope: Time (s), Gyroscope x/y/z (rad/s), Absolute (rad/s)

## Known limitations / gotchas
- Phone orientation drift: R(t) uses only accelerometer gravity; fast movements and lateral acceleration can bias the heading.
- Constant step length is a simplification; real λ varies with gait/speed.
- Start/stop trimming is important: keep 5–10 s idle at the start for gravity initialisation, and remove end artifacts.
- Sampling mismatch between ACC and GYRO must be handled by masking to a common time window.
