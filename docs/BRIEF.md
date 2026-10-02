# a wireless charging bay

Design a complete **11-channel wireless charging dock PCB** based on the following architecture and component selections.

## TARGET

* 11 independent wireless charging channels
* Target wireless output: **45 W per channel**
* Maximum total wireless output: **495 W**
* Main DC bus: **48 V**
* Main PSU: **Mean Well RSP-750-48, 48 V / 750 W**
* Each channel must be independently controlled, monitored and protected.
* Design for simultaneous operation of all 11 channels.
* This is a high-power wireless charging system; prioritize thermal management, creepage/clearance, protection and serviceability.

## POWER ARCHITECTURE

48 V DC PSU
→ main input protection
→ 11-way power distribution
→ individual channel protection
→ MOSFET switching stage
→ resonant compensation network
→ matched 45–60 W TX coil
→ wireless link
→ matched 45 W RX coil/module
→ USB-C PD output

Do NOT assume that a generic 5 W/10 W Qi receiver can provide 45 W.

---

# EXACT COMPONENTS

### Main PSU

**Mean Well RSP-750-48**

* 48 V DC
* 750 W
* Main input supply

Use the manufacturer datasheet for electrical limits.

### MOSFET

**Infineon IPT015N10N5**

* 100 V N-channel MOSFET
* Use in the high-frequency switching stage.
* Select the appropriate number of MOSFETs based on the final bridge topology.
* Use proper gate resistors, gate-source protection and local DC-link decoupling.

### Gate Driver

**Texas Instruments UCC27211**

Use one UCC27211 per wireless TX channel.

Quantity: **11**

The gate driver must be powered according to its datasheet and connected with proper bootstrap circuitry if the selected topology requires it.

### Current Sense

**Texas Instruments INA240**

Use one INA240 monitoring circuit per channel.

Quantity: **11**

Monitor TX-channel current and provide current feedback to the controller.

### MCU

Use **STM32G0 series MCU**, one per channel initially.

Quantity: **11**

Each MCU should control:

* Channel enable/disable
* TX power control
* Current monitoring
* Temperature monitoring
* Fault detection
* Over-current shutdown
* Over-temperature shutdown

Provide a communication bus between all 11 channel controllers and a central controller.

Use CAN or RS-485 if appropriate.

### Temperature

Use NTC thermistors on:

1. MOSFET heatsink
2. TX coil
3. Resonant capacitor/power stage

Approximately 3 sensors per channel.

Total: **33 NTC sensors**

---

# WIRELESS POWER STAGE

For each channel:

48 V DC
→ DC-link capacitor
→ MOSFET high-frequency bridge
→ resonant compensation network
→ TX coil

The target is:

**45 W continuous wireless output per channel.**

Use the correct resonant topology and switching frequency based on the selected high-power wireless charging protocol and matched TX/RX coil system.

IMPORTANT:

Do NOT invent coil inductance, compensation capacitance, switching frequency or receiver parameters.

If the exact 45 W TX/RX coil/module datasheet is not available, mark these values as:

**TBD — REQUIRE MANUFACTURER DATASHEET**

and create clearly labelled parameter blocks rather than fabricating values.

---

# RX SIDE

Each channel requires a matched:

* High-power RX coil
* Ferrite backing
* 45 W receiver module
* Rectification/regulation stage
* USB-C PD output

Target:

**45 W continuous output**

The RX system must be compatible with the TX system.

Do not substitute generic low-power Qi receiver boards.

Create the RX interface as a clearly defined connector/interface if the exact receiver PCB is an external module.

---

# MAIN POWER DISTRIBUTION

Create:

48 V INPUT
→ main fuse
→ reverse-polarity protection
→ surge/transient protection
→ bulk capacitor
→ 11 individual fused outputs

Each channel must have its own:

* Fuse
* Current sensing
* Local bulk capacitance
* Emergency shutdown capability

Calculate trace widths and copper requirements for the expected channel current.

---

# PROTECTION

Implement:

* Input over-current protection
* Per-channel over-current protection
* Over-voltage protection
* Under-voltage protection
* MOSFET over-temperature protection
* Coil over-temperature protection
* Resonant-stage fault detection
* Short-circuit protection
* Foreign-object detection interface
* Emergency shutdown

If a protection circuit cannot be safely implemented from the available component data, expose it as a clearly labelled design block rather than guessing.

---

# THERMAL DESIGN

The system may produce approximately **70–100 W of heat** at full load depending on final wireless efficiency.

Design the PCB and enclosure interface around:

* Large aluminium thermal plate
* MOSFET heatsinking
* Forced-air cooling
* Dedicated airflow paths
* Temperature sensors
* Automatic power derating

Keep high-power switching components physically close to the heatsink/thermal interface.

Separate sensitive MCU/sensing circuitry from the high-current/high-frequency switching section.

---

# PCB REQUIREMENTS

Create a professional production-oriented PCB.

Use:

* 4-layer PCB minimum
* 2 oz copper on power layers where practical
* Short high-current paths
* Very small switching loops
* Proper gate-driver layout
* Kelvin current-sense connections
* Separate power and control grounds where appropriate
* Controlled return paths
* Thermal vias beneath power components
* Large copper pours around high-current nodes
* Adequate creepage/clearance
* Test points for every important rail and signal

Keep the 11 power channels physically separated where practical to reduce EMI and thermal coupling.

---

# PCB ORGANIZATION

Prefer a modular architecture:

### Board 1

Main 48 V power distribution + protection + central controller.

### Boards 2–12

One identical 45 W wireless TX channel per board.

Each channel PCB should contain:

* Input protection
* UCC27211
* IPT015N10N5 MOSFETs
* Resonant power stage
* INA240
* STM32G0
* NTC interfaces
* TX coil connector
* Communication connector
* Debug/programming connector

This modular architecture is preferred over putting all 11 high-frequency power stages onto one enormous PCB.

---

# SCHEMATIC OUTPUT

First generate the complete electrical schematic.

For every component show:

* Reference designator
* Exact part number
* Value/rating
* Pin connections
* Net names
* Connector pinout
* Protection components
* Test points

Create hierarchical blocks:

1. MAIN_POWER
2. POWER_DISTRIBUTION
3. TX_CHANNEL_01
4. TX_CHANNEL_02
5. ...
6. TX_CHANNEL_11
7. CONTROL_BUS
8. THERMAL_MONITORING
9. RX_INTERFACE

Do not duplicate the channel design manually if the CAD system supports hierarchical/multi-channel schematic blocks.

---

# PCB OUTPUT

After schematic validation:

1. Assign footprints.
2. Perform ERC.
3. Generate PCB.
4. Place components according to the power/thermal architecture.
5. Route power first.
6. Route gate-driver paths.
7. Route current sensing.
8. Route MCU/control signals.
9. Perform DRC.
10. Generate 3D view.
11. Generate manufacturing outputs.

---

# REQUIRED OUTPUTS

Generate:

1. Complete schematic
2. 11-channel hierarchical schematic
3. PCB layout
4. 3D PCB model
5. Complete BOM
6. Component values
7. Footprints
8. Netlist
9. ERC report
10. DRC report
11. PCB manufacturing files/Gerbers
12. Pick-and-place file
13. Assembly drawing
14. Power-loss/thermal estimate
15. Per-channel current calculation
16. Main PSU loading calculation

## CRITICAL RULE

Do not fabricate missing wireless-power parameters.

Where the exact 45 W TX/RX coil or receiver datasheet is required, explicitly mark the parameter **TBD** and identify exactly what information is required.

The design objective is **11 × 45 W wireless output**, not merely 45 W total input power.

Prioritize a technically valid and manufacturable design over completing missing values with assumptions.
