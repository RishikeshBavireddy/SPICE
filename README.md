# Circuits and Network Analysis – Circuit Simulation using ngspice  

**Course:** Circuits and Network Analysis - EE1101
**Semester:** Fall 2025  
**Instructor:** Prof. Seshadri Sravan Kumar, IIT Hyderabad  

---

## About this Repo

This repository is the central place where we will keep all the **Ngspice circuit programs and tutorials** used in the course.  
consider this as your **digital notebook** , you can find ready-to-run SPICE netlists, example waveforms, and short explanations for each concept we cover in lectures and labs.  

---

## Folder Structure

Here’s how the repo is organized:

```
ngspice-circuits/
│
├── tutorials/      # Step-by-step Markdown guides for each analysis
├── examples/       # Ready-to-run .cir files (grouped by topic: diode, bjt, opamp…)
├── figures/        # Plots and screenshots (to match with tutorials)
```
---

## How to Use

1. Install Ngspice 
2. Clone this repo:  
   ```bash
   git clone https://github.com/RishikeshBavireddy/SPICE.git
   cd ngspice-circuits
   ```
3. Browse `tutorials/` → read the concept.  
4. Open the corresponding `.cir` file from `examples/` → run it in Ngspice:  
   ```bash
   ngspice examples/diode/diode_iv_sweep.cir
   ```

---

## Why This Repo Exists

- To support the **Circuits and Network Analysis** course with hands-on examples.  
- To give students a single reference for Ngspice netlists, notes, and outputs.  
- To encourage you to experiment, tweak circuits, and learn by doing.  

---

## Contributions

Students are welcome to add:  
- New `.cir` files (in correct folders).  
- Screenshots of interesting results.  
- Short notes or clarifications in `tutorials/`.  

We’ll review and merge useful contributions, so this repo becomes a shared learning space.  

---

