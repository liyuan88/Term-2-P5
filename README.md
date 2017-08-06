# Term-2-P5
# The Model (state, actuators and update equations)
The model implements the kinematic model which ignores tire forces, gravity and mass, as introcduced in the class. At low and moderate speeds, kinematic models often approximate the actual vehicle dynamics. The vehicle state vector includes the vehicle's x and y positions in global coordinates, current orienatation angle of the vehicle (ψ), velocity (v). The actuator vector includes the steering angle (δ) and the throttle rate (a). 

The kinematic model predicts the state (t+1) as follows. 

![equations](./Kinematic.png)

The error terms (CTE and angle error) are updated as follows. 

![equations](./CTE.png)

# Timestep Lengeth (N) and Elapsed Duration (dt)
There comes a trade off between the N and dt. The larger N provides more accuracy but slows down the model. I started out by setting dt equals to the system latency (0.1s) and played around the N value. I choose N=15 after some trials as it seems to provide a good balance between accuracy and calculation time. 

# Polynomial Fitting and MPC Preprocessing
The x and y in the global coordinates are transformed into the vehicle's coordinates. This significantly simplies the calculations in the MPC model as now the x, y and the orientation angle are all zero in the vehicle's coordinates. 

The polyfit function then fits the transformed waypoints into a 3rd degree polynomial. The polyeval function calculates the CTE in the vehicle's coordinates. 

# Model Predictive Control with Latency
To account for the latency in the system, I predict the the vehicle's state after 0.1s and then calculate what the vehicle needs to do based on the predicted state, instead of the current state. This is done in the Main.cpp, line 125-130. Notice that it's done in the vehicle's coordinates so the original x, y and orientation are all zeros, which again simplifies the calculation. This is working pretty well in the final result. 

