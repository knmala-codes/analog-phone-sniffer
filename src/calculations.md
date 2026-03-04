# Calculations and Design Notes – Phone Sniffer (LM358)

This document summarizes key calculations and design decisions used in the analog mobile phone detector circuit.

## 1. Target RF Frequency Bands

Mobile phones operate in several standardized RF bands:

- 2G GSM: 850, 900, 1800, 1900 MHz (uplink and downlink pairs).
- 3G UMTS: 850, 900, 1900, 2100 MHz.
- 4G LTE: multiple bands including 700, 800, 1800, 2100 MHz.
- 5G NR Sub‑6: typical bands in the 3300–4200 MHz region, and others around 2.5–2.7 GHz.

These correspond to wavelengths on the order of 7–42 cm, so a short wire or strip acts as a practical pickup antenna for near‑field detection.

## 2. LM358 Amplifier Gain

The LM358 is configured as a non‑inverting amplifier in this design. The voltage gain is:

Av = 1 + (Rf / Ri)

Where:
- Rf is the feedback resistor,
- Ri is the input resistor.

Set Rf and Ri to achieve a high gain (e.g., Av in the range of tens to hundreds) while remaining stable within the LM358’s bandwidth.

Example placeholder (replace with your actual values):

- Rf = 470 kΩ
- Ri = 4.7 kΩ

Then:

Av = 1 + (470000 / 4700) ≈ 101

Adjust these to match your actual schematic values.

## 3. Input Coupling High‑Pass Filter

The RF pickup stage uses a coupling capacitor C1 with an input resistance Rin to block DC and pass AC signals:

fc = 1 / (2 × π × Rin × C1)

Where:
- fc is the cutoff frequency,
- Rin is the effective input resistance (e.g., 100 kΩ),
- C1 is the coupling capacitor (e.g., 1 µF).

Example (placeholder):

- Rin = 100 kΩ
- C1 = 1 µF

fc ≈ 1 / (2 × π × 100000 × 1 × 10⁻⁶) ≈ 1.6 Hz

This very low cutoff ensures the envelope of the RF signal (audio‑range modulation) passes while DC offsets are blocked.

## 4. Output RC and LED Drive

If an RC network is used at the output for smoothing / envelope detection, its cutoff can be designed similarly:

fc_out = 1 / (2 × π × Rload × Cout)

Choose Rload and Cout such that the LED sees the envelope variations at a visible blink rate (a few Hz to tens of Hz).

The LED current is limited by resistor RL:

I_LED ≈ (VCC − V_LED − V_sat) / RL

Where V_LED is the LED forward voltage and V_sat is transistor saturation voltage.

## 5. Power Consumption Estimate

Approximate power consumption at 5 V supply:

P ≈ VCC × Itotal

With LM358 quiescent current plus LED and any buzzer current, design to keep P < 100 mW for portable operation.

## 6. Design Trade‑Offs

- Higher gain improves sensitivity but increases susceptibility to noise and false triggers.
- Antenna length and placement affect detection range and directionality.
- RC time constants must balance responsiveness (fast blinking) with stability (avoid random flicker).

Document your final chosen resistor and capacitor values here once fixed, and attach any simulation plots or oscilloscope captures in the `images/` or `src/simulation/` folders.
