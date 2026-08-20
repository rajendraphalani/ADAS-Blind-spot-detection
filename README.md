The radar system uses four input channels. The received signals are converted using ADC and processed using FFT-based signal processing.

🧠 Signal Processing
1. ADC Conversion

The received analog radar signals are converted into digital samples.

2. Windowing

Windowing is applied before FFT processing to improve the frequency spectrum and reduce unwanted side lobes.

3. 1D FFT

The first FFT is used to extract frequency information associated with the target range.

4. 2D FFT

A second FFT is used to obtain additional information related to target velocity and angle.

5. Range-Doppler Mapping

The processed radar data is represented using a Range-Doppler Map (RDM).

This helps identify targets based on:

Range   → Distance from vehicle
Doppler → Relative velocity
6. Peak Detection

Significant peaks in the processed radar signal are detected as potential targets.

7. Target Tracking

# ADAS Blind-Spot Detection

An automotive blind-spot detection (BSD) research project based on FMCW radar signal processing, target tracking, and a proposed machine-learning threat assessment layer.

> **Project status:** Research and implementation blueprint. This repository currently contains the project documentation and source research paper; a runnable radar-processing pipeline, trained model, and public dataset are not included yet.

## Overview

Blind-spot detection is an Advanced Driver Assistance System (ADAS) capability that warns a driver when another road user is present in a difficult-to-see area beside or behind the vehicle. This project defines a radar-first approach that estimates each target's:

- Range: distance from the vehicle
- Radial velocity: relative approach or recession speed
- Azimuth angle: horizontal position relative to the vehicle

These measurements are used for detection, tracking, blind-spot-zone classification, and eventually threat prioritization.

## System Architecture

```text
FMCW radar
    |
    v
ADC sampling -> Windowing -> 1D FFT -> 2D FFT
    |
    v
Range-Doppler map -> Peak detection -> Target measurements
    |
    v
Target tracking -> Feature extraction -> Threat assessment
    |
    v
SAFE / CAUTION / HIGH RISK -> Driver warning
```

### Signal-processing pipeline

1. **ADC conversion:** Convert received analog radar signals into digital samples.
2. **Windowing:** Reduce spectral leakage and unwanted sidelobes before FFT processing.
3. **1D FFT:** Extract frequency information associated with target range.
4. **2D FFT:** Extract Doppler information associated with target velocity; antenna-channel data can also support angle estimation.
5. **Range-Doppler mapping:** Represent target energy by range and relative velocity.
6. **Peak detection:** Identify significant responses as candidate targets.
7. **Tracking:** Associate measurements with existing tracks using a gating-based procedure, or start a new track when no suitable match exists.

The reference design also discusses alpha-beta and alpha-beta-gamma filters for predicting target motion.

## Reference Radar Specifications

The following values are reported by the reference research paper and describe the reference system, not a confirmed hardware configuration for this repository.

| Parameter | Value |
| --- | --- |
| Operating frequency | 76.5 GHz |
| Transmit power | 10 dBm |
| Transmit antenna gain | 7 dBi |
| Receive antenna gain | 12 dBi |
| 1D FFT points | 256 |
| 2D FFT points | 256 |
| Detection range | 1-6 m |
| Range accuracy | 0.5 m |
| Range resolution | 0.5 m |
| Velocity detection range | +/-187 km/h |
| Velocity accuracy | 1.46 km/h |
| Velocity resolution | 1.46 km/h |
| Azimuth field of view | +/-75 degrees |
| Update time | 25 ms |

## Dataset Strategy

The reference paper describes simulation and real-world vehicle testing, but does not specify an accompanying public machine-learning dataset. A future implementation can use [RadarScenes](https://radar-scenes.com/) or another appropriately licensed automotive radar dataset.

Recommended preprocessing:

```text
Raw radar data
    -> Remove invalid measurements
    -> Transform coordinates into the vehicle frame
    -> Define left and right blind-spot zones
    -> Identify objects inside each zone
    -> Assign labels
    -> Split into training, validation, and test sets
```

At the labeling stage, an object is marked as a blind-spot target when its transformed position lies within the selected left or right BSD region. Zone boundaries, class definitions, and labeling thresholds must be documented with the experiment so results remain reproducible.

## Threat Assessment

A rule-based baseline can be implemented before training an AI model:

```text
IF target is inside a blind-spot zone
AND target is approaching
AND distance is below the configured threshold
THEN issue a high-risk warning
```

Candidate model inputs include distance, relative velocity, angle, time to collision, target position, movement direction, and track duration. Possible model families include Random Forest, XGBoost, SVM, neural networks, LSTM models, and transformer-based tracking models. The baseline should remain available for comparison with learned models.

## Proposed Project Structure

```text
ADAS-Blind-spot-detection/
|-- README.md
|-- dataset/
|   |-- raw/
|   |-- processed/
|   `-- labels/
|-- src/
|   |-- preprocessing/
|   |-- radar_processing/
|   |-- tracking/
|   |-- blind_spot_detection/
|   `-- ai_model/
|-- models/
|-- notebooks/
|-- results/
|-- requirements.txt
`-- main.py
```

This structure is proposed for the implementation phase. It does not imply that those directories or files currently exist.

## Technology Stack

The planned implementation may use:

- Python for the processing and machine-learning pipeline
- NumPy, SciPy, and Pandas for numerical and tabular data processing
- Matplotlib for signal and result visualization
- scikit-learn, PyTorch, or TensorFlow for model development
- OpenCV for camera processing in a future sensor-fusion phase
- MATLAB for algorithm prototyping and comparison
- FMCW radar hardware and CAN communication for vehicle integration

No runtime dependencies or installation command are published yet because the application code has not been added to the repository.

## Evaluation Plan

The completed system should be evaluated using recorded or simulated scenarios containing vehicles, motorcycles, cyclists, and pedestrians. Evaluation should include:

- Detection precision, recall, and missed-target rate
- False-warning rate and warning latency
- Range, velocity, and angle estimation error
- Track continuity and identity switches
- Performance with multiple simultaneous targets
- Runtime and update rate against the 25 ms reference target
- Results separated by object type, traffic density, and driving condition

The warning should activate when a target enters the configured BSD region and deactivate after it leaves, subject to any persistence or hysteresis rules defined by the implementation.

## Known Limitations and Safety

Radar tracking can degrade when targets move at high speed, several objects are present, driving conditions are irregular, or multiple targets must be maintained simultaneously. Smaller and faster objects, including motorcycles, may require dedicated validation.

This is a research project and must not be used as a safety-critical driver-warning system without extensive hardware-in-the-loop testing, road testing, failure analysis, and compliance review. A prototype warning must never be treated as a substitute for the driver's observation or judgment.

## Roadmap

1. Add a reproducible dataset download and preprocessing pipeline.
2. Implement the radar signal-processing and range-Doppler visualization stages.
3. Add peak detection, angle estimation, and multi-target tracking.
4. Establish the rule-based BSD baseline and automated evaluation metrics.
5. Train and compare threat-classification models.
6. Add time-to-collision estimation and target prioritization.
7. Investigate camera-radar fusion for object classification.
8. Validate on embedded hardware with real-time constraints and CAN integration.

## Research Reference

Kim, W., Yang, H., & Kim, J. (2023). *Blind Spot Detection Radar System Design for Safe Driving of Smart Vehicles*. **Applied Sciences, 13**, 6147. [https://doi.org/10.3390/app13106147](https://doi.org/10.3390/app13106147)

The paper provides the foundation for the FMCW radar approach, processing chain, radar specifications, tracking methods, and vehicle testing scenarios described here.

## Contributing

Contributions should include a clear description of the change, reproducible experiment settings, dataset and model provenance, and relevant evaluation results. Do not commit private, restricted, or unlicensed driving data.

## License

No license has been declared for this repository yet. Add a license before distributing source code, datasets, trained models, or derivative work.
Combine radar with a camera:
