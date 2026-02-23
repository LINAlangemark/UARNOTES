we have a set of generalized coordinats for each free body 
![[Pasted image 20260209122109.png]]
Tau has a moment of inertia specified in a matrix ($\Gamma$): nxm --> m is te number of actuators
an underactuated system has m < n --> n is the number of degrees of freedom

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_002.png]]



---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_003.png]]

- 

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_004.png]]

- 

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_005.png]]

- 

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_006.png]]

- 

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_007.png]]

- Assume we meassure all states --> we dont address the problem ofderived meassuring of states
- f, g, h are matrices

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_008.png]]
substitution --> as 
- 

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_009.png]]

- this is the 2nd order diff equation (as it is underactuated??)

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_010.png]]

We partition into "control and state terms"
this is state:
	![[Pasted image 20260209123424.png]]
this is control:
	![[Pasted image 20260209123439.png]]
gamma-dependant (intertia of tau)

for ss-models we have twice as many states as coordinates (2nd order?)

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_011.png]]

- **dynamics of pendulm**
Moment of intertia
	![[Pasted image 20260209123731.png]]
Gravity (torque)
![[Pasted image 20260209123745.png]]
Friction
	![[Pasted image 20260209123758.png]]
Control
![[Pasted image 20260209123803.png]]


---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_012.png]]

- 

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_013.png]]

- omega is angular speed
- omega_dot is angular velocity

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_014.png]]

- omega_dot is therefore --> 2nd (time) derivative of q

in this course we "handle non-linearity without linearization"
	by using the invrse of the nonlinear forces

---
- Phase plot
![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_015.png]]


say: tau = 0 (intet input)
for each point, evaluate vector, to find direction of solution/change
I 90 grader vil pilen være "nul"/ikke eskistere, siden tyngdekraften går mod bevægelsesretningen (modvirker bevægelsesretningen) 
("unstable equilibrium")
![[Pasted image 20260209125306.png]]
i 180 grader vil den pege nedad (tyngdekraften trækker), 
-90 grader --> "stable equillibrium"
![[Pasted image 20260209125127.png]]

men ligeså snart vi kommer forbi -90 vil pilen pege opad fordi accelerationen modvirker tyngekraften tilstrækkeligt

vi vil gerne sørge for at der kun er stable equilibriums


---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_016.png]]

- 

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_017.png]]

- 

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_018.png]]

- Linearization --> Taylor approx around point
	- xdot should be 0 here (equilibrium point)

xhat, uhat --> deviations from the point of linearization


---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_019.png]]

- 

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_020.png]]
vi vælger arbitrært punkt
![[Pasted image 20260209131322.png]]
til at linearisere --> så bliver dt til equillibriumpunktet
derfor er xbar og ybar konstanter (det er et punkt)
- to go from uhat to u, add ubar (a constant)
	![[Pasted image 20260209125757.png]]

the plant is still not linearized, so the output is also handled as:
	![[Pasted image 20260209125827.png]]
	

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_021.png]]

- evaluate the derivaties at point of linearization
	- ![[Pasted image 20260209125949.png]]
	- ![[Pasted image 20260209125956.png]]
	

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_022.png]]

- we evaluate partial derivative at point of linearization, therefore here it is thetabar
	- ![[Pasted image 20260209130157.png]]
remember: in g(x), we already replaced x with xbar (a constant)
	![[Pasted image 20260209130230.png]]
so this part just transfers directly
	![[Pasted image 20260209130245.png]]
this is the torque that can compensate for gravity (hold pendulum at particular position)
	![[Pasted image 20260209130407.png]]
---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_023.png]]

red: linear
blue: nonlinear
-when we go  further away from point of equilibrium the error between these two is larger
(as we linearize in the point of equillibrium)

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_024.png]]

- 

---
**we only talk controllability and observability of linearized system, not non-linear (too complicated)**

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_025.png]]

- asymptotically stable if state converges to zero as time goes to inf, for all initial states

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_026.png]]

- only for linearized systems

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_027.png]]

- for most non-linear systems it doesnt make sense to find solution

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_028.png]]

- 

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_029.png]]

- 

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_030.png]]

- all the exponential terms has to go towards zero, which requires the real part to be negative
- ![[Pasted image 20260209132146.png]]

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_031.png]]

- 

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_032.png]]

- løsning til andengradsligningen finder eigenværdierne
- notice the eignvalue can be positive if the sqrt term is positive
	- then the system would be unstable
	

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_033.png]]

- 

---



**THIS would happen if the system doesnt have friction --> it is a problem, analysis is meaningless if this is the case**

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_035.png]]

- 

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_036.png]]

- 

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_037.png]]

- Controlability is checked through the rank of a controlability matrix

---
Controlability matrix of the linearized system
![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_038.png]]

- full rank --> controllable
	- found through determinant

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_039.png]]

- 

---
Pole Placement (can be done if system is controllable)
![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_040.png]]

- 

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_041.png]]

- 

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_042.png]]

Non-linear: blue arrows, yellow trajectory
linear: red arrows, blue trajectory

pole placement applied to linear and nonlinear
	different only in extremes

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_043.png]]

- 

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_044.png]]

- 

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_045.png]]

- 

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_046.png]]

- 

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_047.png]]

- 
christopher bruger "simscape multibody"
---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_048.png]]

- 

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_049.png]]

- 

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_050.png]]

- this uses numerical integration (through matlab)

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_051.png]]

-  euler integration gives angle at next step

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_052.png]]

- 

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_053.png]]

- 

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_054.png]]

- this shows how simple it is to go from scalar to vector with ss-models
	- k (index) is used instead t
	- step length should be carefully chosen (ode45)
discrete time model
	doesnt assume linearity
	
---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_055.png]]

- ode45 uses (non-fixed step size) --> adapts step length 
dx --> xdot
only really change the diff-equation


---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_056.png]]

- 

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_057.png]]

- simulation of the linear model

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_058.png]]

- 

---
we need to verify code both through mathematical tool and graphical tool --> simscape
![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_059.png]]

This can be used to build models from joints, transformations etc
	-every line corresponds to a coordinate frame that we can make transformations between
torque etc can be applied to joiny, sensing, meassurements

simulates the same thing as ode45


---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_060.png]]

- evt find en quadcopter CAD model?

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_061.png]]

- Gazebo is also fine

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_062.png]]

- 

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_063.png]]

- 

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_064.png]]

- 

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_065.png]]


---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_066.png]]

- - work from BOTH conservative and non-conservative forces
	iif there is no non-consercative forces last term wouldnt be present
		the total mechanical energy would have to decrease
	
this can be used as verification
---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_067.png]]

- 

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_068.png]]

- 

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_069.png]]

- 

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_070.png]]

- as there is friction in the pendulum, the total mechanical enery decreases
	there is negative work done by non-conservative forces, that accumulates
if the system is completely conservative the black and green would be the same

this  can also be helpful to determine step size of integration
if this "osilates" it might be due to wrong physics...due to step size

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_071.png]]

- eigen values should all be positive
- check if 2 is skew symetric (next time, it has to do with stability) 
	- skew symmetric(?): it would equal zero


---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_072.png]]

- 

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_073.png]]

- model is written on ss-form to simulate

---

![[images/Lecture_2_-_Simulation_of_Robot_Dynamics/slide_074.png]]

- 
report:
	do derivation
	make simulation of energy conservation 
	![[Pasted image 20260209135856.png]]
		with all parts


---

