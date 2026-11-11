# Industrial_3_phase_induction_motor_control_and_protection
MATLAB/Simulink-based industrial three-phase squirrel-cage induction motor control and protection system featuring DOL starting, RMS overcurrent detection, time-delayed tripping, S-R latch protection, and AutoCAD Electrical schematic design.
# ⚡ Industrial 3-Phase Induction Motor Control & Protection System

A MATLAB/Simulink-based simulation project focused on the control, monitoring, and overload protection of a three-phase squirrel-cage induction motor, supported by an AutoCAD Electrical schematic.

## 📌 Overview

Three-phase induction motors are widely used in industrial applications because of their reliability, robustness, and operational efficiency. However, abnormal operating conditions such as excessive starting current and sustained overload can affect motor performance and damage electrical equipment.

This project presents the development and simulation of an **Industrial 3-Phase Squirrel-Cage Induction Motor Control & Protection System** using MATLAB/Simulink.

The system incorporates a Direct-On-Line (DOL) starting arrangement, contactor-based motor control, current monitoring, overcurrent detection, timer-based tripping, and a latching protection mechanism.

An accompanying electrical schematic was developed using AutoCAD Electrical to represent the power and control circuits.

The project focuses on understanding motor starting behaviour, overload characteristics, protection coordination, and control circuit operation through simulation-based testing.

## 🎯 Objectives

- Model a three-phase squirrel-cage induction motor in MATLAB/Simulink.
- Implement a Direct-On-Line (DOL) motor starting arrangement.
- Analyse motor starting current and steady-state operating conditions.
- Develop an overcurrent protection mechanism using RMS current measurement.
- Implement time-delayed tripping using an ON-delay timer.
- Use an S-R latch to maintain the trip condition.
- Evaluate motor response under different mechanical load conditions.
- Develop the corresponding power and control circuit schematic using AutoCAD Electrical.

## ⚙️ System Specifications

### Induction Motor Parameters

| Parameter | Value |
|---|---:|
| Motor Type | 3-Phase Squirrel-Cage Induction Motor |
| Rated Apparent Power | 15 kVA |
| Rated Voltage | 220 V |
| Supply Frequency | 60 Hz |
| Number of Poles | 2 |
| Number of Pole Pairs | 1 |
| Stator Resistance (Rs) | 0.25 Ω |
| Stator Leakage Reactance (Xls) | 0.40 Ω |
| Rotor Resistance (Rr') | 0.14 Ω |
| Rotor Leakage Reactance (Xlr') | 0.41 Ω |
| Magnetising Reactance (Xm) | 17 Ω |
| Moment of Inertia (J) | 0.01 kg·m² |

### Protection and Control Parameters

| Parameter | Configuration |
|---|---|
| Starting Method | Direct-On-Line (DOL) |
| Current Measurement | RMS Current Measurement |
| Protection Element | Overcurrent Relay Logic |
| Trip Mechanism | ON-Delay Timer |
| Timer OFF Delay | 0 s |
| Latching Mechanism | S-R Flip-Flop |
| Switching Element | Three-Phase Contactor |
| Motor Disconnection | Contactor De-energisation |

## 🔌 Working Principle

The system operates through the following stages:

### 1. Motor Starting

The motor is connected to the three-phase supply through a circuit breaker and a three-pole contactor.

When the START pushbutton is pressed, the contactor coil energises and closes the main contacts, allowing the motor to start.

A normally open auxiliary contact provides the holding (self-latching) arrangement.

### 2. Current Monitoring

The motor current is continuously monitored through current measurement blocks.

The measured current is converted into an RMS value and compared against a predefined pickup threshold.

### 3. Overcurrent Detection

When the measured RMS current exceeds the selected pickup threshold, the protection logic initiates the ON-delay timer.

The timer allows short-duration current variations while identifying sustained overcurrent conditions.

### 4. Time-Delayed Tripping

If the overcurrent condition persists beyond the configured ON-delay, the S-R latch receives the trip signal.

The latch maintains the trip state even if the measured current subsequently falls below the threshold.

### 5. Motor Disconnection

The trip signal operates the contactor control logic.

The KM1 contactor coil de-energises, opening all three main power contacts and disconnecting the induction motor from the supply.

The system requires a reset operation before restarting after a latched trip.

## 🧠 Protection Logic

The protection system follows the sequence:

**Current Measurement → RMS Calculation → Threshold Comparison → ON-Delay Timer → S-R Latch → Contactor Trip**

The overcurrent condition is represented by:

`I_RMS > I_pickup`

The timer allows the overcurrent condition to persist for the configured delay before initiating the trip.

The S-R latch maintains the trip output until reset.

Two different pickup configurations were investigated during simulation:

- **6 A RMS:** Used for initial protection characterisation and overload threshold investigation.
- **9 A RMS:** Used for a separate series of tests with the corrected 220 V source configuration.

These configurations are separate simulation test series and should not be interpreted as a single validated physical relay setting.

## 🧪 Simulation and Test Cases

A total of **17 numbered test cases were completed**, covering motor starting, overload response, trip characteristics, and latch operation.

### Test Series 1 — Starting Behaviour

Configuration: 6 A RMS pickup threshold.

| Test Case | ON Delay | Observation | Result |
|---|---:|---|---|
| TC-01 | 0.30 s | Protection operated during motor starting | TRIP |
| TC-02 | 0.50 s | Motor successfully reached normal speed | NO TRIP |
| TC-03 | 0.45 s | Normal starting operation; steady current ≈ 4.07 A | NO TRIP |

**Observation:** The starting-delay tests demonstrate the importance of selecting an appropriate time delay to avoid unwanted tripping during motor acceleration.

### Test Series 2 — Overload Characterisation

Configuration:
- Current pickup: 6 A RMS
- ON delay: 0.45 s
- OFF delay: 0 s
- Mechanical load applied at 1 s

| Test Case | Mechanical Load | Observation | Result |
|---|---:|---|---|
| TC-04 | −5.00 N·m | Sustained overload | TRIP |
| TC-05 | −1.00 N·m | Current ≈ 4.45 A | NO TRIP |
| TC-06 | −3.00 N·m | Current exceeded 6 A | TRIP |
| TC-07 | −2.00 N·m | Current ≈ 5.51 A | NO TRIP |
| TC-08 | −2.50 N·m | Current exceeded 6 A | TRIP |
| TC-09 | −2.25 N·m | Current ≈ 5.845 A | NO TRIP |
| TC-10 | −2.35 N·m | Current ≈ 5.985 A | NO TRIP |
| TC-11 | −2.40 N·m | Current exceeded 6 A | TRIP |
| TC-12 | −2.375 N·m | Current exceeded 6 A | TRIP |

**Key Observation:**

The simulated trip boundary was observed between:

- −2.35 N·m: NO TRIP
- −2.375 N·m: TRIP

This boundary applies only to the recorded simulation configuration.

### Test Series 3 — S-R Latch Verification

| Test Case | Test Condition | Observation | Result |
|---|---|---|---|
| TC-13 | −5 N·m load at 1 s | Trip output changed to logic HIGH at approximately 1.6 s and remained HIGH after current returned to zero | PASS |

**Key Observation:**

The S-R latch successfully maintained the trip condition after the overcurrent event, confirming that the protection output did not automatically reset when the current decreased.

### Test Series 4 — Corrected 220 V Supply Configuration

Configuration:
- Source voltage: 220 V line-to-line RMS
- Current pickup: 9 A RMS
- ON delay: 0.45 s
- OFF delay: 0 s
- Load application time: 1 s for loaded tests

| Test Case | Mechanical Load | Current | Result |
|---|---:|---:|---|
| TC-14 | 0 N·m | 7.28 A | NO TRIP |
| TC-15 | −1 N·m | 7.34 A | NO TRIP |
| TC-16 | −3 N·m | 7.88 A | NO TRIP |
| TC-17 | −5 N·m | 8.928 A | NO TRIP |

**Key Observation:**

Even under a mechanical load of −5 N·m, the measured current reached approximately 8.928 A, remaining below the configured 9 A pickup threshold.

Therefore, the protection system did not initiate a trip, and the motor continued operating at approximately 1 pu rotor speed.

## 📊 Overall Test Summary

| Category | Count |
|---|---:|
| Total Completed Numbered Test Cases | 17 |
| TRIP Results | 6 |
| NO TRIP Results | 10 |
| S-R Latch Verification | 1 PASS |

The test register includes the individual test conditions, pickup thresholds, timer settings, observations, and outcomes.

## 📁 Project Structure

```text
Induction_Motor_Control_Protection_Project/
│
├── README.md
│
├── MATLAB_Simulink/
│   └── Industrial_IM_Control_Protection.slx
│
├── AutoCAD_Electrical/
│   ├── 3_Phase_IM_Protection.dwg
│   └── 3_Phase_IM_Protection_Schematic.pdf
│
└── Results/
    └── test_case_register.xlsx
```

*The filenames above represent the intended repository organisation. Add the actual model and drawing files before publishing the final repository.*

## 💻 Software and Tools Used

- MATLAB R2021a
- Simulink
- Simscape Electrical / Specialized Power Systems
- AutoCAD Electrical
- Microsoft Excel
- Git and GitHub

## 📈 Key Learning Outcomes

Through this project, the following concepts and practical skills were explored:

- Three-phase induction motor modelling and analysis.
- DOL motor starting and contactor control.
- RMS current measurement and overcurrent detection.
- Timer-based protection and trip coordination.
- S-R latch implementation and verification.
- Motor response under varying mechanical loads.
- Electrical control and power circuit schematic development.
- Simulation-based testing and result documentation.
- Technical documentation and version-controlled project organisation.

## 🚀 Future Scope

The project can be extended through:

- Implementation of a thermal overload protection model.
- Short-circuit and phase-loss protection.
- Ground-fault detection.
- Integration of an actual microcontroller-based protection unit.
- Hardware-in-the-loop testing.
- Comparison of simulated protection behaviour with practical relay characteristics.
- Development of an industrial monitoring interface using SCADA concepts.

## ⚠️ Project Limitations

- The protection system is a simulation-based implementation.
- The pickup thresholds used in the test series are model-specific demonstration settings, not validated physical relay settings.
- The results have not been validated through a physical induction motor test bench.
- Practical implementation would require proper relay coordination, equipment ratings, CT selection, and hardware testing.

## 👩‍💻 Author

**Electrical Engineering Student**

Techno Main Salt Lake

Project: Industrial 3-Phase Induction Motor Control & Protection System

---

*This project was developed as an academic engineering project to explore induction motor control, electrical protection, simulation, and schematic design.*
