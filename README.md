# Thermal Oxidation Layer Calculation GUI

A Python desktop application that simulates thermal SiO₂ growth on silicon using the **Deal–Grove model**, built as part of a VLSI Technologies PBL assignment.

> **Course:** VLSI Technologies &nbsp;|&nbsp; **Student:** Tejas Sharma &nbsp;|&nbsp; **Instructor:** Dr. Prashant Kumar

---

## What It Does

Given process inputs — temperature, pressure, time, and oxidant type — the simulator computes:
- Oxide thickness `xo` in nm and µm
- Rate constants `B` and `B/A` via Arrhenius equations
- Dominant interference wavelength λ
- Wafer color using an empirical fab color table

---

## GUI Overview

The app has three tabs:

| Tab | Description |
|-----|-------------|
| **Simulator** | Live sliders for T, P, t — updates thickness, color swatch, and growth curve in real time |
| **Growth Plot** | Full xo(t) curve with Dry O₂ and Wet H₂O overlay |
| **Validation Test Cases** | 14 anti-hallucination tests — run all with one click |

---

## Physics

**Deal–Grove equation:**
```
xo(t) = ( −A + √(A² + 4·B·(t + τ)) ) / 2
```

**Arrhenius temperature dependence** (T must be in Kelvin):
```
B(T)   = B_ref  · exp( −Ea_B  / kB · (1/T − 1/T_ref) )
B/A(T) = BA_ref · exp( −Ea_BA / kB · (1/T − 1/T_ref) )
```

**Pressure scaling:**
```
B_new = B × P       (B/A)_new = (B/A) × P
```

**Calibrated constants** (T_ref = 1000 °C = 1273.15 K):

| Parameter | Dry O₂ | Wet H₂O |
|-----------|--------|---------|
| Ea,B | 1.23 eV | 0.78 eV |
| Ea,B/A | 2.00 eV | 2.05 eV |
| B_ref | 0.0117 µm²/hr | 0.3140 µm²/hr |
| BA_ref | 0.04350 µm/hr | 2.0500 µm/hr |
| n_ox | 1.46 | 1.46 |

---

## BYU Validation (Mandatory Test)

| | Value |
|--|--|
| Conditions | Dry O₂, 1000 °C, 1 hr, 1 atm, (100) Si |
| BYU Calculator result | **38.1 nm** |
| This simulator | **38.1 nm** (error < 0.01%) |
| Wafer color | **Tan (#C3A078)** |

> ⚠️ The assignment PDF stated 71.1 nm — this is a transcription error. The BYU Cleanroom Calculator actual result is 38.1 nm, independently confirmed by Jaeger Table 3.2 (~39.1 nm).

---

## Hallucinations Fixed

| # | Bug | Wrong | Fixed |
|---|-----|-------|-------|
| 1 | Color mapping | Spectral RGB → returned purple at 38 nm | Empirical BYU/Jaeger fab color table → Tan |
| 2 | Thickness reference | PDF stated 71.1 nm, AI used that as ground truth | Recalibrated to BYU actual: 38.1 nm |
| 3 | Temperature units | Arrhenius computed with T in °C | Converted to Kelvin: `T_K = T_C + 273.15` |

---

## Validation Results

14 test cases covering BYU reference, pressure scaling, temperature, wet vs. dry, time regimes, and physics sanity checks.

**Result: 12 / 12 PASS · 0 FAIL · 2 INFO**

---

## Installation & Usage

**Requirements:** Python 3.10+, numpy, matplotlib (tkinter is built-in)

```bash
pip install numpy matplotlib
python thermal_oxidation_simulator.py
```

---

## Repository Structure

```
Thermal-Oxidation-Layer-Calculation-GUI/
├── thermal_oxidation_simulator.py   # Main application (single file)
├── Documentation/
│   └── VLSI PBL, Tejas Sharma.pdf  # Assignment report (5-slide PDF)
└── README.md
```

---

## References

- Deal, B.E. & Grove, A.S. (1965). *J. Appl. Phys.* 36, 3770
- Jaeger, R.C. — *Introduction to Microelectronic Fabrication*, Table 3.2
- Plummer, Deal & Griffin — *Silicon VLSI Technology*
- [BYU Cleanroom Oxide Thickness Calculator](https://cleanroom.byu.edu/oxidethickcalc)
