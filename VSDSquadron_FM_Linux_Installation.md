# VSDSquadron FM FPGA Tool Installation on Linux

This guide lists the tools needed to build, simulate, and program the VSDSquadron FM board using the Lattice iCE40UP5K FPGA.

## Tools and purposes

| Tool | Purpose |
|---|---|
| Yosys | Synthesizes Verilog code into a logic netlist. |
| nextpnr-ice40 | Places and routes the FPGA design. |
| Project IceStorm | Converts the design into an FPGA bitstream and provides `iceprog` for programming supported boards. |
| Git | Downloads source code and example projects. |
| Make | Runs project build commands from a Makefile. |
| GCC / G++ / build-essential | Provides compiler and build tools needed by software and some FPGA tool packages. |
| Icarus Verilog (optional) | Simulates Verilog designs. |
| GTKWave (optional) | Opens simulation waveform files. |

## 1. Update package lists

Open a terminal and run:

```bash
sudo apt update
```

## 2. Install the FPGA tools

On Ubuntu or a compatible Debian-based distribution, run:

```bash
sudo apt install -y \
  git make gcc g++ build-essential \
  yosys nextpnr-ice40 fpga-icestorm \
  iverilog gtkwave
```

The `iverilog` and `gtkwave` packages are optional if you only want to build and flash an existing project.

## 3. Verify the installations

Run these commands:

```bash
yosys -V
nextpnr-ice40 --version
iceprog -h
git --version
make --version
iverilog -V
gtkwave --version
```

Some tools may print help text or warnings rather than a simple version number; that is normal. If a command reports “command not found,” check whether its package installed successfully.

## 4. Download the VSDSquadron FM example projects

```bash
cd ~
git clone https://github.com/VSDSquadron/VSDSquadron_FM_Kit.git
cd VSDSquadron_FM_Kit
```

If the repository has already been downloaded, do not clone it again; use `cd ~/VSDSquadron_FM_Kit` instead.

## 5. Check whether the board is detected

Connect the board to USB, then run:

```bash
lsusb
```

Look for an FTDI or other USB device associated with the board. If it does not appear, try a known-good USB data cable and another USB port.

## 6. Build and flash an example

The exact example directory and Makefile targets can vary by repository version. First inspect the available examples:

```bash
cd ~/VSDSquadron_FM_Kit
ls
find . -maxdepth 2 -iname 'Makefile' -o -iname '*.mk'
```

If the repository contains a `led_counter` example with the documented targets, use:

```bash
cd ~/VSDSquadron_FM_Kit/led_counter
make clean build
sudo make flash
```

Only run those last commands if the `led_counter` directory exists and its Makefile defines `build` and `flash` targets. Otherwise, follow the instructions in the example's README or Makefile.

## Troubleshooting

- **`Unable to locate package`**: Run `sudo apt update`, check the Linux distribution/version, and search with `apt search yosys`, `apt search nextpnr`, or `apt search icestorm`.
- **USB device not detected**: Check the cable, port, and `lsusb` output.
- **Permission denied while flashing**: As a temporary diagnostic, try the repository's documented command with `sudo`. For regular use, configure the appropriate USB/udev permissions according to the board's official instructions.
- **`make: *** No rule to make target ...`**: Check the example's README and Makefile for the correct target names.
- **Tool version or package unavailable**: Package availability differs by Linux distribution and release. Use the official project instructions if a package is missing.

## Official example repository

https://github.com/VSDSquadron/VSDSquadron_FM_Kit

**Note:** These commands target Ubuntu/Debian-based Linux. Other distributions may use different package managers and package names.
