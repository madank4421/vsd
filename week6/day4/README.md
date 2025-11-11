
# WEEK 6 - Day 4 - Pre-layout timing analysis and importance of good clock tree

Let us check whether our inverter layout satisfies some conditions for good layout.

First, open the layout:

```tcl
cd Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign
magic -T ./lib/sky130A.tech sky130_inv.mag &
```

![image](images/magic1.png)

## Conditions for Good Layout

1. The input and output port of a standard cell must be between the vertical and horizontal grid lines.
2. The width of the standard cell must be an odd multiple of the horizontal grid value.
3. The height should be three times the vertical grid value.

The value of the vertical and horizontal grid can be determined from the **tracks.info** file located in the directory shown below:

![image](images/tracks_command.png)

![image](images/tracks_file.png)

From this **tracks.info** file, we can determine:

* Horizontal grid length = $$0.46,\mu m$$
* Vertical grid length = $$0.34,\mu m$$

So let us set  the grid accordingly in our layout

```
grid 0.46um 0.34um 0.23um 0.17um
```

![image](images/grid_command.png)


### Condition 1 Check

![image](images/c1.png)

Hence, it satisfies the first condition.


### Condition 2 Check

![image](images/c2.png)

The width of the standard cell is:

$$
1.38,\mu m - 0.00,\mu m = 1.38,\mu m = 3 \times 0.46,\mu m
$$

Hence, it satisfies the second condition.

### Condition 3 Check

![image](images/c3.png)

The height of the standard cell is:

$$
2.72,\mu m - 0.00,\mu m = 2.72,\mu m = 8 \times 0.34,\mu m
$$

Hence, it satisfies the third condition.


Now save the layout.

```tcl
# Command to save in tkcon window
save sky130_vsdinv.mag
```

Then exit:

```tcl
exit
```

Now open the saved layout:

```tcl
magic -T ./lib/sky130A.tech sky130_vsdinv.mag &
```

![image](images/magic2.png)

Then write the LEF file.
In tkcon window, type:

```tcl
lef write
```

![image](images/lef_write.png)

LEF file is created:

![image](images/lef_created.png)


## Using Custom Inverter in Picorv32a Synthesis and Layout

Now copy the `.lef` files to the `picorv32a/src/` directory.


```bash
cp sky130_vsdinv.lef ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/

cp libs/sky130_fd_sc_hd__* ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/
```

Now modify the `config.tcl` file to include all the necessary `.lib` files.  
Add this line:

```tcl
set ::env(LIB_SYNTH) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"
set ::env(LIB_FASTEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__fast.lib"
set ::env(LIB_SLOWEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__slow.lib"
set ::env(LIB_TYPICAL) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"
set ::env(EXTRA_LEFS) [glob $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/src/*.lef]
````

![Alt text](images/config.png)

Now invoke OpenLane:

```tcl
cd ~/Desktop/work/tools/openlane_working_dir/openlane

alias docker='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21'

docker
```

Now in the bash shell, invoke OpenLane:

```tcl
./flow.tcl -interactive
```

![Alt text](images/openlane1.png)

Then run:

```tcl
package require openlane 0.9

prep -design picorv32a -tag 30-10_11-30 -overwrite

set lefs [glob $::env(DESIGN_DIR)/src/*.lef]

add_lefs -src $lefs

run_synthesis
```

![Alt text](images/set_lef.png)

Now note that our custom inverter is used in the synthesis.

![Alt text](images/inv1.png)

The chip area and the TNS can be seen:

![Alt text](images/area1.png)

![Alt text](images/tns1.png)

We got a negative slack which is bad.
Now let us improve timing by compromising the area. This can be done by changing a few parameters.
The details of these parameters can be seen in
`~/Desktop/work/tools/openlane_working_dir/openlane/configuration/` under `README.md`.

![Alt text](images/readme.png)

```bash
prep -design picorv32a -tag new -overwrite

set lefs [glob $::env(DESIGN_DIR)/src/*.lef]

add_lefs -src $lefs

echo $::env(SYNTH_STRATEGY)

set ::env(SYNTH_STRATEGY) "DELAY 3"

echo $::env(SYNTH_BUFFERING)

echo $::env(SYNTH_SIZING)

set ::env(SYNTH_SIZING) 1

echo $::env(SYNTH_DRIVING_CELL)
```

![Alt text](images/set_env.png)

Now run synthesis:

```tcl
run_synthesis
```

![Alt text](images/area2.png)

![Alt text](images/tns2.png)

As we can see, The total negative slack is reduced and became 0. However the area in increased when compared to previously synthesized layout. Thus we trade area to improve the timing.

Area before = 1,47,712 units

Area after = 1,81,730 units

TNS before = -711.59

TNS after = 0

Now run the floorplan:

```tcl
run_floorplan
```

![Alt text](images/error.png)

We get an error message when we run the `run_floorplan` command.
Let us use another command instead.
The details of other floorplan commands can be found in
`~/Desktop/work/tools/openlane_working_dir/openlane/docs/source/` directory under `OpenLANE_commands.md`.

![Alt text](images/readme2.png)

Let us run these alternate commands:

```tcl
init_floorplan
place_io
tap_decap_or
```

![Alt text](images/init1.png)

![Alt text](images/init2.png)

Now let us run placement:

```tcl
run_placement
```

![Alt text](images/placement.png)

Now let us view the `picorv32a` layout using Magic:

```bash
cd ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/new/results/placement/

magic -T ~/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech \
lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```

![Alt text](images/magic3.png)

Let us find our custom inverter in this layout:

![Alt text](images/what.png)

We can expand the view of this inverter using the `expand` command in the tkcon window:

![Alt text](images/expand.png)



````markdown
# Timing Analysis with Ideal Clocks using OpenSTA

In this section, we will perform post-synthesis timing analysis using the OpenSTA tool, without any prior parameter changes. The goal is to understand timing performance and how various synthesis parameters or cell replacements can influence slack values.

To begin, launch the OpenLane Docker container and prepare the environment.

```bash
cd ~/Desktop/work/tools/openlane_working_dir/openlane

alias docker='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT \
-u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21'

docker
````

Once inside Docker, invoke OpenLane in interactive mode and set up the design environment.

```bash
./flow.tcl -interactive
package require openlane 0.9

prep -design picorv32a

set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

set ::env(SYNTH_SIZING) 1
run_synthesis
```

![Alt text](images/run_synthesis10.png)

Next, create the `pre_sta.conf` file under the `openlane/` directory to perform STA.

```tcl
set_cmd_units -time ns -capacitance pF -current mA -voltage V -resistance kOhm -distance um

read_liberty -max ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/sky130_fd_sc_hd__slow.lib
read_liberty -min ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/sky130_fd_sc_hd__fast.lib
read_verilog ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/11-11_16-13/results/synthesis/picorv32a.synthesis.v
link_design picorv32a
read_sdc ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/my_base.sdc

report_checks -path_delay min_max -fields {slew trans net cap input_pin fanout}
report_tns
report_wns
```

Also, create `my_base.sdc` under `openlane/designs/picorv32a/src` directory.

```tcl
set ::env(CLOCK_PORT) clk
set ::env(CLOCK_PERIOD) 24.73
# set ::env(SYNTH_DRIVING_CELL) sky130_vsdinv
set ::env(SYNTH_DRIVING_CELL) sky130_fd_sc_hd__inv_8
set ::env(SYNTH_DRIVING_CELL_PIN) Y
set ::env(SYNTH_CAP_LOAD) 17.653
set ::env(IO_PCT) 0.2
set ::env(SYNTH_MAX_FANOUT) 6

create_clock [get_ports $::env(CLOCK_PORT)]  -name $::env(CLOCK_PORT)  -period $::env(CLOCK_PERIOD)
set input_delay_value [expr $::env(CLOCK_PERIOD) * $::env(IO_PCT)]
set output_delay_value [expr $::env(CLOCK_PERIOD) * $::env(IO_PCT)]
puts "\[INFO\]: Setting output delay to: $output_delay_value"
puts "\[INFO\]: Setting input delay to: $input_delay_value"

set_max_fanout $::env(SYNTH_MAX_FANOUT) [current_design]

set clk_indx [lsearch [all_inputs] [get_port $::env(CLOCK_PORT)]]
#set rst_indx [lsearch [all_inputs] [get_port resetn]]
set all_inputs_wo_clk [lreplace [all_inputs] $clk_indx $clk_indx]
#set all_inputs_wo_clk_rst [lreplace $all_inputs_wo_clk $rst_indx $rst_indx]
set all_inputs_wo_clk_rst $all_inputs_wo_clk


# correct resetn
set_input_delay $input_delay_value  -clock [get_clocks $::env(CLOCK_PORT)] $all_inputs_wo_clk_rst
#set_input_delay 0.0 -clock [get_clocks $::env(CLOCK_PORT)] {resetn}
set_output_delay $output_delay_value  -clock [get_clocks $::env(CLOCK_PORT)] [all_outputs]

# TODO set this as parameter
set_driving_cell -lib_cell $::env(SYNTH_DRIVING_CELL) -pin $::env(SYNTH_DRIVING_CELL_PIN) [all_inputs]
set cap_load [expr $::env(SYNTH_CAP_LOAD) / 1000.0]
puts "\[INFO\]: Setting load to: $cap_load"
set_load  $cap_load [all_outputs]
```

The load capacitance, fanout, and other parameters are determined from the
`sky130_fd_sc_hd__typical.lib` located under `openlane/vsdstdcelldesign/libs/` directory.

![Alt text](images/lib.png)

Now run OpenSTA to perform Static Timing Analysis (STA).

```bash
cd ~/Desktop/work/tools/openlane_working_dir/openlane
sta pre_sta.conf
```

![Alt text](images/sta_1.png)
![Alt text](images/sta_2.png)

The TNS (Total Negative Slack) and WNS (Worst Negative Slack) are undesirable. Let us try to improve them by modifying synthesis parameters such as `SYNTH_MAX_FANOUT`.

Re-prepare the design with updated parameters.

```tcl
prep -design picorv32a -tag 11-11_16-13 -overwrite

set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# To set the value for SYNTH_SIZING
set ::env(SYNTH_SIZING) 1

# To set the value for SYNTH_MAX_FANOUT
set ::env(SYNTH_MAX_FANOUT) 4

# To check the current driving cell
echo $::env(SYNTH_DRIVING_CELL)

run_synthesis
```

![Alt text](images/run_synthesis11.png)

Now run STA again using OpenSTA in a new terminal.

```bash
cd ~/Desktop/work/tools/openlane_working_dir/openlane/
sta pre_sta.conf
```

![Alt text](images/sta1.png)

Even after changing the fanout, we still get negative slack. Let’s further optimize by replacing some cells that are underperforming.

![Alt text](images/cell1.png)

For example, the cell `sky130_fd_sc_hd__or3_2` has a drive strength of 2 but drives 4 fanouts.
We will replace it with a stronger version.

```tcl
# To report connections of the given net
report_net -connections _11672_

# Replace with a cell of higher drive strength
replace_cell _14510_ sky130_fd_sc_hd__or3_4
```

![Alt text](images/replace1.png)

After replacement, report the new timing:

```tcl
report_checks -fields {net cap slew input_pins fanout} -digits 4
```

![Alt text](images/sta2.png)

Still, there is negative slack. Let’s identify another problematic cell and replace it.

![Alt text](images/cell2.png)

The cell `sky130_fd_sc_hd__or4_2` drives `sky130_fd_sc_hd__o2111a_2`, causing a large delay. Replace it with a stronger drive variant.

```tcl
# To report connections of the given net
report_net -connections _11643_

# Replace with cell of more driving strength
replace_cell _14481_ sky130_fd_sc_hd__or4_4
```

![Alt text](images/replace2.png)

Now re-check timing:

```tcl
report_checks -fields {net cap slew input_pins fanout} -digits 4
```

![Alt text](images/sta3.png)

After this, negative slack still persists. Let us try one more cell replacement.

![Alt text](images/cell3.png)

Again, consider another instance of `sky130_fd_sc_hd__or4_2` driving a delay-critical path. Replace it similarly:

```tcl
# To report connections of the given net
report_net -connections _11668_

# Replace with cell of higher drive strength
replace_cell _14506_ sky130_fd_sc_hd__or4_4
```

![Alt text](images/replace3.png)

Check timing again:

```tcl
report_checks -fields {net cap slew input_pins fanout} -digits 4
```

![Alt text](images/sta4.png)

This time, the WNS shifted to a different path.
To check timing for the specific path we were previously optimizing, explicitly specify the start and end points:

```tcl
report_checks -from _29043_ -to _30440_ -through _14506_
```

Now let’s perform one more replacement to further minimize delay.

![Alt text](images/cell4.png)

```tcl
# To report connections of the given net
report_net -connections _12198_

# Replace with cell of higher drive strength
replace_cell _15216_ sky130_fd_sc_hd__or2_4
```

![Alt text](images/replace4.png)

Then, recheck the timing again:

```tcl
report_checks -fields {net cap slew input_pins fanout} -digits 4
```

![Alt text](images/sta5.png)

After this optimization, the WNS reduced from **-23.90** to **-22.7658**, an improvement of **1.1342 ns**.
This optimization was achieved through **ECO (Engineering Change Order)** changes.

To save this updated netlist, first make a backup of the old synthesis file and then write the new Verilog netlist.

```bash
cd ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/11-11_16-13/results/synthesis/

cp picorv32a.synthesis.v picorv32a.synthesis_old.v
```

Now, in the OpenSTA shell, export the modified netlist:

```tcl
write_verilog ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/11-11_16-13/results/synthesis/picorv32a.synthesis.v
exit
```

![Alt text](images/write_verilog.png)

Finally, verify that the written Verilog file contains the replaced cells:

![Alt text](images/check_verilog.png)

Yes, the modified Verilog netlist includes all the replaced cells, confirming that our ECO updates have been successfully reflected in the design.



