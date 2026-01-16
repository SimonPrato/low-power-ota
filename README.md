# Low-Power Operational Transconductance Amplifier (OTA)
## Project Overview

This project implements a low-power operational transconductance amplifier (OTA) designed in Cadence, operating at VDD = 1V with a bias current of 1µA. The goal was to achieve a transconductance amplifier with Gm < 5µS while minimizing power consumption.

## Design Specifications

- **Supply Voltage:** VDD = 1V
- **Bias Current:** IB = 1µA
- **Target Transconductance:** Gm < 5µS
- **Operation Region:** Differential pair in weak inversion

## Key Design Decisions

### 1. Input Differential Pair (Weak Inversion)

The PMOS input transistors were designed to operate in the weak inversion region (VSG < Vt) to maximize the gm/ID ratio:

- **Channel Width:** W = 200µm
- **Channel Length:** L = 647nm (increased from minimum to raise Vt)
- **Achieved gm/ID:** ≈ 26 V⁻¹ (theoretical maximum ≈ 29 V⁻¹)
- **Transconductance:** gm ≈ 13.26µS
- **Operating Point:** VSG ≈ 500mV, ID = 500nA each

**Rationale:** Weak inversion provides high transconductance efficiency (gm/ID ratio), enabling low-power operation. The increased channel length elevates the threshold voltage, further enhancing the gm/ID ratio.

### 2. Input Common Mode Range

The design achieves an input common mode voltage range of approximately 100mV to 650mV:

- **Lower Limit (≈100mV):** Constrained by the need to keep the bias current source in saturation (requires minimum voltage headroom)
- **Upper Limit (≈650mV):** Limited by NMOS current mirror saturation requirement

Below 100mV, the PMOS differential pair's gds increases dramatically, reducing gain. Above 650mV, the bias current source cannot provide adequate current.

### 3. NMOS Current Mirror Evolution

#### Initial Design
- **Dimensions:** W = 0.35µm (minimum), L = 2.5µm
- **Issue:** Could not simultaneously achieve Gm < 5µS and proper DC biasing
- **Result:** Gm = 11.8µS (exceeded specification)

#### Final Design: Series/Parallel Configuration

The breakthrough came from implementing a series/parallel NMOS current mirror with N = 2 transistors per branch:

- **Output Current Reduction:** Iout = IB/(2N²) = IB/8 = 125nA
- **Channel Length:** L = 1µm
- **Benefits:**
  - Reduced output current naturally lowers overall Gm
  - Increased output impedance (Rout = 8.67MΩ)
  - Larger headroom for input common mode range
  - Achieved **Gm = 3.50µS** ✓ (meets specification)

### 4. PMOS Current Mirror

To achieve DC output bias at VDD/2 = 500mV while minimizing gds:

- **Dimensions:** W = 135.72µm, L = 370nm
- **Trade-off:** Larger width needed to set proper DC bias point, though this increases gds contribution to output resistance

### 5. Bias Current Source

The ideal current source was replaced with a PMOS transistor (M0):

- **Dimensions:** W = 50µm, L = 1µm
- **Biasing:** VSG = 677.30mV (Vbias = 322.7mV)
- **Saturation Voltage:** Vdsat ≈ 60mV (provides 100mV headroom as specified)
- **Channel Length Rationale:** Increased to L = 1µm to reduce channel length modulation (λ effect)

## Performance Metrics

| Parameter | Value |
|-----------|-------|
| Overall Transconductance (Gm) | 3.50µS |
| DC Voltage Gain (A0) | 30.37 (29.65 dB) |
| Output Resistance (Rout) | 8.67MΩ |
| Power Consumption | 1µW (1V × 1µA) |
| DC Output Voltage | 500mV (VDD/2) |
| Input Common Mode Range | ~100mV to ~650mV |

## Circuit Topology

The final schematic consists of:
- **M0:** PMOS bias current source (50µm/1µm)
- **M1, M2:** PMOS differential input pair (200µm/647nm) - weak inversion
- **M3-M6:** NMOS series/parallel current mirror (parallel at input, series at output)
- **M7-M12:** PMOS current mirror providing active loads

## Key Findings

1. **Weak Inversion Efficiency:** Operating the input pair in weak inversion achieves gm/ID ratios approaching the theoretical limit, crucial for ultra-low-power designs.

2. **Series/Parallel Current Mirror Innovation:** This topology was essential to meet the Gm < 5µS specification while maintaining reasonable gain and output impedance.

3. **Design Trade-offs:** Achieving VDD/2 output biasing required larger PMOS mirror dimensions, which compromised gds minimization. This represents the fundamental trade-off between DC biasing and AC performance.

4. **Common Mode Sensitivity:** The OTA's performance is highly dependent on input common mode voltage due to internal biasing constraints in a 1V supply environment.

## Simulation Results

All simulations performed in Cadence demonstrate:
- Stable operation across the specified common mode range
- Gain stability with negligible deviation between ideal and transistor-based current sources
- Meeting all design specifications with minimal power consumption

## Conclusion

This design successfully demonstrates ultra-low-power OTA implementation using weak inversion operation and innovative current mirror topology. The series/parallel NMOS configuration proved critical in achieving the required transconductance specification while maintaining acceptable gain and output resistance in a severely constrained 1V supply environment.
