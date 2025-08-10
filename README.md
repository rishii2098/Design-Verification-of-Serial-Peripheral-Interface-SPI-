# Design-Verification-of-Serial-Peripheral-Interface-SPI-

# SPI Master–Slave Verification using SystemVerilog

## Overview
This repository implements and verifies a **Serial Peripheral Interface (SPI)** communication between a Master and a Slave using SystemVerilog.  
It supports **12-bit, LSB-first** data transfer with a robust, layered testbench using transaction-based communication (mailboxes, virtual interface, etc.).

You can **view and run the full design and testbench** on EDA Playground:  
**https://edaplayground.com/x/FfZK**

---

## Design Under Test (DUT)

- **spi_master**
  - Generates `sclk` from `clk`
  - Controls `cs` (chip select)
  - Shifts out data on `mosi`, LSB first
  - FSM controls data transfer

- **spi_slave**
  - Listens to `mosi` edges, builds 12-bit word
  - Asserts `done` when complete

- **top**
  - Connects master ↔ slave internally
  - Exposes inputs: `clk`, `rst`, `newd`, `din`
  - Exposes outputs: `dout`, `done`

---

## Testbench Architecture

1. **spi_if**: SystemVerilog interface bundling signals for easy access across TB
2. **transaction**: Encapsulates `din`, `newd`, `dout` and supports `copy()`
3. **generator**: Randomizes inputs and schedules transactions
4. **driver**: Applies stimulus to DUT via interface
5. **monitor**: Observes and collects DUT output
6. **scoreboard**: Compares expected vs observed data, logs results
7. **environment**: Wires everything together and orchestrates the test
8. **tb**: Top-level testbench module that runs the simulation and waveform dump

---

