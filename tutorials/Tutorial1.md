# Ngspice Tutorial

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
I will be using Ubuntu 22.05, it is highly adviced that you use a Linux machine. If you are on windows try WSL, if you are on Mac I think you won't face any issue, the flow will be more or less the same

---

**Installation Process**
Just open your terminal, and run the following command
```bash
sudo apt install ngspice
```

## What we’ll cover today ?   

In this part (considering what seshadri sir covered till now) , we’ll keep it simple and talk about two basic but important things:  

1. **DC Operating Point Analysis**  
   → Finding the voltages and currents in your circuit at steady state.  

2. **DC Sweep Analysis**  
   → Seeing how your circuit behaves when you change (or sweep) an input, like varying a source voltage.

3. **Transient Analysis**  
   → Analysing the transient response of the circuit.

    

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
## Structure of an Ngspice File

Before jumping into simulations, let’s first understand how an Ngspice file (commonly written as `.cir`) is structured.  
At a high level, an Ngspice code has **two main parts**:

1. **Netlist (Circuit Description)**  
   This is where you describe your circuit using components and their connections.  
   - Each component has a name (like `R1`, `V1`, `D1`) and a set of nodes it connects to.  
   - Example:  
     ```spice
     V1 in 0 DC 10   ; 10 V source between node 'in' and ground
     R1 in out 5k    ; 5kΩ resistor between node 'in' and 'out'
     R2 out 0 10k    ; 10kΩ resistor between 'out' and ground
     ```

2. **Control Statements (Simulation Commands)**  
   After the netlist, you tell Ngspice *what to do with your circuit*.  
   This part contains analysis commands like:  
   - `.op` → find DC operating point (node voltages and currents)  
   - `.dc` → sweep a source across a range and record results  
   - `.ac` → perform small-signal AC analysis  
   - `.tran` → transient (time-domain) analysis  

   Example:  
   ```spice
   .op
   .dc V1 0 10 1
   .end
   
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
Note: `i(V1)` reports the current through the voltage source using SPICE's sign convention. In this file the current flowing **out of** the positive terminal appears negative, so `-i(V1)` plots the positive-valued current through the resistor (makes the I–V line slope positive).

---


## 2. DC Sweep Analysis

A **DC sweep** varies a voltage or current source over a range of values and records how the circuit responds.
This is often used to generate I–V characteristics of devices like diodes and transistors.

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

## 3. Transient Analysis

Transient analysis shows how voltages/currents change over time

### Example: Charging of a capacitor - RC Circuit

Create a file named `rc_charging.cir` with the following content:

```spice
* Charging of a capcitor with a series resistance
V1 in 0 DC 1
R1 in out 100
C out 0 1u

.control
.tran 10u 10m
wrdata charging.dat v(out)
.endc

Explanation:

* 
* 
* 




### Running the Simulation

Run:

```bash
ngspice rc_charging.cir
```


## Some general stuff

* Use `.print` to print specific voltages and currents:

  ```spice
  .print dc v(out) i(R1)
  ```
* Most of the time you will need to plot stuff instead of just looking at the data, there is an inbuilt plot function in ngspice, but I would recommend you to start using gnuplot as it will come handy durig a lot of stuff like comparing experimental and theoretical data in experiments, curve fitting, interpoltion...


---

## References

* [Ngspice User Manual](http://ngspice.sourceforge.net/docs.html)
* [Spice Syntax Guide](https://www.seas.upenn.edu/~jan/spice/spice.overview.html)
