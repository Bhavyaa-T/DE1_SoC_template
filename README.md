# DE1_SoC_template 

A reusable Quartus Prime project template for the Terasic DE1-SoC development board.

This repository provides a clean starting point for projects using the Cyclone V SoC FPGA and its Hard Processor System (HPS), avoiding the need to repeatedly configure board-level pin assignments and HPS settings.

## Target Hardware

- **Board:** Terasic DE1-SoC
- **FPGA:** Intel/Altera Cyclone V SoC
- **Device:** `5CSEMA5F31C6`

## Repository Structure

```text
DE1_SoC_template/
├── quartus/
│   ├── DE1_SoC_template.qpf
│   ├── top.qsf
│   ├── top.sv
│   └── platform_designer_module.tcl
│
├── tests/
│   ├── Makefile
│   └── my_testbench.py
│
├── requirements.txt
├── .gitignore
└── README.md
```

## Python Setup

From the repository root:

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

## Platform Designer

The Platform Designer system is stored as:

```text
quartus/platform_designer_module.tcl
```

The generated `.qsys` file is not tracked.

To recreate it:

```bash
cd quartus
export PATH="$PATH:/path/to/quartus/sopc_builder/bin"
qsys-script --script=platform_designer_module.tcl
```

This creates:

```text
platform_designer_module.qsys
```

After making changes in Platform Designer, regenerate the system HDL and update the Tcl representation where required.

## Adding RTL

Project-specific RTL can be added inside `quartus/`.

For example:

```text
quartus/
├── top.sv
├── custom_peripheral.sv
└── ...
```

Add new source files to the Quartus project as normal.

## Testing with cocotb

Tests are stored in:

```text
tests/
```

The template Makefile uses:

```make
SIM = icarus
WAVES = 1
```

Update these entries for your design:

```make
VERILOG_SOURCES = ../quartus/custom_peripheral.sv
COCOTB_TOPLEVEL = custom_peripheral
COCOTB_TEST_MODULES = my_testbench
```

Then run:

```bash
cd tests
make
```

Simulation output is generated in:

```text
tests/sim_build/
```

Waveforms can be opened in GTKWave.

## Generated Files

Generated Quartus, Platform Designer, Python and simulation files are excluded using `.gitignore`.

Examples:

```text
db/
output_files/
.qsys_edit/
platform_designer_module/
*.qsys
sim_build/
```

## Typical Workflow

```text
Create project from template
        ↓
Create / activate .venv
        ↓
Install requirements
        ↓
Generate Platform Designer .qsys
        ↓
Write RTL
        ↓
Test with cocotb + Icarus
        ↓
Compile in Quartus
        ↓
Program DE1-SoC
```

---

*This README was generated with the assistance of AI and reviewed by the project author.*