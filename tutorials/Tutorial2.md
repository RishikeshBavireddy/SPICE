# TUTORIAL 2

**What we’ll cover today ?**  

In this tutorial, we’ll have a quick review of last tutorial and then continue with the following topics  

1. **Transient Analysis**  
   → Analysing the circuit response over time  

2. **AC Analysis**  
   → Analysing the variation of the circuit response with the frequency of the AC signal
   
---

**Contents**

1. [Review](#1-review-of-session1)
2. [Transient Analysis](#2-transient-analysis)
3. [AC Analysis](#3-ac-analysis)
---

## 1. Review
What did we do in the previous session ?
![Recap](./tutorials/recap.png)


## 2. Transient Analysis
In simple words it is the time-domain response of the circuit

**Example: RC circuit -- charging**

Create a file named `rc_charge.cir` with the following content:


```spice
*RC circuit transient analysis

V1 in 0 DC 10
R1 in out 5k
C1 out 0 10u

.tran 1ms 100ms
.print tran v(out)
.plot tran v(out)
.end

```


* `V1 in 0 DC 10` defines a 10 V DC source between node `in` and ground (`0`).
* `R1` and `C1` form a simple RC circuit.
* `op` tells Ngspice to compute the operating point.
* `.end` marks the end of the file.
* 
**Running the Simulation**

Save the file and run:

```bash
ngspice rc_ckt.cir
```
Expected result:

* `V(out)` ≈ 10 V
* `i(V1)` ≈ 0 A

---

**Example: RC circuit -- discharging**

Create a file named `rc_discharge.cir` with the following content:


```spice
*RC circuit transient analysis

V1 in 0 DC 10
R1 in out 5k
C1 out 0 10u

.tran 1ms 100ms
.print tran v(out)
.plot tran v(out)
.end

```


* `V1 in 0 DC 10` defines a 10 V DC source between node `in` and ground (`0`).
* `R1` and `C1` form a simple RC circuit.
* `op` tells Ngspice to compute the operating point.
* `.end` marks the end of the file.
* 
**Running the Simulation**

Save the file and run:

```bash
ngspice rc_ckt.cir
```
Expected result:

* `V(out)` ≈ 10 V
* `i(V1)` ≈ 0 A

---




