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

\[
A_v = 1 + \frac{R_f}{R_i}
\]

Where:  
- \(R_f\) is the feedback resistor,  
- \(R_i\) is the input resistor.

Set \(R_f\) and \(R_i\) to achieve a high gain (e.g., \(A_v\) in the range of tens to hundreds) while remaining stable within the LM358’s bandwidth.

Example placeholder (replace with your actual values):

- \(R_f = 470\,\text{k}\Omega\)  
- \(R_i = 4.7\,\text{k}\Omega\)  

Then:

\[
A_v = 1 + \frac{470\,000}{4\,700} \approx 101
\]

Adjust these to match your actual schematic values.

## 3. Input Coupling High‑Pass Filter

The RF pickup stage uses a coupling capacitor \(C_1\) with an input resistance \(R_{\text{in}}\) to block DC and pass AC signals:

\[
f_c = \frac{1}{2\pi R_{\text{in}} C_1}
\]

Where:  
- \(f_c\) is the cutoff frequency,  
- \(R_{\text{in}}\) is the effective input resistance (e.g., 100 kΩ),  
- \(C_1\) is the coupling capacitor (e.g., 1 µF).

Example (placeholder):

- \(R_{\text{in}} = 100\,\text{k}\Omega\)  
- \(C_1 = 1\,\mu\text{F}\)

\[
f_c \approx \frac{1}{2\pi \cdot 100\,000 \cdot 1 \times 10^{-6}} \approx 1.6\,\text{Hz}
\]

This very low cutoff ensures the envelope of the RF signal (audio‑range modulation) passes while DC offsets are blocked.

## 4. Output RC and LED Drive

If an RC network is used at the output for smoothing / envelope detection, its cutoff can be designed similarly:

\[
f_{c,\text{out}} = \frac{1}{2\pi R_{\text{load}} C_{\text{out}}}
\]

Choose \(R_{\text{load}}\) and \(C_{\text{out}}\) such that the LED sees the envelope variations at a visible blink rate (a few Hz to tens of Hz).

The LED current is limited by resistor \(R_L\):

\[
I_{\text{LED}} \approx \frac{V_{\text{CC}} - V_{\text{LED}} - V_{\text{sat}}}{R_L}
\]

Where \(V_{\text{LED}}\) is the LED forward voltage and \(V_{\text{sat}}\) is transistor saturation voltage.

## 5. Power Consumption Estimate

Approximate power consumption at 5 V supply:

\[
P \approx V_{\text{CC}} \times I_{\text{total}}
\]

With LM358 quiescent current plus LED and any buzzer current, design to keep \(P < 100\,\text{mW}\) for portable operation.

## 6. Design Trade‑Offs

- Higher gain improves sensitivity but increases susceptibility to noise and false triggers.  
- Antenna length and placement affect detection range and directionality.  
- RC time constants must balance responsiveness (fast blinking) with stability (avoid random flicker).  

Document your final chosen resistor and capacitor values here once fixed, and attach any simulation plots or oscilloscope captures in the `images/` or `src/simulation/` folders.
