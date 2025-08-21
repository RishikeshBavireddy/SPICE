# Circuits and Network Analysis – Ngspice Programs  

**Course:** Circuits and Network Analysis  
**Semester:** Fall 2025  
**Instructor:** Prof. Seshadri Sravan Kumar, IIT Hyderabad  
**Maintained by:** Teaching Assistant(s)  

---

## 📖 About this Repo

This repository is the central place where we will keep all the **Ngspice circuit programs, tutorials, and notes** used in the course.  
Think of it as your **digital lab notebook** – you can find ready-to-run SPICE netlists, example waveforms, and short explanations for each concept we cover in lectures and labs.  

If you are new to Ngspice, don’t worry. Everything here is meant to be beginner-friendly:  
- Copy the netlist files → run in Ngspice → observe waveforms.  
- Follow the tutorials → understand step by step how the analysis works.  
- Use this as a reference when doing lab assignments or practicing on your own.  

---

## 📂 Folder Structure

Here’s how the repo is organized:

```
ngspice-circuits/
│
├── tutorials/      # Step-by-step Markdown guides for each analysis
├── examples/       # Ready-to-run .cir files (grouped by topic: diode, bjt, opamp…)
├── figures/        # Plots and screenshots (to match with tutorials)
└── docs/           # Extra notes, references, cheat-sheets
```

- **`tutorials/`** → Explains each topic (DC, AC, transient, etc.) with examples.  
- **`examples/`** → All the actual Ngspice netlist files you can run directly.  
- **`figures/`** → Supporting images (waveforms, plots, diagrams).  
- **`docs/`** → Extra reference material.  

---

## 🚀 How to Use

1. Install Ngspice (Linux: `sudo apt install ngspice`, Windows: use installer).  
2. Clone this repo:  
   ```bash
   git clone https://github.com/yourusername/ngspice-circuits.git
   cd ngspice-circuits
   ```
3. Browse `tutorials/` → read the concept.  
4. Open the corresponding `.cir` file from `examples/` → run it in Ngspice:  
   ```bash
   ngspice examples/diode/diode_iv_sweep.cir
   ```

---

## 🎯 Why This Repo Exists

- To support the **Circuits and Network Analysis** course with hands-on examples.  
- To give students a single reference for Ngspice netlists, notes, and outputs.  
- To encourage you to experiment, tweak circuits, and learn by doing.  

---

## ✨ Contributions

Students are welcome to add:  
- New `.cir` files (in correct folders).  
- Screenshots of interesting results.  
- Short notes or clarifications in `tutorials/`.  

We’ll review and merge useful contributions — so this repo becomes a shared learning space.  

---

Happy learning & simulating ⚡  
