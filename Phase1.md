Modeling-Project
====================

BMED 4784 Project
Problem 1 (Only Membrane Potential)

V_m' = I_i/C_m
M' = a_m*(1-M)-(b_m*M)
N' = a_n*(1-N)-(b_n*N)
H' = a_h*(1-H)-(b_h*H)

M = 0.00000363
N = 0.000895
H = 0.99

a_m = 0.1*((25-V_m)/(exp[(25-V_m)/10]-1))
b_m = 4*exp[-V_m/18]
a_n = 0.01*((10-V_m)/(exp[(10-V_m)/10]-1))
b_n = 0.125*exp[-V_m/80]
a_h = 0.07*exp[-V_m/20]
b_h = 1/(exp[(30-V_m)/10]+1)

I_NA = (M^3)*g_NA_open*H*(V_m-E_NA)
I_K = (N^4)*g_K_open*(V_m-E_K)
I_L = g_L*(V_m-E_L)
I_i = I-I_K-I_NA-I_L

g_K_open = 36
g_NA_open = 120
g_L = 0.3
E_K = -12
E_NA = 115
E_L = 10.6
V_m = -70
C_m = 1
I = 0

t0 = 0
hr = 0.1
tf = 100
