# SE-Grid-07 Simulation

A browser-based simulation of a Swedish 400 kV / 130 kV substation incorporating a 250 MVA OFAF transformer, developed for the master's thesis *From Compliance to Resilience: A Sociotechnical Analysis of NIS2 Regulations and Multi-Vector SCADA Vulnerabilities in the Swedish Power Grid*.

## What this simulation does

The simulation models the interaction between three subsystems:

- **The physical transformer**, following the IEC 60076-7 thermal model (separate oil and winding time constants, hotspot factor, insulation ageing)
- **The Modbus TCP protocol layer**, faithfully reproducing the cleartext, unauthenticated behaviour of real Modbus TCP RTUs
- **The SCADA operator HMI**, following ANSI/ISA-101.01 design conventions

It allows the researcher to inject MITRE ATT&CK for ICS techniques T0832 (Manipulation of View) and T0855 (Unauthorized Command Message) and measure the Integrity Visibility Metric (IVM) — the absolute difference between physical reality and the value displayed to the operator.

## How to run it

1. Download `thesis_simulation_v6.html` from this repository
2. Open the file in any modern web browser (Chrome, Firefox, or Safari)
3. No installation, no network dependencies, no server required

The control panel on the right-hand side allows you to activate the three attack stages described in the thesis:

- **Stage 1** — Normal operation (no attack)
- **Stage 2** — T0855 only (kinetic strike without perception manipulation)
- **Stage 3** — T0832 + T0855 (full multi-vector attack)

Click the **Export CSV** button at any time to download a record of every value at every simulation tick.

## Reproducibility

JavaScript executes single-threaded in the browser. The simulation has no race conditions, no timing variability, and no random seeds beyond a deterministic sensor-noise generator. The same inputs produce the same outputs on every execution. The Stage 3 IVM result reported in the thesis (45.8 °C at the moment of blackout) can be verified by any third party who downloads and runs this file.

## Technical specifications

| Component | Standard / Reference |
|---|---|
| Thermal model | IEC 60076-7:2018 |
| Alarm hierarchy | EEMUA 191, IEC 62682:2014 |
| HMI design | ANSI/ISA-101.01-2015 |
| Buchholz relay | IEEE C57.104-2019 |
| Protocol behaviour | Modbus Application Protocol Specification v1.1b3 |
| Attack techniques | MITRE ATT&CK for ICS T0832, T0855 |
| SCADA polling cycle | NIST SP 800-82 Rev 3 (1 second) |
| Reference incident | FrostyGoop (Lviv, January 2024) |

## File structure

The simulation is a single HTML file containing approximately 1,500 lines:

- Lines 1–169: HTML head and CSS styling
- Lines 170–665: HTML body and UI components
- Lines 666–820: Three.js 3D visualisation
- Lines 820–950: IEC 60076-7 thermal model
- Lines 950–1100: Modbus TCP protocol layer
- Lines 1100–1230: Attack injection logic (T0832, T0855)
- Lines 1230–1350: Alarm hierarchy (EEMUA 191, IEC 62682)
- Lines 1350–1450: Data recording and CSV export
- Lines 1450–1532: Initialisation and event handlers

## How to cite this work

If you use this simulation in your research, please cite the thesis:

> Bekele, N. (2026). *From Compliance to Resilience: A Sociotechnical Analysis of NIS2 Regulations and Multi-Vector SCADA Vulnerabilities in the Swedish Power Grid* [Master's thesis, Swedish Defence University].

And the software:

> Bekele, N. (2026). *SE-Grid-07: A SCADA simulation for studying multi-vector cyberattacks on Swedish power infrastructure* ( . GitHub. https://github.com/nbekele-code/SE-Grid-07-simulation

## Ethical use

This simulation is published for academic and educational purposes. It models attack techniques that have been observed in publicly documented real-world incidents (Ukraine 2015, FrostyGoop 2024). The simulation itself has no network capability and cannot reach any real industrial control system; running it cannot harm any production infrastructure. The techniques modelled are already documented in the MITRE ATT&CK for ICS knowledge base and in peer-reviewed academic literature. The thesis neither generates novel attack capability nor exposes any specific operator to risk.

## Licence

This software is published under the MIT Licence. See `LICENSE` for details.

## Contact

For questions about the simulation or the underlying thesis, please open an issue on this repository or contact the author through Swedish Defence University.

---

*This work was conducted as part of master's programme 2IFS1 at Försvarshögskolan (Swedish Defence University), 2025–2026.*
