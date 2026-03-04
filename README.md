<div align="center">
  <h1>Respiratory Rate Estimation from PPG</h1>
  
  <p>
    Estimating respiratory rate from Photoplethysmogram signals using a digital resonator and FFT
  </p>

<!-- Badges -->
<p>
  <a href="https://github.com/kvsh2050/ppg-respiratory-rate/graphs/contributors">
    <img src="https://img.shields.io/github/contributors/kvsh2050/ppg-respiratory-rate" alt="contributors" />
  </a>
  <a href="">
    <img src="https://img.shields.io/github/last-commit/kvsh2050/ppg-respiratory-rate" alt="last update" />
  </a>
  <a href="https://github.com/kvsh2050/ppg-respiratory-rate/network/members">
    <img src="https://img.shields.io/github/forks/kvsh2050/ppg-respiratory-rate" alt="forks" />
  </a>
  <a href="https://github.com/kvsh2050/ppg-respiratory-rate/stargazers">
    <img src="https://img.shields.io/github/stars/kvsh2050/ppg-respiratory-rate" alt="stars" />
  </a>
  <a href="https://github.com/kvsh2050/ppg-respiratory-rate/issues/">
    <img src="https://img.shields.io/github/issues/kvsh2050/ppg-respiratory-rate" alt="open issues" />
  </a>
  <a href="https://github.com/kvsh2050/ppg-respiratory-rate/blob/master/LICENSE">
    <img src="https://img.shields.io/github/license/kvsh2050/ppg-respiratory-rate.svg" alt="license" />
  </a>
</p>

<h4>
  <a href="https://github.com/kvsh2050/ppg-respiratory-rate">View Demo</a>
  <span> · </span>
  <a href="https://github.com/kvsh2050/ppg-respiratory-rate">Documentation</a>
  <span> · </span>
  <a href="https://github.com/kvsh2050/ppg-respiratory-rate/issues/">Report Bug</a>
  <span> · </span>
  <a href="https://github.com/kvsh2050/ppg-respiratory-rate/issues/">Request Feature</a>
</h4>
</div>

<br />

<!-- Table of Contents -->
# :notebook_with_decorative_cover: Table of Contents

- [About the Project](#star2-about-the-project)
  * [Features](#dart-features)
- [Methodology](#microscope-methodology)
- [Dataset](#floppy_disk-dataset)
- [Getting Started](#toolbox-getting-started)
  * [Prerequisites](#bangbang-prerequisites)
  * [Installation](#gear-installation)
  * [Run Locally](#running-run-locally)
- [Results](#chart_with_upwards_trend-results)
- [Reference Paper](#books-reference-paper)
- [Contributing](#wave-contributing)
- [License](#warning-license)
- [Contact](#handshake-contact)
- [Acknowledgements](#gem-acknowledgements)


<!-- About the Project -->
## :star2: About the Project

This project implements a **robust algorithm** to estimate **respiratory rate (RR)** from **Photoplethysmogram (PPG)** signals. The technique leverages a **digital resonator filter** followed by **frequency-domain analysis (FFT)** to extract the dominant respiratory frequency — enabling non-invasive, wearable-ready respiration monitoring.

### :dart: Features

- Bandpass filtering to isolate the respiratory frequency band (0.1–0.4 Hz)
- **Digital resonator** centered at 0.25 Hz for signal enhancement
- **FFT-based** dominant frequency extraction
- Outputs respiratory rate directly in **breaths per minute (BPM)**
- Adjustable sampling rate — defaults to 100 Hz


<!-- Methodology -->
## :microscope: Methodology

The pipeline processes a raw PPG signal through four stages:

**1. Load** — Raw PPG data sampled at 100 Hz (`.mat` or `.csv`)

**2. Bandpass Filter** — Isolate the 0.1–0.4 Hz respiratory frequency band

**3. Digital Resonator** — Enhance the dominant respiratory component centered at 0.25 Hz:

```
y[n] = 2·cos(w0)·y[n-1] − y[n-2] + x[n] − 2·cos(w0)·x[n-1] + x[n-2]
```

**4. FFT + Peak Detection** — Identify the dominant frequency and convert to BPM

```python
# Design digital resonator
w0 = 2 * np.pi * 0.25 / sampling_rate
a = [1, -2 * np.cos(w0), 1]
b = [1, -2 * np.cos(w0), 1]
resonated_signal = signal.lfilter(b, a, bandpassed_signal)

# Apply FFT and extract respiratory rate
spectrum = np.fft.fft(resonated_signal)
freqs = np.fft.fftfreq(len(spectrum), d=1/sampling_rate)
resp_band = (freqs > 0.1) & (freqs < 0.4)
peak_freq = freqs[resp_band][np.argmax(np.abs(spectrum[resp_band]))]
respiratory_rate = peak_freq * 60
```


<!-- Dataset -->
## :floppy_disk: Dataset

| Field           | Details                                      |
|-----------------|----------------------------------------------|
| **Source**      | PhysioNet or simulated PPG datasets          |
| **Format**      | `.mat` / `.csv`                              |
| **Sampling Rate**| 100 Hz (adjustable in config)               |
| **Frequency Band**| 0.1–0.4 Hz (typical respiration range)    |


<!-- Getting Started -->
## :toolbox: Getting Started

### :bangbang: Prerequisites

Python 3.8+ and the following packages:

```bash
pip install numpy scipy matplotlib pandas
```

### :gear: Installation

Clone the repository:

```bash
git clone https://github.com/kvsh2050/ppg-respiratory-rate.git
cd ppg-respiratory-rate
pip install -r requirements.txt
```

### :running: Run Locally

Run the estimation script directly:

```bash
python estimate_rr.py
```

Or open the Jupyter notebook:

```bash
jupyter notebook Respiratory_Rate_Estimation.ipynb
```


<!-- Results -->
## :chart_with_upwards_trend: Results

The implemented method consistently estimates respiratory rate with high accuracy, even in the presence of mild noise and signal variation.

**Sample console output:**

```
Peak frequency in Hz: 0.28
Estimated Respiratory Rate: 16.8 BPM
```

The FFT spectrum plot highlights the dominant frequency peak within the 0.1–0.4 Hz respiratory band. Intermediate signals (bandpassed, resonated) can also be visualized for step-by-step analysis.


<!-- Reference Paper -->
## :books: Reference Paper

This implementation is based on:

> **"An Automated Algorithm for Estimating Respiration Rate from PPG Signals"**  
> *K. Manojkumar, S. Boppu, M. Sabarimalai Manikandan*  
> Published in: **IEEE Sensors Journal**  
> DOI: *(add here if available)*

The paper proposes a low-complexity digital resonator combined with spectral analysis for efficient RR estimation from wearable sensors.


<!-- Contributing -->
## :wave: Contributing

Contributions are welcome! See `contributing.md` to get started.


<!-- License -->
## :warning: License

Distributed under the MIT License. See `LICENSE.txt` for more information.


<!-- Contact -->
## :handshake: Contact

Kavya — [@kvsh2050](https://github.com/kvsh2050) — email@email_client.com

Project Link: [https://github.com/kvsh2050/ppg-respiratory-rate](https://github.com/kvsh2050/ppg-respiratory-rate)


<!-- Acknowledgements -->
## :gem: Acknowledgements

- Manojkumar et al. — algorithm and methodology from their IEEE Sensors Journal paper
- [PhysioNet](https://physionet.org/) — open-access physiological datasets
- [Shields.io](https://shields.io/)
