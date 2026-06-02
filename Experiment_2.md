
# MOSFET Amplifier Analysis in 180 nm CMOS


---

## 1. Introduction

MOSFET amplifiers are the backbone of analog integrated circuits. They are used to amplify weak sensor signals, provide front-end gain in communication systems, and form the core input stages of operational amplifiers. Across all of the circuits studied here, the same design ideas appear again and again:

- the transistor bias point must be inside saturation,
- the gain depends strongly on the load implementation,
- source degeneration improves linearity but lowers gain,
- active loads improve output resistance and integration density,
- differential pairs reject common-mode noise and amplify only the difference between inputs.

The report is divided into two parts:

1. **Common-source amplifier configurations**
2. **Differential amplifier configurations**

Each part includes DC, transient, and AC analysis together with the most useful simulation figures.

---

# Part I — Common-Source Amplifier Configurations

---

## 2. Experiment 2A — Common Source Amplifier with PMOS Active Load and Source Degeneration

### Circuit diagram
<img width="611" height="542" alt="Circuit 2A" src="https://github.com/user-attachments/assets/36bd7707-1d34-4c4c-9641-d3b50926e8b9" />

### 2.1 Circuit idea

This circuit uses:

- **M1** as the amplifying NMOS transistor,
- **M2** as the PMOS active load,
- **Rs = 600 Ω** as the source degeneration resistor,
- **VDD = 1.5 V**,
- a sinusoidal input of **10 mV at 1 kHz**.

The PMOS active load replaces a passive resistor. This raises the output resistance and improves gain while keeping the circuit compact. The source resistor adds local negative feedback, which improves linearity and stabilizes the operating point.

### 2.2 DC design values

| Quantity | Value |
|---|---:|
| Supply voltage | 1.5 V |
| Power target | about 0.5 mW |
| Drain current | 300 µA |
| Overdrive voltage | 0.25 V |
| NMOS gate bias | 0.81 V |
| Source drop across Rs | 0.2 V |
| Output bias | 0.95 V |
| Source resistor | 600 Ω |

The bias relation is:

\[
V_{GS} = V_{TH} + V_{OV} = 0.366 + 0.25 = 0.61\text{ V}
\]

\[
V_{out} = \frac{V_{DD}}{2} + 0.2 = 0.95\text{ V}
\]

\[
R_S = \frac{0.2}{300\ \mu A} = 600\ \Omega
\]

The NMOS gate voltage used for biasing is:

\[
V_G = V_{GS} + V_{RS} = 0.61 + 0.2 = 0.81\text{ V}
\]

### 2.3 Saturation check

For the NMOS device:

\[
V_{DS} \geq V_{GS} - V_T
\]

Using the chosen bias point, this condition is satisfied, so the transistor remains in saturation.

For the PMOS active load:

\[
V_{SG} = V_{OV} + |V_{TP}| = 0.25 + 0.39 = 0.64\text{ V}
\]

\[
V_G = V_{DD} - V_{SG} = 1.5 - 0.64 = 0.86\text{ V}
\]

The PMOS device also remains in saturation.

### 2.4 Practical gain from transient response

The measured small-signal gain is:

\[
A_v = \frac{\Delta V_{out}}{\Delta V_{in}} = \frac{0.992 - 0.423}{0.819 - 0.8003}
\]

\[
A_v = 6.309\ \text{V/V}
\]

\[
A_v(dB) = 20\log_{10}(6.309) \approx 16\text{ dB}
\]

### 2.5 Theoretical gain

With channel-length modulation included:

\[
g_m = \frac{2I_D}{V_{OV}} = \frac{2 \times 200\ \mu A}{0.25} = 1.6\ \text{mS}
\]

\[
r_o = \frac{1}{\lambda I_D} = \frac{1}{0.1 \times 200\ \mu A} = 50\text{ k}\Omega
\]

\[
r_{o1}\parallel r_{o2} = 25\text{ k}\Omega
\]

\[
A_v = \frac{-g_m(r_{o1}\parallel r_{o2})}{1 + g_mR_S}
= \frac{- (1.6\times10^{-3})(25\times10^3)}{1 + (1.6\times10^{-3})(600)}
\]

\[
A_v \approx -15.38\ \text{V/V}
\]

\[
A_v(dB) \approx 23.74\text{ dB}
\]

The gap between theory and simulation is expected because the hand calculation uses idealized assumptions, while SPICE includes finite output resistance, channel-length modulation, and parasitic capacitances.

### 2.6 Transient behavior
<img width="1910" height="829" alt="Transient 2A" src="https://github.com/user-attachments/assets/0d539043-e116-4903-a16f-c47d3b0eeee9" />

The output waveform remains sinusoidal and does not clip. That confirms the circuit is operating in the intended linear region for small-signal amplification.

### 2.7 AC response
<img width="1904" height="761" alt="AC 2A-1" src="https://github.com/user-attachments/assets/cc08549c-7730-4b3b-80b3-ecd62f104316" />
<img width="1910" height="814" alt="AC 2A-2" src="https://github.com/user-attachments/assets/fa3fda83-2f89-47bf-af96-22ff5688ab1c" />

From the frequency response:

| Parameter | Value |
|---|---:|
| Midband gain | 16.473 dB |
| High cutoff frequency | 371.525 MHz |
| Lower cutoff frequency | approximately 0 Hz |
| Bandwidth | 371.525 MHz |

Using the linear midband gain:

\[
A_v \approx 10^{16.473/20} \approx 6.3
\]

The direct 0 dB crossing gives a unity-gain frequency near **4.03 GHz**, while the gain-bandwidth estimate gives about **5.7 GHz**. The difference comes from non-dominant poles and parasitic capacitances.

---

## 3. Experiment 2B — Common Source Amplifier with NMOS Current Source and PMOS Active Load

### Circuit diagram
<img width="1920" height="1200" alt="Circuit 2B" src="https://github.com/user-attachments/assets/47ef5df8-8cc9-4037-bc75-eabc65034644" />

### 3.1 Circuit idea

This configuration replaces the source resistor with an NMOS current source. That improves bias stability and keeps the operating point more rigid. The PMOS active load still provides high output resistance, but the finite resistance of the tail device introduces a degeneration-like effect that reduces gain.

### 3.2 DC operating point

<img width="1920" height="1200" alt="DC 2B" src="https://github.com/user-attachments/assets/ef543fa7-7d9a-4ff0-8a08-05e58cdc49f6" />

The reported DC values are:

| Quantity | Value |
|---|---:|
| Output voltage | 1.05035 V |
| \(I_D(M1)\) | 302.842 µA |
| \(I_D(M2)\) | 302.842 µA |
| \(I_D(M3)\) | 302.842 µA |

The current is very close to the intended 300 µA target, which shows that the biasing is well controlled.

### 3.3 Transient gain
<img width="1920" height="1200" alt="Transient 2B" src="https://github.com/user-attachments/assets/258814bb-f5f9-4d4e-9b61-79519456082a" />

The measured input and output swings are:

\[
V_{in(p-p)} = 0.9095 - 0.8913 = 18.2\text{ mV}
\]

\[
V_{out(p-p)} = 1.0656 - 1.0372 = 28.4\text{ mV}
\]

\[
A_v = \frac{28.4}{18.2} = 1.5604
\]

\[
A_v(dB) = 20\log_{10}(1.5604) = 3.8649\text{ dB}
\]

### 3.4 Small-signal interpretation

The source degeneration effect caused by the finite output resistance of the NMOS current source can be written as:

\[
A_v = \frac{-g_{m1}r_{o2}}{1 + g_{m1}r_{o3}}
\]

Using the values from the design:

\[
g_{m1} = 2.4\text{ mS}, \quad r_{o2} = r_{o3} = 33.33\text{ k}\Omega
\]

\[
A_v \approx -0.99
\]

That hand estimate is lower than the transient result because the full circuit behavior includes device interaction and the precise SPICE model used in the simulation.

### 3.5 AC response
<img width="1920" height="1200" alt="AC 2B" src="https://github.com/user-attachments/assets/e087b91e-bc22-4298-b815-366ae97b9c4c" />

From the Bode plot:

| Parameter | Value |
|---|---:|
| Midband gain | 3.530 dB |
| Upper cutoff frequency | 184.562 MHz |
| Lower cutoff frequency | approximately 0 Hz |
| Bandwidth | 184.562 MHz |

This version gives lower gain than Experiment 2A, but the bias point is more robust and the design behaves cleanly at higher frequency.

---

## 4. Experiment 2C — Common Source Amplifier with Diode-Connected NMOS and PMOS Active Load

### Circuit diagram
<img width="1920" height="1200" alt="Circuit 2C" src="https://github.com/user-attachments/assets/19e29823-e10f-43ca-a4ef-79c9f3866342" />

### 4.1 Circuit idea

In this circuit, the source degeneration element is replaced by a diode-connected NMOS. Since the gate and drain are shorted, the device behaves like a nonlinear small-signal resistor:

\[
r_d \approx \frac{1}{g_m + g_{ds}}
\]

This improves bias stability and makes the circuit easier to self-bias, while still keeping the implementation compact.

### 4.2 DC operating point

The important bias relations used in the design are:

\[
V_{S1} = V_{GS3} = 0.61\text{ V}
\]

\[
V_{IN} = V_{GS1} + V_{S1} = 1.22\text{ V}
\]

\[
V_{OUT} = \frac{V_{DD}}{2} + V_{DS3} = 1.36\text{ V}
\]

### 4.3 Transient response
<img width="1920" height="1200" alt="Transient 2C" src="https://github.com/user-attachments/assets/a454f8f7-4d56-4ca6-b032-471facbc9cba" />

The measured values are:

\[
V_{in(p-p)} = 1.239 - 1.220 = 19\text{ mV}
\]

\[
V_{out(p-p)} = 1.631 - 1.155 = 0.476\text{ V}
\]

\[
A_v = \frac{0.476}{0.019} = 25.05
\]

\[
A_v(dB) = 20\log_{10}(25.05) = 27.97\text{ dB}
\]

### 4.4 Theoretical gain

The gain expression used for this configuration is:

\[
A_v = \frac{-g_{m1}(r_{o1}\parallel r_{o2})}{1 + g_{m1}r_{d3}}
\]

For the same bias level, the small-signal transconductance is about:

\[
g_{m1} = 2.4\text{ mS}
\]

and the PMOS load resistance is about:

\[
r_{o2} = 33.3\text{ k}\Omega
\]

The analytical estimate gives a gain around **40 V/V**, or about **32.04 dB**.

### 4.5 AC response
<img width="1920" height="1200" alt="AC 2C" src="https://github.com/user-attachments/assets/deab99a6-7507-4d84-9299-510fc22bf445" />

From the AC plot:

| Parameter | Value |
|---|---:|
| Midband gain | 9 dB |
| Gain at −3 dB point | 6 dB |
| Upper cutoff frequency | 500 MHz |
| Bandwidth | 500 MHz |

Among the common-source versions, this configuration shows a strong gain with a comparatively wide bandwidth.

---

# Part II — Differential Amplifier Configurations

---

## 5. Differential Amplifier Fundamentals

A differential amplifier amplifies the difference between two input voltages and suppresses the component that is common to both inputs. This makes it a natural front-end stage for low-noise analog systems.

The key variables are:

\[
V_{id} = V_{in1} - V_{in2}
\]

\[
V_{CM} = \frac{V_{in1} + V_{in2}}{2}
\]

The differential pair works by steering current between two branches. For small differential inputs, the current is shared and the output remains linear. For large inputs, one transistor begins to dominate and the output becomes nonlinear.

The operating range is limited by overdrive voltage and saturation headroom:

\[
|V_{id}| \leq \sqrt{2}V_{OV}
\]

---

## 6. Circuit 1 — NMOS Differential Pair with Resistive Load

### Circuit diagram
<img width="895" height="735" alt="Diff C1" src="https://github.com/user-attachments/assets/c08e28ac-6d43-47cd-9f4a-e71198fd52ce" />

### 6.1 DC design values

| Quantity | Value |
|---|---:|
| \(V_{DD}\) | 0.9 V |
| \(V_{SS}\) | -0.9 V |
| Tail current | 1 mA |
| Branch current | 0.5 mA |
| Load resistance | 1.8 kΩ |
| \(V_{GS}\) | 0.7 V |
| \(V_{OV}\) | 0.34 V |

From the quiescent current:

\[
I_{D1} = I_{D2} = \frac{I_{SS}}{2} = 0.5\text{ mA}
\]

\[
R_D = \frac{0.9}{0.5\text{ mA}} = 1.8\text{ k}\Omega
\]

\[
V_{inCM(min)} = -0.7 + 0.36 = -0.34\text{ V}
\]

\[
V_{inCM(max)} = 0 + 0.36 = 0.36\text{ V}
\]

So the useful input common-mode range is:

\[
-0.34\text{ V} \leq V_{inCM} \leq 0.36\text{ V}
\]

### 6.2 Linear and nonlinear response
<img width="1904" height="843" alt="Linear differential output" src="https://github.com/user-attachments/assets/a4b87a21-6349-4af1-9d21-f0819cd16d2e" />
<img width="1912" height="836" alt="Nonlinear differential output" src="https://github.com/user-attachments/assets/2d816808-bfe0-4157-b356-cd920770f804" />

The differential pair is linear when:

\[
|V_{id}| < \sqrt{2}V_{OV}
\]

and becomes nonlinear when the input exceeds that limit.

### 6.3 Gain results

| Metric | Value |
|---|---:|
| Practical gain | 4.629 V/V |
| Practical gain | 12.606 dB |
| Theoretical gain | 4.498 V/V |
| Theoretical gain | 13.06 dB |

The small difference between the measured and hand-calculated gain is expected because the simulation includes channel-length modulation and device non-idealities.

### 6.4 AC response
<img width="1915" height="864" alt="Diff C1 AC" src="https://github.com/user-attachments/assets/8221a5b2-876f-44f9-85ee-88ca6cedde38" />

| Parameter | Value |
|---|---:|
| Midband gain | 13.358 dB |
| Linear gain | 4.78 V/V |
| Bandwidth | 10.186 GHz |
| UGB | 48.68 GHz |

This is the highest-bandwidth differential stage in the set because the resistive load keeps the output node relatively fast.

---

## 7. Circuit 2 — Differential Amplifier with PMOS Active Load and NMOS Tail Current Source

### Circuit diagram
<img width="900" height="650" alt="Diff C2" src="https://github.com/user-attachments/assets/9cb741ce-a076-4d21-b389-7cac4ff3d927" />

### 7.1 Circuit idea

This circuit uses an NMOS differential pair with a PMOS current-mirror active load. The active load replaces the resistor and raises the effective output resistance, which improves integration and allows single-ended output conversion.

### 7.2 DC operating point
<img width="1402" height="696" alt="Diff C2 DC" src="https://github.com/user-attachments/assets/c3cd4d5a-f20b-4b80-9f64-0e0bfa82f659" />

The reported values are:

| Quantity | Value |
|---|---:|
| \(V_{out1}\) | 7.147 mV |
| \(V_{out2}\) | 7.147 mV |
| \(I_D(M1)\) | 0.500 mA |
| \(I_D(M2)\) | 0.500 mA |
| \(I_D(M5)\) | 1.000 mA |

The outputs are balanced, which confirms that the pair is correctly biased.

### 7.3 Common-mode range

The range that keeps the transistors in saturation is:

\[
V_{CM(min)} \approx -0.334\text{ V}
\]

\[
V_{CM(max)} \approx 1.06\text{ V}
\]

### 7.4 Transient response
<img width="1919" height="460" alt="Diff C2 transient" src="https://github.com/user-attachments/assets/80713d01-863f-464c-a0f4-a429dc0ee16d" />

The output waveform remains sinusoidal for zero DC input, which confirms proper small-signal operation.

### 7.5 AC response
<img width="1893" height="470" alt="Diff C2 AC" src="https://github.com/user-attachments/assets/e72cdb91-4541-4004-b714-3928aa423799" />

| Parameter | Value |
|---|---:|
| Midband gain | 5.5504 dB |
| Linear gain | 1.896 V/V |
| Phase at low frequency | −194.822° |

This stage has lower gain than the resistive-load version, but it is much more suitable for integrated implementation because the PMOS mirror load removes the need for large resistors.

---

## 8. Circuit 3 — CMOS Differential Amplifier with PMOS Current Mirror Load and Bias Control

### Circuit diagram
<img width="1079" height="665" alt="Diff C3" src="https://github.com/user-attachments/assets/c36c9b83-c1c1-4db0-80d5-73bc9a80cd4a" />

This version adds explicit bias control using **VB1** and **VB2**. The extra control points improve operating-point tuning and help set the common-mode headroom more precisely.

### 8.1 DC operating point
<img width="853" height="627" alt="Diff C3 DC" src="https://github.com/user-attachments/assets/2992dc79-1ea6-4a53-b3e0-66c8f034bc5f" />

The reported bias values are:

| Quantity | Value |
|---|---:|
| \(V_{out1}\) | 0.8876 V |
| \(V_{out2}\) | 0.8876 V |
| \(V_p\) | −0.7218 V |
| \(I_D(M5)\) | 1.0487 mA |
| \(I_D(M1)\) | 0.5243 mA |
| \(I_D(M2)\) | 0.5243 mA |

The equal branch currents confirm balanced operation.

### 8.2 Common-mode boundary behavior

At the lower common-mode limit, the tail source is just inside saturation and the input pair loses transconductance. At the upper common-mode limit, the active load approaches triode and the gain collapses.

The linear operating region is still governed by:

\[
|V_{id}| \leq \sqrt{2}V_{OV}
\]

### 8.3 Large-signal response
<img width="1915" height="438" alt="Diff C3 linear" src="https://github.com/user-attachments/assets/6d262478-d51d-471a-aab6-09713a9b81f0" />
<img width="1907" height="463" alt="Diff C3 nonlinear" src="https://github.com/user-attachments/assets/b9088edd-8658-473a-bc5a-a8958090b160" />

For the small-signal case:

\[
V_{id} = 0.2\text{ V} < \sqrt{2}V_{OV}
\]

the circuit remains linear and both transistors share current.

For the large-signal case:

\[
V_{id} = 0.5\text{ V} > \sqrt{2}V_{OV}
\]

the current shifts toward one side and the output becomes nonlinear.

### 8.4 Gain and frequency response

| Parameter | Value |
|---|---:|
| Practical gain | 40.84 V/V |
| Practical gain | 32.22 dB |
| Bandwidth | 411.958 MHz |
| UGB | 17.85 GHz |

This is the highest-gain differential configuration in the set because the bias-controlled active load provides a much larger effective output resistance.

---

# 9. Comparison of the Main Results

## Common-source amplifiers

| Circuit | Practical gain | Gain in dB | Bandwidth | Main strength |
|---|---:|---:|---:|---|
| 2A — PMOS load + \(R_S\) | 6.309 V/V | 16 dB | 371.525 MHz | good balance of gain and linearity |
| 2B — NMOS current source + PMOS load | 1.5604 V/V | 3.8649 dB | 184.562 MHz | strong bias stability |
| 2C — diode-connected NMOS + PMOS load | 25.05 V/V | 27.97 dB | 500 MHz | highest gain among CS versions |

## Differential amplifiers

| Circuit | Practical gain | Gain in dB | Bandwidth | Main strength |
|---|---:|---:|---:|---|
| Circuit 1 — resistive load | 4.629 V/V | 12.606 dB | 10.186 GHz | widest bandwidth |
| Circuit 2 — PMOS active load | 1.896 V/V | 5.5504 dB | narrow low-frequency response | compact integration |
| Circuit 3 — bias-controlled | 40.84 V/V | 32.22 dB | 411.958 MHz | highest gain |

---

# 10. Final Remarks

The circuits show the classic analog design trade-offs very clearly:

- **Source degeneration** improves linearity but reduces gain.
- **Active loads** improve output resistance and make integrated design practical.
- **Current sources** stabilize bias but can reduce gain through finite output resistance.
- **Differential pairs** provide common-mode rejection and clean current steering.
- **Bias control** increases tuning flexibility and can significantly improve gain.

The simulation results and the hand calculations are close enough to confirm that the design assumptions are reasonable, while the remaining differences are explained by second-order MOS effects that are always present in real devices.

---

# 11. Short conclusion

The amplifier set demonstrates that a good analog design is always a balance between gain, bandwidth, linearity, and headroom. The resistive-load differential pair gives the widest bandwidth, the bias-controlled differential amplifier gives the highest gain, and the common-source stages show how different load choices shape the final performance.

