# Ngspice Tutorial

So this repo is where we’re gonna store all the SPICE programs that we will cover in the series of tutorial sessions over the entire course  
This README file is like your main reference for the concepts

Note : I will keep updating this README file after each tutorial session


## What is SPICE and why learn it?  

SPICE stands for **Simulation Program with Integrated Circuit Emphasis**.  
It’s basically a software which can be used to simulate circuits,instead of solving those big equations from mesh and nodal analysis,you can analyse the circuit on your laptop itself.(Nice, is'nt it ?)  

---

**Why should you care?**
- Check if your circuit actually works before building it and analyse it    
- It’s completely open source and free !  
- You can verify your practice-set problem solution if you have any doubt(or you doubt the answer key🙃)  

---
---
## Getting started 
I will be using fedora 42 kde, it is highly adviced that you use a Linux machine for the entire work-flow, if you are on windows(ditch it or maybe use WSL), if you are on Mac(I think you won't face any issue, the flow will be more or less the same)

---

**Installation Process**
Just open your terminal, and run the following command
```bash
sudo dnf install ngspice
```

## What we’ll cover today ?   

In this part (considering what seshadri sir covered till now) , we’ll keep it simple and talk about two basic but important things:  

1. **DC Operating Point Analysis**  
   → Finding the voltages and currents in your circuit at steady state.  

2. **DC Sweep Analysis**  
   → Seeing how your circuit behaves when you change (or sweep) an input, like varying a source voltage.  

We’ll write `.cir` files, run them in Ngspice, and check the outputs.  
Follow along, try to copy-paste the code examples first and then try them out yourself.  

---


## Contents

1. [DC Operating Point Analysis](#1-dc-operating-point-analysis)
2. [Voltage Divider Example](#example-voltage-divider)
3. [I–V of a Simple Resistor](#iv-of-a-simple-resistor)
4. [DC Sweep Analysis](#2-dc-sweep-analysis)
5. [Diode I–V Example](#example-diode-iv-curve)
6. [Applications](#applications)
7. [Tips & Tricks](#tips--tricks)

---

## 1. DC Operating Point Analysis

The **DC operating point** (also called the bias point) is the set of node voltages and currents in a circuit when only DC sources are applied.
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

* `V1 in 0 DC 10` defines a 10 V DC source between node `in` and ground (`0`).
* `R1` and `R2` form a simple resistor divider.
* `.op` tells Ngspice to compute the operating point.
* `.end` marks the end of the file.

### Running the Simulation

Save the file and run:

```bash
ngspice divider.cir
```

If `.op` is present, Ngspice automatically prints the node voltages and branch currents.

Expected result:

* `V(out)` ≈ 3.33 V
* `I(R1)` = `I(R2)` ≈ 0.33 mA

---

## I–V of a Simple Resistor

A simple way to demonstrate DC sweep is to measure the current through a resistor while sweeping a voltage source. This produces a straight line I–V curve that follows Ohm's law:
$I = \dfrac{V}{R}$

### Circuit file: `resistor_iv.cir`

```spice
* Resistor I-V characteristic using DC sweep
* Sweep a source from 0 to 10 V and measure current through the source

V1 in 0 0       ; voltage source to be swept (node 'in' to ground)
R1 in 0 1k      ; 1 kohm resistor between node 'in' and ground

.dc V1 0 10 0.5 ; sweep V1 from 0 V to 10 V in 0.5 V steps
.print dc v(in) i(V1)
.end
```

### How to run

Save as `resistor_iv.cir` and run:

```bash
ngspice resistor_iv.cir
```

At the ngspice prompt you can also type:

```
plot v(in) -i(V1)
```

Note: `i(V1)` reports the current through the voltage source using SPICE's sign convention. In this file the current flowing **out of** the positive terminal appears negative, so `-i(V1)` plots the positive-valued current through the resistor (makes the I–V line slope positive).

### Expected numeric results (Ohm's law)

With $R = 1\text{k}\Omega$:

* For 0 V → 0 mA
* For 1 V → 1 mA
* For 5 V → 5 mA
* For 10 V → 10 mA

### Small sample table (for 0.5 V steps)

| V (V) |    I (A) | I (mA) |
| ----: | -------: | -----: |
|   0.0 | 0.000000 |    0.0 |
|   0.5 | 0.000500 |    0.5 |
|   1.0 | 0.001000 |    1.0 |
|   2.0 | 0.002000 |    2.0 |
|   5.0 | 0.005000 |    5.0 |
|  10.0 | 0.010000 |   10.0 |

### Exporting data

```spice
set filetype=ascii
wrdata resistor_iv_data.txt v(in) i(V1)
```

If you want positive currents, post-process the file (multiply by -1) or use `-i(V1)` directly in `.print` or `wrdata` if supported.

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

* `V1 anode 0 0` defines a voltage source to be swept.
* `D1` is a diode with model `Dmodel`.
* `R1` is a load resistor.
* `.model Dmodel D` defines a default diode model.
* `.dc V1 0 1 0.01` sweeps V1 from 0 V to 1 V in 0.01 V steps.
* `.print dc v(anode) i(V1)` prints the voltage at node `anode` and the current through `V1`.

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

* **Operating point analysis**

  * Biasing point of BJTs and MOSFETs in amplifiers.
  * Verifying DC node voltages in biasing networks.
  * Establishing initial conditions before AC or transient simulations.

* **DC sweep analysis**

  * Obtaining diode, BJT, and MOSFET I–V characteristics.
  * Measuring transfer functions (Vout vs Vin).
  * Exploring the effect of varying supply voltages.

---

## Tips & Tricks

* Use `.print` to print specific voltages and currents:

  ```spice
  .print dc v(out) i(R1)
  ```
* Both `.op` and `.dc` can be used in the same file.
* To save simulation data for plotting in external tools:

  ```spice
  set filetype=ascii
  wrdata results.txt v(out) i(V1)
  ```
* Ngspice plots can be exported to `.png` files for including screenshots in your documentation:

  ```spice
  hardcopy plot.png
  ```

---

## References

* [Ngspice User Manual](http://ngspice.sourceforge.net/docs.html)
* [Spice Syntax Guide](https://www.seas.upenn.edu/~jan/spice/spice.overview.html)
