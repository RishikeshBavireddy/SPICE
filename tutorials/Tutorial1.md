# TUTORIAL 1

**What is SPICE ?**

SPICE stands for **Simulation Program with Integrated Circuit Emphasis**.  
It’s basically a software which can be used to simulate circuits, instead of solving things on paper using KCL and KVL, you can analyse the circuit on your laptop itself. (Nice, is'nt it ?)  

---

**Why should you care?**
- Check if your circuit actually works before building it and analyse it    
- It’s completely open source and free !  

---

**What we’ll cover today ?**  

In this tutorial, we’ll get started with some basic things you covered before the mid-sem break like operating point analysis  

1. **DC Operating Point Analysis**  
   → Finding the voltages and currents in your circuit at steady state.  

2. **DC Sweep Analysis**  
   → Seeing how your circuit behaves when you change (or sweep) an input, like varying a source voltage. (reread the statement if you are in doubt, we want to observe variation of output w.r.t input, this is **NOT** transient analysis !) 

We’ll write `.cir` files, run them in Ngspice, and check the outputs.  
Follow along, try to copy-paste the code examples first and then try them out yourself.  

---

**Contents**

1. [DC Operating Point Analysis](#1-dc-operating-point-analysis)
2. [DC Sweep Analysis](#2-dc-sweep-analysis)
3. [Using GNU Plot](#GNU-PLOT)
---


**Getting started** 

I will be using Ubuntu, it is highly adviced that you use a Linux machine. If you are on windows try WSL, if you are on Mac I think you won't face any issue, the flow will be more or less the same

---

**Installation Process**

Just open your terminal, and run the following command
```bash
sudo apt install ngspice
```

**Structure of an Ngspice File**

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

**Example: Voltage Divider**

Create a file named `divider.cir` with the following content:

```spice
*Voltage Divider circuit

V1 in 0 DC 10
R1 in out 5k
R2 out 0 10k

*control block
.control
op
print v(out) i(V1)
.endc

.end
```

* `V1 in 0 DC 10` defines a 10 V DC source between node `in` and ground (`0`).
* `R1` and `R2` form a simple resistor divider.
* `op` tells Ngspice to compute the operating point.
* `.end` marks the end of the file.

**Running the Simulation**

Save the file and run:

```bash
ngspice divider.cir
```
Expected result:

* `V(out)` ≈ 6.66 V
* `i(V1)` ≈ -0.66 mA

---

**Example: RC Circuit**

Create a file named `rc_ckt.cir` with the following content:

```spice
*RC circuit op analysis

V1 in 0 DC 10
R1 in out 5k
C1 out 0 10u

*control block
.control
op
print v(out) i(V1)
.endc

.end
```

* `V1 in 0 DC 10` defines a 10 V DC source between node `in` and ground (`0`).
* `R1` and `C1` form a simple RC circuit.
* `op` tells Ngspice to compute the operating point.
* `.end` marks the end of the file.
* since we are doing operating point analysis current does not flow as the capacitor charge got saturated
**Running the Simulation**

Save the file and run:

```bash
ngspice rc_ckt.cir
```
Expected result:

* `V(out)` ≈ 10 V
* `i(V1)` ≈ 0 A

---

## 2. DC Sweep Analysis

A **DC sweep** varies a voltage or current source over a range of values and records how the circuit responds.
This is often used to generate I–V characteristics of devices like diodes and transistors.


**Example: Resistor I–V Curve**

A simple way to demonstrate DC sweep is to measure the current through a resistor while sweeping a voltage source. This produces a straight line I–V curve that follows Ohm's law

**Circuit file: `resistor_iv.cir`**

```spice
* Resistor I-V characteristic using DC sweep
* Sweep a source from 0 to 10 V and measure current through the source

V1 x 0 DC 0   
R1 x 0 10k    

*sweep V1 from 0 V to 10 V in 0.5 V steps
.control
dc V1 0 10 0.5 
print -i(V1)
.endc

.end
```
**How to run**

Save as `resistor_iv.cir` and run:

```bash
ngspice resistor_iv.cir
```
Note: `i(V1)` reports the current through the voltage source using SPICE's sign convention. In this file the current flowing **out of** the positive terminal appears negative, so `-i(V1)` plots the positive-valued current through the resistor (makes the I–V line slope positive).

---


**Example: Diode I–V Curve**

Create a file named `diode_iv.cir` with the following content:

```spice
* Diode IV characteristic using DC sweep

V1 in 0 DC 1
R1 in x 100
D1 x 0 DI_1N4007
.include 1N4007.mod

.control
dc V1 0.1 5 0.2
print -i(V1) 
.endc

.end
```

Explanation:

* `V1 in 0 DC 1` defines a voltage source to be swept.
* `D1` is a diode with model `DI_1N4007`.
* `R1` is a current limiting resistor.
* `dc V1 0.1 5 0.2` sweeps V1 from 0 V to 1 V in 0.01 V steps.
* `print -i(V1) ` prints the current through the diode.

**Running the Simulation**

Run:

```bash
ngspice diode_iv.cir
```

**Note :**

* Most of the time you will need to plot stuff instead of just looking at the data, there is an inbuilt plot function in ngspice, but I would recommend you to start using gnuplot as it will come handy durig a lot of stuff like comparing experimental and theoretical data in experiments, curve fitting, interpoltion...


---

## GNU Plot

Till now, we have seen the use of inbuilt print and plot functions of ngspice. Now, we will explore an other open-source tool called Gnuplot for plotting output stuff.
You might feel I am good with the plot function of ngspice, why use this gnuplot ?

- **Quality output**: Better customization, multiple export formats (PNG, PDF, SVG), and professional formatting for reports
- **Flexible workflow**: Easy post-processing of saved data without re-running simulations, and can combine multiple simulation runs.

**Getting Started with gnuplot**


**Installation Process**
Just open your terminal, and run the following command
```bash
sudo apt install gnuplot
```

To use **Gnuplot**, you can either run it interactively in the terminal or execute commands from a script file. (for now, we will just run it interactively from the terminal)

**1. Launching Gnuplot**
```bash
gnuplot
```

**2. Preparing Your Data File**

Create a data file `data.txt` with space or tab-separated values:
```
# X    Y
1      2
2      4
3      6
4      8
5      10
```

**3. Basic Plotting from Data**

**Plot Simple Data**
```bash
plot "data.txt"
```

**Plot with Lines**
```bash
plot "data.txt" with lines
```

**Plot with Points**
```bash
plot "data.txt" with points
```

**Plot with Lines and Points**
```bash
plot "data.txt" with linespoints
```

**4. Plotting Specific Columns**

For data with multiple columns `multidata.txt`:
```
# X    Y1    Y2
1     2     1
2     4     3
3     6     5
4     8     7
5     10    9
```

Plot specific columns:
```bash
plot "multidata.txt" using 1:2 with lines
```

We are digressing a bit at this point, so now let us get back to the original topic -- **using gnuplot to visualise ngspice output**
So, how do we change our ngspice code to use this. The change is simple, we have to modify the control block to write the data using wrdata function
Below example illustartes the use of gnuplot in combination with ngspice

**Example: Diode I–V Curve**

Create a file named `diode_gnu.cir` with the following content:

```spice
*forward bias diode i-v characterestics

V1 in 0 DC 1
R1 in x 100
D1 x 0 DI_1N4007
.include 1N4007.mod

.control
dc V1 0.1 5 0.2
wrdata forward.dat -i(V1) 
.endc

.end
```

Now run this in ngspice :

```bash
ngspice diode_gnu.cir
```

Now, this will create a file called forward.dat in the working directory, we will use gnuplot to plot the contents of the forward.dat file

Run:

```bash
gnuplot plot forward.dat using 1:2 with lines
```

**References**

* [Ngspice User Manual](http://ngspice.sourceforge.net/docs.html)
* [Gnuplot official website](http://www.gnuplot.info/)
