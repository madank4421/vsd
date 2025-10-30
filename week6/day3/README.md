## Work in Progress... 🏗️

## IO Pin Configurations in OpenLANE

There are four possible configurations to place IO pins in a layout.
To visualize them, we first open the layout generated earlier in Magic.



```
cd ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/29-10_00-31/results/floorplan
magic -T ~/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```

![image](images/magic_window_1.png)

You can zoom and observe the IO cells by clicking `z`

![image](images/io_zoomed_1.png)

In the file `floorplan.tcl` located in the `openlane/configurations/` directory, the parameter `FP_IO_MODE` determines the IO placement configuration.
It can take values such as 0, 1, 2, etc., corresponding to different pin arrangements.

After changing the value of `FP_IO_MODE` to 2, rerun the floorplan:

```
set ::env(FP_IO_MODE) 2
run_floorplan
```

Then, open the layout again in Magic to observe the new pin configuration:

```
cd ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/30-10_11-30/results/floorplan
magic -T ~/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```

![image](images/magic_window_2.png)

You can zoom and observe the IO cells by clicking `z`

![image](images/io_zoomed_1.png)

## SPICE Simulation for CMOS Inverter

### Static Analysis

The simulation is performed for two transistor sizing conditions:


$$
i) W_p = W_n = 0.375,\mu m, \quad L_p = L_n = 0.25,\mu m
$$

$$
ii) W_p = W_n = 0.9375,\mu m, \quad L_p = L_n = 0.25,\mu m
$$

When the PMOS width increases, the inverter’s transfer curve shifts toward the right.
This means the switching threshold voltage  $$V_m$$ increases.

To calculate the switching threshold, draw the line ( x = y ) on the VTC curve.
At the switching threshold $$V_m$$ , the condition is:

$$
V_{in} = V_{out}
$$

For the first inverter: 

$$
V_m \approx 0.98,V
$$

For the second inverter: 

$$ 
V_m \approx 1.2,V 
$$

### Dynamic Simulation

To perform a transient (dynamic) simulation, the following input pulse is used:

```
vin in 0 0 pulse 0 2.5 0 10p 10p 1n 2n
```

This line defines:

* Input from 0 V to 2.5 V
* No initial delay (0)
* Rise time = 10 ps
* Fall time = 10 ps
* ON time (duty cycle) = 1 ns
* Period = 2 ns


### Delay Calculations

The propagation delays are calculated as the time difference between the 50% points of input and output waveforms.
