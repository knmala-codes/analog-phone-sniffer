# Phone Sniffer – Analog Mobile Phone Detector (LM358)

This project is an analog **mobile phone detector** that senses RF activity from nearby phones using an LM358 op‑amp, a small antenna, and a simple LED indicator. It can detect active phones during calls, SMS, or data transmission, even in silent mode, within a short range.

---

## 1. Project overview

Modern phones constantly exchange RF signals with nearby base stations whenever they make/receive calls, send SMS, or use mobile data. This circuit picks up those RF bursts and turns them into a visible LED indication.

Key points:

- Detects RF activity from **2G–5G GSM bands** around 850, 900, 1800, and 1900 MHz.  
- Uses a **wire antenna** to capture RF, **LM358** to amplify the detected signal, and a **BC547 transistor + LED** to indicate detection.  
- Powered from **4.5–5 V**, with low current consumption suitable for battery operation.  
- Intended for use in **exam halls, secure rooms, hospitals, prisons, courtrooms**, and other phone‑restricted areas.  

---

## 2. How it works (concept)

### 2.1 RF pickup

- A short **wire antenna (5–10 cm copper wire)** acts as a crude broadband antenna in the GSM bands.  
- When a nearby phone transmits, the RF field induces a tiny voltage (microvolts–millivolts) on the antenna through **mutual induction and capacitive coupling**.  

### 2.2 Envelope detection and amplification

- The RF signal is AC‑coupled through a **1 µF capacitor** into the LM358 input, while a **100 kΩ resistor** sets a DC bias.  
- Internally, the RF carrier is effectively rectified/envelope‑detected by the antenna and coupling network, so the op‑amp only needs to amplify the **low‑frequency envelope** (100 Hz–a few kHz), not the GHz carrier.  
- The LM358 is configured as a **high‑gain non‑inverting amplifier**. A feedback network (220 kΩ + 47 kΩ preset) sets the gain and allows **sensitivity adjustment**.  

### 2.3 LED indication via transistor

- The op‑amp output drives a **BC547/BC548** transistor through a 1 kΩ base resistor.  
- When the output voltage exceeds about **0.6–0.7 V**, the transistor starts conducting and current flows through a **220 Ω resistor and the LED**.  
- Because the RF envelope is bursty and modulated, the LED **blinks** rather than staying steady, giving a clear visual sign that a phone is transmitting nearby.  

The 47 kΩ preset adjusts the threshold: you tune it so that the LED stays off without phones nearby and blinks when a phone transmits within the desired range.

---

## 3. Circuit architecture

The design is logically split into three stages.

### 3.1 Stage 1 – RF pickup & input

- **Antenna**: 5–10 cm insulated copper wire connected directly to LM358 input.  
- **Coupling capacitors (1 µF)**: Pass RF/AC components, block DC.  
- **Bias resistor (100 kΩ)**: Sets the DC bias at the non‑inverting input so the op‑amp operates around mid‑supply.  

### 3.2 Stage 2 – LM358 amplifier

- **IC: LM358** (dual op‑amp, single‑supply).  
- Supply: **Pin 8 = +4.5–5 V**, **Pin 4 = GND**, with a **100 µF** decoupling capacitor for stability.  
- Primary amplifier:  
  - Non‑inverting input (Pin 3): receives RF envelope through coupling/bias network.  
  - Inverting input (Pin 2): connected to feedback network (47 kΩ preset + 220 kΩ resistor) to set gain and detection threshold.  
  - Output (Pin 1): amplified signal driving next stage.  
- Unused op‑amp (second LM358 channel) pins are tied appropriately (inputs to ground) to avoid noise.  

The closed‑loop gain is high enough to boost microvolt‑level envelope signals up to several volts at the output, which is sufficient to drive the transistor and LED.

### 3.3 Stage 3 – Transistor + LED output

- **BC547/BC548 NPN** transistor is used as a low‑side switch.  
- Connections:  
  - Base: from LM358 Pin 1 through **1 kΩ** resistor.  
  - Collector: to +4.5/5 V through a **1 kΩ “collector load”** and **220 Ω LED series resistor** (depending on the exact version of your build).  
  - Emitter: to GND.  
- **LED**: 5 mm red LED, limited to ~10–20 mA via the 220 Ω resistor.  

When RF is detected and amplified, the transistor saturates and the LED lights; when no RF is present, the transistor is off and the LED is dark.

---

## 4. Key components

Table of the main components and their role.

| Component          | Value / Part      | Role in circuit                                      |
|-------------------|-------------------|------------------------------------------------------|
| LM358             | Dual op‑amp IC    | RF envelope amplifier, high gain, single supply     |
| BC547 / BC548     | NPN transistor    | Switch to drive LED (and optional buzzer)           |
| Antenna           | 5–10 cm wire      | Picks up RF from nearby phones                      |
| R (bias)          | 100 kΩ            | Sets op‑amp input bias level                        |
| R (feedback)      | 220 kΩ            | Sets op‑amp gain (with preset)                      |
| VR1 (preset)      | 47 kΩ             | Sensitivity / threshold adjustment                  |
| R (base)          | 1 kΩ              | Limits base current into BC547                      |
| R (LED)           | 220 Ω             | Limits LED current                                  |
| C (coupling)      | 1 µF (×2)         | RF/AC coupling, DC blocking                         |
| C (bypass)        | 100 µF            | Power supply decoupling / noise reduction           |
| LED               | 5 mm red LED      | Visual indication of phone detection                |
| Power supply      | 4.5–5 V DC        | Battery pack or regulated adapter                   | 

Optional: a small buzzer can be added in parallel with the LED (with proper series resistor or transistor drive) for audible indication.

---

## 5. Connections

### 5.1 LM358 core connections

| LM358 pin | Name          | Connection / function                                |
|-----------|---------------|------------------------------------------------------|
| 1         | OUT A         | To BC547 base via 1 kΩ, and to VR1 wiper            |
| 2         | IN A−         | To VR1 fixed end, antenna coupling network          |
| 3         | IN A+         | From antenna via 1 µF + 100 kΩ bias network         |
| 4         | VEE (GND)     | Ground                                              |
| 5         | IN B+         | Tied to ground (unused channel)                     |
| 6         | IN B−         | Tied to ground (unused channel)                     |
| 7         | OUT B         | Left open (unused channel)                          |
| 8         | VCC           | +4.5–5 V supply, with 100 µF bypass capacitor       | 

### 5.2 Input, feedback, and output parts

| Part                 | From                  | To                          | Purpose                               |
|----------------------|----------------------|-----------------------------|---------------------------------------|
| Antenna              | Wire                 | LM358 Pin 2 or 3 (per build)| RF pickup                             |
| 1 µF coupling cap    | Antenna              | LM358 input                 | AC coupling                           |
| 100 kΩ resistor      | LM358 Pin 3          | GND                         | Input bias                            |
| 220 kΩ resistor      | LM358 Pin 3          | +V (4.5–5 V)                | Pull‑up / gain setting                |
| 47 kΩ preset (VR1)   | LM358 Pin 2 (end)    | LM358 Pin 1 (wiper)         | Sensitivity / feedback threshold      |
| 1 kΩ (base resistor) | LM358 Pin 1          | BC547 base                  | Limits base current                   |
| 1 kΩ (collector load)| +V                   | BC547 collector             | Load for transistor (version‑dependent)|
| 220 Ω                | BC547 collector      | LED anode                   | LED current limit                     |
| LED cathode          | LED cathode          | GND                         | Return path                           |

Note: your exact lab build may have slight variations (e.g., whether the 1 kΩ collector resistor and 220 Ω are in series, or LED straight from collector with just 220 Ω). Keep the README aligned with whatever version you push to the repo.

---

### 6 Limitations

- Not a calibrated or certified RF instrument; it is a **hobby/academic‑level detector**.  
- Susceptible to other RF sources (Wi‑Fi, Bluetooth, etc.), although careful tuning and placement reduce false alarms.  
- Detection range depends heavily on **environment, antenna orientation, and phone transmit power**.  



