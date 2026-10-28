# CCSDS 851.0-M-1 SOIS Subnetwork Packet Service (SPS) Engineering Specification

## 1\. Executive Summary & Normative Baseline

- **Document Identifier**: CCSDS 851.0-M-1 (Magenta Book, Issue 1, Recommended Practice)  
- **Publication Date**: December 2009  
- **Title**: Spacecraft Onboard Interface Services (SOIS) — Subnetwork Packet Service (SPS)  
- **Subnetwork Working Group**: Consultative Committee for Space Data Systems (CCSDS)  
- **Implementation**: SPSForge Pro (v1.0.0)

This document formalizes the architectural principles, service primitives, priority queuing semantics, Quality of Service (QoS) guarantees, channel segregation, and subnetwork encapsulation rules established by CCSDS 851.0-M-1 for onboard spacecraft communications networks.

---

## 2\. Architectural Role in SOIS Reference Model (§2)

The SOIS Subnetwork Packet Service (SPS) provides a uniform, bus-independent interface for transmitting discrete, bounded packets between software entities and avionics subsystems across diverse physical spacecraft subnetworks (SpaceWire, MIL-STD-1553B, CAN, Ethernet, and point-to-point serial links).

  ┌─────────────────────────────────────────────────────────────┐

  │       Spacecraft Flight Software Application Entities       │

  │      (ADCS, FDIR, Telemetry, Command Handling, Payloads)     │

  └──────────────────────────────┬──────────────────────────────┘

                                 │

                   SPS Service Primitives (§3.2)

         \[PACKET\_SEND.request / PACKET\_RECEIVE.indication\]

                                 │

  ┌──────────────────────────────▼──────────────────────────────┐

  │         SOIS Subnetwork Packet Service (SPS) Provider       │

  │   • Multi-Tier Priority Scheduling: 8 Levels (0..7) (§3.1.4)│

  │   • Dual Quality of Service: Best Effort vs Assured (§2.4)   │

  │   • Logical Channel Segregation: Channels 0..15 (§3.1.3)    │

  │   • Fault Isolation & PACKET\_FAILURE.indication (§3.2.4)    │

  └──────────────────────────────┬──────────────────────────────┘

                                 │

                   Subnetwork Framing Adapters (§2.3)

                                 │

  ┌──────────────────────────────▼──────────────────────────────┐

  │                 Spacecraft Physical Subnetworks             │

  ├──────────────────────┬──────────────────────┬───────────────┤

  │ SpaceWire Protocol 0x02  MIL-STD-1553B 32-W   CAN 2.0B Frames │

  │  (ECSS-E-ST-50-12C)  │ (Subaddress Blocks)  │ (Multi-Frame) │

  └──────────────────────┴──────────────────────┴───────────────┘

---

## 3\. Service Parameters & Attributes (§3.1)

The Subnetwork Packet Service operates on five fundamental service parameters:

1. **Destination Subnetwork Address (§3.1.2)**: An abstract address identifying the destination subnetwork entity or node. Supports both point-to-point addresses and broadcast addresses (`0xFF`) for multi-drop buses.  
2. **Channel Identifier (§3.1.3)**: An integer (typically 0 to 15\) identifying a logical communication path between communicating entities. Allows traffic segregation within the same physical subnetwork without mutual interference.  
3. **Priority Level (§3.1.4)**: An integer value ranging from 0 (lowest / background bulk payload) to 7 (highest / critical telecommand and FDIR triggers). Higher-priority packets preempt lower-priority packets in subnetwork transmission queues.  
4. **Quality of Service (QoS) (§2.4)**:  
   - **Best Effort**: Unacknowledged transmission without automatic retransmission or delivery guarantees.  
   - **Assured Delivery**: Guaranteed in-order packet delivery with error detection, retransmission, and flow control.  
5. **Packet Data**: A sequence of octets comprising the packet payload passed from the service user.

---

## 4\. Service Primitives (§3.2)

CCSDS 851.0-M-1 explicitly defines three and only three service primitives:

### 4.1 PACKET\_SEND.request (§3.2.2)

PACKET\_SEND.request (

    Destination Subnetwork Address,

    Channel,

    Priority,

    Quality of Service,

    Packet Data

)

- Dispatched by the service user to transmit an onboard packet.  
- Enqueued into the corresponding subnetwork priority queue based on the requested priority level.

### 4.2 PACKET\_RECEIVE.indication (§3.2.3)

PACKET\_RECEIVE.indication (

    Source Subnetwork Address,

    Destination Subnetwork Address,

    Channel,

    Priority,

    Quality of Service,

    Packet Data

)

- Emitted by the SPS provider to deliver an incoming subnetwork packet to the receiving service user entity.  
- Guarantees in-order arrival for Assured QoS streams within a given channel and priority.

### 4.3 PACKET\_FAILURE.indication (§3.2.4)

PACKET\_FAILURE.indication (

    Destination Subnetwork Address,

    Channel,

    Priority,

    Quality of Service,

    Packet Data,

    Failure Reason

)

- Emitted to notify the service user entity when a packet cannot be delivered.  
- Reason codes include: `UNREACHABLE_DESTINATION`, `INVALID_CHANNEL`, `QUEUE_OVERFLOW`, and `LINK_DOWN`.

---

## 5\. Subnetwork Bus Encapsulation Mappings (§2.3)

1. **SpaceWire (ECSS-E-ST-50-12C)**:  
   - Prepends Target Logical Address, SOIS Packet Protocol ID (`0x02`), and Channel octet.  
   - Terminates packet with Normal End of Packet (`EOP`) marker.  
2. **MIL-STD-1553B**:  
   - Transmits packet payload across 16-bit word boundaries within assigned Subaddress blocks.  
3. **CAN 2.0B / CANopen**:  
   - Segments packet into 8-byte payload frames using CAN 29-bit identifiers with message group prioritization.