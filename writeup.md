### MPC Model

The system state consists of `x and y` positions of the car, angle of the car heading with respect to x-axis (`psi`) and the velocity of the car (`v`). Additionally cross track error (`CTE`) and error in the angle (`epsi`) are included in the system state.

Actuators are the actual control input to the accelerator and the steering wheel. We aren't considering brake as separate entity since the accelerator input is modeled as values ranging from -1 to 1. A negative value implies braking scenario and positive value implies acceleration. The actuators we consider here are throttle (`a`) and steering angle (`delta`).

---
### Update Equations

I have used the following update equations given in the course material

*x[t+1] = x[t] + v[t] * cos(psi[t]) * dt
y[t+1] = y[t] + v[t] * sin(psi[t]) * dt
psi[t+1] = psi[t] - v[t] / Lf * delta[t] * dt
v[t+1] = v[t] + a[t-1] * dt
cte[t+1] = f(x[t]) - y[t] + v[t] * sin(epsi[t]) * dt
epsi[t+1] = psi[t] - psides[t] - (v[t] * delta[t-1] / Lf * dt)*

---
### Timestep Length and Frequency (N and dt)

I chose timestep length `(N) as 10` and timestep frequency `(dt) as 0.1`

Timestep length should be large enough to make viable plans ahead but small enough to allow for the computational cost. First I tried with 20 for N. But it caused car to drive erratically due to the higher computational cost associated with it. Then I reduced the value of N to 5. But it was not enough to predict the future path. Then by increasing in steps of 1, I finally found that 10 gives a reasonable value for N time step.

Similarly, the frequency should be large enough to plan more way points and small enough to reduce the computational cost. I started with 0.05 and incrementing by 0.01, I found 0.1 gave me satisfactory result. Beyond that the accuracy of the system got reduced. Since larger the dt, less the number of samples being calculated in-turn, reduces accuracy.

A time step length of `10` and dt of `0.1` proved to be a good combination overall, hence I settled with these values.

---
### Preprocessing and Polynomial Fitting

The preprocessing consists of rotating(transforming) the way points to vehicle coordinate space. The x and y is set to the origin on vehicle coordinate space, i.e. (0,0). The angle (psi) is rotated such that psi is 0. The velocity vector V is moved along the x-axis so that when fitting the polynomial, we get equation such `y=f(x)` which is easy to compute.

Another processing is that the steering value is converted into the vehicle space ranging from -1 to 1. The psi update equation is slightly modified to accommodate the negative steering function of the simulator.

---
### Latency

A contributing factor to latency is actuator dynamics. To overcome the latency of the model, I used previous-previous actuator state (t-2) to influence the current state rather than last actuator state(t-1). 

Best Regards,
Vivek Mano
