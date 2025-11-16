# WEEK 7 — BabySoC Physical Design & Post-Route SPEF Generation


## Installing OpenROAD-flow-scripts

Clone the repository and install all required dependencies:

```
git clone --recursive https://github.com/The-OpenROAD-Project/OpenROAD-flow-scripts
cd OpenROAD-flow-scripts
sudo ./setup.sh
./build_openroad.sh --local
```

![Alt text](images/build.png)

After installation, verify the tools:

```
source ./env.sh
yosys -help
openroad -help
```

![Alt text](images/help1.png)

![Alt text](images/help2.png)

Build the flow infrastructure:

```
cd flow
make
```

To open the graphical interface in OpenROAD:

```
make gui_final
```

![Alt text](images/gui.png)

---

## Preparing the VSDBabySoC Environment

Move into:

```
cd ~/OpenROAD-flow-scripts/flow/designs/src/vsdbabysoc/
```

Copy the following Verilog and config files from your original VSDBABYSOC directory to the current directory:

* avsddac.v
* avsdpll.v
* clk_gate.v
* macro.cfg
* pin_order.cfg
* rvmyth.v
* rvmyth_gen.v
* testbench.rvmyth.post-routing.v
* testbench.v
* vsdbabysoc.v

Next, go to:

```
cd ~/OpenROAD-flow-scripts/flow/designs/sky130hd/
```

Copy these directories/files from the VSDBABYSOC source to current directory:

* gds/
* include/
* lef/
* lib/
* macro.cfg
* pin_order.cfg
* vsdbabysoc_synthesis.sdc

---

## Creating the config.mk File

Create a file named **config.mk** inside:

```
OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc/
```

Add the following content:

```
export DESIGN_NICKNAME = vsdbabysoc
export DESIGN_NAME = vsdbabysoc
export PLATFORM    = sky130hd
export DESIGN_HOME = /home/madank/OpenROAD-flow-scripts/flow/designs

# redefine comments: specifying optional blackbox verilog
# export VERILOG_FILES_BLACKBOX = $(DESIGN_HOME)/src/$(DESIGN_NICKNAME)/IPs/*.v
# redefine comments: automatic discovery of verilog sources
# export VERILOG_FILES = $(sort $(wildcard $(DESIGN_HOME)/src/$(DESIGN_NICKNAME)/*.v))

# explicit source list for clarity
export VERILOG_FILES = $(DESIGN_HOME)/src/$(DESIGN_NICKNAME)/vsdbabysoc.v \
                       $(DESIGN_HOME)/src/$(DESIGN_NICKNAME)/rvmyth.v \
                       $(DESIGN_HOME)/src/$(DESIGN_NICKNAME)/clk_gate.v

export SDC_FILE      = $(DESIGN_HOME)/$(PLATFORM)/$(DESIGN_NICKNAME)/vsdbabysoc_synthesis.sdc

export vsdbabysoc_DIR = $(DESIGN_HOME)/$(PLATFORM)/$(DESIGN_NICKNAME)

export VERILOG_INCLUDE_DIRS = $(wildcard $(vsdbabysoc_DIR)/include/)

export ADDITIONAL_GDS  = $(wildcard $(vsdbabysoc_DIR)/gds/*.gds)
export ADDITIONAL_LEFS = $(wildcard $(vsdbabysoc_DIR)/lef/*.lef)

# clock configurations
export CLOCK_PORT = CLK
export CLOCK_NET  = $(CLOCK_PORT)

# pin and macro placement
export FP_PIN_ORDER_CFG = $(vsdbabysoc_DIR)/pin_order.cfg
export MACRO_PLACEMENT_CFG = $(vsdbabysoc_DIR)/macro.cfg

# floorplan and dimensions
export DIE_AREA   = 0 0 1600 1600
export CORE_AREA  = 20 20 1590 1590

# placement
export PLACE_PINS_ARGS = -exclude left:0-600 -exclude left:1000-1600 -exclude right:* -exclude top:* -exclude bottom:*

# timing adjustments
export TNS_END_PERCENT     = 100
export REMOVE_ABC_BUFFERS  = 1

# CTS tuning
export CTS_BUF_DISTANCE = 600
export SKIP_GATE_CLONING = 1

# magic options
export MAGIC_ZEROIZE_ORIGIN = 0
export MAGIC_EXT_USE_GDS    = 1
```

---

Move into OpenROAD-flow-scripts/flow:

```
cd OpenROAD-flow-scripts/flow
```

---

## Running RTL-to-GDSII Flow

### Synthesis

```
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk synth
```

![Alt text](images/synth.png)

Synthesis result netlist:

![Alt text](images/synth_file.png)

Synthesis statistics:

![Alt text](images/synth_stat.png)

Check for synthesis issues:

![Alt text](images/synth_check.png)

---

## Floorplan

```
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk floorplan
```

![Alt text](images/floorplan.png)

View floorplan in GUI:

```
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk gui_floorplan
```

![Alt text](images/floorplan_window.png)

![Alt text](images/floorplan_zoomed.png)

---

## Placement

```
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk place
```

![Alt text](images/placement.png)

Open placement GUI:

```
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk gui_place
```

![Alt text](images/placement_normal.png)

To visualize density, enable **Placement Density** under Heat Maps:

![Alt text](images/placement_density.png)

![Alt text](images/placement_density_zoomed.png)

Pin density can also be examined:

![Alt text](images/pin_density.png)

![Alt text](images/pin_density_zoomed.png)

---

## Clock Tree Synthesis (CTS)

```
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk cts
```

![Alt text](images/cts.png)

View CTS in GUI:

```
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk gui_cts
```

![Alt text](images/cts_window.png)

Open **Clock Tree Viewer** through the GUI menu:

![Alt text](images/cts_tree.png)

Inspect buffer properties:

![Alt text](images/cts_buffer.png)

Check setup timing details:

![Alt text](images/setup_timing.png)

Hold timing details:

![Alt text](images/hold_timing.png)

Slack histograms:

![Alt text](images/setup_histogram.png)

![Alt text](images/hold_histogram.png)

CTS report is available at:

```
OpenROAD-flow-scripts/flow/reports/sky130hd/vsdbabysoc/base/4_cts_final.rpt
```

![Alt text](images/cts_report.png)

---

## Routing

```
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk route
```

![Alt text](images/routing.png)

Routing completes the final step of the flow, generating DEF, GDS, SPEF, and timing reports.


This completes the entire OpenROAD-flow-scripts pipeline setup and execution for **vsdbabysoc**.
