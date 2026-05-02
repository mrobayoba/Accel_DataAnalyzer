# Accelerometer Data Analysis - IIS3DWB Sensor

## Overview

This project provides comprehensive analysis tools for 3-axis accelerometer data collected from the **IIS3DWB sensor** by STM32. The analysis includes time-domain visualization, frequency-domain analysis using FFT, spectrograms, and advanced spectral analysis techniques.

**Author**: Mateo Robayo B.  
**Institution**: Universidad Nacional de Colombia, Sede Medellín - Laboratorio de Instrumentación Electrónica (Bloque 58)

## Sensor Specifications

- **Sensor**: IIS3DWB 3-axis accelerometer (STM32 firmware)
- **Configuration**: FIFO continuous mode, WTM = 20, Sensitivity = ±4g
- **Resolution**:
  - Timestamp: 12.5 μs/LSB
  - Acceleration: 0.122 mg/LSB
- **Nominal ODR**: ~26 667 Hz

## Binary File Format

Recordings are stored as `.bin` files with the following layout:

```
BINARY FORMAT: 10 bytes per sample
Each sample: uint32_t timestamp(LSB), int16_t X, int16_t Y, int16_t Z
Timestamp: 1 LSB = 12.5us, Accel: 1 LSB = 0.122mg, +-4g
WTM=20 ODR~26667Hz file_index=<N>
DATA_START
<binary data — N × 10 bytes>
```

Each 10-byte record:

| Field | Type | Scale | Unit |
|-------|------|-------|------|
| timestamp | `uint32` little-endian | × 12.5 μs | seconds |
| accel X | `int16` little-endian | × 0.122 | mg |
| accel Y | `int16` little-endian | × 0.122 | mg |
| accel Z | `int16` little-endian | × 0.122 | mg |

## Features

### Interactive Visualizations
- **Time Series Analysis**: Plotly plots with zoom, pan, and hover capabilities
- **Frequency Spectrum (FFT)**: Interactive visualization with automatic peak annotation
- **Spectrograms**: Matplotlib spectrograms with Gouraud shading
- **Welch PSD**: Individual and combined power spectral density plots

### Analysis Capabilities
- Header metadata parsing (ODR, WTM, file index)
- Automatic sampling frequency detection from timestamps
- 32-bit timestamp wrap-around correction
- DC component removal and Hanning windowing
- Peak detection and dominant frequency ranking
- Large-dataset decimation for fast browser rendering

### Configurable Parameters
- Maximum frequency range for all spectral plots
- Colormap for spectrograms
- Plot dimensions and styling
- Per-feature sample limits (FFT, spectrogram)

## Installation

### Required Dependencies

```bash
pip install numpy pandas matplotlib scipy plotly
```

Alternatively, uncomment and run the first cell of the notebook:

```python
%pip install numpy pandas matplotlib scipy plotly
```

## Usage

### 1. Data Preparation

Place your `.bin` recording in the same directory as the notebook, **or** set an absolute path / Google Drive path in the configuration cell.

### 2. Configuration

Edit the configuration cell (cell 5):

```python
# Binary recording file
data_file = 'rec_A0.bin'

# Maximum frequency shown in all spectral plots (Hz)
USER_MAX_FREQ = 6300

# Spectrogram colormap
COLOR_MAP = 'inferno'   # 'viridis', 'plasma', 'inferno'
```

### 3. Google Drive (Colab only)

Uncomment and run the Google Drive cell to mount your Drive, then set `data_file` to the full path:

```python
data_file = '/content/drive/MyDrive/recordings/rec_A0.bin'
```

### 4. Running the Analysis

Execute cells in order:

1. **Package installation** — install dependencies (uncomment if needed)
2. **Google Drive mount** — optional, Colab only
3. **Library import & configuration** — imports and user settings
4. **Data loading** — parses the binary file, prints header metadata and dataset summary
5. **Time series** — interactive per-axis and combined plots
6. **FFT analysis** — frequency domain computation
7. **Frequency spectrum** — interactive FFT magnitude plots with peak labels
8. **Power Spectral Density** — Welch PSD + dominant frequency table
9. **Advanced spectral analysis** — spectrograms + Welch PSD overlay
10. **Comprehensive spectrum** — linear/log magnitude, peak detection, histogram

## File Structure

```
Accel_DataAnalyzer/
├── DataAnalyzer.ipynb   # Main analysis notebook
├── README.md            # This file
└── rec_*.bin            # Binary recordings (not tracked by git)
```

## Analysis Output

### Time Domain
- Per-axis interactive time series (decimated for display)
- Combined 3-axis matplotlib plot
- Statistical summary (mean, std, min, max, range) over the full dataset

### Frequency Domain
- Detected sampling frequency and Nyquist frequency
- FFT magnitude spectrum with top-5 peak annotations per axis
- Power Spectral Density (FFT-based)
- Dominant frequency table (top 5 per axis)

### Advanced Spectral
- **Spectrograms** (Matplotlib, Gouraud shading, dB scale)
- **Welch PSD** — individual subplots and combined overlay
- **Peak detection** — `scipy.signal.find_peaks` with height threshold
- **Peak frequency histogram** across all axes

## Key Parameters

| Parameter | Default | Description |
|-----------|---------|-------------|
| `USER_MAX_FREQ` | `6300` | Upper frequency limit for all plots (Hz) |
| `COLOR_MAP` | `'inferno'` | Spectrogram colormap |
| `PLOT_MAX_POINTS` | `10 000` | Max points rendered per axis in time-series |
| `FFT_MAX_SAMPLES` | `500 000` | Max samples used for FFT computation |
| `SPECTROGRAM_MAX_SAMPLES` | `200 000` | Max samples used per axis for spectrogram |

## Troubleshooting

| Problem | Cause | Fix |
|---------|-------|-----|
| `File not found` | Wrong path or filename | Check `data_file` in the configuration cell |
| `DATA_START marker not found` | Not a valid binary recording | Verify the file was produced by the IIS3DWB firmware |
| Slow rendering | Too many plot points | Lower `PLOT_MAX_POINTS` or `SPECTROGRAM_MAX_SAMPLES` |
| Plots not showing | Plotly not initialised | Re-run the imports cell |

## Contributing

This project is part of ongoing research at Universidad Nacional de Colombia. For questions, suggestions, or collaborations, please contact the Laboratorio de Instrumentación Electrónica (Bloque 58).

## License

Developed for academic and research purposes at Universidad Nacional de Colombia.
