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
//insert a block diagram


## 2. Transient Analysis
In simple words it is the time-domain response of the circuit

**Example: RC circuit**

Create a file named `rc_time_response.cir` with the following content:


```spice
*RC circuit transient analysis

V1 in 0 DC 10
R1 in out 5k
C1 out 0 10u

*control block
.control
tran
print v(out) 
.endc

.end
```

