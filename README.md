# Industrial 3-Phase Induction Motor Control and Protection System

## 1. Introduction

Three-phase induction motors are widely used in industrial applications because of their simple construction, reliability and ability to operate under different load conditions. However, abnormal operating conditions such as excessive starting current and sustained overload can affect motor performance and may cause damage if suitable protection is not provided.

This project focuses on the modelling, control and protection of a three-phase squirrel-cage induction motor using MATLAB/Simulink. A Direct-On-Line (DOL) starting arrangement is implemented along with an overcurrent protection mechanism that monitors the motor current and disconnects the motor when a sustained overcurrent condition is detected.

The project also includes an electrical power and control circuit schematic developed using AutoCAD Electrical.

The main objective was to understand the behaviour of an induction motor under different operating conditions and study how a time-delayed protection system responds to variations in motor current and mechanical load.

## 2. Project Objectives

- Develop a three-phase squirrel-cage induction motor model in MATLAB/Simulink.
- Implement a DOL starting arrangement using a three-phase contactor.
- Monitor motor current using RMS measurement.
- Develop an overcurrent detection mechanism using a predefined pickup threshold.
- Implement time-delayed tripping using an ON-delay timer.
- Use an S-R latch to maintain the trip condition.
- Analyse motor response under different mechanical load conditions.
- Verify the protection logic through simulation-based test cases.
- Develop the corresponding electrical power and control circuit using AutoCAD Electrical.

## 3. Software and Tools

| Software / Tool | Application |
|---|---|
| MATLAB R2021a | Simulation environment |
| Simulink | Motor modelling and control logic |
| Simscape Electrical / Specialized Power Systems | Electrical system modelling |
| AutoCAD Electrical | Power and control circuit schematic |
| Microsoft Excel | Test case documentation |
| GitHub | Project documentation and version control |

## 4. Motor Specifications

The following parameters were used in the working simulation model.

| Parameter | Value |
|---|---:|
| Motor type | Three-phase squirrel-cage induction motor |
| Rated apparent power | 15 kVA |
| Rated voltage | 220 V |
| Supply frequency | 60 Hz |
| Number of poles | 2 |
| Number of pole pairs | 1 |
| Stator resistance (Rs) | 0.25 Ω |
| Stator leakage reactance (Xls) | 0.40 Ω |
| Rotor resistance (Rr') | 0.14 Ω |
| Rotor leakage reactance (Xlr') | 0.41 Ω |
| Magnetising reactance (Xm) | 17 Ω |
| Moment of inertia (J) | 0.01 kg·m² |

## 5. System Description

The system consists of a three-phase induction motor, a DOL starter, current measurement blocks, an overcurrent detection unit, a timer and a latching protection mechanism.

### 5.1 Motor Starting Circuit

The motor is connected to the three-phase supply through a circuit breaker and a three-pole contactor.

When the START pushbutton is pressed, the contactor coil energises and closes its three main normally open contacts. This connects the motor to the supply.

An auxiliary normally open contact is used to provide a holding arrangement, allowing the contactor to remain energised after the START pushbutton is released.

The STOP pushbutton is normally closed and is used to interrupt the contactor control circuit.

### 5.2 Current Measurement

The motor current is continuously monitored using current measurement blocks.

The measured current is processed through an RMS calculation block. The resulting RMS value is compared with a predefined current pickup threshold.

### 5.3 Overcurrent Detection

When the measured RMS current exceeds the selected pickup threshold, the overcurrent detection logic initiates the ON-delay timer.

The time delay prevents immediate operation of the protection mechanism during short-duration current variations.

If the overcurrent condition persists for the configured delay, a trip signal is generated.

### 5.4 Time-Delayed Tripping

The timer output operates the trip logic through an S-R flip-flop.

The timer's normally closed timed contact is used in the contactor control circuit. When the timer operates, this contact opens and interrupts the KM1 contactor coil circuit.

As a result, the contactor de-energises and disconnects the motor from the three-phase supply.

### 5.5 S-R Latch Operation

An S-R latch is used to retain the trip condition.

Once the trip signal is generated, the latch output remains HIGH even if the motor current subsequently falls below the pickup threshold.

The system therefore requires a reset operation before normal restarting.

## 6. Protection Logic

The protection sequence implemented in Simulink is:

**Current Measurement → RMS Calculation → Threshold Comparison → ON-Delay Timer → S-R Latch → Contactor Trip**

The overcurrent detection condition is:

`I_RMS > I_pickup`

The timer operates only when the overcurrent condition persists for the configured delay.

The S-R latch maintains the trip output until reset.

Two different pickup settings were investigated during the simulation.

| Test Series | Pickup Threshold | Purpose |
|---|---:|---|
| Initial characterisation | 6 A RMS | Starting and overload response analysis |
| Corrected voltage configuration | 9 A RMS | Motor response under different mechanical loads |

These are separate simulation configurations. The pickup values are model-specific settings used to demonstrate protection behaviour and are not validated physical relay settings.

## 7. Simulation and Test Results

A total of 17 numbered test cases were completed. The tests were divided into four groups to study different aspects of motor operation and protection.

### 7.1 Test Series 1: Motor Starting Behaviour

Configuration:

- Current pickup: 6 A RMS
- S-R latch protection
- OFF delay: 0 s

| Test Case | ON Delay | Observation | Result |
|---|---:|---|---|
| TC-01 | 0.30 s | Protection operated during motor starting | TRIP |
| TC-02 | 0.50 s | Motor successfully reached normal speed | NO TRIP |
| TC-03 | 0.45 s | Normal starting operation; steady-state current approximately 4.07 A | NO TRIP |

**Observation:**

The tests demonstrate the effect of timer settings on motor starting behaviour. A shorter delay resulted in unwanted tripping during starting, whereas longer delays allowed the motor to accelerate successfully.

### 7.2 Test Series 2: Overload Characterisation

Configuration:

- Current pickup: 6 A RMS
- ON delay: 0.45 s
- OFF delay: 0 s
- Mechanical load application time: 1 s

| Test Case | Mechanical Load | Observation | Result |
|---|---:|---|---|
| TC-04 | −5.00 N·m | Sustained overload condition | TRIP |
| TC-05 | −1.00 N·m | Current approximately 4.45 A | NO TRIP |
| TC-06 | −3.00 N·m | Current exceeded 6 A | TRIP |
| TC-07 | −2.00 N·m | Current approximately 5.51 A | NO TRIP |
| TC-08 | −2.50 N·m | Current exceeded 6 A | TRIP |
| TC-09 | −2.25 N·m | Current approximately 5.845 A | NO TRIP |
| TC-10 | −2.35 N·m | Current approximately 5.985 A | NO TRIP |
| TC-11 | −2.40 N·m | Current exceeded 6 A | TRIP |
| TC-12 | −2.375 N·m | Current exceeded 6 A | TRIP |

**Observation:**

The simulation showed a trip boundary between −2.35 N·m and −2.375 N·m for the selected model configuration.

At −2.35 N·m, the motor continued operating without tripping. At −2.375 N·m, the protection mechanism operated.

This boundary represents the observed behaviour of the simulation and should not be treated as a universal motor overload limit.

### 7.3 Test Series 3: S-R Latch Verification

| Test Case | Mechanical Load | Observation | Result |
|---|---:|---|---|
| TC-13 | −5 N·m at 1 s | Trip output changed to logic HIGH at approximately 1.6 s and remained HIGH after the current returned to zero | PASS |

**Observation:**

The S-R latch successfully retained its trip state after the overcurrent event.

Even after the measured current decreased, the latch output remained HIGH until reset. This verified the intended latching behaviour of the protection logic.

### 7.4 Test Series 4: Corrected 220 V Supply Configuration

Configuration:

- Source voltage: 220 V line-to-line RMS
- Current pickup: 9 A RMS
- ON delay: 0.45 s
- OFF delay: 0 s
- Load application time: 1 s for loaded tests

| Test Case | Mechanical Load | Measured Current | Result |
|---|---:|---:|---|
| TC-14 | 0 N·m | 7.28 A | NO TRIP |
| TC-15 | −1 N·m | 7.34 A | NO TRIP |
| TC-16 | −3 N·m | 7.88 A | NO TRIP |
| TC-17 | −5 N·m | 8.928 A | NO TRIP |

**Observation:**

The measured current increased with the applied mechanical load.

In TC-17, the current reached approximately 8.928 A under a mechanical load of −5 N·m. Since this value remained below the selected 9 A pickup threshold, the protection system did not initiate a trip.

The rotor speed remained approximately 1 pu.

## 8. Overall Test Summary

| Test Category | Completed Tests | Result |
|---|---:|---|
| Starting behaviour | 3 | 2 NO TRIP, 1 TRIP |
| Overload characterisation | 9 | 5 TRIP, 4 NO TRIP |
| S-R latch verification | 1 | PASS |
| Corrected 220 V configuration | 4 | 4 NO TRIP |
| **Total** | **17** | **6 TRIP, 10 NO TRIP, 1 PASS** |

The test cases demonstrate the response of the motor and protection logic under different starting delays, pickup thresholds and mechanical load conditions.

## 9. AutoCAD Electrical Schematic

An electrical schematic was developed using AutoCAD Electrical to represent the power and control circuits of the system.

The schematic includes:

- Three-phase supply.
- Three-pole circuit breaker (CB1).
- Current transformers (CT1, CT2 and CT3).
- Three-pole main contactor (KM1).
- START and STOP pushbuttons.
- Contactor auxiliary holding contact.
- Overcurrent relay functional block (OCR1) and its output contact.
- ON-delay timer (KT1) and normally closed timed contact.
- Three-phase induction motor (M1).

The drawing separates the power and control circuits to make the system connections and protection sequence easier to follow.

## 10. Repository Structure

```text
Industrial-3-Phase-Induction-Motor-Control-and-Protection/
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

The filenames represent the intended folder structure. Actual model and drawing filenames should be used when uploading the project files.

## 11. Key Learning Outcomes

Working on this project provided practical exposure to:

- Three-phase induction motor modelling.
- DOL starting and contactor-based motor control.
- RMS current measurement and overcurrent detection.
- Timer-based protection logic.
- S-R latch implementation and verification.
- Motor response under varying mechanical load conditions.
- Electrical power and control circuit design.
- Simulation-based testing and result analysis.
- Technical documentation and GitHub project organisation.

## 12. Limitations and Future Scope

### Limitations

- The system has been developed and tested in a simulation environment.
- The protection pickup settings are demonstration values specific to the model configuration.
- The system has not been validated using a physical induction motor test bench.
- Practical implementation would require appropriate CT selection, relay coordination, equipment ratings and hardware testing.

### Future Scope

The project can be extended by incorporating:

- Thermal overload protection.
- Phase failure and phase imbalance detection.
- Short-circuit protection.
- Earth-fault protection.
- Microcontroller-based motor protection.
- Real-time motor monitoring.
- Hardware-in-the-loop testing.
- Integration with a SCADA-based monitoring system.

## 13. Conclusion

This project demonstrates the modelling and simulation of a three-phase squirrel-cage induction motor with a DOL starting arrangement and a time-delayed overcurrent protection system.

Different simulation conditions were used to study motor starting behaviour, overload response and trip characteristics. The S-R latch verification confirmed that the protection logic could maintain its trip state after an overcurrent event.

The accompanying AutoCAD Electrical schematic provides a representation of the power and control circuits used in the system.

Overall, the project provided practical experience in induction motor modelling, electrical protection logic, simulation-based testing and electrical schematic development.

## Author

ADRIJA 
Techno Main Salt Lake  
Kolkata, West Bengal, India

**Project:** Industrial 3-Phase Induction Motor Control and Protection System
