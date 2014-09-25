Modeling-Project
====================

BMED 4784 Project
Problem 1 (Only Membrane Potential)

V_m' = I_ion/C_m
M' = a_m*(1-M)-(b_m*M)
N' = a_n*(1-N)-(b_n*N)
H' = a_h*(1-H)-(b_h*H)
G_K' = (N^4)*(g_K_max)
G_NA' = (M^3)*(g_NA_max)*H

M = 3.6E-6
N = 8.9E-4
H = 0.99
G_K = 2.25E-11
G_NA = 5.54E-15
V_m = -70

a_m = 0.1*((25-V_m)/(exp[(25-V_m)/10]-1))
b_m = 4*exp[-V_m/18]
a_n = 0.01*((10-V_m)/(exp[(10-V_m)/10]-1))
b_n = 0.125*exp[-V_m/80]
a_h = 0.07*exp[-V_m/20]
b_h = 1/(exp[(30-V_m)/10]+1)

I_NA = (M^3)*(g_NA_max)*H*(V_m-E_NA)
I_K = (N^4)*(g_K_max)*(V_m-E_K)
I_L = g_L*(V_m-E_L)
I_ion = I-I_K-I_NA-I_L

g_K_max = 36
g_NA_max = 120
g_L = 0.3
E_K = -12
E_NA = 115
E_L = 10.6
C_m = 1E-6
I = 0

!! G_K G_NA

t0 = 0
hr = 0.1
tf = 100
