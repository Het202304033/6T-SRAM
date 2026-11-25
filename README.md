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
| **Technology / PDK** | `gpdk180` (180nm CMOS
