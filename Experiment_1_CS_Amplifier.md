# Experiment 1: DC, AC, and Transient Analysis of a Common Source (CS) Amplifier

**Name:** [Your Name]  
**USN:** [Your USN]  
**Subject:** Linear Integrated Circuits Lab (BEC456B)  
**Date:** [Date of Experiment]  
**Tool Used:** LTspice XVII

---

## 1. Aim

To design and simulate a Common Source (CS) MOSFET amplifier using TSMC 180nm technology in LTspice, and to perform DC Operating Point, Transient, and AC Frequency Response analyses to extract key amplifier parameters.

---

## 2. Design Specifications

| Parameter | Value |
|---|---|
| Technology Node | TSMC 180 nm |
| Supply Voltage (V_DD) | 2 V |
| Power Budget | ≤ 1.2 mW |
| Load Capacitance (C_L) | 0.7 pF |
| Channel Length (L) | 180 nm |
| MOSFET Model | NMOS (CMOSN) from tsmc018.lib |

---

## 3. Theory

### 3.1 What is a Common Source Amplifier?

A Common Source (CS) amplifier is a fundamental single-stage MOSFET amplifier configuration used extensively in analog CMOS circuit design. In this topology:

- The **input signal** is applied to the **gate** terminal
- The **output signal** is taken from the **drain** terminal
- The **source** terminal is connected to ground (common to both input and output)

The working principle relies on the MOSFET's ability to convert a voltage signal at its gate into a proportional drain current. This current, flowing through a drain resistor R_D, produces an amplified and inverted output voltage.

### 3.2 Operating Regions and Bias Point

For the CS amplifier to function as a linear amplifier, the MOSFET must be biased in the **saturation region**, where:

```
V_DS ≥ V_GS − V_TH
```

- If V_GS < V_TH → transistor is in **cutoff** (no current, V_out ≈ V_DD)
- If V_DS < V_GS − V_TH → transistor enters **triode/linear region** (not suitable for amplification)

The Q-point (DC operating point) must be chosen so that the MOSFET stays in saturation for the full expected signal swing.

### 3.3 Small-Signal Voltage Gain

In small-signal analysis, the MOSFET is replaced by its equivalent model. A small AC input v_in at the gate produces a gate-source voltage v_gs, which drives a dependent current source:

```
i_d = gm × v_gs
```

This current through R_D generates the output:

```
v_out = −gm × v_gs × R_D
```

The small-signal voltage gain is therefore:

```
Av = v_out / v_in = −gm × R_D
```

The **negative sign** confirms a 180° phase inversion between input and output — a defining characteristic of the CS configuration.

### 3.4 Transconductance (g_m)

```
gm = 2 × I_D / (V_GS − V_TH) = 2 × I_D / V_ov
```

where V_ov = V_GS − V_TH is the overdrive voltage. Higher g_m leads to higher gain.

---

## 4. Pre-Design Calculations

### 4.1 Technology Parameters (from tsmc018.lib)

| Parameter | Symbol | Value |
|---|---|---|
| Threshold Voltage | V_TH | 0.366 V |
| Carrier Mobility | μ_n | 273.809 cm²/V·s |
| Oxide Thickness | t_ox | 4.1 × 10⁻⁹ m |
| Relative Permittivity (SiO₂) | ε_r | 3.453 |

### 4.2 Target Drain Current from Power Budget

Given V_DD = 2 V and P ≤ 1.2 mW:

```
I_D(max) = P / V_DD = 1.2 mW / 2 V = 0.6 mA
```

A target of **I_D = 200 µA** is selected to stay well within the budget with margin.

### 4.3 Drain Resistor (R_D) Calculation

For maximum output voltage swing, Q-point is set at V_DS = V_DD / 2 = 1 V.

Applying KVL:
```
V_DD = I_D × R_D + V_DS
2 = 200 × 10⁻⁶ × R_D + 1
R_D = 1 / (200 × 10⁻⁶) = 5 kΩ
```

### 4.4 Power Verification

```
P = V_DD × I_D = 2 × 200 µA = 0.4 mW ≤ 1.2 mW ✓
```

### 4.5 Oxide Capacitance (C_ox)

```
ε_ox = ε_0 × ε_r = 8.854 × 10⁻¹² × 3.453 = 3.057 × 10⁻¹¹ F/m
C_ox = ε_ox / t_ox = 3.057 × 10⁻¹¹ / 4.1 × 10⁻⁹ ≈ 7.456 mF/m²
```

### 4.6 Channel Width (W) Calculation

Using the saturation current equation with V_GS = 0.9 V:

```
I_D = (1/2) × μn × Cox × (W/L) × (V_GS − V_TH)²
200 × 10⁻⁶ = (1/2) × μn × Cox × (W / 180nm) × (0.534)²
```

Solving iteratively:

| Width W (µm) | Simulated I_D (µA) | Status |
|---|---|---|
| 1.07 | ~165 | Below target |
| 1.377 | ~185 | Close |
| 1.508 | ~200 | ✓ Target achieved |

**Selected: W = 1.508 µm, L = 180 nm**

---

## 5. Circuit Description

The resistive-load CS amplifier consists of the following components:

| Component | Value / Specification |
|---|---|
| M1 (NMOS) | W = 1.508 µm, L = 180 nm, model = CMOSN |
| R_D | 5 kΩ (drain load resistor) |
| V_GS (DC bias) | 0.9 V |
| V_DD | 2 V |
| C_L | 0.7 pF (load capacitance) |
| AC input (v_in) | 10 mV amplitude, 1 kHz |

The gate is biased at 0.9 V to keep the transistor in saturation. For transient and AC analysis, a small sinusoidal signal is superimposed on this DC bias.

**SPICE Directives:**
```spice
.lib tsmc018.lib
.op
.tran 0.01m 5m
.ac dec 100 1 1000G
.dc V_GS 0 2 0.01
```

---

## 6. Simulation Results and Analysis

### 6.1 DC Operating Point (.op Analysis)

After running the `.op` simulation, the SPICE output log yielded:

| Node / Parameter | Value |
|---|---|
| V_G (Gate Voltage) | 0.9 V |
| V_D (Drain / V_out) | ~1.0 V |
| V_S (Source) | 0 V |
| V_GS | 0.9 V |
| V_DS | ~1.0 V |
| I_D | ~200 µA |
| Operating Region | **Saturation** ✓ |

**Verification:** V_DS (1.0 V) > V_GS − V_TH (0.534 V) → MOSFET is in saturation ✓

---

### 6.2 Parametric Study: Effect of W on I_D

With V_GS fixed at 0.9 V and R_D = 5 kΩ, varying the channel width produced the following:

| W (µm) | I_D (µA) | V_DS (V) | Observation |
|---|---|---|---|
| 1.07 | ~165 | ~1.175 | Under-target |
| 1.377 | ~185 | ~1.075 | Approaching target |
| 1.508 | ~200 | ~1.00 | ✓ Design target met |

Drain current scales proportionally with W because a wider channel provides more conducting area, allowing more charge carriers to flow. This directly follows from the long-channel MOSFET equation where I_D ∝ W/L.

---

### 6.3 Parametric Study: Effect of R_D on V_DS

With I_D ≈ 200 µA, varying R_D shifts the Q-point:

| R_D (kΩ) | V_DS (V) | Region |
|---|---|---|
| 2.2 | 1.528 | Deep saturation |
| 3.0 | 1.369 | Saturation |
| 5.0 | 0.998 | ✓ Optimal Q-point |
| 6.0 | 0.828 | Near edge of saturation |

As R_D increases, greater voltage drops across it, pulling V_DS lower. If R_D is too large, V_DS drops below V_GS − V_TH and the device enters triode, disrupting linear amplification.

---

### 6.4 DC Sweep: Voltage Transfer Characteristics (V_out vs V_in)

A DC sweep of V_in from 0 V to 2 V was performed:

| V_in Range | MOSFET Region | V_out Behavior |
|---|---|---|
| V_in < 0.366 V | Cutoff | V_out ≈ 2 V (no current) |
| 0.366 V < V_in < ~1.0 V | **Saturation** | V_out drops steeply — **amplification zone** |
| V_in > ~1.2 V | Triode | V_out ≈ 0.1–0.2 V (transistor fully ON) |

The steep negative slope in the saturation region represents high voltage gain. The amplifier can only be used linearly within this narrow window.

---

### 6.5 Transient Analysis (Time-Domain)

A 1 kHz sinusoidal input (amplitude = 10 mV, DC offset = 0.9 V) was applied. The simulation ran for 5 ms.

**Measured Values:**

| Quantity | Value |
|---|---|
| V_in (peak) | 0.910 V |
| V_in (trough) | 0.890 V |
| ΔV_in | 0.020 V |
| V_out (peak) | 1.075 V |
| V_out (trough) | 0.975 V |
| ΔV_out | 0.100 V |

**Practical Voltage Gain:**
```
Av = ΔV_out / ΔV_in = 0.100 / 0.020 = 5.0
Av (dB) = 20 × log10(5.0) ≈ 13.98 dB
```

**Theoretical Voltage Gain:**
```
V_ov = V_GS − V_TH = 0.9 − 0.366 = 0.534 V
gm = 2 × I_D / V_ov = 2 × 200 µA / 0.534 = 749 µS
Av = gm × R_D = 749 µS × 5 kΩ = 3.745
Av (dB) = 20 × log10(3.745) ≈ 11.46 dB
```

**Key observations from transient waveform:**
- Output is **phase-inverted by 180°** relative to input ✓
- Waveform is sinusoidal and undistorted ✓
- MOSFET remains in saturation throughout the signal swing ✓

The difference between theoretical and practical gain arises from channel length modulation (finite output resistance r_o), which is ignored in the ideal small-signal model.

---

### 6.6 AC Analysis — Frequency Response (Without Bypass Capacitor)

An AC sweep from 1 Hz to 1 THz was performed to obtain the Bode plot.

| Parameter | Practical (Simulated) | Theoretical (Ideal) |
|---|---|---|
| Midband Gain | ~8.833 dB | ~8.478 dB |
| −3 dB Gain Level | ~5.833 dB | ~5.478 dB |
| Upper −3 dB Frequency | ~52.3 GHz | ~59.1 GHz |

**Key observations:**
- The gain is flat and stable across a wide mid-band frequency range
- At high frequencies, intrinsic MOSFET capacitances (C_gs, C_gd) cause gain roll-off
- No lower cutoff frequency exists because no coupling or bypass capacitors are in the signal path

---

### 6.7 AC Analysis — Frequency Response (With Source Bypass Capacitor)

When a source resistor R_S is added and bypassed with capacitor C_S:

| Configuration | Midband Gain (dB) | Upper −3 dB Frequency |
|---|---|---|
| Without C_S (source degeneration) | ~8.833 dB | ~52.3 GHz |
| With C_S (bypass capacitor) | ~9.465 dB | ~35.7 MHz |

**Explanation:**

Without C_S, the source resistor R_S is present in the AC signal path. This introduces **source degeneration**, which reduces the effective transconductance and thus lowers the gain. The bandwidth, however, remains large.

When C_S is added and shorts R_S for AC signals:
- Effective g_m increases → Gain improves
- C_S introduces a new RC pole in the frequency response → Bandwidth decreases dramatically

This is a clear demonstration of the **gain–bandwidth tradeoff** in amplifier design.

---

## 7. Summary of Results

| Parameter | Value |
|---|---|
| Technology | TSMC 180 nm |
| W / L | 1.508 µm / 180 nm |
| Q-point: I_D | ~200 µA |
| Q-point: V_DS | ~1.0 V |
| R_D | 5 kΩ |
| Power Consumed | 0.4 mW |
| Practical Gain (Transient) | 5.0 (≈ 13.98 dB) |
| Theoretical Gain | 3.745 (≈ 11.46 dB) |
| Upper −3 dB Frequency (no bypass cap) | ~52.3 GHz |
| Upper −3 dB Frequency (with bypass cap) | ~35.7 MHz |

---

## 8. Observations

1. The MOSFET was successfully biased in the saturation region at I_D ≈ 200 µA, V_DS ≈ 1 V with W = 1.508 µm and R_D = 5 kΩ.
2. Drain current increases proportionally with channel width W, consistent with long-channel MOSFET theory.
3. The drain resistor R_D directly controls the Q-point location; excessive R_D pushes the transistor toward the triode boundary.
4. Transient analysis confirmed 180° phase inversion and undistorted signal amplification at 1 kHz.
5. The AC frequency response is flat across a wide bandwidth, with roll-off at high frequencies due to parasitic capacitances C_gs and C_gd.
6. Adding a source bypass capacitor improves midband gain but introduces a pole that reduces bandwidth — a fundamental gain–bandwidth tradeoff.
7. Practical and theoretical gains differ slightly because the ideal model ignores channel length modulation and output resistance r_o.

---

## 9. Conclusion

A Common Source MOSFET amplifier was designed, implemented in LTspice, and characterized through DC, transient, and AC analyses. The design satisfied all given constraints: power consumption (0.4 mW << 1.2 mW), stable saturation biasing, and measurable voltage gain.

The three simulation analyses validated theoretical predictions and provided practical insight into:
- The importance of Q-point selection for linear operation
- How transistor dimensions (W, L) and bias conditions affect current and gain
- The frequency limitations imposed by intrinsic MOSFET capacitances
- The gain–bandwidth tradeoff introduced by bypass capacitor configurations

This experiment forms the foundation for understanding more complex analog amplifier topologies built using CMOS technology.

---

## 10. References

1. Razavi, B. — *Design of Analog CMOS Integrated Circuits*, McGraw-Hill, 2nd Edition
2. Sedra, A. S. & Smith, K. C. — *Microelectronic Circuits*, Oxford University Press, 7th Edition
3. TSMC 180nm Process Design Kit — tsmc018.lib BSIM3 Model Parameters
4. LTspice XVII Documentation — Analog Devices

---

*Report submitted as part of Linear Integrated Circuits Lab | Department of Electronics and Communication Engineering*
