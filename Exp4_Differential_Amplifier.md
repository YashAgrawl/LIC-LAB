
## DC, Transient, AC, and Large-Signal Study in 180 nm CMOS

---

## 1. Abstract

This report presents a detailed study of MOS differential amplifier topologies implemented in 180 nm CMOS technology. Three variants are considered:

1. **Circuit 1:** NMOS differential pair with resistive load  
2. **Circuit 2:** Differential amplifier with PMOS active load and NMOS tail current source  
3. **Circuit 3:** CMOS differential amplifier with PMOS current-mirror load and bias-controlled NMOS tail current source

For each circuit, the report examines the DC operating point, input common-mode range, output common-mode range, differential input range, transient response, AC frequency response, and gain behavior. The analysis combines hand calculations and LTspice simulation results. In addition to numerical results, the report explains the operating principles of differential pairs, current steering, saturation conditions, linear versus nonlinear operation, and the practical impact of device non-idealities.


---

## 2. Objective

The experiment aims to:

- design and analyze MOS differential amplifier circuits in 180 nm CMOS,
- verify transistor biasing and saturation conditions,
- determine gain, bandwidth, and unity-gain bandwidth,
- observe linear and nonlinear operation under small and large differential inputs,
- compare resistive-load and active-load architectures,
- study the effect of common-mode input level on circuit behavior.

---

## 3. Core Theory

A differential amplifier is an analog circuit that amplifies the difference between two input signals while suppressing any signal that is common to both inputs. This makes it one of the most important front-end blocks in operational amplifiers, instrumentation systems, communication receivers, and sensor readout circuits.

### 3.1 Differential and Common-Mode Inputs

The two input variables are defined as:

\[
V_{id} = V_{in1} - V_{in2}
\]

\[
V_{cm} = \frac{V_{in1} + V_{in2}}{2}
\]

The differential input is the useful signal, while the common-mode input usually represents noise, interference, or bias level.

### 3.2 Tail Current Concept

A differential pair uses a constant tail current source. The total current is fixed and is redistributed between the two branches depending on the input difference:

\[
I_{tail} = I_1 + I_2
\]

When both inputs are equal, the current splits equally. When one input becomes larger than the other, more current flows through the corresponding transistor. This current steering action is the mechanism that creates differential amplification.

### 3.3 Transconductance and Gain

The small-signal transconductance of a MOS transistor is:

\[
g_m = \frac{2I_D}{V_{ov}}
\]

where \(V_{ov}\) is the overdrive voltage. The gain of a differential stage depends on the load type:

- **Resistive load:**  
  \[
  A_d = g_m R_D
  \]

- **Active load:**  
  \[
  A_d = g_m (r_{o2} \parallel r_{o4})
  \]

A larger output resistance generally produces higher gain.

### 3.4 Common-Mode Rejection

A differential amplifier ideally amplifies only the difference between inputs and rejects common-mode signals. The effectiveness of this rejection is measured by:

\[
CMRR = \frac{A_d}{A_{cm}}
\]

A larger CMRR indicates better noise suppression and better immunity to unwanted interference.

### 3.5 Linear and Nonlinear Operation

For small differential inputs, both transistors in the pair remain on and in saturation. The output is approximately proportional to the input difference. For large differential inputs, one transistor begins to dominate while the other cuts off. This produces nonlinear behavior, distortion, and clipping.

A common small-signal approximation for the linear region is:

\[
|V_{id}| \leq V_{ov}
\]

A broader current-steering limit is often expressed as:

\[
|V_{id}| \leq \sqrt{2}V_{ov}
\]

Beyond this range, one branch may carry nearly all the current.

---

# Circuit 1 — NMOS Differential Amplifier with Resistive Load

---

## 4. Circuit Description

This topology uses two matched NMOS transistors as the differential pair and resistors as the drain loads. A constant tail current source biases the source nodes. The circuit is simple, easy to analyze, and useful for establishing the fundamental behavior of differential pairs.

### Circuit Diagram
<img width="895" height="735" alt="image" src="https://github.com/user-attachments/assets/c08e28ac-6d43-47cd-9f4a-e71198fd52ce" />

---

## 5. Design Specifications

| Parameter | Value |
|---|---:|
| \(V_{DD}\) | 0.9 V |
| \(V_{SS}\) | -0.9 V |
| \(V_T\) | 0.36 V |
| \(V_{inCM}\) | 0 V |
| \(V_P\) | -0.7 V |
| Power limit | ≤ 1.8 mW |
| \(\varepsilon_r\) | 3.9 |
| \(\varepsilon_0\) | \(8.854 \times 10^{-12}\) F/m |
| \(t_{ox}\) | \(4.1 \times 10^{-9}\) m |
| \(\mu_n\) | 273.809 cm²/Vs |
| \(\mu_p\) | 115.689 cm²/Vs |
| \(L\) | 480 nm |

---

## 6. Working Principle

The resistive-load differential amplifier works by steering current between the two input branches.

- When \(V_{in1} = V_{in2}\), the tail current splits equally.
- When \(V_{in1} > V_{in2}\), M1 conducts more current and M2 conducts less.
- When \(V_{in1} < V_{in2}\), M2 conducts more current and M1 conducts less.

Because the load elements are resistors, changes in current are converted directly into voltage changes at the drains.

The output relation is:

\[
V_{out} = V_{DD} - I_D R_D
\]

This means increased drain current lowers the corresponding output node voltage.

---

## 7. DC Calculations

### 7.1 Tail Current

Using the power constraint:

\[
P = VI
\]

\[
1.8\text{ mW} = 1.8\text{ V} \times I
\]

\[
I_{SS} = 1\text{ mA}
\]

### 7.2 Branch Current

Since the pair is balanced at quiescent bias:

\[
I_{D1} = I_{D2} = \frac{I_{SS}}{2}
\]

\[
I_D = 0.5\text{ mA}
\]

### 7.3 Load Resistance

At the midpoint output condition:

\[
R_D = \frac{V_{DD}}{I_D}
\]

\[
R_D = \frac{0.9}{0.5\times10^{-3}} = 1.8\text{ k}\Omega
\]

### 7.4 Gate-Source Voltage

\[
V_{GS} = V_G - V_S
\]

\[
V_{GS} = 0 - (-0.7) = 0.7\text{ V}
\]

### 7.5 Overdrive Voltage

\[
V_{ov} = V_{GS} - V_T
\]

\[
V_{ov} = 0.7 - 0.36 = 0.34\text{ V}
\]

### 7.6 Saturation Check

\[
V_{DS} = V_D - V_S = 0 - (-0.7) = 0.7\text{ V}
\]

\[
V_{DS} \geq V_{ov}
\]

\[
0.7 \geq 0.34
\]

So the devices operate in saturation.

### 7.7 Oxide Capacitance

\[
C_{ox} = \frac{\varepsilon_r\varepsilon_0}{t_{ox}}
\]

\[
C_{ox} = \frac{3.9 \times 8.854\times10^{-12}}{4.1\times10^{-9}}
\]

\[
C_{ox} = 8.42\times10^{-3}\text{ F/m}^2
\]

### 7.8 Mobility Conversion

\[
\mu_n = 273.809 \times 10^{-4} = 0.02738\text{ m}^2/\text{Vs}
\]

### 7.9 Width Estimation

Using the saturation current equation:

\[
I_D = \frac{1}{2}\mu_n C_{ox}\left(\frac{W}{L}\right)V_{ov}^2
\]

The calculated width is approximately:

\[
W \approx 18\ \mu\text{m}
\]

The report notes a length adjustment to **17.37 µm** to obtain the required 0.5 mA current in simulation.

---

## 8. Common-Mode and Differential Ranges

### 8.1 Input Common-Mode Range

\[
V_{inCM(min)} = V_S + V_T = -0.7 + 0.36 = -0.34\text{ V}
\]

\[
V_{inCM(max)} = V_D + V_T = 0 + 0.36 = 0.36\text{ V}
\]

Final range:

\[
-0.34\text{ V} \leq V_{inCM} \leq 0.36\text{ V}
\]

### 8.2 Output Common-Mode Range

\[
V_{outCM(max)} = V_{DD} = 0.9\text{ V}
\]

\[
V_{outCM(min)} = V_S + V_{ov} = -0.7 + 0.34 = -0.36\text{ V}
\]

Final range:

\[
-0.36\text{ V} \leq V_{outCM} \leq 0.9\text{ V}
\]

### 8.3 Differential Input Range

Linear region:

\[
|V_{id}| \leq V_{ov} = 0.34\text{ V}
\]

Current steering limit:

\[
|V_{id}| \leq \sqrt{2}V_{ov} \approx 0.48\text{ V}
\]

---

## 9. Operating Cases

### Case (i): \(V_{in1} = V_{in2}\)

The pair is perfectly balanced:

- \(I_{D1} = I_{D2}\)
- output nodes become equal
- the circuit rejects common-mode input ideally

### Case (ii): \(V_{in1} > V_{in2}\)

- M1 conducts more current
- M2 conducts less current
- \(V_{out1}\) decreases
- \(V_{out2}\) increases relative to \(V_{out1}\)

### Case (iii): \(V_{in1} < V_{in2}\)

- M2 conducts more current
- M1 conducts less current
- \(V_{out2}\) decreases
- \(V_{out1}\) becomes larger than \(V_{out2}\)

The output voltages are always opposite in nature because the current steering action converts the input difference into complementary drain-voltage variation.

---

## 10. Transient Analysis

### 10.1 Linear Condition

The report checks the condition:

\[
|V_{id}| < \sqrt{2}V_{ov}
\]

With \(V_{id} = 10\text{ mV}\), the circuit is well inside the linear region.

### Linear Output Waveform
<img width="1904" height="843" alt="image" src="https://github.com/user-attachments/assets/a4b87a21-6349-4af1-9d21-f0819cd16d2e" />

The output is sinusoidal and follows the input without visible clipping.

### 10.2 Nonlinear Condition

A much larger input is then applied:

\[
|V_{id}| > \sqrt{2}V_{ov}
\]

The report uses \(0.9\text{ V} > 0.48\text{ V}\), confirming nonlinear behavior.

### Nonlinear Output Waveform
<img width="1912" height="836" alt="image" src="https://github.com/user-attachments/assets/2d816808-bfe0-4157-b356-cd920770f804" />

The output becomes distorted, and the circuit no longer behaves as a linear amplifier.

---

## 11. Linear vs Nonlinear Operation

| Parameter | Linear Region | Nonlinear Region |
|---|---|---|
| Condition | \(|V_{id}| < \sqrt{2}V_{ov}\) | \(|V_{id}| > \sqrt{2}V_{ov}\) |
| Transistor state | Both ON | One OFF |
| Current distribution | Shared | Nearly all in one branch |
| Output | Sinusoidal / proportional | Distorted / clipped |
| Distortion | Low | High |
| Gain | Approximately constant | Variable |

---

## 12. Practical Gain

The simulated gain is derived as:

\[
A_v = \frac{\Delta V_{out}}{\Delta V_{in}}
\]

\[
A_v = \frac{46.29 + 46.29}{20} = 4.629\ \text{V/V}
\]

Gain in dB:

\[
A_v(dB) = 20\log(4.629) = 12.606\text{ dB}
\]

---

## 13. Theoretical Gain

Assuming:

\[
\lambda = 0.1\text{ V}^{-1}
\]

Output resistance:

\[
r_o = \frac{1}{\lambda I_D}
\]

\[
r_o = \frac{1}{0.1 \times 0.5\times10^{-3}} = 20\text{ k}\Omega
\]

Effective output resistance:

\[
r_{o,eff} = r_{o1}\parallel r_{o2} = 10\text{ k}\Omega
\]

Overall output resistance:

\[
R_{out} = r_{o,eff}\parallel R_D \approx 1.53\text{ k}\Omega
\]

Transconductance:

\[
g_m = \frac{2I_D}{V_{ov}} = \frac{2\times0.5\times10^{-3}}{0.34} = 2.94\text{ mS}
\]

Gain:

\[
A_v = g_m R_{out} = 4.498\ \text{V/V}
\]

Gain in dB:

\[
A_v(dB) = 13.06\text{ dB}
\]

### Gain Comparison

| Metric | Practical | Theoretical |
|---|---:|---:|
| Gain | 4.629 V/V | 4.498 V/V |
| Gain in dB | 12.606 dB | 13.06 dB |

The small discrepancy is expected due to channel-length modulation, parasitics, finite device matching, and simulation-model effects.

---

## 14. AC Analysis

### AC Response Plot
<img width="1915" height="864" alt="image" src="https://github.com/user-attachments/assets/8221a5b2-876f-44f9-85ee-88ca6cedde38" />

### Extracted Results

| Metric | Value |
|---|---:|
| Midband gain | 13.358 dB |
| Lower cutoff | 0 Hz |
| Upper cutoff | 10.186 GHz |
| Bandwidth | 10.186 GHz |

### Linear Gain

\[
A_v = 10^{13.358/20} \approx 4.78\ \text{V/V}
\]

### -3 dB Gain

\[
A_v(-3\text{ dB}) = 13.358 - 3 = 10.358\text{ dB}
\]

### Unity Gain Bandwidth

\[
UGB = A_v \times f_H = 4.78 \times 10.186\text{ GHz} \approx 48.68\text{ GHz}
\]

### Gain-Bandwidth Product

\[
GBP = A_v \times BW \approx 48.68\text{ GHz}
\]

---

## 15. Circuit 1 Summary

Circuit 1 is structurally the simplest of the three designs. It offers a clear illustration of differential current steering, but the resistive load limits integration efficiency and increases chip area. Its major advantage is conceptual clarity; its main limitation is the smaller gain achievable relative to active-load topologies.

---

# Circuit 2 — Differential Amplifier with PMOS Active Load and NMOS Tail Current Source

---

## 16. Circuit Description

This circuit replaces the resistive load with a PMOS current-mirror active load. The NMOS differential pair remains at the input, and an NMOS tail current source controls the branch current. This architecture is more compact and is typical of integrated op-amp input stages.

### Circuit Diagram
<img width="900" height="650" alt="image" src="https://github.com/user-attachments/assets/9cb741ce-a076-4d21-b389-7cac4ff3d927" />

### Main Advantages

- higher gain than resistive loading,
- improved output resistance,
- better power efficiency,
- smaller chip area,
- better common-mode rejection,
- single-ended output conversion through current mirroring.

---

## 17. Device Sizing and Saturation Analysis

### 17.1 M5 — NMOS Tail Current Source

The tail device is designed to provide approximately 1 mA. The report states:

- \(V_G \approx -0.334\text{ V}\)
- \(V_{GS} = 0.566\text{ V}\)
- \(V_{ov} = 0.2\text{ V}\)
- \(V_{DS} = 0.2\text{ V}\)

This places M5 at the edge of saturation.

Estimated width:

\[
W \approx 104\ \mu\text{m}
\]

The report notes a length adjustment to **219.7 µm** to obtain the target current.

### 17.2 M1 and M2 — NMOS Differential Pair

For the input pair:

\[
V_{GS} = 0.7\text{ V}
\]

\[
V_{ov} = 0.34\text{ V}
\]

\[
I_D = 0.5\text{ mA}
\]

\[
g_m = 2.94\text{ mS}
\]

Estimated width:

\[
W \approx 18\ \mu\text{m}
\]

The report notes a length adjustment to **30.625 µm** to obtain the required current.

### 17.3 M3 and M4 — PMOS Active Load

For the PMOS mirror load:

\[
V_{SG} = 0.9\text{ V}
\]

\[
V_{ov} = 0.54\text{ V}
\]

\[
V_{SD} = 0.9\text{ V}
\]

Estimated width:

\[
W \approx 16.9\ \mu\text{m}
\]

The report notes a length adjustment to **38.21 µm** to obtain the required current.

---

## 18. Input Common-Mode and Output Common-Mode Limits

### Input Common-Mode Range

\[
V_{inCM(min)} = V_{SS} + V_{ov(M5)} + V_T = -0.34\text{ V}
\]

\[
V_{inCM(max)} = V_{DD} - V_{ov(PMOS)} + V_T = 1.06\text{ V}
\]

Final range:

\[
-0.34\text{ V} \leq V_{inCM} \leq 1.06\text{ V}
\]

### Output Common-Mode Range

\[
V_{outCM(min)} = V_S + V_{ov} = -0.36\text{ V}
\]

\[
V_{outCM(max)} = V_{DD} = 0.9\text{ V}
\]

Final range:

\[
-0.36\text{ V} \leq V_{outCM} \leq 0.9\text{ V}
\]

### Interpretation

Compared with the resistive-load stage, the active-load differential pair can tolerate a wider common-mode input range, especially on the high side, because the PMOS mirror supports better headroom utilization.

---

## 19. Transient Analysis

### Linear Condition
<img width="1916" height="861" alt="image" src="https://github.com/user-attachments/assets/160def4a-bd40-4aeb-a5d0-6ac966117f6e" />

The circuit satisfies:

\[
|V_{id}| < \sqrt{2}V_{ov}
\]

### Nonlinear Condition
<img width="1913" height="847" alt="image" src="https://github.com/user-attachments/assets/52201c49-16df-45e8-9dd7-f6f6f48c6fe0" />

The large-signal case satisfies:

\[
|V_{id}| > \sqrt{2}V_{ov}
\]

This confirms the same underlying small-signal and large-signal transition behavior seen in Circuit 1.

---

## 20. Gain Calculation

### Practical Gain

\[
A_v = \frac{20.17 + 17.68}{20} = 1.89\ \text{V/V}
\]

\[
A_v(dB) = 20\log(1.89) = 5.529\text{ dB}
\]

### Theoretical Gain

The report uses:

\[
A_v = \sqrt{\frac{\mu_n(W/L)_n}{\mu_p(W/L)_p}}
\]

Substituting the values listed in the report:

\[
A_v \approx 1.718\ \text{V/V}
\]

\[
A_v(dB) \approx 4.70\text{ dB}
\]

### Gain Comparison

| Metric | Practical | Theoretical |
|---|---:|---:|
| Gain | 1.89 V/V | 1.72 V/V |
| Gain in dB | 5.529 dB | 4.70 dB |

The practical gain is slightly higher than the hand-calculated value because the hand model simplifies the transistor behavior and omits several second-order effects.

---

## 21. AC Analysis

### Frequency Response Plot
<img width="1907" height="891" alt="image" src="https://github.com/user-attachments/assets/9b3923fc-e60e-47c7-8c63-527d439951fa" />

### Midband Gain

\[
A_v(dB) = 5.55\text{ dB}
\]

\[
A_v = 10^{5.55/20} = 1.894\ \text{V/V}
\]

### Bandwidth

From the plot:

\[
f_L = 0\text{ Hz}
\]

\[
f_H = 2.96\text{ GHz}
\]

\[
BW = 2.96\text{ GHz}
\]

### Unity Gain Bandwidth

The plot shows:

\[
UGB = 5.19\text{ GHz}
\]

The theoretical estimate is:

\[
UGB = f_{3dB} \times A_v = 2.96 \times 1.894 \approx 5.60\text{ GHz}
\]

### Final AC Summary

| Parameter | Value |
|---|---:|
| Midband gain | 5.55 dB |
| Midband gain (linear) | 1.894 V/V |
| Bandwidth | 2.96 GHz |
| UGB (practical) | 5.19 GHz |
| UGB (theoretical) | 5.60 GHz |

---

## 22. Circuit 2 Summary

Circuit 2 offers a more integrated and efficient topology than Circuit 1. The active load increases output resistance and improves gain without requiring resistors. The trade-off is that the circuit becomes more sensitive to biasing and frequency compensation, especially when used in closed-loop systems.

---

# Circuit 3 — CMOS Differential Amplifier with PMOS Current Mirror Load and NMOS Tail Current Source

---

## 23. Circuit Description

Circuit 3 is the most complete integrated version of the differential amplifier studied in this experiment. It uses:

- NMOS differential pair,
- PMOS current-mirror load,
- NMOS tail current source,
- bias-controlled common-mode operation.

This is the most op-amp-like configuration in the report.

### Circuit Diagram
<img width="1079" height="665" alt="image" src="https://github.com/user-attachments/assets/c36c9b83-c1c1-4db0-80d5-73bc9a80cd4a" />

---

## 24. DC Analysis and Saturation Verification

### Given Conditions

- \(V_{DD} = 0.9\text{ V}\)
- \(V_{SS} = -0.35\text{ V}\)
- \(V_{in1}\) and \(V_{in2}\) are differential sinusoids with 20 mV differential amplitude
- \(V_{out1} = 0.3\text{ V}\)
- \(V_{out2} = 0.3\text{ V}\)
- \(V_p = -0.7\text{ V}\)
- \(V_{g5} = 0.366\text{ V}\)
- \(V_t \approx 0.366\text{ V}\)

### Saturation Conditions

For NMOS:

\[
V_{DS} \geq V_{GS} - V_t
\]

For PMOS:

\[
V_{SD} \geq V_{SG} - |V_t|
\]

### Device Region Results

#### M1 and M2
- \(V_{GS} = 0.7\text{ V}\)
- \(V_{DS} = 1.0\text{ V}\)

Since \(1.0 \geq 0.334\), both NMOS input devices are in saturation.

#### M3 and M4
- \(V_{SD} = 0.6\text{ V}\)
- \(V_{SG} = 0.6\text{ V}\)

Since \(0.6 \geq 0.234\), both PMOS load devices are in saturation.

#### M5
The tail transistor is at the boundary of saturation:

\[
V_{GS5} = 0.716\text{ V}
\]

\[
V_{DS5} = 0.35\text{ V}
\]

\[
V_{DS5} = V_{GS5} - V_t
\]

So M5 operates at the edge of the saturation region.

### Operating Point Values

From the report:

- \(V_{out1} = 0.8876\text{ V}\)
- \(V_{out2} = 0.8876\text{ V}\)
- \(V_p = -0.7218\text{ V}\)
- \(I_D(M5) = 1.0487\text{ mA}\)
- \(I_D(M1) = I_D(M2) = 0.5243\text{ mA}\)

This confirms equal current splitting and balanced biasing.

### DC Snapshot
<img width="853" height="627" alt="image" src="https://github.com/user-attachments/assets/2992dc79-1ea6-4a53-b3e0-66c8f034bc5f" />

---

## 25. Transient Response at Zero DC Input

### Simulation Setup

- \(V_{in1} = \text{SINE}(0, 10\text{ mV}, 1\text{ kHz})\)
- \(V_{in2} = \text{SINE}(0, -10\text{ mV}, 1\text{ kHz})\)

The differential input is 20 mV peak-to-peak.

### Observed Output

- sinusoidal waveform,
- centered near the DC operating point,
- no visible distortion,
- small output amplitude around the quiescent bias.

### Transient Plots
<img width="1899" height="428" alt="image" src="https://github.com/user-attachments/assets/1d7edbd6-d2d3-458c-bc75-6ce0eacb78d2" />
<img width="1900" height="450" alt="image" src="https://github.com/user-attachments/assets/9f59e6e1-cff8-4c79-b1eb-27df99c727e1" />
<img width="1898" height="387" alt="image" src="https://github.com/user-attachments/assets/fe0d7791-d13d-47cb-b1f1-3e09705dcd61" />

### Interpretation

The clean sinusoidal response indicates that the amplifier is operating in the intended linear region. The small output swing is consistent with the low supply voltage and the finite headroom required by the stacked MOS devices.

---

## 26. AC Analysis

### AC Plot
<img width="1919" height="486" alt="image" src="https://github.com/user-attachments/assets/a2fb9fb8-4477-46cb-947c-80f569511f34" />

### Simulation Setup

The report indicates a small-signal sweep and measures frequency response, gain, and bandwidth.

### Results Mentioned in the Report

- Midband gain: **5.5504 dB**
- Linear gain: **1.896 V/V**
- Phase at low frequency: **−194.822°**
- Roll-off: approximately **−20 dB/decade**
- Dominant pole: roughly **1–5 Hz**

### Interpretation

The close match between simulated and estimated gain suggests that the active load behaves as intended. The negative excess phase points to additional poles and parasitic capacitances that are not captured by the simplest hand calculations.

---

## 27. Common-Mode Boundary Behavior

The report studies operation at both the minimum and maximum common-mode input levels.

### 27.1 At \(V_{CM(min)}\)

- the tail source is just at the edge of saturation,
- the input NMOS pair is barely on,
- the output shifts upward,
- gain becomes very small or vanishes.

### 27.2 At \(V_{CM(max)}\)

- the PMOS load approaches triode,
- output is pulled low,
- output amplitude shrinks,
- gain collapses near the boundary.

### Boundary Summary
<img width="1919" height="433" alt="image" src="https://github.com/user-attachments/assets/c65eea41-9d66-4b7c-91ff-53275472aef4" />
<img width="1919" height="452" alt="image" src="https://github.com/user-attachments/assets/36fc352a-bfec-42f0-b98a-d70308f0010c" />
<img width="1908" height="439" alt="image" src="https://github.com/user-attachments/assets/ae1932d7-7569-4834-9565-601a0e12a8fa" />
<img width="1912" height="431" alt="image" src="https://github.com/user-attachments/assets/3796a5bd-35aa-485d-b246-3718ae6b2f9f" />

### Interpretation

At the lower boundary, the circuit loses transconductance because the differential pair is nearly off. At the upper boundary, the active load loses headroom and the gain drops due to reduced saturation margin. In both cases, the circuit remains mathematically biased but ceases to be an effective amplifier.

---

## 28. Large-Signal Analysis

The report includes two explicit large-signal cases to demonstrate current steering.

### Case 1: \(V_{id} < \sqrt{2}V_{ov}\)

Given:

- \(V_g = 0\text{ V}\)
- \(V_s = -0.7\text{ V}\)
- \(V_t = 0.366\text{ V}\)
- \(V_{in1} = 0.3\text{ V}\)
- \(V_{in2} = 0.1\text{ V}\)

Compute:

\[
V_{gs} = 0 - (-0.7) = 0.7\text{ V}
\]

\[
V_{ov} = 0.7 - 0.366 = 0.334\text{ V}
\]

\[
\sqrt{2}V_{ov} = 0.472\text{ V}
\]

\[
V_{id} = 0.3 - 0.1 = 0.2\text{ V}
\]

Since \(0.2 < 0.472\), the circuit operates in the linear region.

Operating-point results:

- \(I_D(M1) = 0.0005067\text{ A}\)
- \(I_D(M2) = 0.0004810\text{ A}\)
- \(I_D(M5) = 0.0009877\text{ A}\)
- \(V_{out1} = -0.660\text{ V}\)
- \(V_{out2} = -0.626\text{ V}\)

### Case 1 Waveform
<img width="1915" height="438" alt="image" src="https://github.com/user-attachments/assets/6d262478-d51d-471a-aab6-09713a9b81f0" />

### Case 2: \(V_{id} > V_{ov}\) and \(V_{id} > \sqrt{2}V_{ov}\)

Given:

- \(V_{in1} = 0.7\text{ V}\)
- \(V_{in2} = 0.2\text{ V}\)

Then:

\[
V_{id} = 0.7 - 0.2 = 0.5\text{ V}
\]

\[
V_{id} > V_{ov} = 0.334\text{ V}
\]

\[
V_{id} > \sqrt{2}V_{ov} = 0.472\text{ V}
\]

This confirms nonlinear current steering.

Operating-point results:

- \(I_D(M1) = 0.000511\text{ A}\)
- \(I_D(M2) = 0.000486\text{ A}\)
- \(I_D(M5) = 0.000997\text{ A}\)
- \(V_{out1} = -0.666\text{ V}\)
- \(V_{out2} = -0.633\text{ V}\)

### Case 2 Waveform
<img width="1907" height="463" alt="image" src="https://github.com/user-attachments/assets/b9088edd-8658-473a-bc5a-a8958090b160" />

### Interpretation

In the linear case, current splits more or less evenly, and the output remains a faithful amplified version of the input. In the nonlinear case, current shifts toward one side, and the amplifier begins to behave more like a switching device than a linear analog stage.

---

## 29. Comparison of Linear and Nonlinear Regions

| Parameter | \(V_{id} < \sqrt{2}V_{ov}\) | \(V_{id} > \sqrt{2}V_{ov}\) |
|---|---|---|
| MOSFET state | Both in saturation | One approaches cutoff |
| Current split | Shared | Unequal |
| Output shape | Sinusoidal | Distorted |
| Linearity | High | Low |
| Gain | Approximately constant | Input-dependent |
| Symmetry | Preserved | Broken |

---

## 30. AC Analysis Interpretation for Circuit 3

The AC response section in the report indicates:

- midband gain around 5.55 dB,
- a linear gain around 1.896 V/V,
- gain roll-off beyond the low-frequency band,
- phase lag exceeding the ideal \(-180^\circ\) due to parasitics.

The report attributes the deviation between ideal and simulated values to:

- channel-length modulation,
- body effect,
- finite output resistance,
- parasitic capacitances,
- internal poles,
- simplified hand-calculation assumptions.

These are expected in a real BSIM-based model.

---

# 31. Cross-Circuit Comparison

| Feature | Circuit 1 | Circuit 2 | Circuit 3 |
|---|---|---|---|
| Load type | Resistive | PMOS active load | PMOS current mirror load |
| Tail source | Current source | NMOS current source | Bias-controlled NMOS source |
| Gain | Moderate | Higher than C1 in small-signal form | Similar active-load behavior |
| Integration density | Low | Higher | Higher |
| Common-mode headroom | Narrower | Wider | Controlled by biasing |
| Bandwidth | Very wide in the resistive case | Several GHz in the report | Strongly influenced by parasitics |
| Complexity | Lowest | Medium | Highest |

### Boundary Behavior Summary

| Condition | Observation |
|---|---|
| At \(V_{CM(min)}\) | Transistor barely on, \(g_m\) collapses, output becomes flat |
| At \(V_{CM(max)}\) | Load loses saturation margin, gain falls sharply |

---

# 32. Practical Discussion

## 32.1 Why the practical and theoretical values differ

The report repeatedly shows that hand calculations and LTspice simulations are close but not identical. This is expected because the hand calculations generally assume ideal or first-order models. In practice, the following effects matter:

- channel-length modulation,
- finite output resistance,
- mismatch between devices,
- body effect,
- parasitic capacitances,
- wiring and layout parasitics,
- model-specific second-order effects.

## 32.2 Why active loads are preferred

Active loads improve gain because they replace a simple resistor with a transistor operating as a high-value dynamic load. That raises output resistance and increases amplification per unit current.

## 32.3 Why common-mode limits matter

The common-mode range tells you where the amplifier is actually usable. Outside the range:

- the tail source can leave saturation,
- input transistors can lose conduction,
- active loads can enter triode,
- gain falls,
- output distortion increases.

---

# 33. Final Conclusion

This experiment demonstrates the behavior of MOS differential amplifiers across three useful topologies. The resistive-load design is the simplest and offers intuitive current-steering behavior. The active-load design improves integration efficiency and gain. The bias-controlled CMOS design provides a more realistic front-end stage and clearly shows how saturation margins and common-mode limits govern amplifier operation.

The most important conclusions are:

- differential amplifiers amplify input difference and reject common-mode signals,
- linear operation requires the input difference to remain within the overdrive-based limit,
- the circuit gain depends strongly on the load implementation,
- common-mode headroom determines whether the circuit remains functional,
- simulation results support the theoretical behavior expected from MOS differential pair theory.


---

## 34. Quick Result Snapshot

| Circuit | Practical Gain | Practical Gain (dB) | Key Frequency Result |
|---|---:|---:|---|
| Circuit 1 | 4.629 V/V | 12.606 dB | 13.358 dB midband, 10.186 GHz BW |
| Circuit 2 | 1.89 V/V | 5.529 dB | 5.55 dB midband, 2.96 GHz BW |
| Circuit 3 | ~1.896 V/V | ~5.55 dB | Dominant pole behavior, parasitic phase lag |

---

