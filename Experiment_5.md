
# Experiment 5 — Operational Amplifier Configurations in LTspice
## Voltage Follower and Non-Inverting Amplifier Using uA741

---

## Abstract

This experiment studies two canonical negative-feedback op-amp configurations: the voltage follower and the non-inverting amplifier. Both circuits were implemented and simulated using a uA741 operational amplifier model. The work focuses on closed-loop gain, waveform fidelity, supply-limited output swing, and frequency response. The voltage follower is evaluated as a unity-gain buffer, while the non-inverting amplifier is evaluated as a fixed-gain voltage amplifier.

The purpose of this experiment is not only to verify textbook equations, but also to observe how an actual op-amp model behaves under finite supply rails, practical output limits, and high-frequency non-idealities.

---

## 1. Introduction

Operational amplifiers are used in analog circuits whenever a signal must be buffered, scaled, or conditioned with high input impedance and low output impedance. In closed-loop operation, the amplifier does not behave like an uncontrolled high-gain device; instead, the feedback network determines the effective gain and stability.

Two of the most important closed-loop op-amp circuits are:

- **Voltage follower**: output directly follows the input, giving unity gain.
- **Non-inverting amplifier**: output remains in phase with the input and is amplified by a resistor-set gain.

These two circuits are fundamental because they demonstrate the main strengths of op-amps: input buffering, controlled gain, and strong rejection of loading effects.

---

## 2. Design Summary

| Parameter | Value |
|---|---:|
| Op-amp model | uA741 |
| Supply rails | ±13 V |
| Input frequency | 1 kHz |
| Input amplitude | 5 V peak (10 Vpp) |
| Load resistance | 4.7 kΩ |
| Non-inverting gain target | 11 |
| Feedback network | 10 kΩ / 1 kΩ |

---

# Circuit 1 — Voltage Follower (Buffer)

---

## 3. Objective

The voltage follower is used to verify unity gain behavior. It is one of the simplest closed-loop op-amp configurations and is commonly used whenever a source must drive a load without suffering from loading effects.

Typical applications include:

- signal buffering,
- impedance matching,
- isolating amplifier stages,
- driving low-resistance loads,
- preserving waveform shape.

---

## 4. Circuit Diagram

<img width="808" height="463" alt="Voltage follower schematic" src="https://github.com/user-attachments/assets/311cdfff-2989-4935-bd4b-4033ae829907" />

---

## 5. Theory

A voltage follower is a special case of a non-inverting amplifier. The output is fed directly back to the inverting input, so the feedback factor is unity.

For a non-inverting amplifier:

\[
A_v = 1 + \frac{R_f}{R_1}
\]

For a follower:

\[
R_f = 0,\quad R_1 \to \infty
\]

Therefore:

\[
A_v = 1
\]

and

\[
V_{out} = V_{in}
\]

Because the op-amp uses negative feedback, the input terminals are forced to nearly the same voltage:

\[
V_+ \approx V_-
\]

This is the virtual-short condition. The circuit does not amplify; it reproduces the input with essentially no change in magnitude or phase.

---

## 6. DC Behavior

<img width="1498" height="672" alt="Voltage follower DC analysis" src="https://github.com/user-attachments/assets/bae4ba49-dad8-4bc7-aaae-66d405db06b2" />

The DC operating condition confirms that the output sits at the same bias level as the input. Since the follower gain is unity, the output node is simply the buffered version of the source node.

At this stage, the most important observation is that the output remains comfortably inside the available supply range. With ±13 V rails, the op-amp has adequate headroom for the chosen input amplitude.

---

## 7. Transient Analysis

<img width="1915" height="878" alt="Voltage follower transient" src="https://github.com/user-attachments/assets/4e850f6f-d01b-4289-b9df-63ed2e8b13c0" />

### 7.1 Input waveform

The input is a sinusoid:

\[
V_{in}(t)=5\sin(2\pi \cdot 1\,\text{kHz} \cdot t)
\]

with:

- amplitude = 5 V,
- peak-to-peak value = 10 V,
- frequency = 1 kHz.

### 7.2 Output waveform

The output waveform has the same amplitude, same frequency, and same phase as the input:

\[
V_{out}(t) \approx V_{in}(t)
\]

### 7.3 Observations

- No phase inversion is present.
- No amplitude scaling is visible.
- No clipping occurs.
- The waveform shape is preserved exactly.
- The load does not disturb the source.

### 7.4 Interpretation

This is the expected behavior of an ideal buffer. The main value of the circuit is not gain, but isolation: the source is protected from the load, and the load receives the same waveform with low output impedance.

---

## 8. AC Analysis

<img width="1919" height="882" alt="Voltage follower AC analysis" src="https://github.com/user-attachments/assets/7dd0c26b-c535-4613-affa-ac707745aa19" />

### 8.1 Frequency-response interpretation

The follower maintains approximately unity gain through the useful band. At very high frequency, the response shows a dip due to the internal pole-zero structure of the uA741 model and the wide sweep range used in simulation.

This is not a normal low-frequency design effect. It reflects the non-ideal internal behavior of the op-amp model when the analysis extends far beyond the practical operating range.

### 8.2 Key result

| Quantity | Result |
|---|---:|
| Closed-loop gain | 1 |
| Phase shift | approximately 0° |
| Distortion | negligible |
| Buffering action | excellent |

---

## 9. Voltage Follower Result

The voltage follower behaves correctly as a unity-gain buffer. It reproduces the input waveform without amplitude loss, phase shift, or clipping, making it suitable for impedance matching and signal isolation.

---

# Circuit 2 — Non-Inverting Amplifier

---

## 10. Objective

The non-inverting amplifier is used to verify controlled gain amplification with preserved phase. Unlike the follower, this circuit applies a resistive feedback network, so the output magnitude is intentionally greater than the input.

---

## 11. Circuit Diagram

<img width="627" height="492" alt="Non-inverting amplifier schematic" src="https://github.com/user-attachments/assets/52e41770-1bf0-4818-a1f8-be09877d09ea" />

---

## 12. Theory

For a non-inverting amplifier, the closed-loop gain is:

\[
A_v = 1 + \frac{R_f}{R_1}
\]

The input is applied to the non-inverting terminal, while the inverting terminal receives a divided version of the output through the feedback network. Negative feedback forces the inverting node to track the non-inverting node, so the gain is fixed by resistor ratio.

This configuration has three major strengths:

- high input impedance,
- no phase inversion,
- predictable gain.

---

## 13. Design Values

| Parameter | Value |
|---|---:|
| Supply rails | ±13 V |
| Desired closed-loop gain | 11 |
| \(R_1\) | 1 kΩ |
| \(R_f\) | 10 kΩ |
| Input amplitude | 5 V peak |
| Input peak-to-peak | 10 Vpp |

Using the gain equation:

\[
A_v = 1 + \frac{10k\Omega}{1k\Omega} = 11
\]

So the ideal output relation is:

\[
V_{out} = 11V_{in}
\]

---

## 14. DC Analysis

<img width="1449" height="688" alt="Non-inverting amplifier DC analysis" src="https://github.com/user-attachments/assets/9b2fe40e-a608-4034-87ac-ad3c527f1201" />

The DC operating point shows that the circuit is correctly biased before the transient signal is applied. In a closed-loop op-amp, a valid DC operating point is important because it confirms that the amplifier is not already saturated before the AC or transient input begins.

---

## 15. Transient Analysis

<img width="1915" height="852" alt="Non-inverting amplifier transient" src="https://github.com/user-attachments/assets/03642766-0efe-4fd1-b969-0aabe5f285da" />

### 15.1 Ideal output expectation

With an input peak of 5 V:

\[
V_{out} = 11 \times 5 = 55V
\]

But the supply rails are only ±13 V, so the op-amp cannot produce that swing.

### 15.2 Practical result

Because the output demand exceeds the rail limits, the waveform saturates near the supply boundaries and clipping appears at the peaks.

### 15.3 Interpretation

- The amplifier still provides phase-preserving gain.
- The output is larger than the input.
- Saturation occurs because the requested output exceeds the allowed output swing.
- The distortion is caused by supply limitations, not by incorrect feedback.

---

## 16. Frequency Response

<img width="1916" height="826" alt="Non-inverting amplifier AC analysis" src="https://github.com/user-attachments/assets/76d95900-242a-44f7-a7e8-0b5b597dea07" />

### 16.1 Midband gain

For the chosen feedback network:

\[
A_v = 11
\]

\[
A_v(dB) = 20\log_{10}(11) \approx 20.8\,\text{dB}
\]

### 16.2 Bandwidth interpretation

The gain stays approximately constant in the midband region and then rolls off at higher frequency. This is the expected behavior of a practical op-amp because the open-loop gain drops with frequency.

### 16.3 High-frequency behavior

At high frequency:

- the closed-loop gain starts falling,
- phase lag increases,
- the amplifier becomes less ideal,
- the finite gain-bandwidth product becomes visible.

---

## 17. Non-Inverting Amplifier Result

The non-inverting amplifier achieves the expected closed-loop gain of 11 in theory, but the practical waveform clips because the op-amp is limited by ±13 V rails. The circuit remains phase-preserving and provides substantial amplification, but the signal amplitude must be chosen so the output stays within the supply limits.

---

# Comparative Discussion

---

## 18. Voltage Follower vs Non-Inverting Amplifier

| Feature | Voltage Follower | Non-Inverting Amplifier |
|---|---|---|
| Closed-loop gain | 1 | 11 |
| Phase relation | In phase | In phase |
| Feedback network | Direct connection | Resistor divider |
| Output swing risk | Low | High |
| Clipping behavior | None | Present at large input |
| Main use | Buffering | Amplification |

---

## 19. Practical Observations

### 19.1 Why the follower is useful

The follower is the best choice when the input source must not be loaded. It keeps the signal intact while providing a low-impedance drive to the next stage.

### 19.2 Why the non-inverting amplifier clips

The gain of 11 demands an output swing that exceeds the available supply voltage for the chosen input amplitude. The op-amp cannot create an output beyond its rails, so the waveform flattens near the top and bottom limits.

### 19.3 Why the AC plots matter

The frequency response confirms that the op-amp is not ideal. The finite bandwidth and internal compensation of the uA741 shape the response at higher frequencies, which is exactly what is expected from a realistic model.

---

# 20. Final Results

| Quantity | Voltage Follower | Non-Inverting Amplifier |
|---|---:|---:|
| Gain | 1 | 11 |
| Gain in dB | 0 dB | 20.8 dB |
| Output phase | In phase | In phase |
| Output distortion | None | Clipping due to rails |
| Main function | Buffer | Amplifier |

---

# 21. Conclusion

This experiment demonstrated the two most important closed-loop op-amp configurations.

The voltage follower behaved as a true unity-gain buffer: the output tracked the input, the phase remained unchanged, and the circuit prevented loading of the source.

The non-inverting amplifier provided the expected gain of 11, but the selected input amplitude caused the output to exceed the ±13 V supply rails, producing clipping. This clearly shows that op-amp gain alone does not determine the usable output; the supply limits must also be respected.

Overall, the experiment confirms the role of negative feedback in setting gain and preserving stability, while also showing the practical limits imposed by real op-amp models.
