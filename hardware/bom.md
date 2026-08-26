# Bill of Materials (BOM)

Everything you need to build **one** Shellphone.

| # | Part | Qty | Notes / spec |
|---|------|-----|--------------|
| 1 | Raspberry Pi Zero 2 W | 1 | With WiFi (the "2 W"). Needs the 40-pin header soldered on. |
| 2 | microSD card | 1 | 8 GB or larger, for the Pi's software |
| 3 | Shellphone audio board (this project's PCB) | 1 | Export Gerbers from the KiCad project (`hardware/pcb/`) and order from a fab |
| 4 | MAX98357A I2S amplifier breakout | 1 | The blue audio amp module |
| 5 | Bone-conduction transducer, with wires | 1 | **8 Ω, 1 W** — vibrates the shell to make sound |
| 6 | 5 mm LEDs | 2 | The snail's "eyes" (any color) |
| 7 | Resistor, 470 Ω | 2 | One per LED |
| 8 | 2×20 male pin header, 2.54 mm | 1 | Soldered to the Raspberry Pi |
| 9 | 2×20 female socket, 2.54 mm | 1 | On the audio board, plugs onto the Pi |
| 10 | 1×07 female socket, 2.54 mm | 1 | On the audio board, for the MAX98357A |
| 11 | 3D-printed shell enclosure | 1 | Print from the files in `enclosure/` |
| 12 | USB power supply + cable | 1 | 5 V, micro-USB, to power the Pi |
| 13 | Hook-up wire | a little | To connect the transducer to the board |

### Notes
- Items 8–10 (the headers/sockets) are the connectors that let the boards plug together
  instead of being soldered permanently — easier to assemble and repair.
