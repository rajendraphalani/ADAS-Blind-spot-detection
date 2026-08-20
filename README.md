# FMCW Radar-Based Blind Spot Detection System

## 📌 Overview

This project is based on the research paper:

**"Blind Spot Detection Radar System Design for Safe Driving of Smart Vehicles"**

by Wantae Kim, Heejin Yang, and Jinhong Kim, published in *Applied Sciences* in 2023.

The project focuses on developing a **Blind Spot Detection (BSD) system using Frequency Modulated Continuous Wave (FMCW) radar**.

The system detects surrounding vehicles and determines their:

- Distance (Range)
- Relative Velocity
- Angle
- Position
- Movement

The detected information can then be used to determine whether a vehicle has entered the blind-spot region and generate a warning for the driver.

The research paper describes a radar system that was integrated into a vehicle and tested in real-world driving environments.

---

## 🎯 Objectives

The main objectives of this project are:

1. Detect vehicles in the blind-spot region.
2. Estimate the distance between the host vehicle and surrounding objects.
3. Determine the relative velocity of detected objects.
4. Determine the angle/position of surrounding vehicles.
5. Track multiple targets simultaneously.
6. Reduce false detections using signal processing and tracking.
7. Generate an alert when a vehicle enters a dangerous blind-spot region.
8. Explore the possibility of using AI for intelligent target prioritization.

---

## 🚗 Blind Spot Detection

A vehicle's blind spot is an area around the vehicle that cannot be properly observed using conventional mirrors.

During a lane-change maneuver, another vehicle in this region can create a collision risk.

The proposed system continuously monitors the surrounding area using FMCW radar.

### Basic Working

```text
FMCW Radar
     ↓
Transmit Chirp Signal
     ↓
Signal Reflected by Vehicle
     ↓
Radar Receiver
     ↓
ADC
     ↓
1D FFT
     ↓
2D FFT
     ↓
Range-Doppler Map
     ↓
Target Detection
     ↓
Target Tracking
     ↓
Blind Spot Decision
     ↓
Driver Warning
