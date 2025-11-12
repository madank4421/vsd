# WEEK 6 - Day 5: Final Steps in RTL-to-GDS using TritonRoute and OpenSTA

## Detailed Routing Using TritonRoute

In this section, we will perform **detailed routing** using **TritonRoute** and analyze the results through Magic and OpenROAD.

```
# Display the current DEF file location being used by OpenLANE
echo $::env(CURRENT_DEF)

# Display the routing strategy value defined in configuration
echo $::env(ROUTING_STRATEGY)

# Execute the detailed routing process using TritonRoute engine
run_routing
```

![Alt text](images/zeroth.png)

![Alt text](images/first.png)

![Alt text](images/second.png)

![Alt text](images/last.png)

After each routing iteration, the total wire length and number of violations gradually decrease as the design becomes cleaner and more optimized.

![Alt text](images/routing.png)

![Alt text](images/routing2.png)

## Visualizing the Routed Layout in Magic

Once routing is completed, you can visualize the final routed layout in **Magic VLSI**.

```
# Navigate to the directory containing the routed DEF file
cd ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/11-11_16-13/results/routing

# Launch Magic and load the routed DEF and technology LEF
magic -T ~/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.def &
```

![Alt text](images/magic.png)

The above layout represents the design after performing detailed routing.
You can use zoom and pan tools within Magic to inspect the routed interconnections closely.

![Alt text](images/magic1.png)

![Alt text](images/magic2.png)

![Alt text](images/magic3.png)

Next, let’s take a look at the **FastRoute guide** files available in the following directory:

`openlane/designs/picorv32a/runs/11-11_16-13/tmp/routing/`

![Alt text](images/fastroute_guide.png)

## Post-Routing Parasitic Extraction (SPEF Extraction)

After routing, we extract the **Standard Parasitic Exchange Format (SPEF)** file, which contains detailed parasitic capacitance and resistance values for accurate post-route timing analysis.

```
# Navigate to the directory containing the SPEF extractor
cd ~/Desktop/work/tools/openlane_working_dir/openlane/scripts/spef_extractor

# Execute the Python SPEF extraction script using merged LEF and routed DEF
python3 main.py -l ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/11-11_16-13/tmp/merged.lef -d ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/11-11_16-13/results/routing/picorv32a.def
```

![Alt text](images/spef_extraction.png)

You can now view the generated `.spef` file inside:

`~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/11-11_16-13/results/routing/`

![Alt text](images/spef.png)

## Post-Routing OpenSTA Timing Analysis Using SPEF

Finally, we’ll perform **post-routing timing analysis** in **OpenROAD (OpenSTA)**, which uses the extracted parasitics to generate accurate setup and hold timing reports.

```
# Launch OpenROAD shell for timing analysis
openroad

# Load the technology and cell LEFs (used for layout geometry and standard cells)
read_lef /openLANE_flow/designs/picorv32a/runs/11-11_16-13/tmp/merged.lef

# Load the final routed DEF (includes interconnections and routing information)
read_def /openLANE_flow/designs/picorv32a/runs/11-11_16-13/results/routing/picorv32a.def

# Save OpenROAD database for reference or reuse
write_db pico_route.db

# Optionally reload the saved database
read_db pico_route.db

# Load the Verilog netlist generated after synthesis
read_verilog /openLANE_flow/designs/picorv32a/runs/11-11_16-13/results/synthesis/picorv32a.synthesis_preroute.v

# Load the complete Liberty timing models
read_liberty $::env(LIB_SYNTH_COMPLETE)

# Link the design’s top-level module with its corresponding libraries
link_design picorv32a

# Load the custom SDC file containing clock and timing constraints
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

# Mark all defined clocks as propagated to include clock tree delays
set_propagated_clock [all_clocks]

# Load the extracted parasitic data for accurate delay calculations
read_spef /openLANE_flow/designs/picorv32a/runs/11-11_16-13/results/routing/picorv32a.spef
```

![Alt text](images/openroad.png)

```
# Generate a timing report showing both setup and hold paths
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4
```

![Alt text](images/sta.png)

