# Accelerometer Data Analysis - IIS3DWB Sensor

## Overview

This project provides comprehensive analysis tools for 3-axis accelerometer data collected from the **IIS3DWB sensor** by STM32. The analysis includes time-domain visualization, frequency-domain analysis using FFT, spectrograms, and advanced spectral analysis techniques.

**Author**: Mateo Robayo B.  
**Institution**: Universidad Nacional de Colombia, Sede Medellín - Laboratorio de Instrumentación Electrónica (Bloque 58)

## Sensor Specifications

- **Sensor**: IIS3DWB 3-axis accelerometer by STM32
- **Configuration**: FIFO continuous mode WTM = 20, Sensitivity = ±4g
- **Resolution**: 
  - Timestamp: 12.5μs/LSB
  - Acceleration: 0.122mg/LSB
- **Data Format**: Timestamp(μs), accelX(mg), accelY(mg), accelZ(mg)

## Features

### 📊 Interactive Visualizations
- **Time Series Analysis**: Interactive Plotly plots with zoom, pan, and hover capabilities
- **Frequency Spectrum Analysis**: Interactive FFT visualization with peak detection
- **Spectrograms**: High-quality matplotlib spectrograms with Gouraud shading
- **Power Spectral Density**: Welch's method for improved frequency resolution

### 🔧 Analysis Capabilities
- Automatic sampling frequency detection
- DC component removal and signal preprocessing
- Window functions (Hanning) for spectral leakage reduction
- Peak detection and dominant frequency analysis
- Time-frequency representation via spectrograms
- Combined multi-axis visualization

### ⚙️ Configurable Parameters
- Maximum frequency range for analysis
- Colormap selection for spectrograms
- Plot appearance and styling options
- Interactive vs. static plot selection

## Installation

### Required Dependencies

```bash
pip install numpy pandas matplotlib scipy plotly
```

### Alternative Installation
Run the first cell of the notebook to automatically install all required packages:

```python
%pip install numpy pandas matplotlib scipy plotly
```

## Usage

### 1. Data Preparation
Place your accelerometer data file in the same directory as the notebook. The data file should have the following format:

```
# Header lines (first 6 lines are automatically skipped)
timestamp_us  accel_x_mg  accel_y_mg  accel_z_mg
12500000      102.5       -45.3       980.2
12512500      98.7        -42.1       985.6
...
```

### 2. Configuration
Before running the analysis, configure the parameters in the setup cell:

```python
# SET FILE NAME HERE
data_file = 'your_data_file.txt'  # Replace with your data file

# USER-CONTROLLABLE PLOTTING PARAMETERS
USER_MAX_FREQ = 1600  # Maximum frequency for analysis (Hz)
COLOR_MAP = 'inferno'  # Colormap for spectrograms
```

### 3. Running the Analysis
Execute the notebook cells in order:

1. **Package Installation** - Install required dependencies
2. **Library Import & Configuration** - Set up the analysis environment
3. **Data Loading** - Load and preprocess accelerometer data
4. **Time Series Analysis** - Visualize acceleration data over time
5. **FFT Analysis** - Frequency domain analysis
6. **Frequency Spectrum Visualization** - Interactive frequency plots
7. **Power Spectral Density** - PSD analysis using Welch's method
8. **Advanced Spectral Analysis** - Spectrograms and enhanced visualizations
9. **Comprehensive Analysis** - Peak detection and frequency distribution

## File Structure

```
METRO-UN/
├── DataAnalyzer.ipynb          # Main analysis notebook
├── README.md                   # This file
├── record_1.txt               # Sample data file (if available)
└── example_data.txt           # Example dataset for testing
```

## Analysis Output

### Time Domain Analysis
- Statistical summary (mean, std, min, max, range)
- Interactive time series plots for all three axes
- Combined view of all axes

### Frequency Domain Analysis
- Sampling frequency and Nyquist frequency calculation
- FFT magnitude and phase spectrum
- Power spectral density analysis
- Peak frequency detection and ranking

### Advanced Spectral Analysis
- **Spectrograms**: Time-frequency representation with high-quality shading
- **Welch PSD**: Individual and combined power spectral density plots
- **Peak Detection**: Automated identification of dominant frequencies
- **Frequency Distribution**: Histogram of detected peaks

## Customization Options

### Plot Configuration
```python
PLOT_OPTIONS = {
    'width': 1200,        # Plot width
    'height': 800,        # Plot height
    'template': 'plotly_white',  # Plotly theme
    'line_width': 1.5,    # Line thickness
    'font_size': 12       # Font size
}
```

### Analysis Parameters
- `USER_MAX_FREQ`: Maximum frequency for analysis (Hz)
- `COLOR_MAP`: Colormap for spectrograms ('viridis', 'plasma', 'inferno')
- `LINE_ALPHA`: Transparency for overlaid plots
- `FIGURE_DPI`: Resolution for matplotlib figures

## Technical Details

### Signal Processing
- **Windowing**: Hanning window applied to reduce spectral leakage
- **Detrending**: DC component removal before analysis
- **Overlap**: 50% overlap in spectrogram calculations
- **Frequency Limiting**: User-defined maximum frequency for focused analysis

### Visualization Technologies
- **Interactive Plots**: Plotly for time series, frequency spectra, and PSD analysis
- **Static Plots**: Matplotlib for spectrograms with superior shading quality
- **Hybrid Approach**: Combines best features of both libraries

## Troubleshooting

### Common Issues

1. **File Not Found Error**
   - Ensure your data file is in the correct directory
   - Check the filename in the `data_file` variable

2. **Memory Issues with Large Files**
   - Reduce the `USER_MAX_FREQ` parameter
   - Consider data downsampling for very large datasets

3. **Plot Display Issues**
   - Ensure Plotly is properly installed
   - Try refreshing the notebook kernel

### Performance Optimization

- For large datasets, consider reducing the frequency range (`USER_MAX_FREQ`)
- Adjust spectrogram parameters (`nperseg`, `noverlap`) for different time-frequency resolutions
- Use static plots instead of interactive ones for better performance with very large datasets

## Contributing

This project is part of ongoing research at Universidad Nacional de Colombia. For questions, suggestions, or collaborations, please contact the Laboratorio de Instrumentación Electrónica.

## License

This project is developed for academic and research purposes at Universidad Nacional de Colombia.

---

**Note**: This analysis tool is specifically designed for IIS3DWB accelerometer data but can be adapted for other 3-axis accelerometer sensors with similar data formats.
