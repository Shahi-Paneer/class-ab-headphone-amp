# Class AB Headphone Amplifier

An independent engineering project focused on the design, simulation, prototyping, and implementation of a low-distortion stereo headphone amplifier optimized for high-impedance headphones.

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
| Maximum output voltage | 5 V RMS |
| Output power at 300 Ω | 83 mW per channel |
| Output current at rated load | 16.7 mA RMS |
| Output impedance | < 1 Ω |
| Selectable gain | 1× / 3× |
| Quiescent current | Adjustable, initially targeting 5 mA |
| DC output offset | < 5 mV |
| THD+N | < 0.01% |

These specifications represent design targets rather than verified performance measurements.

## Circuit Architecture

- NE5532 operational amplifier voltage gain stage
- BD139/BD140 complementary emitter-follower output stage
- Adjustable VBE multiplier for quiescent current control (in development)
- Emitter degeneration for improved bias stability
- Global negative feedback
- Dual-rail DC power supply

## Design Revisions

### Rev 1.0 — Initial Prototype

The initial design utilized diode-based transistor biasing and was developed around a nominal 51 Ω test load.

Circuit simulation and breadboard experimentation identified limitations in quiescent current control, motivating a redesign of the biasing network.

### Rev 2.0 — High-Impedance Redesign

The second revision targets a nominal 300 Ω headphone load and focuses on:

- Adjustable quiescent current
- Improved thermal stability
- Reduced crossover distortion
- Increased emitter degeneration
- Low output impedance
- Improved voltage swing and linearity

## Development Roadmap

- [x] Establish preliminary design requirements
- [x] Research amplifier topologies
- [x] Simulate individual amplifier stages
- [x] Integrate voltage and current amplification stages
- [x] Construct initial single-channel breadboard prototype
- [x] Investigate limitations of initial biasing network
- [ ] Implement adjustable biasing network
- [ ] Validate operating point and thermal stability
- [ ] Characterize frequency response and distortion
- [ ] Validate revised single-channel breadboard prototype
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

**Rev 2.0 — In Development | November 2026**

The initial amplifier topology has been simulated and prototyped. Current development focuses on redesigning the transistor biasing network to achieve predictable quiescent current, improved thermal stability, and reliable operation into a 300 Ω load.

## License

Original project files are licensed under the MIT License. Third-party component models remain subject to their respective licenses.
