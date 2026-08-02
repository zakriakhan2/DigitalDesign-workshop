```markdown
# Digital Logic Design Workshop Tasks

This repository contains Verilog RTL implementations, block schematics, and FSM state diagrams for two hardware design tasks: a **Pulse Width Modulation (PWM) Controller** and a **Multi-Product Vending Machine Controller**. Both tasks utilize decoupled Finite State Machine and Datapath (FSM+D) design principles.

---

## 1. PWM Task

### Directory & Files
```text
PWM task/
├── pwm datapath.png        # Datapath schematic (counter, registers, comparator)
├── Pwm fsm.png             # FSM state transition diagram
├── pwm top module.png      # Top-level module block diagram & pinouts
├── pwm_datapath.v          # Datapath RTL implementation
├── pwm_fsm.v               # FSM control unit RTL implementation
└── pwm_top.v               # Top-level wrapper connecting FSM and Datapath

```

### Architecture Overview

The PWM core produces a precise square wave with variable duty cycle control using a counter-comparator architecture managed by an FSM control unit:

* **`pwm top module`**: Top-level wrapper interconnecting the control unit and datapath modules.
* **`Pwm fsm`**: Controls operation states, driving register load enables and counter initialization sequence.
* **`pwm datapath`**: Houses the period counter, buffered duty cycle register (preventing output glitches during mid-period dynamic updates), and magnitude comparator generating `pwm_out`.

### Module Interface

| Port Name | Direction | Bit Width | Description |
| --- | --- | --- | --- |
| `clk` | Input | 1 | Primary system clock |
| `rst` | Input | 1 | Synchronous reset line |
| `duty_in` | Input | `[N-1:0]` | Parallel input threshold for duty cycle |
| `load_en` | Input | 1 | Control trigger to update duty cycle threshold |
| `pwm_out` | Output | 1 | Generated PWM square-wave output |

### Verification & Waveform Inspection

1. Import all Verilog source files (`pwm_datapath.v`, `pwm_fsm.v`, `pwm_top.v`) into your simulation environment (e.g., Vivado, ModelSim, Icarus Verilog).
2. Reference `pwm top module.png` for top-level port bindings during testbench instantiation.
3. Compare output signal waveforms directly against state transitions in `Pwm fsm.png` and functional blocks in `pwm datapath.png`.

---

## 2. Vending Machine Task

### Directory & Files

```text
Vending Machine task/
├── datapath.png                  # Datapath schematic (registers, arithmetic, MUXes)
├── FSM.png                       # FSM Moore state transition diagram
├── top module block diagram.png  # Top-level system interconnections
├── datapath.v                    # Datapath RTL implementation
├── fsm.v                         # FSM control unit RTL implementation
└── vending_top.v                 # Top-level module connecting FSM and Datapath

```

### Architecture Overview

The vending machine controller isolates high-bit data manipulation and storage from state transition decision logic:

```text
                  +-----------------------------------+
                  |     vending_top (Top Module)      |
                  |                                   |
Inputs  --------->|  +-----------+     Control     +--|------> Outputs
                  |  |    FSM    |================>|  |
Outputs <---------|--| (Control) |<================|  |
                  |  +-----------+     Status      +--|
                  |                            datapath
                  +-----------------------------------+

```

* **`FSM.png` (Control Path):** A Moore-type finite state machine handling state progression (`IDLE`, `SELECT`, `DISPENSE`, `RETURN_CHANGE`, `CANCEL`, `INSUFFICIENT`, `OUT_OF_STOCK`) driven by datapath status flags.
* **`datapath.png` (Datapath):** Contains product selection registers, saturating balance registers ($10 limit), inventory stock counters with active-high non-zero checks (`stock > 0`), and a resource-shared subtractor block.
* **`top module block diagram.png`:** Illustrates direct port bindings connecting FSM control outputs to datapath inputs and datapath status outputs to FSM inputs.

### Key Hardware Features

* **Unified Selection Processing:** Employs a generalized selection register and MUX structure in the datapath, keeping the core state machine compact and scalable.
* **Resource Sharing:** Reuses a single subtractor block for both purchase price deduction (`Balance - Price`) and transaction cancellations/refunds (`Balance - 0`).
* **Active-High Logic Consistency:** All status signals (`balance_ok`, `in_stock`) evaluate high (`1`) when conditions are valid.

### Control & Status Signal Interface

#### Control Signals (FSM -> Datapath)

* `dispense_en`: Actuates output dispenser logic for the selected product.
* `write_stock_en`: Decrements inventory counter for the selected product slot.
* `cancel_mux_sel`: Routes item price or `$0` to the subtractor input.
* `clear_balance`: Resets balance register following transaction completion or refund.

#### Status Signals (Datapath -> FSM)

* `balance_ok`: High when `current_balance >= item_price`.
* `in_stock`: High when selected product inventory is greater than zero (`stock > 0`).

### Verification & Cross-Reference

* **FSM Debugging:** Trace state transitions in your waveform viewer against the node path defined in `FSM.png`.
* **Datapath Debugging:** Verify arithmetic results (balance updates, price deductions, stock decrements) against hardware connections in `datapath.png`.

```

```
