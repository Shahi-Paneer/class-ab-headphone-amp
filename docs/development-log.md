# Development Log

## Revision 1 — Initial Amplifier Design

**Development period:** November 17–23, 2026  
**Entry type:** Retrospective summary

### Objective

Develop a single-channel Class AB headphone amplifier using an operational amplifier and a discrete complementary BJT output stage, initially targeting a nominal 51 Ω load.

### Design Architecture

- NE5532 operational amplifier
- BD139 and BD140 complementary output transistors
- 1N4148 diode-based biasing network
- Global negative feedback
- 0.47 Ω emitter resistors
- ±12 V DC power supply

### Development Process

Initial simulations investigated voltage amplification and transistor biasing independently before integrating both stages into a complete amplifier circuit.

LTspice was used to investigate DC operating points, transient response, transistor conduction and quiescent current.

Following simulation, a single-channel breadboard prototype was constructed to investigate physical circuit behaviour and compare it with simulated results.

### Problems Encountered

- Sensitivity of quiescent current to biasing voltage and transistor characteristics.
- Limited control over output-stage idle current.
- Difficulty measuring small voltage drops across the emitter resistors.
- Unexpected current draw and supply rail collapse during breadboard testing.
- Discrepancies between simulated and physical circuit behaviour.

The root cause of the physical supply instability was not conclusively established.

### Lessons Learned

- Successful circuit simulation does not guarantee correct physical operation.
- Output-stage biasing requires consideration of component tolerances and temperature dependence.
- Larger emitter resistances may improve bias stability and simplify current measurements.
- Individual circuit stages should be validated before complete system integration.
- Physical validation requires appropriate instrumentation and controlled testing procedures.

### Outcome

Revision 1 established the fundamental amplifier architecture but did not achieve sufficient confidence in output-stage bias control or physical reliability.

The design was retained as a reference for subsequent development.

---

## Revision 2 — High-Impedance Redesign

**Initiated:** November 23, 2026

### Motivation

Following the acquisition of Sennheiser HD 600 headphones, the nominal design load was revised from 51 Ω to 300 Ω.

The increased load impedance reduces output current requirements and provides an opportunity to prioritize linearity, thermal stability and predictable quiescent current.

### Proposed Changes

- Replace fixed diode biasing with an adjustable VBE multiplier.
- Investigate increased emitter resistance.
- Establish an initial quiescent current target of 5 mA.
- Reevaluate bias network current requirements.
- Validate DC offset, distortion and thermal stability.

### Status

Design and simulation in progress.