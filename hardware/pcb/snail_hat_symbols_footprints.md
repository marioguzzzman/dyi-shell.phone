# Sound Snail HAT — KiCad Symbols & Footprints Reference

A cheat-sheet for the custom snail PCB. The board is a **HAT**: it plugs down onto
the Raspberry Pi Zero 2 W. Stack from bottom to top: **Pi → (your components) → snail PCB**.

Because it's a HAT, the Pi interface on *your* board is a **female socket on the
bottom side**. You solder a normal male 2×20 header to the Pi; your board's socket
drops onto it.

---

## 1. Raspberry Pi Zero 2 W — 40-pin GPIO (the "row of 20 pins", ×2)

The Zero 2 W header is **2×20 = 40 pins**, 2.54 mm pitch (same layout as every Pi).

| | KiCad library:name |
|---|---|
| **Symbol** | `Connector:Raspberry_Pi_2_3` — 40-pin symbol with every GPIO already named (SDA, SCL, GPIO18, 5V, GND…). Much easier to wire than a blank header. |
| **Footprint** | `Connector_PinSocket_2.54mm:PinSocket_2x20_P2.54mm_Vertical` — **female** socket. |

Placement notes:
- Put the socket footprint on the **bottom copper layer** (place it, then press **F** to flip to back), so it faces the Pi.
- Keep the header in the official HAT position relative to the mounting holes if you want it to line up mechanically.

---

## 2. MAX98357A I2S amplifier

You're using the **breakout module** (the blue board in your drawing), so treat it as a
connector — you're not placing the bare chip.

| | KiCad library:name |
|---|---|
| **Symbol (control pins)** | `Connector_Generic:Conn_01x07` (7 pins: LRC, BCLK, DIN, GAIN, SD, GND, Vin) |
| **Footprint** | `Connector_PinHeader_2.54mm:PinHeader_1x07_P2.54mm_Vertical` (or `PinSocket_1x07…` if you want it removable) |
| **Speaker out symbol** | `Connector_Generic:Conn_01x02` (+ and −) |
| **Speaker out footprint** | `Connector_PinHeader_2.54mm:PinHeader_1x02_P2.54mm_Vertical`, or a screw terminal `TerminalBlock_Phoenix:TerminalBlock_Phoenix_MKDS-1,5-2_1x02_P5.00mm_Horizontal` |

*(If you ever switch to the bare IC instead of the breakout: symbol isn't in the default
libs — package is TQFN-16 3×3 mm, footprint `Package_DFN_QFN:TQFN-16-1EP_3x3mm_P0.5mm_EP1.8x1.8mm`.
Hard to hand-solder; stick with the breakout.)*

---

## 3. LEDs (the two snail eyes) — using 5 mm assortment

| | KiCad library:name |
|---|---|
| **Symbol** | `Device:LED` |
| **Footprint (THT 5 mm)** | `LED_THT:LED_D5.0mm` ← using this |

**Brightness note (assorted colors):** 470 Ω on a 3.3 V GPIO gives ~2.8 mA —
great for **red / yellow / orange / green**. **Blue and white** LEDs have a forward
voltage near 3.3 V, so they'll be dim. For bright blue/white eyes, drop to ~150–220 Ω
for those, or drive from 5 V through a small transistor.

---

## 4. Resistors — using 470 Ω

| | KiCad library:name |
|---|---|
| **Symbol** | `Device:R` (value 470) |
| **Footprint (THT axial)** | `Resistor_THT:R_Axial_DIN0207_L6.3mm_D2.5mm_P10.16mm_Horizontal` |
| **Footprint (SMD 0805)** | `Resistor_SMD:R_0805_2012Metric` |

One 470 Ω in series with each LED.

---

## 5. Optional but recommended

| Part | Symbol | Footprint |
|---|---|---|
| Mounting holes (Pi uses M2.5) | — | `MountingHole:MountingHole_2.7mm_M2.5` |
| Decoupling cap near amp (e.g. 100 nF / 10 µF) | `Device:C` | `Capacitor_SMD:C_0805_2012Metric` |

---

## 6. How to wire it (net connections)

**MAX98357A → Pi (I2S / PCM):**

| Amp pin | Pi signal | Physical pin |
|---|---|---|
| BCLK | GPIO18 (PCM_CLK) | 12 |
| LRC  | GPIO19 (PCM_FS)  | 35 |
| DIN  | GPIO21 (PCM_DOUT) | 40 |
| Vin  | 5V | 2 or 4 |
| GND  | GND | 6 (or any GND) |
| SD   | leave unconnected (breakout defaults to ON) | — |
| GAIN | leave unconnected (= 9 dB gain) | — |

*(Only the 5 wires above — Vin, GND, DIN, BCLK, LRCLK — are needed. SD and GAIN
stay unconnected on the breakout.)*

**Speaker** → the amp's + and − output pads (4–8 Ω speaker).

**LEDs (snail eyes):** each LED anode → a free GPIO (e.g. GPIO17 = pin 11, GPIO27 = pin 13)
through its resistor; cathode → GND.

---

### Quick workflow in KiCad
1. Schematic editor → **Add Symbol (A)** → drop the symbols above.
2. Wire the nets per section 6, add net labels (5V, 3V3, GND, BCLK, LRC, DIN).
3. **Assign footprints** (or set them inline as above) → run **Annotate** + **ERC**.
4. **Update PCB from Schematic (F8)** → place parts, flip the Pi socket to the back layer.
5. Import your snail outline SVG as the **Edge.Cuts** / silk art, then route the red traces.
