# Novel pipeline for denoising EMG signals

STRD stands for Spatial-Temporal Residual Decoupling. It is an advanced, multi-stage signal processing pipeline developed specifically for multi-axis or multi-channel sensor data (such as EMG, accelerometers, or IMUs) to reduce noise while preserving the true signal characteristics.

STRD was designed to solve the "Filtering Paradox"—the issue where standard filters (like Butterworth or Moving Average) clean noise but distort the signal by rounding off sharp peaks or causing phase delays.

## The Three Stages of the STRD Pipeline
The STRD methodology decouples the noise and signal into three distinct layers:

### Stage 1: Adaptive Temporal Despiking
#### What it does: 
Scans the time series for sudden, extreme noise spikes (such as motion artifacts or loose electrode connections).

#### How it works: 
It uses a local window to calculate the rolling Z-score. If a point deviates significantly (e.g., >3 standard deviations), it is classified as a glitch and interpolated using local statistics.

### Stage 2: Spatial Decoupling (Differential Mode PCA)
#### What it does: 
Uses multi-channel correlation to separate "shared" true signals from independent background noise.

#### How it works: 
It applies Principal Component Analysis (PCA) to all columns together.

The dominant principal components (which represent the shared physical movement or muscle contraction) are smoothed using a zero-lag Savitzky-Golay filter.

The noise subspace is isolated and filtered more aggressively.

### Stage 3: Information-Weighted Recombination
#### What it does: 
Dynamically controls the "cleanliness" versus the "sharpness" of the signal.

#### How it works: 
It computes the local variance (activity level) of the signal:

When resting (low activity): The pipeline applies heavy smoothing to eliminate background electrical hiss or baseline wander.

When active (high activity): The pipeline reduces the smoothing level and re-injects raw signal residuals to preserve the peak amplitude of movements or muscle contractions.

## Why STRD is Highly Effective for Biomedical Data, EMG here?
### No Phase Shift (Zero-Lag): 
STRD uses symmetric, centered windows and forward-backward filtering to ensure that the output is perfectly aligned in time with the raw data.

### Maintains Burst Peaks: 
Traditional filters round off the sharp spikes of muscle bursts (motor unit action potentials). STRD’s "Smart Gate" keeps contraction peaks intact.

### Multivariate Capability: 
It takes advantage of data from multiple sensor channels simultaneously, analyzing how they correlate.
