

---

# Thermodynamic Cycle Analysis for Internal Combustion Engines

## Overview

This repository contains a detailed computational analysis of four fundamental thermodynamic cycles used in internal combustion engines: **Dual**, **Otto**, **Diesel**, and **Atkinson**. The project aims to:

- Model and compare the performance metrics (efficiency, work output, heat input) of these cycles under specified conditions.
- Optimize the Dual cycle for maximum efficiency and work output, adhering to a maximum temperature constraint of 2500 K.
- Evaluate NOx emissions across all cycles using the Zeldovich mechanism.
- Assess practical applications, vehicle speed dynamics, and valve flow characteristics for a selected engine.

The analysis employs **Python** for modeling, leveraging libraries such as **NumPy**, **Matplotlib**, **SciPy**, and **Cantera** to perform thermodynamic calculations, optimizations, and visualizations. All cycles are modeled assuming ideal gas behavior, with specific enhancements like temperature-dependent specific heats and combustion modeling via the Wiebe function for realism.

---

## Repository Contents

### Scripts
The repository includes the following Python scripts, each addressing specific tasks:

- **`code_1_1.py`**: Plots P-V diagrams for Dual, Otto, and Diesel cycles at a compression ratio of 12 with fixed heat input (1476.48 kJ/kg).
- **`code_1_2.py`**: Calculates efficiencies for Dual (r=12), Otto (r=18), and Diesel (r=18) cycles under a 2500 K temperature constraint and generates corresponding P-V diagrams.
- **`code_1_3.py`**: Optimizes the Dual cycle for maximum thermal efficiency using a brute-force approach, varying compression ratio (r), pressure ratio (r_p), and cut-off ratio (r_c).
- **`code_1_4.py`**: Optimizes the Dual cycle for maximum net work output, subject to the 2500 K constraint.
- **`code_1_5.py`**: Provides flexible optimization of the Dual cycle, allowing user-specified parameters or automatic optimization for maximum work.
- **`code_2_1.py`**: Analyzes the Atkinson cycle (r_c=12, r_e=17), calculating work output, efficiency, and heat input, and plotting its P-V diagram.
- **`code_4_p1.py`**: Plots valve lift profiles for intake and exhaust valves as a function of crankshaft angle, optimized for performance with a 40° overlap.
- **`code_4_p2.py`**: Plots throat velocity (V_t) and mass flux (ṁ/A) versus upstream pressure, identifying subsonic and sonic flow regions.
- **`code_dual_nox.py`**: Computes NOx emissions for the Dual cycle using the Zeldovich mechanism, incorporating Cantera and Wiebe function combustion modeling.
- **`code_diesel_nox.py`**: Computes NOx emissions for the Diesel cycle with similar advanced modeling techniques.

### Outputs
- **Figures**: P-V diagrams for all cycles, valve lift profiles, flow velocity/mass flux plots, and NOx formation curves.
- **Data**: CSV files (e.g., `simulation_results.csv`) containing detailed simulation outputs for NOx analysis.

---

## Dependencies

To run the scripts, install the following Python libraries:

- **Python 3.x**
- **NumPy**: For numerical computations.
- **Matplotlib**: For plotting diagrams and results.
- **SciPy**: For optimization and differential equation solving.
- **Cantera**: For advanced thermodynamic and chemical modeling (NOx calculations).

Install them using:
```bash
pip install numpy matplotlib scipy cantera
```

---

## Usage

1. **Running Scripts**:
   - Execute each script individually via the command line, e.g.:
     ```bash
     python code_1_1.py
     ```
   - Some scripts (e.g., `code_1_3.py`, `code_1_5.py`) prompt for user inputs such as initial temperature (T1), pressure (P1), and maximum temperature (T_max).

2. **Interpreting Outputs**:
   - Scripts print key metrics (efficiency, work output, NOx concentrations) to the console.
   - Generated plots (e.g., P-V diagrams, NOx curves) are saved as PNG files in the repository.

3. **Customization**:
   - Modify input parameters (e.g., compression ratio, heat input) within the scripts or via prompts to explore different scenarios.

---

## Methodology

### Thermodynamic Modeling
- **Cycles**: Modeled with ideal gas assumptions, using constant or temperature-dependent specific heats (γ tailored per cycle).
- **Optimization**: The Dual cycle is optimized using brute-force (codes 1-3, 1-4) and Brentq methods (NOx scripts) to maximize efficiency or work, constrained by T_max = 2500 K.
- **Combustion**: The Wiebe function simulates heat release, with cycle-specific parameters (e.g., crank angle duration: 40° Otto, 70° Diesel).

### NOx Emissions
- **Zeldovich Mechanism**: Implemented via ODE systems solved with SciPy’s `solve_ivp` (LSODA method), capturing thermal NOx formation.
- **Enhancements**: Cantera’s GRI-Mech 3.0 mechanism and fuel-specific heat capacities (gasoline for Otto/Atkinson, diesel for Dual/Diesel) improve accuracy.

### Vehicle Dynamics
- **Engine**: 2020 Honda Civic Si (1.5L turbo, 205 hp) analyzed for mass flow rate (0.077 kg/s at 5700 RPM, η_v=0.9) and vehicle speeds across six gears.
- **Valve Flow**: Modeled as a nozzle, with sonic/subsonic regions identified at a critical pressure of 198.86 kPa.

---

## Results and Findings

### Performance
- **Otto Cycle**: Highest efficiency (65.2% at r=18) due to constant volume heat addition.
- **Dual Cycle**: Balanced performance (63.0% at r=18; optimized to 72.45% at r=25.1 or 938.3 kJ/kg work at r=20).
- **Diesel Cycle**: Lower efficiency (56.3% at r=18) but higher torque potential.
- **Atkinson Cycle**: Peak efficiency (65.65% at r_c=12, r_e=17), ideal for hybrids.

### NOx Emissions
- **Diesel**: Highest (4185.05 ppm) due to prolonged combustion and high compression (r=14.05).
- **Atkinson**: 2909.66 ppm, elevated by extended expansion (r=11.84).
- **Otto**: 2326.33 ppm, moderate due to rapid combustion (r=11.84).
- **Dual**: Lowest (2146.38 ppm), suggesting optimization potential (r=14.05).

### Vehicle Speed
- Speeds range from 43.74 km/h (1st gear) to 232.31 km/h (6th gear) at 5700 RPM, reflecting torque-speed trade-offs via gear ratios (3.643 to 0.686).

---

## Practical Applications

- **Otto Cycle**: Gasoline engines in passenger cars (e.g., Honda Civic), motorcycles, and small aircraft.
- **Diesel Cycle**: Heavy-duty vehicles (e.g., Freightliner trucks), industrial machinery, and marine propulsion.
- **Atkinson Cycle**: Hybrid vehicles (e.g., Toyota Prius) prioritizing fuel efficiency.
- **Dual Cycle**: Theoretical model influencing advanced diesel and HCCI engine design.

---

## Limitations and Future Work

- **Assumptions**: Ideal gas behavior and constant specific heats simplify modeling; variable specific heats are partially addressed in NOx scripts.
- **Scope**: NOx analysis caps T_max at 2500 K; real diesel engines may exceed this, increasing emissions.
- **Future Enhancements**: Incorporate real gas effects, variable valve timing, or CFD for flow dynamics.

---

## References

- Çengel, Y. A., & Boles, M. A. (2015). *Thermodynamics: An Engineering Approach*. McGraw-Hill Education.
- Heywood, J. B. (2018). *Internal Combustion Engine Fundamentals*. McGraw-Hill Education.
- Stone, R. (2012). *Introduction to Internal Combustion Engines*. Palgrave Macmillan.
- Anderson, J. D. (2017). *Fundamentals of Aerodynamics*. McGraw-Hill Education.
- Gillespie, T. D. (1992). *Fundamentals of Vehicle Dynamics*. SAE International.

---

## Acknowledgments

This project was completed under the supervision of **Dr. Mashaikh**, **Engineer Mehrabian**, and **Ms. Sharifi**, with support from the Faculty of Mechanical Engineering. Special thanks to **Mohammad Matin Bazrafshan (Student Number: 40026117)** for contributions to coding and analysis.

