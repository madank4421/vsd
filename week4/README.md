# WEEK 4 Ngspice Sky130

To install Ngspice, follow the steps mentioned [here](/week0/task2/README.md#ngspice)

## NMOS Analysis

Let us understand the DC behavior of an NMOS transistor and its response to varying voltages especially how the NMOS transistor conducts when gate (Vin) and drain (Vdd) voltages change, by designing a simple nmose circuit and obtaining SPICE simulation

```
cd sky130CircuitDesignWorkshop/design
gedit day1_nfet_idvds_L2_W5.spice 
```

![image](images/nmos_analysis_code.png)

```
ngspice day1_nfet_idvds_L2_W5.spice 
```
![image](images/nmos_analysis_terminal.png)

![image](images/nmos_analysis_output.png)

Click on the curve to get the exact values at that point:

![image](images/nmos_analysis_values.png)

Observation:
The output of the NMOS transistor is analyzed by varying Vin and Vdd.
As Vin (gate-to-source voltage) increases, the output current rises until it reaches a saturation point, beyond which further increases in Vin have little effect.
Similarly, increasing Vdd results in a higher output current, reflecting the transistor’s dependence on the drain-to-source voltage.
