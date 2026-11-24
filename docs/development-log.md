---

## 2026-11-23 — Rev 2: Bias Network Optimization

### Objective

Improve the adjustment range of the VBE multiplier, reduce the maximum achievable quiescent current and simplify physical implementation using available component values.

### Changes

- Increased R9 from 1.47 kΩ to 2 kΩ.
- Revised R10 to incorporate a 1 kΩ fixed resistor, a 220 Ω fixed resistor and a 1 kΩ trimmer.
- Established a nominal trimmer resistance of approximately 200 Ω.
- Retained the 5 Ω emitter resistors.

### Results

The initial revised bias network permitted a maximum quiescent current of approximately 35 mA at minimum trimmer resistance. Adding a 220 Ω series resistor reduced this maximum to approximately 15 mA under nominal simulation conditions.

| Parameter | Before Optimization | After Optimization |
|---|---:|---:|
| Maximum nominal quiescent current | 35 mA | 14.9 mA |
| Target quiescent current | 5 mA | 5 mA |
| Nominal trimmer resistance | ~400 Ω | ~200 Ω |
| Minimum quiescent current | Near cutoff | Near cutoff |

The maximum nominal adjustable current was reduced by approximately 57%.

A subsequent thermal simulation using an assumed thermal coupling factor of 0.65 produced a quiescent current ranging from approximately 4.85 mA to 4.00 mA over an output transistor temperature range of 25°C to 75°C.

### Analysis

The original VBE multiplier configuration provided an unnecessarily large adjustment range. Introducing additional fixed resistance limits the maximum bias voltage while preserving sufficient adjustment capability.

The revised network also simplifies physical implementation by using readily available resistor values rather than extensive series resistor combinations.

The maximum bias current remains dependent on transistor characteristics and temperature. The fixed resistor therefore limits the nominal adjustment range but does not constitute an absolute current-limiting mechanism.

### Next Steps

- Evaluate output-stage conduction under sinusoidal excitation.
- Characterize harmonic distortion across multiple output amplitudes.
- Investigate large-signal behaviour and capacitive load stability.

---

## 2026-11-23 — Rev 2: Capacitive Load Instability and Mitigation

### Objective

Characterize the amplifier's large-signal response, identify the source of observed high-frequency oscillations and develop a practical stabilization strategy.

### Changes

- Introduced sinusoidal input excitation at 1 kHz.
- Performed transient analysis across multiple input amplitudes.
- Introduced an output error trace to compare the simulated output against the ideal amplified signal.
- Added 47 Ω base stopper resistors to the BD139 and BD140.
- Investigated the effect of removing the simulated load capacitance.
- Introduced an output isolation resistor outside the global feedback loop.
- Added a separate VLOAD node to distinguish amplifier output voltage from headphone terminal voltage.

### Results

Initial transient simulations demonstrated clean amplification at low and moderate signal amplitudes. Both output transistors remained continuously conducting at sufficiently low output levels.

At higher amplitudes, high-frequency oscillation appeared during portions of the positive half-cycle.

At a 3 V peak input amplitude, the following behaviour was observed:

- Output voltage error approaching ±250 mV.
- Repeated high-frequency oscillation bursts.
- Elevated NPN emitter current.
- Stable operation during most of the negative half-cycle.

Reducing the simulation timestep did not eliminate the observed behaviour.

| Experiment | Observation |
|---|---|
| Reduced simulation timestep | Oscillation persisted |
| Added 47 Ω base stoppers | Minor improvement |
| Removed 1 nF load capacitance | Oscillation eliminated |
| Added 3.3 Ω output isolation | Minor residual high-frequency activity |
| Added 4.7 Ω output isolation | No visible oscillation at the initial test amplitude |
| Increased isolation to 10 Ω | Improved large-signal damping and overload behaviour |

A load capacitance sweep was subsequently conducted using the 4.7 Ω isolation resistor.

| Load Capacitance | Visible Sustained Oscillation |
|---|---|
| 100 pF | None |
| 220 pF | None |
| 470 pF | None |
| 1 nF | None |
| 2.2 nF | None |
| 4.7 nF | None |
| 10 nF | None |

Test conditions: 300 Ω resistive load, 1 kHz input frequency and 3 V peak input amplitude.

### Analysis

Removing the simulated load capacitance eliminated the observed instability, identifying capacitive loading as a significant contributing factor.

The output isolation resistor reduces the interaction between the amplifier output stage and capacitive load. Positioning the resistor outside the global feedback loop improved simulated stability at the expense of increased output impedance.

The asymmetric behaviour of the amplifier may also be influenced by differences between the NPN and PNP drive paths introduced by the VBE multiplier architecture.

Although 4.7 Ω initially eliminated visible oscillation during sinusoidal testing, subsequent large-signal and square-wave tests demonstrated limited damping margin.

A 10 Ω isolation resistor was ultimately selected as a practical compromise between output impedance, stability and circuit complexity.

Further compensation circuitry was considered but deferred pending physical measurements.

### Next Steps

- Characterize harmonic distortion following stabilization.
- Establish the maximum usable output voltage.
- Evaluate frequency response, output impedance and noise.
- Investigate clipping and overload recovery.

---

## 2026-11-23 — Rev 2: Amplifier Performance Characterization

### Objective

Quantitatively characterize the simulated electrical performance of the revised amplifier under representative headphone loading conditions.

### Changes

No major topology changes were introduced during initial performance characterization.

LTspice transient, Fourier, AC and noise analyses were used to investigate:

- Voltage gain.
- Harmonic distortion.
- Maximum output voltage and power.
- Frequency response.
- Output impedance.
- Output noise.
- Square-wave response.
- Overload behaviour and recovery.

### Results

**Harmonic Distortion and Output Power**

The following measurements were obtained using a 300 Ω load, 1 nF parallel load capacitance, 4.7 Ω output isolation resistance and a 1 kHz sinusoidal input.

| Input Peak Voltage | Output RMS Voltage | Output Power | Partial THD |
|---:|---:|---:|---:|
| 3.000 V | 4.177 V | 58.16 mW | 0.000083% |
| 4.000 V | 5.569 V | 103.39 mW | 0.000110% |
| 4.250 V | 5.917 V | 116.72 mW | 0.000128% |
| 4.375 V | 6.092 V | 123.69 mW | 0.000388% |
| 4.500 V | 6.261 V | 130.65 mW | 0.236306% |

The partial harmonic distortion values represent the harmonic components calculated through the twentieth harmonic.

At 4.375 V peak input, minor irregularities became visible in the output error waveform. Increasing the input amplitude to 4.5 V produced renewed oscillation and the onset of negative-side clipping.

A conservative output power objective of approximately 100 mW into 300 Ω was retained.

**Frequency Response**

Small-signal AC analysis demonstrated an approximately flat frequency response across the audible spectrum.

| Parameter | Simulated Result |
|---|---:|
| Passband voltage gain | 1.969 V/V |
| Passband gain | 5.885 dB |
| Audio-band magnitude variation | Visually below 0.001 dB |
| Phase shift at 20 kHz | Approximately −0.31° |

These measurements were obtained using the 4.7 Ω isolation resistor.

**Output Impedance**

Output impedance was characterized by injecting a normalized 1 A small-signal AC current at the headphone output with the input stimulus disabled and the simulated load disconnected.

| Frequency | Intrinsic Output Impedance | Total Output Impedance |
|---|---:|---:|
| 20 Hz | 0.130 mΩ | ~4.700 Ω |
| 1 kHz | 1.314 mΩ | ~4.700 Ω |
| 20 kHz | 26.16 mΩ | ~4.700 Ω |

The intrinsic output impedance was substantially lower than the total impedance introduced by the isolation resistor.

An initial measurement incorrectly indicated approximately 6.7 Ω of output impedance because the input AC stimulus remained enabled. Correcting the test configuration resolved the discrepancy.

**Noise Analysis**

| Parameter | Simulated Result |
|---|---:|
| Output noise spectral density | 29.12 nV/√Hz |
| Estimated integrated noise, 20 Hz–20 kHz | 4.12 µV RMS |
| Estimated SNR at 1 V RMS | 107.7 dB |
| Estimated SNR at 100 mW | 122.5 dB |

The nearly constant simulated noise spectral density permitted an approximate calculation of integrated RMS noise.

The accuracy of these results depends on the noise modelling capabilities of the available semiconductor and operational amplifier models.

**Square-Wave Response**

Square-wave testing was conducted at 10 kHz using input amplitudes of ±0.5 V and ±3 V.

The lower-amplitude test exhibited minor overshoot and rapidly decaying ringing.

The higher-amplitude test revealed more pronounced ringing during the positive-going transition, with an approximate ringing frequency of 6.6 MHz.

Increasing the output isolation resistance from 4.7 Ω to 10 Ω substantially improved damping. A further increase to 22 Ω was investigated but was not selected due to the additional output impedance.

**Overload Behaviour**

The amplifier was subsequently tested using the selected 10 Ω isolation resistor.

| Input Peak Voltage | Output RMS Voltage | Output Power | Reported Total THD |
|---:|---:|---:|---:|
| 4.5 V | 6.154 V | 126.23 mW | 0.230626% |
| 5.0 V | 6.704 V | 149.80 mW | 3.475448% |

Both conditions exhibited clipping but no visible sustained oscillation or high-frequency oscillation bursts.

Asymmetric clipping produced a non-zero average output voltage during sustained overload.

An additional recovery test applied a 5 V peak input for three milliseconds before reducing the amplitude to 0.5 V at a zero crossing.

The amplifier returned to normal signal tracking without visible persistent oscillation or abnormal recovery behaviour at the examined time scale.

### Analysis

The amplifier demonstrated promising simulated linearity, bandwidth and output capability into the intended 300 Ω load.

The output isolation resistor introduced a trade-off between low output impedance and capacitive load stability. Given the relatively high impedance of the intended headphones, a 10 Ω output resistance was considered an acceptable provisional design compromise.

The extremely low simulated harmonic distortion figures must not be interpreted as verified hardware performance. Model limitations, numerical precision, component tolerances, electrical noise and physical construction may substantially influence actual performance.

Additionally, several performance measurements were obtained before the final isolation resistor was selected. These measurements remain useful for documenting development but require confirmation using the finalized circuit configuration.

### Next Steps

- Complete final thermal characterization.
- Establish the final nominal DC operating point.
- Repeat representative performance measurements using the 10 Ω isolation resistor.
- Prepare the design for physical implementation.

---

## 2026-11-23 — Rev 2: Final Thermal Verification and Simulation Milestone

### Objective

Verify the thermal sensitivity and nominal DC operating point of the finalized circuit before proceeding to breadboard construction.

### Changes

- Finalized the output isolation resistor at 10 Ω.
- Retained the 47 Ω base stopper resistors.
- Retained the adjustable VBE multiplier and 5 Ω emitter resistors.
- Conducted uniform temperature and independent output transistor heating simulations.
- Established the final nominal operating point at 25°C.

### Results

**Uniform Temperature Sweep**

| Temperature | Approximate Quiescent Current |
|---:|---:|
| 0°C | 7.30 mA |
| 25°C | 4.75 mA |
| 50°C | 2.93 mA |
| 75°C | 1.72 mA |

The bias network exhibited decreasing quiescent current under uniform heating.

**Independent Output Transistor Heating**

Q1 and Q2 were subjected to increasing temperatures while Q3 was maintained at 25°C to represent an adverse thermal tracking condition.

| Output Transistor Temperature | Nominal Bias | Maximum Adjustable Bias |
|---:|---:|---:|
| 25°C | 4.75 mA | 14.45 mA |
| 75°C | 10.8 mA | 21.8 mA |

At the highest imposed temperature and maximum adjustable bias, the combined output transistor idle dissipation was approximately 523 mW.

These simulations characterize sensitivity to imposed transistor temperatures but do not model dynamic self-heating or establish immunity to thermal runaway.

**Final DC Operating Point**

Test conditions: ±12 V supply, 25°C, zero input voltage, 300 Ω load and 200 Ω nominal trimmer resistance.

| Parameter | Result |
|---|---:|
| Q1 emitter current | 4.75029 mA |
| Q2 emitter current | 4.74994 mA |
| Current mismatch | 0.0074% |
| Output DC offset | 31.33 µV |
| Bias network voltage | 1.72255 V |
| Emitter resistor voltage drop | ~23.75 mV |
| Positive rail current | 13.5223 mA |
| Negative rail current | 13.5272 mA |
| Total idle power per channel | 324.59 mW |

**Final Component Configuration**

| Component | Value |
|---|---|
| U1 | NE5532 |
| Q1 | BD139 |
| Q2 | BD140 |
| Q3 | 2N3904 |
| R1, R2 | 5 Ω |
| R3 | 300 Ω simulated load |
| R4, R5 | 10 kΩ |
| R6 | 5 kΩ |
| R7, R8, R9 | 2 kΩ |
| R10 | 1 kΩ + 220 Ω + 1 kΩ trimmer |
| R11, R12 | 47 Ω |
| R13 | 10 Ω |
| C1 | 1 nF simulated load capacitance |
| C2, C4 | 10 µF |
| C3, C5 | 100 nF |
| Supply | ±12 V DC |

R3 and C1 represent external headphone loading conditions and are not permanent amplifier components.

### Analysis

Revision 2 addresses several fundamental limitations identified during Revision 1, particularly quiescent current adjustment, bias sensitivity and capacitive load stability.

The finalized circuit achieves the nominal bias target within approximately 5% under simulation conditions.

Although imperfect thermal tracking produces an increase in quiescent current, the imposed temperature tests did not reveal excessive current within the examined conditions. Physical thermal coupling and self-heating must nevertheless be evaluated experimentally.

Further simulation optimization was deferred in favour of physical validation. Additional circuit complexity is not justified until actual hardware measurements establish a need for it.

### Outcome

Initial simulation characterization of Revision 2 is complete.

The circuit topology is provisionally frozen pending breadboard validation. The amplifier has not yet been demonstrated to meet its performance objectives in physical hardware.

### Next Steps

- Construct a single-channel breadboard prototype.
- Verify component pinouts, supply polarity and circuit continuity before applying power.
- Initialize the bias trimmer at maximum resistance.
- Apply current-limited power with zero input voltage.
- Adjust quiescent current using the voltage measured across the emitter resistors.
- Monitor quiescent current and transistor temperature during warm-up.
- Verify output DC offset before connecting sensitive equipment.
- Perform oscilloscope-based stability and waveform testing using a 300 Ω dummy load.
- Compare physical measurements against LTspice predictions.
- Verify power-on and power-off transients before connecting headphones.
- Proceed to stereo implementation and PCB development following successful physical validation.