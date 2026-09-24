# DE1_SoC_template

A reusable Quartus Prime project template for the Terasic DE1-SoC development board, used as the starting point for the Part IIA RTL Communication Bus lab.

It gives you a clean starting point for projects using the Cyclone V SoC FPGA and its Hard Processor System (HPS), with the board-level pin assignments and HPS settings already configured.

## Target Hardware

- **Board:** Terasic DE1-SoC
- **FPGA:** Intel/Altera Cyclone V SoC
- **Device:** `5CSEMA5F31C6`

## Getting Your Own Copy

Click **Use this template** → **Create a new repository** at the top right of this page, then clone your copy:

```bash
git clone https://github.com/<your-username>/<your-repo-name>.git
cd <your-repo-name>
```

Your own copy means you can push your work, and it stays separate from everyone else's.

## Repository Structure

```text
<your-repo-name>/
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
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

This installs the pinned versions the lab was tested with: `cocotb==2.1.0` and `cocotb-coverage==2.0`. Stick with these; the example testbenches use cocotb 2.x syntax.

Only activate the venv for cocotb work. Run `deactivate` before launching Quartus, Platform Designer or `qsys-script`.

## Adding Quartus to your PATH

On the lab computers, run this once to add Quartus and `qsys-script` to your `PATH` permanently:

```bash
echo 'export PATH="$PATH:/usr/local/apps/altera_lite/25.1std/quartus/bin"' >> ~/.bashrc
echo 'export PATH="$PATH:/usr/local/apps/altera_lite/25.1std/quartus/sopc_builder/bin"' >> ~/.bashrc
source ~/.bashrc
```

On any other machine, replace `/usr/local/apps/altera_lite/25.1std/quartus` with your own Quartus install path.

## Platform Designer

The Platform Designer system is stored as a Tcl script:

```text
quartus/platform_designer_module.tcl
```

The generated `.qsys` file is not tracked. To recreate it:

```bash
cd quartus
qsys-script --script=platform_designer_module.tcl
```

This creates `platform_designer_module.qsys`. After making changes in Platform Designer, generate the HDL and export the system again with **File → Export System as Platform Designer Script (.tcl)**, overwriting `platform_designer_module.tcl`. The Tcl script, not the `.qsys`, is what gets committed.

If you package your own RTL as a Platform Designer component, commit the component's `_hw.tcl` file too; the system script depends on it.

## Adding RTL

Put project-specific RTL directly in `quartus/`, alongside `top.sv`:

```text
quartus/
├── top.sv
├── custom_peripheral.sv
└── ...
```

Then add the new files to the Quartus project as normal.

## Testing with cocotb

Tests live in `tests/`. The template Makefile already sets:

```make
SIM = icarus
WAVES = 1
```

Point it at your design:

```make
VERILOG_SOURCES = ../quartus/custom_peripheral.sv
COCOTB_TOPLEVEL = custom_peripheral
COCOTB_TEST_MODULES = my_testbench
```

Then run the tests from `tests/`, with the venv active:

```bash
cd tests
source ../.venv/bin/activate
make
```

Simulation output, including the waveform, is written to `tests/sim_build/`. Open the waveform in GTKWave:

```bash
gtkwave sim_build/custom_peripheral.fst
```

Always run `make` from `tests/`, not from `sim_build/`.

## Connecting to the Board

Each board's HPS runs Linux and is reachable over SSH at `eietl-fpga-NN.eng.cam.ac.uk`, where `NN` is your bench number (01–16):

```bash
ssh root@eietl-fpga-NN.eng.cam.ac.uk
```

To copy a C file to the board, run this from the folder containing the file:

```bash
scp <c-file-name.c> root@eietl-fpga-NN.eng.cam.ac.uk:~/
```

See the lab handout for first-login instructions.

## Generated Files

Generated Quartus, Platform Designer, Python and simulation files are excluded by `.gitignore`, for example:

```text
db/
output_files/
.qsys_edit/
platform_designer_module/
*.qsys
.venv/
sim_build/
```

## Typical Workflow

```text
Create your repository from the template
        ↓
Create and activate .venv, install requirements
        ↓
Write RTL
        ↓
Test with cocotb + Icarus
        ↓
Add to Platform Designer and generate HDL
        ↓
Compile in Quartus and program the DE1-SoC
        ↓
Run your C program on the HPS over SSH
```

---

*This README was generated with the assistance of AI and reviewed by the project author.*
