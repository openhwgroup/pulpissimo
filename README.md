[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Documentation Status](https://readthedocs.org/projects/core-v-mcu/badge/?version=latest)](https://core-v-mcu.readthedocs.io/en/latest/?badge=latest)

# CORE-V MCU

> CORE-V MCU originated from PULPissimo and is currently in change to become a
> stand-alone project within OpenHW Group independent from PULPIssimo.
>
> In case you should be interested to join the project please feel free to open
> an issue, or involve yourself in any open issues/discussions.

## Getting Started

Install the required Python tools:

```
pip3 install --user -r python-requirements.txt
```

Install fusesoc: https://fusesoc.readthedocs.io/en/stable/user/installation.html#ug-installation

Install Verilator v4.100: https://verilator.org/guide/latest/install.html

## Building

The build system uses make to capture the required steps.
make with no argments will print a list of the current targets:
```
$ make
all:            generate build scripts, custom build files, doc and sw header files
lint:           run Verilator lint check
docs:           generate documentation (./docs/_build/html/index.html)
sw:             generate C header files (in ./sw)
nexys-emul:     generate bitstream for Nexys-A7-100T (./emulation/core-v-mcu-nexys-a7-100t.bit)
```

## Building an FPGA Image

To target the Nexys-A7-100T board:
```
$ make nexys-emul
```

Make sure you have the latest Xilinx board-parts installed.


Currently unsupported:
```
$ make genesys2
```

## Building documenation
```
$ make docs
```
The resulting documents are accessed using file ./docs/_build/html/index.html

## Building C header files
```
$ make docs
```
The resulting header files are located in ./sw

## Experimental fuseSoC Support

Run Verilator lint target:

```
fusesoc --cores-root . run --target=lint --setup --build openhwgroup.org:systems:core-v-mcu
```

To build Verilator as a library which can be linked into other tools (such as
the debug server):

```
fusesoc --cores-root . run --target=model-lib --setup --build openhwgroup.org:systems:core-v-mcu
```

The library will be in the `obj_dir` subdirectory of the work root.

Once can sanity check the top-level using QuestaSim:

```
fusesoc --cores-root . run --target=sim --setup --build --run openhwgroup.org:systems:core-v-mcu
```

## Contributing: Pre-commit checks

If you are submitting a pull-request, it will be subject to pre-commit checks.  The two that most likely cause problems are the Verilator Lint check and the Verible format check.

### Verilator lint check

The system will run
```
fusesoc --cores-root . run --target=lint --setup --build openhwgroup.org:systems:core-v-mcu
```
If your changes introduce any more Verilator lint warnings, you either need to fix these, or, if appropriate, add a rule to ignore them to `rtl/core-v-mcu/verilator.waiver`.

### Verible format check

Standard formating is enforced by [Verible](https://github.com/google/verible).  The command used is
```
util/format-verible
```
at the top level of the repository, which will correct the format of any file. The check will fail if any file is changed.

Two important things to note.

1.  If you do not have Verible installed (which is likely), then `util/format-verible` will silently do nothing.

2.  You must install the correct version of Verible, currently v0.0-1051-gd4cd328.  GitHub has [prebuilt versions](https://github.com/google/verible/releases/tag/v0.0-1051-gd4cd328).  The version may change in the future.  In the event of the check failing, the details with the failure will tell you which version was used.
