# 6T SRAM Cell & Peripheral Circuit Design (180nm CMOS)

Transistor-level design, stability characterization, and transient verification of a **6T SRAM bitcell** integrated with its core peripheral circuitry—**precharge/equalization**, **write driver**, and **differential sense amplifier**—implemented in **180nm CMOS (`gpdk180`)** using Cadence Virtuoso and Spectre.

---

## Architecture & Sub-Circuits

The memory column (`Memory_test`) integrates four custom-designed blocks built using **`gpdk180` NMOS and PMOS cells** and driven by **`analogLib` sources** at a **1.8 V** nominal supply:

1. **6T SRAM Bitcell (`6T_SRAM`)**
   - Built using two cross-coupled CMOS inverters for bistable storage (`Q`, `QB`) and two NMOS access transistors (`W = 480 nm`, `L = 180 nm`) gated by the Wordline (`WL`) to interface with the Bitline (`BL`) and Bitline Bar (`BLB`).
   - Transistor sizing and inverter switching thresholds were tuned to balance cell stability and writeability.

2. **Precharge & Equalization Circuit (`Precharge_CKT`)**
   - 3-transistor PMOS topology (`W = 480 nm`, `L = 180 nm`) controlled by an active-low precharge clock (`P_clk`).
   - Precharges and equalizes `BL` and `BLB` prior to read operations to minimize voltage mismatch and speed up sensing.

3. **Differential Sense Amplifier (`Sense_Amp`)**
   - Current-mirror differential amplifier with an NMOS enable/tail transistor (`RIN`) and an output inverter buffer (`OUT`).
   - Detects small differential voltage swings between `BL` and `BLB` during read cycles and amplifies them to full-rail logic levels.

4. **Write Driver (`WRITE_CKT`)**
   - Controlled by write enable (`E`) and data input (`IN`), driving complementary logic levels onto `BL` and `BLB` to overwrite the stored state in the 6T cell.

---

## Simulation & Characterization Results

All DC and transient analyses were performed in **Cadence ADE Explorer (Spectre)** using `analogLib` DC and pulse voltage sources, and visualized in **ViVA**.

| Metric | Value | Analysis Type |
| :--- | :--- | :--- |
| **Technology / PDK** | `gpdk180` (180nm CMOS `nmos` / `pmos` cells) | — |
| **Testbench Sources** | `analogLib` (`vdc`, `vpulse`, `gnd`, `vdd`) | — |
| **Supply Voltage ($V_{DD}$)** | 1.8 V | — |
| **Inverter Trip Point** | ~934 mV | DC Sweep |
| **Hold Static Noise Margin (SNM)** | ~608 mV (Upper eye: 610.2 mV, Lower eye: 608.4 mV) | DC Butterfly Curve |
| **Read Delay** | 45.72 ps | Transient |
| **Write Delay** | 613.4 ps | Transient |

### Stability Analysis (Butterfly Curve & SNM)
- Performed DC sweeps on internal storage nodes (`Q` and `QB`) to plot the Voltage Transfer Characteristics (VTC) and generate the butterfly curve.
- Adjusted inverter sizing to center the switching trip point near $V_{DD}/2$ (~934 mV), achieving a symmetric Hold SNM of **~608 mV**.

### Functional Verification (Read/Write Transient)
- Verified end-to-end operation across precharge, write, and read cycles in the top-level `Memory_test` testbench.
- Confirmed clean bitline precharging, successful cell state transitions during write (`613.4 ps`), and fast differential sensing during read (`45.72 ps`).

---

## Tools & Libraries
- **Schematic Capture:** Cadence Virtuoso Schematic Editor L
- **Simulation & Analysis:** Cadence ADE Explorer (Spectre), ViVA Graph
- **Libraries:** `gpdk180` (`nmos`, `pmos`), `analogLib`

---

## Repository Contents
- `6T_SRAM_schematic.png` — 6T SRAM bitcell schematic
- `precharge.png` — 3-PMOS precharge and equalization schematic
- `sense_amplifier.png` — Differential current-mirror sense amplifier schematic
- `SRAM_test_schematic.png` — Top-level memory column testbench (`Memory_test`)
- `butterfly_curve_schematic.png` — DC stability testbench (`6T_SRAM_internal_node_access`)
- `trip_point_adjustments.png` — DC VTC showing ~934 mV trip point
- `SRAM_butterfly_curve.png` & `SRAM_noise_margin.png` — Butterfly curve and SNM measurement
- `Output_SRAM.png` — Transient read/write waveforms and scalar delay outputs

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/aee63136-fb86-4990-8b13-14bd922258c0" />

