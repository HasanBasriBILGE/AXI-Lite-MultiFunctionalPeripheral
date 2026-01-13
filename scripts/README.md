🚀 AXI-Lite Multi-Functional Peripheral

This repository provides a TCL-based automation script to quickly create and configure a Xilinx Vivado project for Zynq-7000 based designs.

It is intended as a clean starting point for developing AXI-Lite based custom peripherals using Verilog.

🧩 What This Script Does

The create_project.tcl script automatically:

Creates a new Vivado project

Sets the FPGA part and board (Digilent Eclypse Z7)

Configures the target language (Verilog)

Adds HDL source files from src/

Adds testbench files from tb/

Adds constraint files from constraints/

Updates compile order

This allows you to start working immediately without manual project setup.

🗂️ Project Structure
AXI-Lite-MultiFunctionalPeripheral/
├── scripts/
│   └── create_project.tcl
├── src/          # HDL source files
├── tb/           # Simulation files
├── constraints/  # XDC constraint files
└── README.md

▶️ How to Run

Open Vivado

Open the TCL Console

Run the script:

source scripts/create_project.tcl


(Optional) You can override the project name:

set user_project_name MyProject
source scripts/create_project.tcl

✅ After Running

Once completed, Vivado will create the project and print:

✅ PROJECT CREATED SUCCESSFULLY!


Next steps:

Add your HDL files to src/

Add testbenches to tb/

Add constraints to constraints/

Run synthesis and implementation

🛠️ Requirements

Xilinx Vivado

Zynq-7000 compatible board (default: Eclypse Z7)

ℹ️ Platform Note

If you plan to use a different FPGA board or platform, the board-related parameters in the .tcl script must be updated.

It is recommended to:

Create a donor Vivado project for the target board

Copy the part and board_part values from that project

Update the corresponding fields in create_project.tcl

This ensures correct board definition and avoids compatibility issues during synthesis and implementation.
