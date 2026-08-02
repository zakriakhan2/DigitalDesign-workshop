# Digital Logic Design Workshop Tasks

This repository contains the hardware design documentation for two digital logic design projects:

- **Pulse Width Modulation (PWM) Controller**
- **Multi-Product Vending Machine Controller**

Both designs follow the **Finite State Machine + Datapath (FSM+D)** design methodology, where the control logic is separated from the datapath for a modular and scalable hardware architecture.

---

# Repository Structure

```text
.
├── PWM task/
│   ├── pwm datapath.png
│   ├── Pwm fsm.png
│   └── pwm top module.png
│
└── Vending Machine task/
    ├── datapath.png
    ├── FSM.png
    └── top module block diagram.png
```

---

# 1. Pulse Width Modulation (PWM) Controller

## Directory

```text
PWM task/
├── pwm datapath.png
├── Pwm fsm.png
└── pwm top module.png
```

---

## Architecture Overview

The PWM controller generates a square wave with a programmable duty cycle using a counter-comparator architecture controlled by a finite state machine.

The design is divided into three components:

- **Top Module** – Connects the control unit and datapath while defining the external interface.
- **FSM (Control Unit)** – Controls state transitions, register loading, and counter initialization.
- **Datapath** – Contains the counter, duty-cycle register, comparator, and PWM output generation logic.

The datapath buffers the duty-cycle value so updates during an active PWM period do not introduce output glitches.

---

## Design Files

| File | Description |
|------|-------------|
| `pwm top module.png` | Top-level block diagram showing module interconnections and external ports |
| `Pwm fsm.png` | FSM state transition diagram |
| `pwm datapath.png` | Datapath schematic showing counters, registers, and comparator |

---

## Verification

- Use the **Top Module** diagram as the overall design reference.
- Follow the **FSM** state diagram to verify state transitions.
- Cross-reference the **Datapath** schematic to validate data flow and hardware operations.

---

# 2. Multi-Product Vending Machine Controller

## Directory

```text
Vending Machine task/
├── datapath.png
├── FSM.png
└── top module block diagram.png
```

---

## Architecture Overview

The vending machine controller follows the **FSM + Datapath** architecture by separating control logic from arithmetic and storage hardware.

```text
                  +-----------------------------------+
                  |     Top Module                    |
                  |                                   |
Inputs  --------->|  +-----------+     Control     +--|------> Outputs
                  |  |    FSM    |================>|  |
Outputs <---------|--| (Control) |<================|  |
                  |  +-----------+     Status      +--|
                  |                           Datapath
                  +-----------------------------------+
```

The design consists of three main components:

- **Top Module** – Connects the FSM and datapath while exposing the external interface.
- **FSM (Control Unit)** – Implements the Moore finite state machine responsible for transaction sequencing.
- **Datapath** – Performs balance management, product selection, inventory tracking, arithmetic operations, and status generation.

---

## Key Hardware Features

- **Unified Selection Processing** using shared selection logic for multiple products.
- **Resource Sharing** by reusing a single subtractor for purchases and refunds.
- **Active-High Status Signals**, where valid conditions are represented by logic high (`1`).

---

## Design Files

| File | Description |
|------|-------------|
| `top module block diagram.png` | Top-level architecture showing module interconnections |
| `FSM.png` | Moore FSM state transition diagram |
| `datapath.png` | Datapath schematic containing registers, multiplexers, arithmetic units, and storage elements |

---

## Verification

- Use the **Top Module** diagram to understand system connectivity.
- Trace controller behavior using the **FSM** state diagram.
- Validate balance updates, inventory operations, and data flow using the **Datapath** schematic.

---

# Design Methodology

Both projects adopt the **Finite State Machine + Datapath (FSM+D)** design methodology.

- The **FSM** is responsible for sequencing and control decisions.
- The **Datapath** performs computation, storage, comparisons, and status generation.
- The **Top Module** integrates both components into a complete digital hardware system.
