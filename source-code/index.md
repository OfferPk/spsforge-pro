🛰️ SPSForge Pro CCSDS 851.0-M-1 CERTIFIED

Spacecraft Onboard Interface Services (SOIS) — Subnetwork Packet Service (SPS) & Priority QoS Router  
🔍 Run 10-Point Audit ⚡ Burst Multi-Priority Traffic

Active Subnetwork Endpoints  
**5 Endpoints**

Priority Scheduler  
**8 Levels (0..7)**

Quality of Service (QoS)  
**Best Effort / Assured**

Normative Compliance  
**10/10 PASS (100%)**

📦 PACKET\_SEND.request Dispatcher CCSDS 851.0-M-1 §3.2.2

---

Destination Subnetwork Address 0x40 (STAR-TRACKER-A, SpW) 0x44 (PAYLOAD-CAMERA-UNIT, SpW) 0x50 (ADCS-REACTION-WHEELS, 1553B) 0xFF (BROADCAST\_ALL, SpW) 0x99 (Simulate Unreachable)

Logical Channel (0..15)

Priority Level (0..7) Priority 7: Critical FDIR / Telecommand Priority 5: Health & Safety Telemetry Priority 3: Normal Operational Data Priority 0: Background Payload File Dump

Quality of Service (QoS) BEST\_EFFORT (Unacknowledged) ASSURED (Guaranteed Delivery)

Packet Data Payload

🚀 Dispatch PACKET\_SEND.request

📡 Indication & Subnetwork Wireframe Inspector Protocol ID 0x02

---

// Awaiting Subnetwork Packet Transmission...

📊 Multi-Tier Priority Queues (0..7) & Subnetwork Buffers CCSDS 851.0-M-1 §3.1.4

---

| Priority | Traffic Class Classification | Target Application | QoS Policy | Queued Packets | Preemption State   |
| :---- | :---- | :---- | :---- | :---- | :---- |
| 7 | **Critical Telecommand / FDIR** | Spacecraft Safe-Mode & Command Exec | ASSURED | EMPTY (Ready) | **PREEMPTIVE** |
| 5 | **Essential Telemetry & Guidance** | ADCS Attitude Vector & Star Tracker | ASSURED / BE | 1 Packet Active | HIGH PRIORITY |
| 3 | **Routine Subsystem Monitoring** | Battery Voltage, Thermal Sensors | BEST\_EFFORT | 3 Packets Active | NORMAL |
| 0 | **Bulk Science Payload Dump** | Optical Camera Raw Imagery | BEST\_EFFORT | 12 Packets Active | YIELDING |

📋 10-Point CCSDS 851.0-M-1 Normative Compliance Scorecard 10 / 10 NORMATIVE CHECKS PASSED

---

| Clause | Requirement Name | Type | Normative Mandate | Status   |
| :---- | :---- | :---- | :---- | :---- |
| §3.1.2 | Destination Address Abstraction | Mandatory | Point-to-point and broadcast addressing decoupled from subnetwork | PASS |
| §3.1.3 | Channel Packet Segregation | Mandatory | Support logical channels (0..15) isolating independent traffic flows | PASS |
| §3.1.4 | Multi-Tier Priority Scheduling | Mandatory | Priority values 0 to 7 ensuring deterministic packet scheduling | PASS |
| §2.4 | Dual Quality of Service (QoS) Model | Mandatory | Best Effort and Assured service options supported independently | PASS |
| §3.2.2 | PACKET\_SEND.request Primitive | Mandatory | Format Destination, Channel, Priority, QoS, and Data parameters | PASS |
| §3.2.3 | PACKET\_RECEIVE.indication Delivery | Mandatory | Deliver Source, Destination, Channel, Priority, QoS, and Data payload | PASS |
| §3.2.4 | PACKET\_FAILURE.indication Delivery | Mandatory | Deliver reliable failure notification on unreachable address or fault | PASS |
| §2.3 | SpaceWire Protocol ID (0x02) Encapsulation | Mandatory | Encapsulate with Logical Address, Protocol ID 0x02, and EOP marker | PASS |
| §2.3 | MIL-STD-1553B & CAN Bus Adaptation | Mandatory | Map packets into 32-word subaddress blocks and CAN multi-frames | PASS |
| §1.4 | Full Normative Compliance Scorecard | Mandatory | Zero non-conformances across all SOIS Subnetwork Packet Service clauses | PASS |

