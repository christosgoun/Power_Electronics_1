# AC/DC/AC Power Transmission System Simulation & Analysis

This repository contains the analytical design, evaluation, and simulation of a line-commutated AC/DC/AC power transmission system interconnecting two asynchronous grids. The project bridges theoretical power electronics concepts with industry-standard simulation verification using **PSIM**.

## 📌 Project Overview

The topology models a simplified Line-Commutated Converter (LCC) system utilized in High-Voltage Direct Current (HVDC) applications for bulk power transfer. Rectification and inversion are handled via 6-pulse three-phase thyristor bridges ($M_1$ and $M_2$), linked through an inductive DC link ($L_{dc} = 50\text{ mH}$) acting as a current source.

### System Configuration (Base Values)
* **Base Power ($S_b$):** $100\text{ kVA}$
* **Base Voltage ($V_b$):** $400\text{ V (L-L, RMS)}$
* **Grid Internal Impedance:** Formulated via Thevenin equivalents ($S_{sc} = 20.4\text{ pu}$)
* **Target Load ($P_{dc-2}$):** $0.88\text{ pu}$ with a fixed $V_{dc-2} = 497\text{ V}$

---

## 🛠️ Key Objectives & Engineering Tasks

1. **Analytical Operating Point Determination:** Exact mathematical derivation of DC link current ($I_{dc}$), intermediate voltages ($V_{dc-1}$), converter firing angles ($\alpha_1, \alpha_2$), and commutation overlap angles ($u_1, u_2$).
2. **Component Sizing & Selection:** Sizing of smoothing/limiting inductors ($L_{f1}, L_{f2}$) and selection of commercial thyristors based on peak inverse voltage ($V_{RRM}$) and average/RMS current ratings.
3. **Harmonic & Power Factor Analysis:** Implementation of Fast Fourier Transform (FFT) to analyze source current harmonics up to the 19th order. Calculation of fundamental reactive power ($Q_1$) and distortion power ($D$).
4. **Passive Harmonic Filter Design:** Sizing and tuning a 3-phase shunt passive harmonic filter at PCC1, tuned at $f_h = 4.3$ to suppress lower-order harmonics and supply reactive power compensation.

---

## 📊 Results & Simulation Verification

The mathematically derived values were verified against the permanent-state simulation data extracted from PSIM.

### Converter Parameter Comparison

| Parameter | Analytical Sizing | PSIM Simulation | Deviation |
| :--- | :---: | :---: | :---: |
| **DC Current ($I_{dc}$)** | $177.06\text{ A}$ | $177.25\text{ A}$ | $+0.11\%$ |
| **Rectifier Voltage ($V_{dc-1}$)** | $505.95\text{ V}$ | $505.94\text{ V}$ | $-0.01\%$ |
| **Firing Angle ($\alpha_1$)** | $9.53^\circ$ | $9.25^\circ$ | $-2.93\%$ |
| **Inverter Angle ($\alpha_2$)** | $150.51^\circ$ | $150.30^\circ$ | $-0.14\%$ |
| **Overlap Angle ($u_1$)** | $17.97^\circ$ | $18.36^\circ$ | $+2.17\%$ |
| **Overlap Angle ($u_2$)** | $15.33^\circ$ | $15.48^\circ$ | $+0.98\%$ |

### Harmonic Current Mitigation (Grid 1)

The table below outlines the source current spectrum before and after connecting the designed shunt passive filter:

| Harmonic Order ($h$) | Theoretical (No Overlap) | PSIM (Before Filter) | PSIM (With Filter) |
| :---: | :---: | :---: | :---: |
| **$h = 1$ (Fundamental)** | $138.05\text{ A}$ | $135.92\text{ A}$ | $129.79\text{ A}$ |
| **$h = 5$** | $27.61\text{ A}$ | $17.48\text{ A}$ | $17.86\text{ A}$ |
| **$h = 7$** | $19.72\text{ A}$ | $12.71\text{ A}$ | $16.06\text{ A}$ |
| **Fundamental Reactive ($Q_{1-1}$)** | $33.51\text{ kVAR}$ | $32.59\text{ kVAR}$ | **$4.14\text{ kVAR}$** |
| **Distortion Power ($D_1$)** | $29.53\text{ kVAR}$ | $25.88\text{ kVAR}$ | **$13.66\text{ kVAR}$** |

> 💡 **Observation:** Connecting the passive filter suppressed the distortion power by **47.2%** and dropped the fundamental reactive power drawn from Grid 1 by **87.3%**, bringing the total displacement power factor ($DPF_1$) close to unity ($0.999$).

---

## 🚀 How to Run the Simulation

1. Clone this repository.
2. Open `hvdc_6pulse_system.psimsch` in **PSIM**.
3. Run the simulation (`F8`).
4. Use **Simview** to plot $I_{dc}$, $V_{dc-1}$, $V_{dc-2}$ and verify the FFT execution on the AC-side currents.
