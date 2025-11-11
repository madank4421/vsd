
# Day 4 - Pre-layout timing analysis and importance of good clock tree

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

