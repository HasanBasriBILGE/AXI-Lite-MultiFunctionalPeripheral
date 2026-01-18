# AXI-Lite Mixed-Signal Validation Peripheral

This project implements a reusable AXI4-Lite peripheral designed to validate UART communication using real mixed-signal instrumentation.
The peripheral integrates a UART receiver/transmitter, a programmable timer, and GPIO for debug and triggering, and is validated using an AWG (signal generator) and a high-speed scope through the Syzygy interface.

The goal is to demonstrate professional-level IP design, verification methodology, and mixed-signal hardware validation.

## Block Diagram

![Block Diagram](./docs/images/AXI-Lite-Mixed-Signal-Validation-Peripheral.png)

---

## System Architecture

    The system consists of:
    - Zynq Processing System (PS)
    - AXI Interconnect
    - AXI-Lite Mixed-Signal Validation Peripheral (this project)
    - UART Core
    - Timer Core
    - GPIO Core
    - Measurement Engine
    - IRQ Controller
    - AWG Module (Syzygy DAC)
    - Scope Module (Syzygy ADC)
    
The AWG generates UART stimulus, the FPGA validates and measures it, and the scope provides reference measurement.

---

# Functional Requirements
## UART
        - Programmable baud-rate via AXI-Lite register
        - UART RX and TX support
        - Framing error detection
        - Measurement of UART bit period and jitter
        - Status reporting through registers
## Timer
        - 32-bit programmable timer
        - One-shot and periodic modes
        - Used for timing and measurement window control
## GPIO
        - 8-bit GPIO
        - Direction control
        - Used for debug and trigger signaling to scope
## Measurement Engine
        - Start/stop controlled via register
        - Measures UART bit period and jitter
        - Generates measurement status and error flags
        - Supports RX and TX validation modes
## Interrupt Controller
        - Optional interrupt generation
        - UART, Timer and Measurement interrupts
        - Maskable via registers
---
## Interface Specifications
| Interface  | Description                                   |
| ---------- | --------------------------------------------- |
| AXI4-Lite  | Control and status interface to Zynq PS       |
| UART RX/TX | Digital UART interface under test             |
| GPIO       | Debug and trigger signals                     |
| IRQ        | Interrupt output to Zynq PS                   |
| Syzygy     | High-speed interface to AWG and Scope modules |

---
## Performance Targets
| Parameter                     | Target            |
| ----------------------------- | ----------------- |
| Baud-rate range               | 9.6 kbps – 3 Mbps |
| Timing resolution             | ≤ 10 ns           |
| Jitter measurement resolution | ≤ 5 ns            |
| Timer width                   | 32-bit            |

---
## Verification Strategy
    - Directed and random AXI-Lite register tests
    - UART protocol compliance tests
    - Measurement engine self-checking tests
    - UVM-based verification environment
    - Assertion-based checks
    - Coverage-driven regression
---
## Hardware Validation Strategy
    - AWG generates UART stimulus
    - Scope measures analog waveform
    - FPGA measures digital timing
    - Results compared against reference measurements
    - Validation report generated
---
## Deliverables
    - RTL source code
    - Verification environment (UVM)
    - Register map documentation
    - System architecture diagram
    - Hardware validation report
    - Coverage and regression reports

---
# Register Map
| Offset | Name             | Description                                                                                                    |
| ------ | ---------------- | -------------------------------------------------------------------------------------------------------------- |
|  0x00  | CTRL             | Global control register. Enables or disables UART, TIMER, and GPIO submodules.                                 |
|  0x04  | STATUS           | Status flags indicating current state of UART and TIMER.                                                       |
|  0x08  | IRQ_ENABLE       | Interrupt enable register for UART and TIMER interrupts.                                                       |
|  0x0C  | IRQ_STATUS       | Interrupt status register. Indicates pending interrupt sources. Writing '1' clears the corresponding interrupt.|
|  0x10  | UART_BAUD        | UART baud-rate configuration register. Defibes baud-rate divider value.                                        |
|  0x14  | UART_TXDATA      | UART transmit data register. Writing a byte initiates transmission.                                            |
|  0x18  | UART_RXDATA      | UART receive data register. Reading returns the last received byte.                                            |
|  0x1C  | UART_STATUS      | UART status register indicating TX busy, RX valid, and error flags.                                            |
|  0x20  | TIMER_LOAD       | Timer load value register. Defines the initial countdown value.                                                |
|  0x24  | TIMER_VALUE      | Current timer counter value. Read-only.                                                                        |
|  0x28  | TIMER_CTRL       | Timer control register. Enables timer, selects mode (one-shot/ periodic).                                      |
|  0x2C  | GPIO_DIR         | GPIO direction register. '1' = output, '0' = input.                                                            |
|  0x30  | GPIO_OUT         | GPIO output data register. Controls output pin values.                                                         |
|  0x34  | GPIO_IN          | GPIO input data register. Reflects current input pin states.                                                   |
|  0x38  | MEAS_CTRL        | Measurement control (start validation).                                                                        |
|  0x3C  | MEAS_STATUS      | Measurement status (done, error flags).                                                                        |
|  0x40  | MEAS_BIT_PERIOD  | Measured UART bit period.                                                                                      |
|  0x44  | MEAS_JITTER      | Measured jitter value.                                                                                         |
