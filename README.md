# High-Power Custom Meshtastic Node (ESP32-C6 + 33dBm LoRa)

> **⚠️ PROJECT STATUS: WORK IN PROGRESS (PROTOTYPE PHASE)**  
> This project is currently under active development and hardware verification. The schematic, PCB layout, and BOM are subject to frequent revisions. Production-ready design files will be released following initial prototype validation and bench testing.

---

## Overview

A fully integrated, high-power, autonomous **Meshtastic** node hardware implementation designed from scratch. Unlike standard off-the-shelf low-power nodes, this design integrates an on-board 2W (+33 dBm) RF front-end, dedicated power path management with dual 18650 cells, USB-C flashing/charging, and multi-constellation GNSS positioning onto a single PCB.

---

## Hardware Architecture & Key Specifications

* **MCU:** Espressif ESP32-C6-WROOM-1-N8
  * 32-bit RISC-V single-core @ 160 MHz
  * 8 MB Quad SPI Flash
  * 2.4 GHz Wi-Fi 6, BLE 5 (LE), 802.15.4 (Zigbee / Thread)
* **LoRa Transceiver & PA:** Ebyte E22-900M33S
  * Semtech SX1262 architecture
  * Integrated discrete RF PA providing up to +33 dBm (~2W) output power
  * Sub-GHz ISM band (868/915 MHz targeting)
* **GNSS Engine:** u-blox MAX-M10S
  * Concurrent reception of GPS, GLONASS, Galileo, and BeiDou
  * Integrated TCXO and LNA for weak signal tracking
  * Ultra-low power consumption profile
* **Power Management & Battery Subsystem:**
  * **Topology:** Dual 18650 Li-ion cell configuration
  * **BMS / Protection:** Integrated overvoltage, undervoltage cutoff, overcurrent, and short-circuit protection circuitry
  * **Charging:** On-board linear/switching charger powered via USB Type-C (standard CC configuration for native PD/5V supply negotiation)
  * **Transient Capability:** Dedicated high-current regulator stage dimensioned for 1.2A–1.5A peak current pulses during +33 dBm TX bursts
* **Flashing & Interface:**
  * Native USB Type-C interface incorporating auto-reset/boot circuitry and on-board USB-to-UART bridging for direct firmware flashing and serial telemetry

---

## Critical Engineering Considerations

* **Transient Loads & Power Rail Integrity:** The E22-900M33S pulls high transient currents at peak TX power (+33 dBm). The design implements a low-ESR bulk decoupling array close to the module's VCC pins, along with dedicated power routing to prevent voltage drops that could trigger an MCU brownout reset (BOR).
* **RF Routing & Layout:** Antenna trace impedance is strictly controlled to 50Ω coplanar waveguide with ground (CPW-G). Solid reference planes are maintained on layer 2 directly below the RF path up to the edge-mount/vertical SMA connector.
* **Thermal Dissipation:** The integrated PA generates localized heat during high-duty-cycle packet bursts. An exposed thermal pad matrix tied to the ground plane with multiple thermal vias is placed directly beneath the module to sink heat effectively.

---

## Project Roadmap

- [x] High-level system architecture and component selection
- [x] Symbol creation and footprint verification for critical ICs/modules
- [ ] Schematic capture and power distribution network (PDN) design
- [ ] PCB routing, 50Ω impedance calculation, and thermal dissipation modeling
- [ ] Prototype fabrication (Rev 1.0) and SMD hand assembly / reflow
- [ ] Meshtastic firmware integration (custom variant board definitions and pin map)
- [ ] RF output power, spectral purity (harmonics), and field range verification

---

## Operational Warnings

1. **RF Load Notice:** Never transmit at full power (+33 dBm) without a tuned 50Ω antenna or calibrated dummy load securely connected to the SMA terminal. High reflected power (VSWR mismatch) can cause permanent damage to the PA output stage.
2. **Regulatory Compliance:** An output power of +33 dBm exceeds standard unlicensed ISM-band EIRP limits in several jurisdictions. Verify local regulations and licensing requirements (e.g., amateur radio license operating parameters) prior to continuous transmission.

---

## License

Hardware design files, schematics, and layouts will be published under an open hardware license (CERN-OHL or similar) upon hardware verification.
