# aerofc-avionics
# AeroFC — Flight Computer & Avionics Engineering Testbench

[![C++ Standard](https://img.shields.io/badge/C%2B%2B-20-blue.svg)](https://en.cppreference.com/w/cpp/20)
[![Build & Test](https://github.com/YOUR-USERNAME/aerofc-avionics/actions/workflows/build_test.yml/badge.svg)](https://github.com/YOUR-USERNAME/aerofc-avionics/actions)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)

An educational, non-certified avionics demonstrator and engineering testbench developed from scratch in modern C++ and Python. 

Rather than focusing solely on flight simulation, **AeroFC** models the systems engineering lifecycle used in safety-critical aerospace environments: derived requirements, bi-directional traceability, architectural isolation, Fault Detection, Isolation, and Recovery (FDIR), and systematic verification evidence.

> **Disclaimer:** This is an independent academic demonstrator. It is not certified for flight and does not claim formal qualification under DO-178C, DO-254, or ARP4754B. It applies selected principles from these standards to an educational architecture.

---

## 1. System Architecture

AeroFC separates the simulation environment, onboard flight software, and telemetry/ground monitoring into distinct domains:

```text
┌─────────────────────────────────────────────────────────────┐
│                 AIRCRAFT SIMULATION (C++)                   │
│   Kinematics (3-DOF/6-DOF) • Atmosphere • Sensor Noise/Bias │
└──────────────────────────────┬──────────────────────────────┘
                               │ Synthetic Sensor Data
                               ▼
┌─────────────────────────────────────────────────────────────┐
│                 FLIGHT COMPUTER CORE (C++)                  │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │ Sensor Manager & Plausibility Validation           │   │
│   ├─────────────────────────────────────────────────────┤   │
│   │ State Estimation & Navigation (Altitude / Attitude) │   │
│   ├─────────────────────────────────────────────────────┤   │
│   │ Flight State Machine (INIT -> LANDED)               │   │
│   ├─────────────────────────────────────────────────────┤   │
│   │ FDIR & Health Monitoring (Timeouts, Range, Stuck)   │   │
│   ├─────────────────────────────────────────────────────┤   │
│   │ Real-Time Cyclic Executive / Rate Monotonic Sched   │   │
│   └─────────────────────────────────────────────────────┘   │
└──────────────────────────────┬──────────────────────────────┘
                               │ UDP Telemetry / Commands
                               ▼
┌─────────────────────────────────────────────────────────────┐
│                 GROUND CONTROL STATION (Python)             │
│   Live Data Plots • State Annunciation • Fault Injection    │
└─────────────────────────────────────────────────────────────┘
