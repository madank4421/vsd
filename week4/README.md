# WEEK 4 Ngspice Sky130

To install Ngspice, follow the steps mentioned [here](/week0/task2/README.md#ngspice)

# NMOS Analysis

Let us understand the DC behavior of an NMOS transistor and its response to varying voltages especially how the NMOS transistor conducts when gate (Vin) and drain (Vdd) voltages change, by designing a simple nmose circuit and obtaining SPICE simulation

```
cd sky130CircuitDesignWorkshop/design
gedit day1_nfet_idvds_L2_W5.spice 
```
### SPICE Code

![image](images/nmos_analysis_code.png)

```
ngspice day1_nfet_idvds_L2_W5.spice
plot -vdd#branch
```

### Plot

![image](images/nmos_analysis_terminal.png)

![image](images/nmos_analysis_output.png)

Click on the curve to get the exact values at that point:

![image](images/nmos_analysis_values.png)

### Observation

The output of the NMOS transistor is analyzed by varying Vin and Vdd.
As Vin (gate-to-source voltage) increases, the output current rises until it reaches a saturation point, beyond which further increases in Vin have little effect.
Similarly, increasing Vdd results in a higher output current, reflecting the transistor’s dependence on the drain-to-source voltage.


# Analysing MOSFET

Short Channel Effect and Velocity Saturation

When the channel length (L) and width (W) of a MOSFET are reduced while maintaining the same W/L ratio, it might be expected that the drain current (Id) would exhibit the same behavior as before. However, in practice, this does not occur.

As the channel length decreases, the relationship between the input voltage (Vin) and the drain current (Id) becomes more linear, unlike in long-channel devices where the relationship is quadratic.

This deviation arises due to short channel effects (SCEs). In short-channel MOSFETs, the electric field in the channel becomes very strong, especially near the drain. The high field causes the carrier velocity to saturate — a condition known as velocity saturation. Once carriers reach this saturation velocity, increasing the gate voltage further does not significantly increase the current, leading to a linear dependence of Id on Vin.

Hence, as the device dimensions shrink:

The quadratic behavior (Id ∝ (Vgs − Vth)²) transitions to a linear behavior (Id ∝ (Vgs − Vth)).

The drain current increases less rapidly with gate voltage.

The short channel effect and velocity saturation dominate the device characteristics.


Let us consider a simple model:

$$
I_D = k_n \left[ (V_{gt} \cdot V_{\min}) - \frac{(V_{\min})^2}{2} \right] (1 + \lambda V_{DS})
$$
where,
$$
V_{\min} = \min(V_{GS}, V_{DS}, V_{DSAT})
$$


Here, we take Vgt = Vgs - Vt for simplicity.


1. When Vgs is minimum, Vmin = Vgs 

$$
I_D = k_n \left( V_{gt}^2 - \frac{1}{2}V_{gt}^2 \right) (1 + \lambda V_{DS})
$$

$$
I_D = \frac{1}{2}k_n V_{gt}^2 (1 + \lambda V_{DS})
$$

2. When Vds is minimum, Vmin = Vds 

$$
I_D = k_n \left[ (V_{gt} \cdot V_{DS}) - \frac{V_{DS}^2}{2} \right] (1 + \lambda V_{DS})
$$

$$
I_D \approx k_n \left[ V_{gt} \cdot V_{DS} - \frac{V_{DS}^2}{2} \right]
$$

3. When Vdsat is minimum, Vmin = Vdsat 

$$
I_D = k_n \left[ (V_{gt} \cdot V_{DSAT}) - \frac{(V_{DSAT})^2}{2} \right] (1 + \lambda V_{DS})
$$



Let us simulate the circuit behavior.

# Id vs Vds of NMOS After reducing Channel length (From 200nm to 150nm)

Let us examine the behaviour of the Id current with respect to Vds after reducing the channel length (L) of the NMOS ans maintaining the same W/L ratio. Ideally, The drain current must be similar to the previous case. but due to short shannal effect and velocity saturation, that is not the case.

### SPICE Code
![image](images/idvds_code.png)

We have reduced the Length but maintained same W/L ratio by reducing width too.
Now let us plot the graph (Id vs Vds)

```
ngspice day2_nfet_idvds_L015_W039.spice
plot -vdd#branch
```

### Plot

![image](images/day2_terminal.png)

![image](images/day2_plot.png)

### Observation:
Here, The drain current (I<sub>D</sub>) of an NMOS transistor was observed as a function of the drain-to-source voltage (V<sub>DS</sub>) after reducing the channel length from 200 nm to 150 nm while keeping other parameters constant. The results show that with a shorter channel length, the device exhibits a higher drain current for the same applied V<sub>DS</sub>, indicating increased current drive capability. However, the transition from the linear to the saturation region becomes less distinct, and the I<sub>D</sub>–V<sub>DS</sub> curve shows early saturation. This behavior is attributed to short channel effects and velocity saturation, where the carriers attain their maximum velocity at lower electric fields. Consequently, the quadratic dependence of I<sub>D</sub> on V<sub>GS</sub> gradually shifts toward a more linear characteristic, reflecting the non-ideal behavior of scaled-down MOSFETs.

| **Before Scaling (L = 200 nm)** | **After Scaling (L = 150 nm)** |
|:-------------------------------:|:------------------------------:|
| <img src="images/day1_plot.png" width="400"/> | <img src="images/day2_plot.png" width="400"/> |

# Id vs Vin of NMOS After reducing Channel length (From 200nm to 150nm)

Similarly, Let us examine the behaviour of the Id current with respect to Vin after reducing the channel length (L) of the NMOS ans maintaining the same W/L ratio. Ideally, The drain current must be similar to the previous case (that is, the relationship should be quadratic). But the relationship between drain current and input voltage Vin is linear.

### SPICE Code

![image](images/idvgs_code.png)

We have reduced the Length but maintained same W/L ratio by reducing width too.
Now let us plot the graph (Id vs Vds)

```
ngspice day2_nfet_idvgs_L015_W039.spice
plot -vdd#branch
```
### Plot

![image](images/idvgs_terminal.png)

![image](images/idvgs_plot.png)

### Observation:

In this experiment, the drain current (I<sub>D</sub>) of an NMOS transistor was studied with respect to the input voltage (V<sub>in</sub>) after reducing the channel length (L) while keeping the width-to-length (W/L) ratio constant. Ideally, the drain current should follow a quadratic relationship with V<sub>in</sub>, similar to the long-channel case. However, the observed behavior shows a nearly linear dependence between I<sub>D</sub> and V<sub>in</sub>. This deviation arises due to short channel effects and velocity saturation, where the carriers reach their maximum drift velocity at lower electric fields, limiting the rate of increase in current. As a result, the device no longer exhibits the ideal square-law characteristic, and the current drive becomes less sensitive to increases in gate voltage.


| **Before Scaling (L = 200 nm)** | **After Scaling (L = 150 nm)** |
|:-------------------------------:|:------------------------------:|
| <img src="images/day1_idvgs_plot.png" width="400"/> | <img src="images/idvgs_plot.png" width="400"/> |



# CMOS inverter - Voltage Transfer Characteristics

The voltage transfer characteristics of a CMOS Inverter is a plot that shows the relationship between the output voltage ($V_{out}$) and the corresponding steady-state (DC) input voltage ($V_{in}$) of the inverter. It essentially maps every possible input voltage level to the resulting output voltage level, defining the inverter's voltage-level response.

### SPICE Code

day3_inv_vtc_Wp084_Wn036.spice

```
*Model Description
.param temp=27


*Including sky130 library files
.lib "sky130_fd_pr/models/sky130.lib.spice" tt


*Netlist Description


XM1 out in vdd vdd sky130_fd_pr__pfet_01v8 w=0.84 l=0.15
XM2 out in 0 0 sky130_fd_pr__nfet_01v8 w=0.36 l=0.15


Cload out 0 50fF

Vdd vdd 0 1.8V
Vin in 0 1.8V

*simulation commands

.op

.dc Vin 0 1.8 0.01

.control
run
setplot dc1
display
.endc

.end
```

let us plot the out vs in graph

```
ngspice day3_inv_vtc_Wp084_Wn036.spice
plot out vs in
```

### Plot

![image](images/day3_inv_vtc_terminal.png)

![image](images/day3_inv_vtc_plot.png)

### Observation
Here, the output swings between 1.8 V, corresponding to $V_{dd}$, and 0 V ($V_{ss}$). The plot demonstrates the circuit's inverting function: when $V_{in}$ is low (near 0 V), $V_{out}$ is high (1.8 V), and conversely, when $V_{in}$ is high (near 1.8 V), $V_{out}$ is low (0 V). The transition between these high and low output states is exceptionally steep, indicating a high voltage gain in this region. This sharp switching occurs at a specific input voltage, the switching threshold ($V_M$), which is observed to be at approximately $V_{in} = 0.85$ V.

### Switching Threshold $$V_m$$

At 50% of the out voltage, find the corresponding in voltage. That is the switching Threshold. Or to find accurate Switching Threshold, Draw a line y=x and the point of intersection of the VTC curve and the line is the switching threshold.


![image](images/day3_vm_value.png)

Switching threshold ($V_m$) = 0.877 V


### Switching Threshold $$V_m$$ and $$(\frac{W_p}{L_p})$$ : $$(\frac{W_n}{L_n})$$ Ratio

When the  $$V_m$$ and $$(\frac{W_p}{L_p})$$ : $$(\frac{W_n}{L_n})$$ Ratio is Increased The value of Switching threshold increases. This is observed by simulating in ngspice.

### SPICE Code

![image](images/day3_inv_vtc_2x_code.png)

```
ngspice day3_inv_vtc_Wp168_Wn036.spice
plot out vs in
```

### Plot

![image](images/day3_inv_vtc_2x_terminal.png)

![image](images/day3_inv_vtc_2x_plot.png)

### Observation
Here, the width of the PMOS transistor is increased, and it can be noted that the switching threshold ($V_m$) has increased slightly from the previous design.

![image](images/day3_inv_vtc_2x_value.png)

Switching threshold ($V_m$) = 0.903 V

Further increasing, 

### SPICE Code

![image](images/day3_inv_vtc_3x_code.png)

```
ngspice day3_inv_vtc_Wp252_Wn036.spice
plot out vs in
```

### Plot

![image](images/day3_inv_vtc_3x_terminal.png)

![image](images/day3_inv_vtc_3x_plot.png)

### Observation
Here, the width of the PMOS transistor is increased, and it can be noted that the switching threshold ($V_m$) has increased slightly from the previous design.

![image](images/day3_inv_vtc_3x_value.png)

Switching threshold ($V_m$) = 0.928 V

# Vout vs Time, Vin

Let us Change the value of in with respect to time and note the output out signal. 

### SPICE Code

day3_inv_tran_Wp084_Wn036.spice

```
*Model Description
.param temp=27


*Including sky130 library files
.lib "sky130_fd_pr/models/sky130.lib.spice" tt


*Netlist Description


XM1 out in vdd vdd sky130_fd_pr__pfet_01v8 w=0.84 l=0.15
XM2 out in 0 0 sky130_fd_pr__nfet_01v8 w=0.36 l=0.15


Cload out 0 50fF

Vdd vdd 0 1.8V
Vin in 0 PULSE(0V 1.8V 0 0.1ns 0.1ns 2ns 4ns)

*simulation commands

.tran 1n 10n

.control
run
.endc

.end
```

Run the simulation.

```
ngspice day3_inv_tran_Wp084_Wn036.spice
plot out vs time in
```

### Plot

![image](images/day3_vout_time_terminal.png)

![image](images/day3_vout_time_plot.png)

### Observation
Here, The out is inverted version of in. The out is not perfectly inverted, And it takes a delay to follow the input. The rise and fall delay of the output can be calculated by finding the time interval between 50% of the input value and 50% of the output value.

### Rise delay and Fall delay

![image](images/day3_vout_find_delay.png)

### Rise Delay

![image](images/day3_vout_time_rise_delay.png)

Rise delay = 2.482 - 2.149 = 0.333 ps

### Fall delay

![image](images/day3_vout_time_fall_delay.png)

Fall delay = 4.334 - 4.050 = 0.284 ps


# Noise Margin

Noise margin is a measure of a digital logic circuit's robustness against voltage noise. It defines the maximum "noise" voltage that can be added to or subtracted from an input signal before the logic gate's output value becomes incorrect or indeterminate. A larger noise margin means the circuit is more reliable and less likely to be upset by noise from sources like power supply fluctuations or crosstalk.

![image](images/VTC_Graph.png)

### Noise Margin Low ($NM_L$) and High ($NM_H$)

For a CMOS inverter, the noise margin is split into two values, which are calculated using four key voltage points from the Voltage Transfer Characteristic (VTC) graph:

* **$V_{OH}$ (Voltage Output High):** The maximum voltage that the inverter outputs for a logic '1'. For an ideal CMOS inverter, **$V_{OH} = V_{dd}$**.
* **$V_{OL}$ (Voltage Output Low):** The minimum voltage that the inverter outputs for a logic '0'. For an ideal CMOS inverter, **$V_{OL} = 0$ V** (or $V_{ss}$).
* **$V_{IL}$ (Voltage Input Low):** The maximum input voltage that is *guaranteed* to be recognized as a logic '0'. On the VTC, this is the point where the slope (gain) first equals -1.
* **$V_{IH}$ (Voltage Input High):** The minimum input voltage that is *guaranteed* to be recognized as a logic '1'. On the VTC, this is the second point where the slope (gain) equals -1.

The "undefined region" or transition region lies between $V_{IL}$ and $V_{IH}$.


Based on these points, the two noise margins are defined as:

1.  **Noise Margin High ($NM_H$)**
    This is the "safety buffer" for the logic '1' state. It's the difference between the high output voltage ($V_{OH}$) from a driving gate and the high input voltage ($V_{IH}$) required by the next gate.

    $$NM_H = V_{OH} - V_{IH}$$

2.  **Noise Margin Low ($NM_L$)**
    This is the "safety buffer" for the logic '0' state. It's the difference between the low input voltage ($V_{IL}$) the gate will accept and the low output voltage ($V_{OL}$) from the previous gate.

    $$NM_L = V_{IL} - V_{OL}$$

### SPICE Code

day4_inv_noisemargin_wp1_wn036.spice

```
*Model Description
.param temp=27


*Including sky130 library files
.lib "sky130_fd_pr/models/sky130.lib.spice" tt


*Netlist Description


XM1 out in vdd vdd sky130_fd_pr__pfet_01v8 w=1 l=0.15
XM2 out in 0 0 sky130_fd_pr__nfet_01v8 w=0.36 l=0.15


Cload out 0 50fF

Vdd vdd 0 1.8V
Vin in 0 1.8V

*simulation commands

.op

.dc Vin 0 1.8 0.01

.control
run
setplot dc1
display
.endc

.end
```

Ngspice:

```
ngspice day4_inv_noisemargin_wp1_wn036.spice
plot out vs in
```

![image](images/day4_noise_margin_1x_terminal.png)

![image](images/day4_noise_margin_1x_plot.png)

### Observation

From the Transfer characteristic curve of CMOS inverter, the two points where slopes are -1 are identified, and the corresponding $V_{OH}$ , $V_{OL}$ , $V_{IL}$ , $V_{IH}$ values are noted. The Noise Margin Low  ($NM_L$) and Noise Margin High ($NM_H$) are calculated.

![image](images/day4_noise_margin_1x_value.png)

$$NM_H = V_{OH} - V_{IH}$$
       = 1.718 - 0.981
       = 0.737 V

$$NM_L = V_{IL} - V_{OL}$$
      = 0.766 - 0.106
      = 0.66 V



# Supply voltage variations

With variation in supply voltage, the Voltage Transfer Characteristics of the CMOS inverter varies. When the supply voltage increases, the Switching threshold ($V_m$) and Noise Margin ($$NM_H$$ and $$NM_L$$) also increases.

### SPICE Code

```
*Model Description
.param temp=27


*Including sky130 library files
.lib "sky130_fd_pr/models/sky130.lib.spice" tt


*Netlist Description


XM1 out in vdd vdd sky130_fd_pr__pfet_01v8 w=1 l=0.15
XM2 out in 0 0 sky130_fd_pr__nfet_01v8 w=0.36 l=0.15


Cload out 0 50fF

Vdd vdd 0 1.8V
Vin in 0 1.8V

.control

let powersupply = 1.8
alter Vdd = powersupply
	let voltagesupplyvariation = 0
	dowhile voltagesupplyvariation < 6
	dc Vin 0 1.8 0.01
	let powersupply = powersupply - 0.2
	alter Vdd = powersupply
	let voltagesupplyvariation = voltagesupplyvariation + 1
      end
 
plot dc1.out vs in dc2.out vs in dc3.out vs in dc4.out vs in dc5.out vs in dc6.out vs in xlabel "input voltage(V)" ylabel "output voltage(V)" title "Inveter dc characteristics as a function of supply voltage"

.endc

.end
```

### Plot

![images](images/day5_supply_terminal.png)

![images](images/day5_supply_plot.png)

### Observation

As expected, When the supply voltage decreases, the Switching threshold ($V_m$) and Noise Margin ($$NM_H$$ and $$NM_L$$) also decreases. Similarly, The Gain of the CMOS inverter increases when the supply voltage decreases. But there are also downsides when it comes to decreasing the supply voltage.

## Transiwnt analysis with different supply voltages

To understand the downside, let us perform transient analysis with different supply voltage (Vdd = 1.8 V and 0.8 V) for example.

### SPICE Code (Vdd = 1.8 V)

![images](images/day5_supply1_code.png)

### Plot

![images](images/day5_supply1_terminal.png)

![images](images/day5_supply1_plot.png)


### SPICE Code (Vdd = 0.8 V)

![images](images/day5_supply2_code.png)

### Plot

![images](images/day5_supply2_terminal.png)

![images](images/day5_supply2_plot.png)


### Comparing

| **Vdd = 1.8 V** | **Vdd = 0.8 V** |
|:-------------------------------:|:------------------------------:|
| <img src="images/day5_supply1_plot.png" width="400"/> | <img src="images/day5_supply2_plot.png" width="400"/> |


### Observation

As we can see, when the supply voltage $$V_{DD}$$ is decreased, the current driving capability of the transistors reduces. This means the time required to charge or discharge the load capacitance $$C_{load}$$ increases, resulting in slower output transitions. Consequently, the output voltage may not reach the full logic levels within the desired time, and the inverter is not properly driven, which can degrade the overall performance of the digital circuit.


# Device process variations

To understand the effect of device process variations on inverter behavior, we analyze the voltage transfer characteristic (VTC) by simulating the inverter with different transistor widths, $$W_p$$ for PMOS and $$W_n$$ for NMOS. By varying these widths, we can observe changes in the switching threshold $$V_m$$,  and overall performance in terms of logic levels and transition sharpness, as well as the noise margins $$NM_H$$ and $$NM_L$$. This analysis provides insight into how process variations impact the CMOS inverter and helps in designing more robust digital circuits.

 
### SPICE Code  (Wp = 7 and Wn = 0.42)

```
*Model Description
.param temp=27


*Including sky130 library files
.lib "sky130_fd_pr/models/sky130.lib.spice" tt


*Netlist Description


XM1 out in vdd vdd sky130_fd_pr__pfet_01v8 w=7 l=0.15
XM2 out in 0 0 sky130_fd_pr__nfet_01v8 w=0.42 l=0.15


Cload out 0 50fF

Vdd vdd 0 1.8V
Vin in 0 1.8V

*simulation commands

.op

.dc Vin 0 1.8 0.01

.control
run
setplot dc1
display
.endc

.end
```

### Plot

![images](images/day5_device_terminal.png)

![images](images/day5_device_plot.png)

### SPICE Code  (Wp = 0.84 and Wn = 0.36)

```
*Model Description
.param temp=27


*Including sky130 library files
.lib "sky130_fd_pr/models/sky130.lib.spice" tt


*Netlist Description


XM1 out in vdd vdd sky130_fd_pr__pfet_01v8 w=0.84 l=0.15
XM2 out in 0 0 sky130_fd_pr__nfet_01v8 w=0.36 l=0.15


Cload out 0 50fF

Vdd vdd 0 1.8V
Vin in 0 1.8V

*simulation commands

.op

.dc Vin 0 1.8 0.01

.control
run
setplot dc1
display
.endc

.end
```

### Plot

![images](images/day3_inv_vtc_plot.png)

### Compare

| **Wp = 7 and Wn = 0.42** | **Wp = 0.84 and Wn = 0.36** |
|:-------------------------------:|:------------------------------:|
| <img src="images/day5_device_plot.png" width="400"/> | <img src="images/day3_inv_vtc_plot.png" width="400"/> |


### Observation

As the transistor widths increase, the switching threshold $$V_m$$ of the CMOS inverter also increases. This is because increasing the PMOS width $$W_p$$ strengthens the pull-up network, while increasing the NMOS width $$W_n$$ strengthens the pull-down network. By adjusting these widths, the point at which the inverter output switches from high to low shifts, affecting the logic levels and the noise margins $$NM_H$$ and $$NM_L$$. Studying the voltage transfer characteristic (VTC) for different width combinations helps us understand how device sizing and process variations influence the inverter's performance.






