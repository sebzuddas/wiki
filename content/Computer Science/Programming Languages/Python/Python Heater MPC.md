


A heater [[Model Predictive Control (MPC)|MPC]] implemented using [[Python]].

```python
#  -*- coding: utf-8 -*-
"""

Author: Finn Haugen, USN
finn.haugen@usn.no

Updated 2022 01 20.

Nonlinear model-predictive control (MPC) of a simulated air heater

http://techteach.no/air_heater/

Process model:

    dT_dt = ((T_env - T) + K_heat*u(t-t_delay) + d)/t_const
    
where:
    
    - T [C ]is outlet air temperature
    - T_env [C] is environmental (room) temp
    - u [V] is control signal to the heater
    - d [V] is a disturbance

Features:

    - Constraint on state (T)
    - Constraint on control signal (u)
    - Constraint on rate of change of control signal (du_dt)
    - Control signal blocking (or grouping) (u)
    - An Extended Kalman Filter which estimates an input disturbance.

Stop time is set with t_stop. Default: 300 s.

Toggle between continuous (online) plot and batch (offline) plot with 
variable cont_plot:
    cont_plot = 1 = continuous (online) plot.
    cont_plot = 0 = batch (offline) plot.
If continuous plot, execute the socalled magic command
%matplotlib auto 
in console which will show plot in an external window.

Originally implemented in Python 3.7.

"""

#%% Imports

import matplotlib.pyplot as plt
import numpy as np
from scipy.optimize import minimize
import time

#%% Definition of functions

def fun_mpc_objective_airheater(u):

    u_at_each_k = np.array([])

    T_k = T_est_k
    d_k = d_est_k

    for ku in range(0, N_blocks_u):
        u_at_each_k = np.append(u_at_each_k, 
                                np.zeros(N_samples_in_blocks)*0 + u[ku])

    u_km1 = u[0]
    J_km1 = 0

    delay_array = np.zeros(N_delay) + u[0]

    # Simulation loop:

    for k in range(0, N_pred):

        u_k = u_at_each_k[k]

        T_sp_k = T_sp_to_mpc_array[k]

        # Time delay:
        u_delayed_k = delay_array[-1]
        delay_array[1:] = delay_array[0:-1]
        delay_array[0] = u_k

        # Solving diff eq:
        dT_dt_k = ((1/t_const)*((T_env - T_k) 
                                    + K_heat*(u_delayed_k + d_k)))
        T_kp1 = T_k + ts*dT_dt_k

        # Updating objective function:
        e_k = T_sp_k - T_k
        du_k = (u_k - u_km1)/ts
        J_k = J_km1 + ts*(C_e*e_k**2 + C_du*du_k**2)

        # Time shift:
        T_k = T_kp1
        u_km1 = u_k
        J_km1 = J_k

    return J_k


def fun_mpc_constraints_airheater(u):

    # The return values are required >= 0 (geq 0)
     
    u_at_each_k = np.array([])

    T_k = T_est_k
    d_k = d_est_k

    for ku in range(0, N_blocks_u):
        u_at_each_k = np.append(u_at_each_k,
                                np.zeros(N_samples_in_blocks)*0
                                + u[ku])
    
    delay_array = np.zeros(N_delay) + u[0]

    # Simulation loop:

    for k in range(0, N_pred):

        u_k = u_at_each_k[k]

        # Time delay:
        u_delayed_k = delay_array[-1]
        delay_array[1:] = delay_array[0:-1]
        delay_array[0] = u_k

        # Solving diff eq:
        dT_dt_k = ((1/t_const)
                   *((T_env - T_k)
                     + K_heat*(u_delayed_k + d_k)))
        T_kp1 = T_k + ts*dT_dt_k
        
        # Rate of change of u:
        du_dt_k = (u_k - u_opt_km1)/ts

        # Storing in array
        T_fun_constraint_array[k] = T_k
        du_dt_fun_constraint_array[k] = du_dt_k

        # Time shift:
        T_k = T_kp1

    geq_zero_equiv_to_T_upperbound_constr = \
        (T_upperbound - np.max(T_fun_constraint_array))

    geq_zero_equiv_to_T_lowerbound_constr = \
        (np.min(T_fun_constraint_array) - T_lowerbound)

    geq_zero_equiv_to_du_dt_upperbound_constr = \
        (du_dt_upperbound - np.max(du_dt_fun_constraint_array))

    geq_zero_equiv_to_du_dt_lowerbound_constr = \
        (np.min(du_dt_fun_constraint_array) - du_dt_lowerbound)

    return np.array([geq_zero_equiv_to_T_upperbound_constr, 
                     geq_zero_equiv_to_T_lowerbound_constr, 
                     geq_zero_equiv_to_du_dt_upperbound_constr, 
                     geq_zero_equiv_to_du_dt_lowerbound_constr])

#%% Process params

K_heat = 3.5  # [deg C/V]
t_const = 23.0  # [s]
t_delay = 3.0  # [s]

T_env = 28.0  # [deg C]

#%% Time settings

ts = 0.5  # Time-step [s]
t_pred_horizon = 8.0  # [s]

N_pred = int(t_pred_horizon/ts)
t_start = 0.0  # [s]
t_stop = 300.0  # [s]
N_sim = int((t_stop-t_start)/ts) + 1

#%% MPC costs

C_e = 1.0
C_du = 20.0  # [(L/s)/min]
mpc_costs = np.array([C_e, C_du])

#%% MPC control blocking (or grouping)

# Number of blocks of control signal:
N_blocks_u = 3
# Number of samples in each control block:
N_samples_in_blocks = int(np.ceil(N_pred/N_blocks_u))

#%% Defining sequence of T setpoint

T_sp_const = 30.0  # [C]
Ampl_step = 2.0  # [C]
Slope = -0.04  # [C/s]
Ampl_sine = 1.0  # [C]
t_period = 50.0  # [s]

t_const_start = t_start
t_const_stop = 100
t_step_start = t_const_stop
t_step_stop = 150
t_ramp_start = t_step_stop
t_ramp_stop = 200
t_sine_start = t_ramp_stop
t_sine_stop = 250
t_const2_start = t_sine_stop
t_const2_stop = t_stop

t = np.linspace(t_start, t_stop, N_sim)
T_sp_array = np.linspace(t_start, t_stop, N_sim)*0

for k in range(0, N_sim):
    if t[k] >= t_const_start and t[k] < t_const_stop:
        T_sp_array[k] = T_sp_const
    if t[k] >= t_step_start and t[k] < t_step_stop:
        T_sp_array[k] = T_sp_const + Ampl_step
    if t[k] >= t_ramp_start and t[k] < t_ramp_stop:
        T_sp_array[k] = T_sp_const + Ampl_step + Slope*(t[k]-t_ramp_start)
    if t[k] >= t_sine_start and t[k] < t_sine_stop:
        T_sp_array[k] = T_sp_const + Ampl_sine*np.sin(2*np.pi*(1/t_period)*(t[k]-t_sine_start))
    if t[k] >= t_const2_start:
        T_sp_array[k] = T_sp_const

#%% Change of disturbance

t_disturbance = 50  # [s]
d_const = -2  # [V]

#%% Initialization of time delay

u_init = 0.0  # [V]
N_delay = int(round(t_delay/ts)) + 1
delay_array = np.zeros(N_delay) + u_init

#%% Initial state of process

T_k = 28.0  # [C]

#%% Initialization of Kalman Filter

# Initial state:
T_est_pred_k = 28.0  # [C]
d_est_pred_k = 0.0  # [V]

T_est_k = T_est_pred_k
d_est_k = d_est_pred_k

n_states = 2

# Initial pred state estim:
x_est_pred_k = np.array([[T_est_k, d_est_k]]).T

# Initial covariance of pred estim error:
stdev_error_T = 0.01
P_est_error_T = (stdev_error_T**2)
stdev_est_error_d = 0.01
P_est_error_d = (stdev_est_error_d**2)
P_est_pred_k = np.diag([0.1*P_est_error_T,
                                  0.1*P_est_error_d])

#%% Tuning of Kalman Filter

stdev_w_T = 0.01
cov_w_T = stdev_w_T**2
stdev_w_d = 0.01
cov_w_d = stdev_w_d**2
Q = np.diag([1*cov_w_T, 1*cov_w_d])

stdev_v_T = 0.01
cov_v_T = stdev_v_T**2
R = np.diag([1*cov_v_T])

#%% Initial value of previous optimal value

u_opt_km1 = u_init

#%% Initial value of time delayed control signal

u_delayed_k = 2

#%% Defining arrays for plotting

t_plot_array = np.linspace(t_start, t_stop, N_sim-N_pred)
T_sp_plot_array = np.zeros(N_sim-N_pred) + T_sp_const 
T_plot_array = np.zeros(N_sim-N_pred) + T_sp_const
T_est_plot_array = np.zeros(N_sim-N_pred) + T_sp_const
T_env_plot_array = np.zeros(N_sim-N_pred) + T_env
u_plot_array = np.zeros(N_sim-N_pred) + 1.07  # 1.07=final steady-state
du_opt_dt_plot_array = np.zeros(N_sim-N_pred)
d_est_plot_array = np.zeros(N_sim-N_pred) - 0.5
d_plot_array = np.zeros(N_sim-N_pred) - 0.5

#%% Settings of optimizer

# Initial guessed optimal control sequence:
u_guess = np.zeros(N_blocks_u) + u_init

# Lower and upper limits of optim variable for use in SLSQP:
u_upperbound = 5
u_lowerbound = 0
bounds_u = np.zeros([N_blocks_u, 2])
bounds_u[:, 0] = u_lowerbound
bounds_u[:, 1] = u_upperbound

# Constraints:
T_upperbound = 31.0  # [C]
T_lowerbound = 27.0  # [C]
T_fun_constraint_array = np.zeros(N_pred)

du_dt_upperbound = 0.25 #1 # 0.25  # [V/s]
du_dt_lowerbound = -0.25 #-1 # -0.25  # [V/s]
du_dt_fun_constraint_array = np.zeros(N_pred)

#%% Selecting between continuous plot and batch plotting

# 0 = off = batch (offline) plot.
# 1 = on = continuous (online) plot. May use external
# figure window by executing %matplotlib auto in console.

cont_plot = 0

real_time_step = 0.01

#%% Preparing for plotting

plt.close("all")
fig1 = plt.figure(num=1, figsize=(30/2.54, 18/2.54))

legend_shadow = True
location = 'upper right'
font_size = 8
handle_length = 1

#%% For-loop for MPC of sim process incl Kalman Filter

tic = time.time()  # Starting timer of simulation execution

for k in range(0, (N_sim-N_pred)):

    t_k = t[k]
    t_plot_array[k] = t_k
    print('t = ', t_k)

    # -----------------------
    # Kalman Filter for estimating T and d using meas of T.
    # These estimates are used by the MPC.
    # Note: The time-delayed u is used as control signal here.

    # Matrices in linearized model:

    A_cont = np.array([[-1/t_const, K_heat/t_const],
                        [0, 0]])
    C_cont = np.array([[1, 0]])
    A_disc = np.eye(n_states) + ts*A_cont
    C_disc = C_cont

    # Kalman K_heat:

    K_k = ((P_est_pred_k@(C_disc.T))
           @(np.linalg.inv(C_disc@P_est_pred_k@(C_disc.T) + R)))

    # Innovation process:
    e_est_k = T_k - T_est_k

    # Measurement-based correction of estimate = the applied estimate:
    # x_est_corr_k = x_est_pred_k + K_k*e_est_k
    T_est_corr_k = T_est_pred_k + K_k[0, 0]*e_est_k
    d_est_corr_k = d_est_pred_k + K_k[1, 0]*e_est_k

    # Applied estimated process meas:
    # T_est_corr_k = x_est_corr_k[0, 0]
    # d_est_corr_k = x_est_corr_k[1, 0]
    T_est_k = T_est_corr_k
    d_est_k = d_est_corr_k

    # Prediction of state estimate for next time-step:
    dT_est_corr_dt_k = ((1/t_const)
                        *((T_env - T_est_corr_k)
                          + K_heat*(u_delayed_k + d_est_corr_k)))
    dd_est_corr_dt_k = 0

    # dx_est_corr_dt_k = np.matrix([dT_est_corr_dt_k,
    #                               dd_est_corr_dt_k]).T

    # x_est_pred_kp1 = x_est_corr_k + ts*dx_est_corr_dt_k
    T_est_pred_kp1 = T_est_corr_k + ts*dT_est_corr_dt_k
    d_est_pred_kp1 = d_est_corr_k + ts*dd_est_corr_dt_k

    # Auto-covariance of error of corrected estimate:

    P_est_corr_k = (np.eye(n_states)
                    - K_k@C_disc)@P_est_pred_k

    # Auto-covariance of error of predicted estimate of next time step:
    P_est_kp1 = A_disc@P_est_corr_k@(A_disc.T) + Q

    # Time shift:
    P_est_pred_k = P_est_kp1

    # x_est_pred_k = x_est_pred_kp1
    T_est_pred_k = T_est_pred_kp1
    d_est_pred_k = d_est_pred_kp1

    # Storage for plotting:
    T_est_plot_array[k] = T_est_k
    d_est_plot_array[k] = d_est_k

    # Setpoint array to optimizer:
    T_sp_to_mpc_array = T_sp_array[k:k+N_pred]
    T_sp_plot_array[k] = T_sp_array[k]

    # Calculating optimal control sequence:
    ineq_cons = {'type':'ineq',
                 'fun':fun_mpc_constraints_airheater} #  >= 0 constraints
    res = minimize(fun_mpc_objective_airheater, 
                   u_guess,
                   method='SLSQP',
                   constraints=[ineq_cons],
                   options={'ftol': 1e-9, 'disp': False},
                   bounds=bounds_u)

    toc = time.time()

    u_opt = res.x
    # print('u_opt = ', u_opt)
    # Optim solution used as guessed sol. in next iteration:
    u_guess = u_opt

    # Optimal control signal (sample) to be applied:
    u_opt_k = u_opt[0]
    # print('u_opt_k = ', u_opt_k)
    u_plot_array[k] = u_opt_k  # Storage for plotting

    # Rate of change of u:
    du_opt_dt_k = (u_opt_k - u_opt_km1)/ts
    du_opt_dt_plot_array[k] = du_opt_dt_k
    
    # Time shift:
    u_opt_km1 = u_opt_k

    # Applying optimal control signal to simulated process:

    if t[k] <= t_disturbance:
        d_k = 0.0
    else:
        d_k = d_const
    
    d_plot_array[k] = d_k

    # Time delay:
    u_delayed_k = delay_array[-1]
    delay_array[1:] = delay_array[0:-1]
    delay_array[0] = u_opt_k
    # Solving diff eq:
    dT_dt_k = ((1/t_const)
               *((T_env - T_k) + K_heat*(u_delayed_k + d_k)))
    T_kp1 = T_k + ts*dT_dt_k

    # Storage for plotting:
    T_plot_array[k] = T_k
    
    # Time shift:
    T_k = T_kp1

    if cont_plot == 1:       

        plt.subplot(2, 2, 1)
        plt.grid(which='both', color='k')
        plt.ylim(26, 34)
        plt.xlim(t_start, t_stop)
        plt.xlabel('t [s]')
        plt.ylabel('[deg C]')
        plt.legend(('T_sp = red', 'T = blue', 
                    'T_env = cyan', 'T_bounds'),
                   loc=location)

        plt.subplot(2, 2, 2)
        plt.grid(which='both', color='k')
        plt.ylim(-1, 6)
        plt.xlim(t_start, t_stop)
        plt.xlabel('t [s]')
        plt.ylabel('[V]')
        plt.legend(('Control signal u', 'u bounds'),
                   loc=location)

        plt.subplot(2, 2, 3)
        plt.grid(which='both', color='k')
        plt.ylim(-3, 3)
        plt.xlim(t_start, t_stop)
        plt.xlabel('t [s]')
        plt.ylabel('[V]')
        plt.legend(('d_est = blue', 'd = green'),
                   loc=location)

        plt.subplot(2, 2, 4)
        plt.grid(which='both', color='k')
        plt.ylim(-2, 2)
        plt.xlim(t_start, t_stop)
        plt.xlabel('t [s]')
        plt.ylabel('[V/s]')
        plt.legend(('Rate of change of control signal, du_dt', 'du_dt bounds'),
                   loc=location)

        if k > 0 and k < N_sim:
            plt.subplot(2, 2, 1)
            plt.plot([t_plot_array[k],t_plot_array[k-1]], 
                     [T_sp_plot_array[k],T_sp_plot_array[k-1]], 'r',
                     [t_plot_array[k],t_plot_array[k-1]], 
                     [T_plot_array[k],T_plot_array[k-1]], 'b',
                     [t_plot_array[k],t_plot_array[k-1]], 
                     [T_env_plot_array[k],T_env_plot_array[k-1]], 'c',
                     [t_plot_array[k],t_plot_array[k-1]], 
                     [T_upperbound,T_upperbound], 'm',
                     [t_plot_array[k],t_plot_array[k-1]], 
                     [T_lowerbound,T_lowerbound], 'm')
            
            plt.subplot(2, 2, 2)
            plt.plot([t_plot_array[k],t_plot_array[k-1]], 
                     [u_plot_array[k],u_plot_array[k-1]], 'b',
                     [t_plot_array[k],t_plot_array[k-1]], 
                     [u_upperbound,u_upperbound], 'm',
                     [t_plot_array[k],t_plot_array[k-1]], 
                     [u_lowerbound,u_lowerbound], 'm')
            
            plt.subplot(2, 2, 3)
            plt.plot([t_plot_array[k],t_plot_array[k-1]], 
                     [d_est_plot_array[k],d_est_plot_array[k-1]], 'b',
                     [t_plot_array[k],t_plot_array[k-1]], 
                     [d_plot_array[k],d_plot_array[k-1]], 'g')
            
            plt.subplot(2, 2, 4)
            plt.plot([t_plot_array[k],t_plot_array[k-1]], 
                     [du_opt_dt_plot_array[k],du_opt_dt_plot_array[k-1]], 'b',
                     [t_plot_array[k],t_plot_array[k-1]], 
                     [du_dt_upperbound,du_dt_upperbound], 'm',
                     [t_plot_array[k],t_plot_array[k-1]], 
                     [du_dt_lowerbound,du_dt_lowerbound], 'm')
            
            fig1.canvas.draw()
            fig1.canvas.flush_events()
    
            time.sleep(real_time_step)

# -------------------------------------------------------------
# Simulation execution time:

toc = time.time()  # Stopping timer for simulation execution
t_elapsed = toc - tic

print('Elapsed time total [s] = {:.2e}'.format(t_elapsed))
print('Elapsed time per time step [s] = {:.2e}'.format(t_elapsed/N_sim))
print('------------------------------------------------')

# -------------------------------------------------------------
# Plotting:

if cont_plot == 0:    
    plt.subplot(2, 2, 1)
    plt.grid(which='both', color='k')
    plt.ylim(26, 34)
    plt.xlim(t_start, t_stop)
    plt.xlabel('t [s]')
    plt.ylabel('[deg C]')
    plt.plot(t_plot_array, T_sp_plot_array, 'r',
             t_plot_array, T_plot_array, 'b',
             t_plot_array, T_env_plot_array, 'c',
             t_plot_array, T_plot_array*0 + T_upperbound, 'm',
             t_plot_array, T_plot_array*0 + T_lowerbound, 'm')
    plt.legend(('T_sp = red', 'T = blue', 
                'T_env = cyan', 'T bounds'),
               loc=location)
    
    plt.subplot(2, 2, 2)
    plt.grid(which='both', color='k')
    plt.ylim(-1, 6)
    plt.xlim(t_start, t_stop)
    plt.xlabel('t [s]')
    plt.ylabel('[V]')
    plt.plot(t_plot_array, u_plot_array, 'b',
             t_plot_array, u_plot_array*0 + u_upperbound, 'm',
             t_plot_array, u_plot_array*0 + u_lowerbound, 'm')
    plt.legend(('Control signal u', 'u bounds'),
               loc=location)
    
    plt.subplot(2, 2, 3)
    plt.grid(which='both', color='k')
    plt.ylim(-3, 3)
    plt.xlim(t_start, t_stop)
    plt.xlabel('t [s]')
    plt.ylabel('[V]')
    plt.plot(t_plot_array, d_est_plot_array, 'b',
             t_plot_array, d_plot_array, 'g')
    plt.legend(('d_est = blue', 'd = green'),
               loc=location)
    
    plt.subplot(2, 2, 4)
    plt.grid(which='both', color='k')
    plt.ylim(-2, 2)
    plt.xlim(t_start, t_stop)
    plt.xlabel('t [s]')
    plt.ylabel('[V/s]')
    plt.plot(t_plot_array, du_opt_dt_plot_array, 'b',
             t_plot_array, du_opt_dt_plot_array*0 
             + du_dt_upperbound, 'm',
             t_plot_array, du_opt_dt_plot_array*0 
             + du_dt_lowerbound, 'm')
    plt.legend(('Rate of change of control signal, du_dt',
                'du_dt bounds'),
               loc=location)
    
    plt.show()
```