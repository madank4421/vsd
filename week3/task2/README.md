# WEEK 3 Part 2 – Fundamentals of STA (Static Timing Analysis) 


# 1.  Introduction and agenda of this course

This course focuses on understanding timing analysis in digital design — analyzing how signals propagate through different paths and ensuring they meet required timing constraints for correct operation.


### Timing paths

A timing path is the route a signal takes from its launch point to its endpoint. Timing analysis ensures that data arrives within the allowed limits defined by the clock period.


### Key timing parameters

- Arrival Time:  
  The time at which a signal reaches an endpoint.  
  → Calculated only at the endpoint.

- Required Time:  
  The latest (or earliest) time by which the signal must arrive for correct circuit operation.

- Slack:  
  Slack = Arrival Time - Required Time  
  - Positive Slack: Timing is met.  
  - Negative Slack: Timing violation.  
  - Max Slack: Represents setup slack.  
  - Min Slack: Represents hold slack.


### Types of setup/hold analysis

| Type | Description |
|------|--------------|
| Register-to-Register (Reg-to-Reg) | Between two flip-flops driven by the same or different clocks. |
| Input-to-Register (In-to-Reg) | From a primary input port to a capturing flip-flop. |
| Register-to-Output (Reg-to-Out) | From a launching flip-flop to a primary output port. |
| Input-to-Output (In-to-Out) | From input to output without any flip-flops in the path. |
| Clock Gating | From another clock signal to the AND gate output used for gating. |
| Recovery/Removal | For asynchronous signals like set/reset — ensures proper timing for assertion and deassertion. |
| Data-to-Data | Paths that do not involve any flip-flops (purely combinational). |
| Latch Analysis | Considers time borrowing or time giving in level-sensitive latches. |


### Slew / Transition analysis

Defines how quickly a signal changes from logic 0 to 1 or vice versa. Slew affects both timing and signal integrity.

- For Data: Checked for both maximum and minimum transition times.  
- For Clock: Checked for maximum and minimum to ensure proper clock shape and duty cycle.


### Load analysis

Ensures that the output drivers are not overloaded and can drive all connected inputs efficiently.

- Fanout Analysis (max/min):  
  Verifies that the number of connected loads (fanouts) does not exceed specified limits.  

- Capacitance Analysis (max/min):  
  Measures total load capacitance on each net to prevent delay and transition issues.


### Clock analysis

Ensures stable and synchronized clock distribution across the design.

- Clock Skew:  
  The difference in clock arrival time between two registers.  
  - Positive skew: Destination clock arrives later than the source.  
  - Negative skew: Destination clock arrives earlier than the source.  

- Pulse Width Analysis:  
  Verifies that the high and low durations of the clock pulse meet the required minimum limits for proper sequential operation.

# 2. Introduction to timing graph

**reg 2 reg**:

### Building Timing Graph

- Consider inputs, sources, and gates as nodes.  
- Consider the delay mapped at edges.  
- Compute actual arrival time (AAT):  
  - Compute for all input paths of a node.  
  - Select the maximum or minimum of these arrival times depending on the type of analysis (setup or hold).  
- Compute required arrival time (RAT):  
  - Calculate in reverse direction.  
  - Consider the minimum or maximum depending on the type of analysis.

### Types of Analysis

- GBA (Graph Based Analysis): Faster method, uses precomputed delays for all paths.  
- PBA (Path Based Analysis): More accurate and realistic, but time consuming as it evaluates full paths individually.

### Pin - Node Conversion

- Build timing graph focusing on the pins as nodes.  
- Consider the worst-case arrival time for each node.




# 3. Clk-to-q delay, library setup, hold time and jitter

![Alt text](images/master_slave.png)

- Setup time: Time required for a data to stabilize before the next clock edge.  
  - In our example, setup time = 3 inverter delays + 1 transmission gate delay.

- Clk-to-Q delay: After application of the positive edge of the clock, the time taken for the data to travel from Qm to Q (1 transmission gate delay + 1 inverter delay) is clk-to-q delay.

- Hold time: Here hold time is zero during the positive edge of the clock.

- Generating jitter values: Eye diagram  
  - Due to clock skew, voltage droop, and ground bounce, the clock signal arriving at different parts of the IC varies.  
  - The combined waveform gives the eye diagram.

- Analyze the eye diagram:  
  - Noise margin: Amount of distortion allowed.  
  - Jitter: Temporary variation of clock period.

- The final condition:

  θ + δ₁ < T + δ₂ - S - SU

 

  Where:  
  - S: Setup time  
  - SU: Setup time uncertainty  
  - δ: Clock delay  
  - θ: Delay of path from launch to capture flip-flop (clk-to-q delay + path delay)



# 4. Textual Timing Reports and Hold Analysis

Setup Analysis (single clock):

```
Δ₁  - b1/a = 0.013 ns  
    - b1/y = 0.043 ns  
    - b2/a = 0.021 ns  
    - b2/y = 0.051 ns  
    - b3/a = 0.032 ns  
    - b3/y = 0.055 ns  

+θ = 0.9 ns  
-----------------------------------
T = 1 ns  

+ Δ₂  - b1/a = 0.013 ns  
      - b1/y = 0.043 ns  
      - b2/a = 0.021 ns  
      - b2/y = 0.051 ns  
      - b4/a = 0.032 ns  
      - b4/y = 0.083 ns  

+S = 0.01 ns  
+SU = 0.09 ns  
```


Hold Analysis (single clock):
```
Δ₁  - b1/a = 0.013 ns  
    - b1/y = 0.043 ns  
    - b2/a = 0.021 ns  
    - b2/y = 0.051 ns  
    - b3/a = 0.032 ns  
    - b3/y = 0.055 ns  

θ = 0.14 ns  
---------------------------
Δ₂  - b1/a = 0.013 ns  
    - b1/y = 0.043 ns  
    - b2/a = 0.021 ns  
    - b2/y = 0.051 ns  
    - b4/a = 0.032 ns  
    - b4/y = 0.083 ns  

+ H = 0.01 ns  
+ HU = 0.05 ns
```


# 5. On-chip Variation

Sources:

i) Etching process variation  
- Consider an inverter chain. All the inverters in the chain may not be ideal or identical.  
- Variations arise due to differences between the ideal and actual mask.  
- Note: Gates in the middle have the same structure on either side.  

```
ID = μn * Cox * (W / L) * [(VG - VTH) * VD - (VD^2 / 2)]
```

ii) Oxide thickness  
- Ideal oxide thickness varies from the actual oxide thickness.  
- This can vary for different inverters in an inverter chain.

```
ID = μn * (εox / tox) * (W / L) * [(VG - VTH) * VD - (VD^2 / 2)]
```

- Drain current is related to the resistance between Vdd and the load capacitance Cl.  
- Propagation delay is related to the resistance in the RC model. Higher resistance leads to higher delay.  

Relation between resistance, drain current, and delay:

```
tpd = f(R) = f(Iₑ) = f(tox, W, L)
```

- Plotting a graph with delay on the x-axis and number of inverters on the y-axis shows certain percentage variations on the LHS and RHS.  
- This is called OCV derates (On-Chip Variation derates).


# 6. OCV Timing and Pessimism Removal

- Decreasing Data Required Time: Clock Pull-in  
- Increasing Data Arrival Time: Clock Push-out  

These techniques give a more realistic and conservative analysis.

- After performing Clock Pull-in, the slack gets reduced.  
- A cell cannot have two delay values at the same time.  
- This introduces additional pessimism which should be removed.

- In hold analysis:  
  - Perform Clock Pull-in at data arrival time.  
  - Perform Clock Push-out at data required time.  
  - This ensures a more conservative analysis.

- In this case, the clock period is not involved, making it challenging to improve slack.

- Similar to setup analysis, additional pessimism arises due to different delays of the same components, which needs to be removed.

# CERTIFICATE

![Alt text](images/certificate.png)
