
# Getting started with Digital VLSI SOC Design and Planning

## Chip Modelling - Summary

## Step 1: Application Compilation

* We take an application in a **C file**, compile it with **GCC**, and obtain the output, named `O0`.

## Step 2: Specification Modeling

* We model the specification in **C format** and test it.
* The output is obtained and named `O1`.

## Step 3: Output Comparison

* Compare the outputs `O0` and `O1`.
* To maintain the same required functionality, both outputs should be equal:

```text
O0 == O1
```

## Step 4: Hardware Modeling

* A softcopy of the hardware is created using high-level languages such as **Bluespec** or **Chisel**.

## Step 5: RTL Simulation

* Run our application on this **RTL model** and obtain the output as `O2`.
* For functional equivalence, both outputs should be equal:

```text
O2 == O1
```

## Step 6: Splitting RTL Code

The RTL code is split into:

* **Processor**
* **Peripherals/IPs**

### Processor

* The processor code is synthesizable.

### Peripherals/IPs

The peripherals/IPs contain:

#### 1. **Macros**

   * Blocks that are initially built and later instantiated whenever required.
#### 2. **Analog IPs**

   * Example: 10-bit ADC, PLL, Clock Multiplier.

## Step 7: SoC Integration

* The **SoC engineer** integrates all these blocks along with GPIOs.
* Our input application is run on this SoC and the output `O3` is obtained.

## Step 8: Physical Design Flow

* Perform the flow from **RTL to GDS**.
* A **GDSII file** is created.
* The GDS file undergoes **DRC/LVS checks**.
* It is then sent to the foundry (**tapeout**).
* Receiving the chip from the foundry is called **tapein**.

## Step 9: Board Integration

* The received chip is integrated with peripherals such as USB Controller, Memory, Voltage Regulator, Crystal Oscillator into a board.

## Step 10: Final Application Run

* We send our application to the chip via USB and run it.
* The output obtained is `O4`.

```text
O1 == O2 == O3 == O4
```

## Timeline

* The entire process takes **12–14 months**, of which about **4–6 months** are spent by the foundry to manufacture and deliver the chip.

## Chip Specifications

* Operating frequency: **100–130 MHz**

## Applications

* iWatch
* Arduino boards
* TV panels
* AC applications
* and more...
