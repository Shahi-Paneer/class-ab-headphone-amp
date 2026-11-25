# Class AB Headphone Amplifier

An independent engineering project focused on the design, simulation, prototyping, and implementation of a low-distortion stereo headphone amplifier optimized for high-impedance headphones.

![Revision 2 breadboard prototype](images/rev2-breadboard.jpg)

*Revision 2 single-channel breadboard prototype following
successful DC operating point and initial thermal stability
validation.*

## Overview

The amplifier combines an NE5532 operational amplifier with a discrete complementary Class AB output stage utilizing BD139 and BD140 bipolar junction transistors.

The project explores analog circuit design, transistor biasing, negative feedback, thermal stability, circuit simulation, hardware debugging, electronic instrumentation, and PCB development.

The current design is optimized around the Sennheiser HD 600 and its nominal impedance of 300 Ω.

## Design Objectives

| Parameter | Target |
|---|---|
| Amplifier topology | Op-amp with discrete Class AB output stage |
| Channels | Stereo |
| Nominal load impedance | 300 Ω |
| Supply voltage | ±12 V DC |
| Frequency response | 20 Hz – 20 kHz |
| Rated output voltage | 5 V RMS |
| Output power at 300 Ω | 83 mW per channel |
| Output current at rated load | 16.7 mA RMS |
| Output impedance | Approximately 10 Ω |
| Voltage gain | 2× nominal |
| Quiescent current | Adjustable, targeting 5 mA |
| DC output offset | < 5 mV |
| THD+N | < 0.01% |

These specifications represent design objectives rather than verified final hardware performance.

## Circuit Architecture

- NE5532 operational amplifier voltage gain stage
- BD139/BD140 complementary emitter-follower output stage
- Adjustable 2N3904 VBE multiplier for quiescent current control
- 5 Ω emitter degeneration resistors for improved bias stability
- 47 Ω base stopper resistors
- 10 Ω output isolation resistor for improved capacitive-load stability
- Global negative feedback
- Dual-rail DC power supply
- Local power supply decoupling

## Design Revisions

### Rev 1.0 — Initial Prototype

The initial design utilized diode-based transistor biasing and was developed around a nominal 51 Ω test load.

Circuit simulation and breadboard experimentation identified limitations in quiescent current control, motivating a redesign of the biasing network.

### Rev 2.0 — High-Impedance Redesign

The second revision targets a nominal 300 Ω headphone load and introduces:

- Adjustable quiescent current through a VBE multiplier
- Increased emitter degeneration
- Improved thermal behaviour
- Base stopper resistors for improved high-frequency stability
- Output isolation for capacitive-load stability
- Characterization of voltage swing, harmonic distortion, and overload recovery

Initial hardware testing identified differences between simulated and physical transistor bias characteristics. The bias adjustment range was subsequently extended, allowing the intended quiescent current to be established without altering the fundamental amplifier topology.

## Preliminary Validation Results

### Simulation

The Revision 2 circuit has undergone LTspice characterization, including:

- DC operating point analysis
- Uniform and differential temperature sweeps
- Transient response analysis
- Harmonic distortion analysis
- Maximum output voltage characterization
- AC frequency response analysis
- Output impedance characterization
- Noise analysis
- Capacitive-load stability testing
- Square-wave response
- Overload behaviour and recovery

Simulation results informed component selection and design modifications prior to physical implementation. Some measurements were performed on intermediate configurations and remain subject to confirmation using the finalized circuit.

### Physical Prototype

Initial DC validation of the single-channel Revision 2 breadboard prototype has been completed.

| Parameter | Measured Result |
|---|---:|
| Initial quiescent current | 5.02 mA |
| Warm NPN quiescent current | 5.26 mA |
| Warm PNP quiescent current | 5.24 mA |
| Positive rail idle current | Approximately 13 mA |
| Negative rail idle current | Approximately 13 mA |
| DC output offset | 0 mV displayed |
| Thermal settling time | Approximately 4 minutes |
| Quiescent current drift over 10 minutes | +5.62% |

Quiescent currents were calculated using measured voltage drops across the nominal 5 Ω emitter resistors.

The prototype demonstrated stable idle behaviour during a 10-minute warm-up test. Physical AC performance, high-frequency stability, and distortion remain to be characterized.

## Development Roadmap

- [x] Establish preliminary design requirements
- [x] Research amplifier topologies
- [x] Simulate individual amplifier stages
- [x] Integrate voltage and current amplification stages
- [x] Construct initial single-channel breadboard prototype
- [x] Investigate limitations of initial biasing network
- [x] Implement adjustable biasing network
- [x] Perform initial simulation characterization
- [x] Construct revised single-channel breadboard prototype
- [x] Establish physical DC operating point
- [x] Perform initial idle thermal stability testing
- [ ] Verify hardware stability using an oscilloscope
- [ ] Characterize physical AC performance
- [ ] Validate frequency response, output power, and distortion
- [ ] Conduct headphone listening tests
- [ ] Develop stereo amplifier circuit
- [ ] Design custom PCB in KiCad
- [ ] Manufacture and assemble PCB
- [ ] Characterize final amplifier performance
- [ ] Design and manufacture enclosure

## Repository Structure

| Directory | Description |
|---|---|
| `simulation/` | LTspice schematics, circuit experiments, and component models |
| `hardware/` | Hardware schematics, PCB design files, and bill of materials |
| `testing/` | Simulation results, bench measurements, and validation procedures |
| `docs/` | Design analysis and engineering documentation |
| `images/` | Circuit diagrams and prototype photographs |
| `enclosure/` | Mechanical design and manufacturing files |

## Tools and Equipment

- LTspice — circuit simulation
- KiCad — schematic capture and PCB design
- Oscilloscope — signal analysis
- Function generator — test signal generation
- Dual-rail bench power supply — controlled power delivery
- Digital multimeter — electrical measurements

## Project Status

**Rev 2.0 — Hardware Validation | November 2026**

Initial simulation characterization and physical DC validation of the revised single-channel amplifier have been completed.

The adjustable bias network has been calibrated to approximately 5 mA of quiescent current, with preliminary testing demonstrating stable idle operation and negligible measured DC output offset.

Current development focuses on physical AC characterization, high-frequency stability verification, and performance testing into a nominal 300 Ω load prior to stereo implementation and PCB development.

## License

Original project files are licensed under the MIT License. Third-party component models remain subject to their respective licenses.