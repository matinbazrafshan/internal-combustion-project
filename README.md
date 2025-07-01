import numpy as np
import matplotlib.pyplot as plt

# Constants
gamma = 1.4
Cv = 0.718  # kJ/kg·K
Cp = 1.005  # kJ/kg·K
R = 0.287   # kJ/kg·K

# Initial conditions
P1 = 100    # kPa
T1 = 300    # K
v1 = R * T1 / P1  # m^3/kg
r = 12


# Dual Cycle
v2 = v1 / r
T2 = T1 * r**(gamma - 1)
P2 = P1 * r**gamma
P3 = 2 * P2
v3 = v2
T3 = T2 * (P3 / P2)
v4 = v3 + 0.05 * (v1 - v2)
P4 = P3
T4 = T3 * (v4 / v3)
v5 = v1
P5 = P4 * (v4 / v5)**gamma

# Otto Cycle
T3_otto = T2 + 1476.48 / Cv
v3_otto = v2
P3_otto = P2 * (T3_otto / T2)
v4_otto = v1
P4_otto = P3_otto * (v3_otto / v4_otto)**gamma

# Diesel Cycle
T3_diesel = T2 + 1476.48 / Cp
v3_diesel = v2 * (T3_diesel / T2)
P3_diesel = P2
v4_diesel = v1
P4_diesel = P3_diesel * (v3_diesel / v4_diesel)**gamma

# Functions
def isentropic_curve(v_start, v_end, P_start, num_points=100):
    v = np.linspace(v_start, v_end, num_points)
    P = P_start * (v_start / v)**gamma
    return v, P

def constant_volume_line(v, P_start, P_end, num_points=100):
    P = np.linspace(P_start, P_end, num_points)
    v = np.full(num_points, v)
    return v, P

def constant_pressure_line(P, v_start, v_end, num_points=100):
    v = np.linspace(v_start, v_end, num_points)
    P = np.full(num_points, P)
    return v, P

# Dual Cycle Curves
v1_2, P1_2 = isentropic_curve(v1, v2, P1)
v2_3, P2_3 = constant_volume_line(v2, P2, P3)
v3_4, P3_4 = constant_pressure_line(P3, v3, v4)
v4_5, P4_5 = isentropic_curve(v4, v5, P4)
v5_1, P5_1 = constant_volume_line(v5, P5, P1)

# Otto Cycle Curves
v1_2_o, P1_2_o = isentropic_curve(v1, v2, P1)
v2_3_o, P2_3_o = constant_volume_line(v2, P2, P3_otto)
v3_4_o, P3_4_o = isentropic_curve(v3_otto, v4_otto, P3_otto)
v4_1_o, P4_1_o = constant_volume_line(v1, P4_otto, P1)

# Diesel Cycle Curves
v1_2_d, P1_2_d = isentropic_curve(v1, v2, P1)
v2_3_d, P2_3_d = constant_pressure_line(P2, v2, v3_diesel)
v3_4_d, P3_4_d = isentropic_curve(v3_diesel, v4_diesel, P3_diesel)
v4_1_d, P4_1_d = constant_volume_line(v1, P4_diesel, P1)

# Plotting
plt.figure(figsize=(12, 8))
plt.plot(v1_2, P1_2, 'b-', label='Dual: 1-2 Isentropic Compression')
plt.plot(v2_3, P2_3, 'b-', label='Dual: 2-3 Const. Volume Heat Add.')
plt.plot(v3_4, P3_4, 'b-', label='Dual: 3-4 Const. Pressure Heat Add.')
plt.plot(v4_5, P4_5, 'b-', label='Dual: 4-5 Isentropic Expansion')
plt.plot(v5_1, P5_1, 'b-', label='Dual: 5-1 Const. Volume Heat Rej.')
plt.plot(v1_2_o, P1_2_o, 'r--', label='Otto: 1-2 Isentropic Compression')
plt.plot(v2_3_o, P2_3_o, 'r--', label='Otto: 2-3 Const. Volume Heat Add.')
plt.plot(v3_4_o, P3_4_o, 'r--', label='Otto: 3-4 Isentropic Expansion')
plt.plot(v4_1_o, P4_1_o, 'r--', label='Otto: 4-1 Const. Volume Heat Rej.')
plt.plot(v1_2_d, P1_2_d, 'g-.', label='Diesel: 1-2 Isentropic Compression')
plt.plot(v2_3_d, P2_3_d, 'g-.', label='Diesel: 2-3 Const. Pressure Heat Add.')
plt.plot(v3_4_d, P3_4_d, 'g-.', label='Diesel: 3-4 Isentropic Expansion')
plt.plot(v4_1_d, P4_1_d, 'g-.', label='Diesel: 4-1 Const. Volume Heat Rej.')
plt.xlabel('Specific Volume (m³/kg)')
plt.ylabel('Pressure (kPa)')
plt.title('PV Diagram: Dual, Otto, and Diesel Cycles at r = 12')
plt.grid(True)
plt.legend(bbox_to_anchor=(1.05, 1), loc='upper left')
plt.tight_layout()
plt.savefig('pv_diagram.png')
