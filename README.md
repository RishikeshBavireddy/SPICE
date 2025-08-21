# Ngspice Tutorial: DC Operating Point & DC Sweep Analysis

This guide introduces **DC operating point analysis** and **DC sweep analysis** in [Ngspice](http://ngspice.sourceforge.net/).  

It is written for beginners: even if you have never used SPICE before, you can follow along by copying the code examples into text files and running them.

---

## Contents

1. [DC Operating Point Analysis](#1-dc-operating-point-analysis)  
2. [Voltage Divider Example](#example-voltage-divider)  
3. [DC Sweep Analysis](#2-dc-sweep-analysis)  
4. [Diode I–V Example](#example-diode-iv-curve)  
5. [Applications](#applications)  
6. [Tips & Tricks](#tips--tricks)  

---

## 1. DC Operating Point Analysis

The **DC operating point** (also called the bias point or Q-point) is the set of node voltages and currents in a circuit when only DC sources are applied.  
It is used to check biasing conditions in circuits like amplifiers.

### Example: Voltage Divider

Create a file named `divider.cir` with the following content:

```spice
* Voltage Divider Circuit
V1 in 0 DC 10
R1 in out 5k
R2 out 0 10k

.op
.end
```

- `V1 in 0 DC 10` defines a 10 V DC source between node `in` and ground (`0`).  
- `R1` and `R2` form a simple resistor divider.  
- `.op` tells Ngspice to compute the operating point.  
- `.end` marks the end of the file.  

### Running the Simulation

Save the file and run:

```bash
ngspice divider.cir
```

If `.op` is present, Ngspice automatically prints the node voltages and branch currents.  

Expected result:

- `V(out)` ≈ 3.33 V  
- `I(R1)` = `I(R2)` ≈ 0.33 mA  

---

## 2. DC Sweep Analysis

A **DC sweep** varies a voltage or current source over a range of values and records how the circuit responds.  
This is often used to generate I–V characteristics of diodes or transistors.

### Example: Diode I–V Curve

Create a file named `diode_iv.cir` with the following content:

```spice
* Diode IV characteristic using DC sweep
V1 anode 0 0
D1 anode cathode Dmodel
R1 cathode 0 1k

.model Dmodel D
.dc V1 0 1 0.01

.print dc v(anode) i(V1)
.end
```

Explanation:

- `V1 anode 0 0` defines a voltage source to be swept.  
- `D1` is a diode with model `Dmodel`.  
- `R1` is a load resistor.  
- `.model Dmodel D` defines a default diode model.  
- `.dc V1 0 1 0.01` sweeps V1 from 0 V to 1 V in 0.01 V steps.  
- `.print dc v(anode) i(V1)` prints the voltage at node `anode` and the current through `V1`.  

### Running the Simulation

Run:

```bash
ngspice diode_iv.cir
```

At the prompt, you can also type:

```
plot v(anode) i(V1)
```

to see the I–V curve of the diode.

---

## Applications

- **Operating point analysis**
  - Biasing point of BJTs and MOSFETs in amplifiers.  
  - Verifying DC node voltages in biasing networks.  
  - Establishing initial conditions before AC or transient simulations.  

- **DC sweep analysis**
  - Obtaining diode, BJT, and MOSFET I–V characteristics.  
  - Measuring transfer functions (Vout vs Vin).  
  - Exploring the effect of varying supply voltages.  

---

## Tips & Tricks

- Use `.print` to print specific voltages and currents:
  ```spice
  .print dc v(out) i(R1)
  ```
- Both `.op` and `.dc` can be used in the same file.  
- To save simulation data for plotting in external tools:
  ```spice
  set filetype=ascii
  wrdata results.txt v(out) i(V1)
  ```
- Ngspice plots can be exported to `.png` files for including screenshots in your documentation:
  ```spice
  hardcopy plot.png
  ```

---

## References

- [Ngspice User Manual](http://ngspice.sourceforge.net/docs.html)  
- [Spice Syntax Guide](https://www.seas.upenn.edu/~jan/spice/spice.overview.html)  
