# WEEK 4 Ngspice Sky130

To install Ngspice, follow the steps mentioned [here](/week0/task2/README.md#ngspice)

## NMOS Analysis

Let us understand the DC behavior of an NMOS transistor and its response to varying voltages especially how the NMOS transistor conducts when gate (Vin) and drain (Vdd) voltages change, by designing a simple nmose circuit and obtaining SPICE simulation

```
cd sky130CircuitDesignWorkshop/design
gedit day1_nfet_idvds_L2_W5.spice 
```
### Code

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


## Analysing MOSFET

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

## Id vs Vds of NMOS After reducing Channel length (From 200nm to 150nm)

Let us examine the behaviour of the Id current with respect to Vds after reducing the channel length (L) of the NMOS ans maintaining the same W/L ratio. Ideally, The drain current must be similar to the previous case. but due to short shannal effect and velocity saturation, that is not the case.

### Code
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

## Id vs Vin of NMOS After reducing Channel length (From 200nm to 150nm)

Similarly, Let us examine the behaviour of the Id current with respect to Vin after reducing the channel length (L) of the NMOS ans maintaining the same W/L ratio. Ideally, The drain current must be similar to the previous case (that is, the relationship should be quadratic). But the relationship between drain current and input voltage Vin is linear.

### Code

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


