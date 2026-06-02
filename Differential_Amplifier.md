

# Experiment 4: MOS Differential Amplifier Analysis (180 nm CMOS)

## Objective
Design and evaluate MOS differential amplifier topologies using 180 nm CMOS technology. The study covers DC biasing, common-mode limits, transient behavior, gain, bandwidth, and frequency response.

## Overview
A differential amplifier amplifies the voltage difference between two inputs while suppressing common-mode signals. This characteristic makes it a fundamental building block in operational amplifiers, sensor interfaces, communication circuits, and analog signal-processing systems.

### Key Relations
- Differential input: `Vid = Vin1 - Vin2`
- Common-mode input: `Vcm = (Vin1 + Vin2)/2`
- Tail current: `Iss = ID1 + ID2`
- Transconductance: `gm = 2ID / Vov`
- Differential gain: `Ad = gm × RD`
- Common-mode rejection ratio: `CMRR = Ad / Acm`

---

# Circuit 1 — NMOS Differential Pair with Resistive Loads

## Circuit Schematic
<img src="https://github.com/user-attachments/assets/c08e28ac-6d43-47cd-9f4a-e71198fd52ce" width="800"/>

## Design Specifications

| Parameter | Value |
|-----------|-------|
| VDD | 0.9 V |
| VSS | -0.9 V |
| VT | 0.36 V |
| VinCM | 0 V |
| VP | -0.7 V |
| Maximum Power | 1.8 mW |
| Channel Length | 480 nm |

## Calculated Results

| Quantity | Value |
|----------|-------|
| Tail Current (Iss) | 1 mA |
| Branch Current | 0.5 mA |
| Load Resistance | 1.8 kΩ |
| VGS | 0.7 V |
| VOV | 0.34 V |
| NMOS Width | ≈ 18 µm |
| Input CM Range | -0.34 V to 0.36 V |
| Output CM Range | -0.36 V to 0.9 V |
| Linear Differential Range | ±0.34 V |

## Operating Cases
- **Vin1 = Vin2** → equal current sharing and equal outputs.
- **Vin1 > Vin2** → M1 draws more current, Vout1 decreases.
- **Vin1 < Vin2** → M2 draws more current, Vout2 decreases.

## Simulation Snapshots
<img src="https://github.com/user-attachments/assets/2ab247f5-b9ea-45e5-980a-d7ffb167a683" width="800"/>

## Transient Analysis
### Linear Region
<img src="https://github.com/user-attachments/assets/a4b87a21-6349-4af1-9d21-f0819cd16d2e" width="800"/>

Condition:
`|Vid| < √2 × VOV`

### Non‑Linear Region
<img src="https://github.com/user-attachments/assets/2d816808-bfe0-4157-b356-cd920770f804" width="800"/>

Condition:
`|Vid| > √2 × VOV`

## Gain Summary

| Parameter | Value |
|-----------|-------|
| Practical Gain | 4.629 V/V |
| Practical Gain (dB) | 12.606 dB |
| Theoretical Gain | 4.498 V/V |
| Theoretical Gain (dB) | 13.06 dB |

## AC Analysis
<img src="https://github.com/user-attachments/assets/8221a5b2-876f-44f9-85ee-88ca6cedde38" width="800"/>

| Metric | Value |
|---------|-------|
| Midband Gain | 13.358 dB |
| Bandwidth | 10.186 GHz |
| UGB | 48.68 GHz |
| GBP | 48.68 GHz |

---

# Circuit 2 — Differential Amplifier with PMOS Active Load

## Circuit Schematic
<img src="https://github.com/user-attachments/assets/9cb741ce-a076-4d21-b389-7cac4ff3d927" width="800"/>

## Architecture
- NMOS differential input pair (M1, M2)
- PMOS current-mirror active load (M3, M4)
- NMOS tail current source (M5)

## Device Sizing Results

| Device | Current | Width |
|----------|---------|--------|
| M5 | 1 mA | ≈ 104 µm |
| M1, M2 | 0.5 mA | ≈ 18 µm |
| M3, M4 | 0.5 mA | ≈ 16.9 µm |

## Common‑Mode Limits

| Parameter | Value |
|-----------|-------|
| VinCM(min) | -0.34 V |
| VinCM(max) | 1.06 V |
| VoutCM(min) | -0.36 V |
| VoutCM(max) | 0.9 V |

<img src="https://github.com/user-attachments/assets/190a9f16-4184-4d29-b843-6e34367229d7" width="800"/>

## Transient Verification

### Linear Operation
<img src="https://github.com/user-attachments/assets/160def4a-bd40-4aeb-a5d0-6ac966117f6e" width="800"/>

### Non‑Linear Operation
<img src="https://github.com/user-attachments/assets/52201c49-16df-45e8-9dd7-f6f6f48c6fe0" width="800"/>

## Gain Comparison

| Metric | Practical | Theoretical |
|----------|-----------|-------------|
| Gain (V/V) | 1.89 | 1.72 |
| Gain (dB) | 5.529 dB | 4.70 dB |

## Frequency Response
<img src="https://github.com/user-attachments/assets/9b3923fc-e60e-47c7-8c63-527d439951fa" width="800"/>

| Parameter | Value |
|-----------|-------|
| Midband Gain | 5.55 dB |
| Linear Gain | 1.894 V/V |
| Bandwidth | 2.96 GHz |
| UGB (Practical) | 5.19 GHz |
| UGB (Theoretical) | 5.60 GHz |

---

# Conclusions

- Differential amplifiers effectively amplify differential signals while rejecting common-mode noise.
- The resistive-load topology provides higher gain but consumes more area.
- The PMOS active-load configuration offers compact implementation and improved bias efficiency.
- Simulated and theoretical results show close agreement, validating the design methodology.
- Both circuits maintain proper operation within their calculated common-mode and differential input ranges.
