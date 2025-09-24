
# Timing libs, hierarchical vs flat synthesis and efficient flop coding styles

## 1. Introduction to timing .libs

The technology library contains various cells and diferent variations of the cells which have same function

here we consider the sky130_fd_sc_hd__tt_025c_1v80.lib 

![Alt text](images/sky130_lib.png)

here, tt represents typical, 025c denotes the temperature, 1v80 represents the voltage.

### PVT
The three important operating conditions that determine the behavior of our silicon chip are:
- **Process**
- **Voltage**
- **Temperature**

![Alt text](images/pvt.png)

### Different flavors of the same cell

Here, there are three different variations of the same 2-input AND gate:

![Alt text](images/flavours_of_and_gate.png)

## 2. Hierarchical vs Flat Synthesis

Now to understand hierarchical vs flat Synthesis, consider an example design used in this lab:

**multiple_modules.v**
```
module sub_module2 (input a, input b, output y);
	assign y = a | b;
endmodule

module sub_module1 (input a, input b, output y);
	assign y = a&b;
endmodule


module multiple_modules (input a, input b, input c , output y);
	wire net1;
	sub_module1 u1(.a(a),.b(b),.y(net1));  //net1 = a&b
	sub_module2 u2(.a(net1),.b(c),.y(y));  //y = net1|c ,ie y = a&b + c;
endmodule
```

The given design corresponds to the following circuit:
### multiple_modules implementation
![Alt text](images/multiple_modules_circuit.png)


here, instances of two sub-modules (sub_module1 and sub_module2) are used in multiple_modules.

let us synthesize the netlist with multiple_modules as top level.

``` 
yosys
```
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
```
read_verilog multiple_modules.v
```
Now when we synthesize with multiple_modules as top level,
```
synth -top multiple_modules
```
![Alt text](images/submodules.png)


```
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
```
show multiple_modules
```
When we execute this command, we expect to get the netlist similar to our circuit shown  [above](#multiple_modules-implementation). But that is not the case. 


![Alt text](images/multiple_modules_netlist.png)

We see the hierarchies. The hierarchies are preserved. 

Now, write the netlist to a verilog file:
```
write_verilog -noattr multiple_modules_hier.v
```
The written verilog file can be viewed using gvim or other tool.

![Alt text](images/multiple_modules_hier.png)

To write a flattened netlist we use "flatten" 
```
flatten
```
View the synthesized netlist
```
show
```
![Alt text](images/multiple_modules_flat_netlist.png)
```
write_verilog -noattr multiple_modules_flat.v
```
![Alt text](images/multiple_modules_flat.png)


![Alt text](images/hier_vs_flat.png)


Now let us Synthesize at sub_module level:
```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_modules.v
synth -top sub_module1
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
```
show
```
![Alt text](images/sub_module1.png)

# 3. Various Flop Coding Styles and optimization

Flip flops are used to overcome the output glitches problem of combinational circuits

## D Flip-flop with Asynchronous reset 
In this model, the reset signal is asynchronous and the output 'q' is driven to zero whenever reset is enabled.

**D flip-flop with asynchronous reset**
```
module dff_asyncres ( input clk ,  input async_reset , input d , output reg q );
always @ (posedge clk , posedge async_reset)
begin
	if(async_reset)
		q <= 1'b0;
	else	
		q <= d;
end
endmodule
```

### Iverilog and gtkwave simulation output
![Alt text](images/dff_asyncres_gtkwave_fullscreen.png)

![Alt text](images/dff_asyncres_gtkwave.png)
### Yosys logic synthesis
![Alt text](images/dff_asyncres_synthesis.png)

![Alt text](images/dff_asyncres_netlist.png)


## D Flip-flop with synchronous reset 
In this model, the reset signal is synchronous and the output 'q' is driven to zero only at the positive edge of clock signal whenever the reset is enabled .

**D flip-flop with synchronous reset**
```
module dff_syncres ( input clk , input async_reset , input sync_reset , input d , output reg q );
always @ (posedge clk )
begin
	if (sync_reset)
		q <= 1'b0;
	else	
		q <= d;
end
endmodule
```

### Iverilog and gtkwave simulation output
![Alt text](images/dff_syncres_gtkwave_fullscreen.png)

![Alt text](images/dff_syncres_gtkwave.png)
### Yosys logic synthesis
![Alt text](images/dff_syncres_synthesis.png)

![Alt text](images/dff_syncres_netlist.png)


## D Flip-flop with Asynchronous reset and synchronous reset  
In this model, there are two reset signals: asynchronous reset and synchronous reset.

**D flip-flop with asynchronous reset and synchronous reset**
```
module dff_asyncres_syncres ( input clk , input async_reset , input sync_reset , input d , output reg q );
always @ (posedge clk , posedge async_reset)
begin
	if(async_reset)
		q <= 1'b0;
	else if (sync_reset)
		q <= 1'b0;
	else	
		q <= d;
end
endmodule
```

### Iverilog and gtkwave simulation output
![Alt text](images/dff_asyncres_syncres_gtkwave_fullscreen.png)

![Alt text](images/dff_asyncres_syncres_gtkwave.png)
### Yosys logic synthesis
![Alt text](images/dff_asyncres_syncres_synthesis.png)

![Alt text](images/dff_asyncres_syncres_netlist.png)

## D Flip-flop with Asynchronous set  
In this model, asynchronous set signal is used which when enabled, sets the 'q' pin to one.

**D flip-flop with asynchronous set**
```
module dff_async_set ( input clk ,  input async_set , input d , output reg q );
always @ (posedge clk , posedge async_set)
begin
	if(async_set)
		q <= 1'b1;
	else	
		q <= d;
end
endmodule
```

### Iverilog and gtkwave simulation output
![Alt text](images/dff_async_set_gtkwave_fullscreen.png)

![Alt text](images/dff_async_set_gtkwave.png)
### Yosys logic synthesis

![Alt text](images/dff_async_set_synthesis.png)

![Alt text](images/dff_async_set_netlist.png)

![Alt text](images/flipflop_signal_types.png)

## Interesting Optimizations
There are some interesting optimizations done by the logic synthesis tools.
some examples are:

consider the below mult_2.v design:
```
module mul2 (input [2:0] a, output [3:0] y);
	assign y = a * 2;
endmodule
```

Instead of implementing multiplier cells to multiply by 2, the tool implements shifting operation.

### Optimization by Yosys

![Alt text](images/mul2_synthesis.png)

![Alt text](images/mul2_netlist.png)

![Alt text](images/mul2_netlist_verilog.png)

Also for this special case, mult_8.v 
```
module mult8 (input [2:0] a , output [5:0] y);
	assign y = a * 9;
endmodule
```

Instead of using multiplier, an optimization is done by the tool:

![Alt text](images/mult8_synthesis.png)

![Alt text](images/mult8_netlist.png)

![Alt text](images/mult8_netlist_verilog.png)

