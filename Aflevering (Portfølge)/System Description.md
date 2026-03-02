Please select the underactuated system that you want to work with in this course. Describe the system, how many degrees of freedom that it has, and which of the degrees of freedom that are actuated.


The selected system is a quadcopter, which can be modeled as a rigid body with six degrees of freedom (x, y,z, phi, theta, psi), which specify the position and orientation of the center of mass of the quadcopter frame. 

The four actuators (rotors), have their own local frames, that each produce a thrust along their own vertical axis, through acceleration of the individual rotor. The individual thrusts combine to generate a total thrust force and control torques on the quadcopter frame.

The quadcopter model will therefore have four control inputs (vertical thrust, roll torque, pitch torque, yaw torque) acting on the quadcopter frame. This makes it an underactuated system,  where vertical motion and orientation are directly actuated and horizontal motion is not directly actuated. 

Horizontal translational motion is achieved by changing the orientation of the quadcoptor frame. Mapping from the individual rotor thrusts to the resulting total thrust and control torques is not addressed in this model. 



