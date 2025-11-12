# WEEK 6 - Day 5: Final Steps in RTL-to-GDS using TritonRoute and OpenSTA

## RISC-V Reference SoC Tapeout Program

In this concluding part of the **Physical Design Flow**, we focus on the stages that follow **Clock Tree Synthesis (CTS)** — namely, the **Power Distribution Network (PDN)** creation, **Routing** using **TritonRoute**, and final verification steps including **Design Rule Check (DRC)** and **post-route timing analysis** using **SPEF extraction** and **OpenSTA**.

---

## 1. Power Distribution Network (PDN) Generation

In the conventional RTL-to-GDSII flow, PDN generation is typically performed before cell placement.
However, **OpenLANE** performs this process **after CTS** to ensure that the power rails are properly aligned and that the inserted clock buffers are well connected to the power grid.

The PDN creation step establishes **VDD and VSS rails and straps**, distributing power uniformly across the design to maintain voltage stability.

### Command to Generate PDN

```bash
gen_pdn
```

This command invokes **OpenROAD’s PDN generator**, which automatically lays out power grids across multiple metal layers using predefined configuration parameters.

![Alt text](images/01.png)

---

## 2. Routing with TritonRoute

**TritonRoute** is the detailed routing tool integrated within OpenLANE.
It translates the global routing guides into precise metal connections on the chip, ensuring that the layout is **DRC-clean** and all nets are fully connected.

### Routing Flow Overview

1. **Global Routing** – Establishes approximate routing paths between components.
2. **Detailed Routing** – Refines these paths using algorithms that determine exact metal tracks and via placements.

### Command to Start Routing

```bash
run_routing
```

This command executes **TritonRoute**, which performs:

* Pin access and track assignment
* Detailed routing with iterative repair
* DRC error detection and correction

The final routed layout satisfies all physical design rules defined by the technology PDK.

| Before Routing             | After Routing              |
| -------------------------- | -------------------------- |
| ![Alt text](images/02.png) | ![Alt text](images/03.png) |

---

## 3. SPEF Generation for Parasitic Extraction

The **Standard Parasitic Exchange Format (SPEF)** represents the parasitic resistance and capacitance of interconnects in the layout.
These parasitics directly influence signal delay and are essential for accurate **post-route Static Timing Analysis (STA)**.

OpenLANE provides a **Python-based SPEF extractor** that computes parasitic information from the routed design.

### Command for SPEF Extraction

```bash
cd <path-to-SPEF_EXTRACTOR-tool-directory>
python3 main.py <path-to-LEF-file> <path-to-DEF-file-after-routing>
```

An example portion of the `.spef` file is shown below:

![Alt text](images/04.png)

This file contains **RC parasitic data** for all interconnects, which will later be used by **OpenSTA** for timing validation.

---

## 4. Design Rule Check (DRC)

Once routing is completed, the layout undergoes a **Design Rule Check** to ensure compliance with all manufacturing rules defined in the technology process (e.g., Sky130).
The built-in DRC engine within **TritonRoute** verifies:

* Spacing and enclosure rules
* Minimum metal width
* Via overlaps and shorts
* Connectivity and open checks

Only **DRC-clean** designs proceed to GDSII generation, ensuring that the layout can be fabricated without physical violations.

---


## 5. Post-Route Timing Validation with OpenSTA

After routing and parasitic extraction, **OpenSTA** is used again for **post-route STA** to analyze the design with real interconnect delays included.

### Command to Perform Timing Analysis

```bash
sta post_route_sta.tcl
```

This final timing verification step confirms that the design meets setup and hold constraints, making it ready for tapeout.
The next phase will focus on **final verification, GDS export, and tapeout preparation**.

