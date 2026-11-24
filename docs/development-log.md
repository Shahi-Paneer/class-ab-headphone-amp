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

## 2026-11-23 — Rev 2: Output Stage Redesign and Thermal Analysis

### Objective

Refactor the Class AB amplifier for the Sennheiser HD 600 (300 Ω) and address the limitations of the original diode-based biasing circuit. Establish an adjustable quiescent current of approximately 5 mA and characterize its temperature dependence through LTspice simulations.

### Changes

- Changed the output load from 51 Ω to 300 Ω.
- Increased emitter resistors to improve bias stability and simplify quiescent current measurements.
- Replaced the diode-based biasing circuit with an adjustable VBE multiplier using a 2N3904 transistor.
- Reduced bias network current by increasing the bias feed resistors.
- Retained the NE5532, BD139/BD140 output pair, ±12 V supply and global negative feedback topology.

| Component | Rev 1 | Rev 2 |
|---|---|---|
| R1, R2 | 0.47 Ω | 5 Ω |
| R3 | 51 Ω | 300 Ω |
| R7, R8 | 1 kΩ | 2 kΩ |
| R9 | N/A | 1.47 kΩ |
| R10 | N/A | 1 kΩ + 1 kΩ trimmer |
| Q3 | N/A | 2N3904 |

### Results

**DC Operating Point**

The VBE multiplier was initially configured with a 1 kΩ collector-to-base resistor. This provided insufficient bias voltage throughout the available trimmer adjustment range. Increasing the resistance to 1.47 kΩ allowed the desired quiescent current to be achieved.

| R9 | Trimmer Resistance | Quiescent Current |
|---|---:|---:|
| 1 kΩ | 1000 Ω | 0.335 µA |
| 1 kΩ | 500 Ω | 0.546 µA |
| 1 kΩ | 0 Ω | 30.29 µA |
| 1.47 kΩ | 250 Ω | 0.378 mA |
| 1.47 kΩ | 50 Ω | 3.862 mA |
| 1.47 kΩ | 25 Ω | 4.935 mA |

The final nominal operating point produced:

- Q1 emitter current: 4.93487 mA
- Q2 emitter current: 4.93451 mA
- Current mismatch: approximately 0.0073%
- Emitter resistor voltage drop: approximately 24.67 mV
- Combined output transistor idle dissipation: approximately 118 mW
- Quiescent current target error: 1.3%

**Thermal Sensitivity Analysis**

Three thermal coupling conditions were simulated over an output transistor temperature range of 25°C to 75°C.

The bias transistor temperature was defined as:

`T_Q3 = 25 + K × Tdelta`

Where K represents the assumed thermal coupling factor between the output transistors and the bias transistor.

| Temperature | K = 0 | K = 0.65 | K = 1 |
|---|---:|---:|---:|
| 25°C | 5.12 mA | 5.12 mA | 5.12 mA |
| 35°C | 6.23 mA | 4.91 mA | 4.23 mA |
| 45°C | 7.43 mA | 4.72 mA | 3.50 mA |
| 55°C | 8.70 mA | 4.54 mA | 2.86 mA |
| 65°C | 10.02 mA | 4.38 mA | 2.32 mA |
| 75°C | 11.44 mA | 4.22 mA | 1.87 mA |
| Total drift | +123% | −17.6% | −63.5% |

Values were approximately extracted from LTspice simulation plots.

### Analysis

The original diode-based biasing circuit lacked a practical means of compensating for differences in transistor and diode forward voltages. Combined with the relatively small emitter resistors, this made the quiescent current highly sensitive to component characteristics and temperature.

The introduction of a VBE multiplier allows the bias voltage to be adjusted independently of the fixed diode forward voltage. Increasing the emitter resistances provides additional local negative feedback and reduces sensitivity to variations in bias voltage.

Thermal simulations demonstrated three distinct behaviours:

- **No thermal coupling (K = 0):** Quiescent current increased significantly as the output transistors heated up.
- **Perfect thermal tracking (K = 1):** The bias network overcompensated, resulting in decreasing quiescent current.
- **Partial thermal coupling (K = 0.65):** Quiescent current remained between approximately 4.22 mA and 5.12 mA throughout the simulated temperature range.

The partial coupling scenario exhibited the smallest current variation of the three tested conditions.

However, the coupling factor represents a simplified static thermal model and cannot be assumed to accurately represent physical transistor placement, heatsink characteristics or transient self-heating.

An additional limitation is that the desired quiescent current occurs at approximately 2.5% of the available trimmer resistance range, leaving insufficient adjustment margin in one direction for component tolerances.

Overall, the revised architecture provides a more controllable and better-characterized biasing mechanism than Rev 1, although physical thermal stability remains unverified.

### Next Steps

- Optimize the VBE multiplier resistor ratio to centre the nominal quiescent current within the trimmer's adjustment range.
- Establish a safe maximum bias current across the full adjustment range.
- Investigate component tolerances and transistor parameter variations.
- Evaluate crossover distortion through transient and Fourier analysis.
- Characterize frequency response across the audible spectrum.
- Determine maximum unclipped output voltage into a 300 Ω load.
- Evaluate feedback-loop stability and capacitive loading.
- Investigate constant-current bias feed as a potential future improvement.
- Validate the revised circuit through breadboard prototyping and instrumentation before connecting headphones.