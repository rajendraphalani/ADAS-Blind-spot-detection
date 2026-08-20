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

Detected targets are associated with existing tracks using a gating-based tracking procedure.

🎯 Target Tracking

Target tracking is an important component of the BSD system.

The system uses measurements such as:

Range
Velocity
Angle

to determine whether a newly detected target belongs to an existing track.

If the measured target is sufficiently close to the predicted position of an existing track, it is associated with that track.

Otherwise, a new track can be created.

The research paper also discusses the use of α-β and α-β-γ tracking filters for predicting target movement.

🔬 Radar Specifications

The radar system described in the research paper includes:

Parameter	Value
Operating Frequency	76.5 GHz
Tx Power	10 dBm
Tx Antenna Gain	7 dBi
Rx Antenna Gain	12 dBi
1D FFT Points	256
2D FFT Points	256
Detection Range	1–6 m
Range Accuracy	0.5 m
Range Resolution	0.5 m
Velocity Detection	±187 km/h
Velocity Accuracy	1.46 km/h
Velocity Resolution	1.46 km/h
Azimuth FOV	±75°
Update Time	25 ms
📊 Dataset

The original research paper describes simulations and real-world vehicle testing, but an accompanying public machine-learning dataset is not specified in the paper.

For an AI-based implementation, a public automotive radar dataset can be used instead.

Recommended Dataset

RadarScenes

RadarScenes provides automotive radar data containing information such as:

Radar detections
Object positions
Radial velocity
Object classes
Tracking information

The dataset can be adapted for blind-spot detection by defining left and right blind-spot regions using the radar coordinates.

Dataset Processing

A possible preprocessing pipeline is:

Raw Radar Dataset
       ↓
Remove Invalid Measurements
       ↓
Coordinate Transformation
       ↓
Define Blind-Spot Region
       ↓
Identify Objects in Blind Spot
       ↓
Assign Labels
       ↓
Training / Validation / Testing

Example:

Object Position
      ↓
Is object inside left/right BSD zone?
      ↓
      ├── YES → Blind Spot = 1
      │
      └── NO  → Blind Spot = 0
🤖 Proposed AI Extension

The research paper identifies intelligent target prioritization as an area for future development.

This project can extend the research by adding an AI-based threat classification layer.

Proposed Architecture
FMCW Radar
     ↓
Radar Signal Processing
     ↓
Range / Velocity / Angle
     ↓
Target Tracking
     ↓
Feature Extraction
     ↓
AI Model
     ↓
Threat Classification
     ↓
┌───────────────────────────┐
│ SAFE                      │
│ CAUTION                   │
│ HIGH RISK                 │
└───────────────────────────┘
     ↓
Driver Alert

Possible input features:

Distance
Relative velocity
Angle
Time-to-collision
Target position
Target movement direction
Track duration
⚠️ Threat Detection

A simple rule-based system could initially be used:

IF vehicle is inside blind-spot zone
AND vehicle is approaching
AND relative distance < threshold
THEN
    HIGH RISK
    → Warning ON

A future AI model could replace these manually selected thresholds.

Possible models include:

Random Forest
XGBoost
SVM
Neural Network
LSTM
Transformer-based tracking models
🧪 Testing

The research paper evaluated the developed BSD radar system in multiple scenarios involving:

Vehicles
Motorcycles
Cyclists
Pedestrians

The warning system was activated when a target entered the defined BSD region and deactivated when the target left the region.

The radar system was installed on a vehicle for performance evaluation in driving environments.

⚠️ Limitations

The research paper identifies some limitations.

In complex driving environments, the system can experience target-tracking failures, particularly when:

Target vehicles are moving at high speed.
Multiple vehicles are present.
Driving conditions are irregular.
Several targets need to be tracked simultaneously.

The paper suggests using AI to determine the priority of target vehicles based on their threat level and allocate tracking resources accordingly.

This provides an opportunity for further research.

🚀 Future Scope
1. AI-Based Target Prioritization

Use machine learning to determine which detected vehicle represents the greatest threat.

2. Camera + Radar Fusion

Combine radar with a camera:

Radar → Distance + Velocity + Angle
Camera → Vehicle Classification
                 ↓
            Sensor Fusion
                 ↓
          Threat Assessment
3. Motorcycle Detection

Improve detection of smaller and faster vehicles such as motorcycles.

4. Multi-Target Tracking

Improve tracking when several vehicles are simultaneously present.

5. Time-to-Collision Estimation

Estimate the remaining time before a potential collision.

6. Real-Time Embedded System

Deploy the complete system on an embedded platform for real-time automotive use.

📁 Suggested Project Structure
blind-spot-detection/
│
├── README.md
│
├── dataset/
│   ├── raw/
│   ├── processed/
│   └── labels/
│
├── src/
│   ├── preprocessing/
│   ├── radar_processing/
│   ├── tracking/
│   ├── blind_spot_detection/
│   └── ai_model/
│
├── models/
│
├── notebooks/
│
├── results/
│
├── requirements.txt
│
└── main.py
🛠️ Technologies

Possible technologies for implementation:

Python
NumPy
Pandas
SciPy
OpenCV
Matplotlib
Scikit-learn
PyTorch / TensorFlow
MATLAB
FMCW Radar
CAN Communication
📚 Research Paper

Kim, W., Yang, H., & Kim, J. (2023).

"Blind Spot Detection Radar System Design for Safe Driving of Smart Vehicles."

Applied Sciences, 13, 6147.

DOI:

https://doi.org/10.3390/app13106147

The paper presents the FMCW radar principle, radar signal processing, target tracking, tracking filters, radar hardware design, and real-world vehicle testing.

📌 Conclusion

This project uses FMCW radar technology as the foundation for an automotive Blind Spot Detection system.

The radar detects surrounding objects and extracts their:

Range + Velocity + Angle

which are then processed through target detection and tracking algorithms.

The research can be further extended by introducing machine learning for:

Threat prediction
Target prioritization
Multi-object tracking
Collision-risk estimation
Intelligent driver warnings

The ultimate goal is to develop a reliable, real-time and intelligent BSD system suitable for Advanced Driver Assistance Systems (ADAS)
