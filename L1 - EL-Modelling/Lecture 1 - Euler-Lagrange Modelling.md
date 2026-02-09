
---
![[images/slide_001.png]]

- 

---

![[images/slide_002.png]]
Euler-Langrange: for many "connected" bodies
Acrobot: 2 revolute dof

---
Definition of Underactuated Robot
![[images/slide_003.png]]



---

![[images/slide_004.png]]

There is only control of the inside system --> which makes the ball roll 
But no control of the ball  directly
--> Underactuated Robot

---

![[images/slide_005.png]]

Flexiibility of the links (included in the model) --> means extra DOFs
Only really intereting for modelling vibrations?
A gear box could also be an extra dof?

---
Underactuated joint model
![[images/slide_006.png]]



---

![[images/slide_007.png]]



---

![[images/slide_008.png]]

Active joint = joint with actuator


---

![[images/slide_009.png]]

the object has DOFs on top of the hands DOFs --> so if we want to control object in hand --> think of it has underactuated System

---

![[images/slide_010.png]]



---

![[images/slide_011.png]]

We work in time domain

---
Course Structure
![[Pasted image 20260202113113.png]]



---

![[images/slide_013.png]]

Conservative System: total mechanical energy is constant --> no energy dissipation
	This is "ideal" - a system with no friction, the basis of modelling

Non-conservative systems models friction using Euler-Lagrange modelling

---
Hamiltons Principle
![[images/slide_014.png]]
instead of free-body diagrams we use Ekin and Epot as base for our model


---

![[images/slide_015.png]]



---

![[images/slide_016.png]]

- 

---

![[images/slide_017.png]]

- 

---

![[images/slide_018.png]]

used instead of N2
first term is the time derivative of partial velocity
the second term is the partial derivative os position (generalized coodinates)
$q = [\theta_1  \theta_2]$
OG ligningen ovenfor indgår --> derfor et system af to ligninger

ma= f,
mx'' = f <-- two derivitions means 2 dofs

---

![[images/slide_019.png]]

2 dofs --> 2 second order diff-equations
hooks law: $= k\theta$

$I theta_´´ = \tau_net$

---

Euler-Lagrange model for rotational mass-spring system
![[images/slide_020.png]]

no translation --> no gravitational potential/kinetic energy



---

![[images/slide_021.png]]

Øverst: partial derivative of langrangian with respect to qdot
Nederst: partial derivative of langrangian with respect to q
![[Pasted image 20260202115540.png]]
Disse er udgangspunktet for nedenstående
![[Pasted image 20260202115714.png]]
hvor theta 1 og theta 2 er byttet om

---

![[images/slide_022.png]]

generalized coordinates: $q = [\theta, x]$
angle contributes 1 dof
there is also variation in x (due to spring)
Epot = mgh

$Epot = 1/2 \ k \ x^2 + m \ g \cos(\theta) \ (l+x)$


$Ekin = 1/2 \ m \ x_{dot}^2+1/2 \ m \ (l + x) \theta_{dot} ^2$

- $E_{pot} = \frac{1}{2} kx^2 - mg(l+x) \cos(\theta)$
- $E_{kin} = \frac{1}{2}m \dot x^2 + \frac{1}{2}m(l+x)^2 \dot \theta^2$
---

![[images/slide_023.png]]



---

![[images/slide_024.png]]

- 

---

![[images/slide_025.png]]

- 

---

![[images/slide_026.png]]

Only dissipitative forces go into capital Q
otherwise they are modelled twice


---

![[images/slide_027.png]]
External force tau added

---

![[images/slide_028.png]]

- tau only applied to second body
![[Pasted image 20260202122105.png]]

---

![[images/slide_029.png]]



---

![[images/slide_030.png]]

-

---

![[images/slide_031.png]]



---

Kinetic and Potential Energy calculation
![[images/slide_032.png]]
Makes it possible to use kinematics --> this is a vector [x, y, z]
![[Pasted image 20260202123730.png]]
systematic approach
first write down kinematics
then we have center of mass
compute potential and kinetic energies

![[Pasted image 20260202123906.png]]
this is invertable (?) so the kinetic energy is positive(?)

---

![[images/slide_033.png]]
positional jacobian
![[Pasted image 20260202124049.png]]


---

![[images/slide_034.png]]



---

![[images/slide_035.png]]

- 

---

![[images/slide_036.png]]

- 

---

![[images/slide_037.png]]

- 

---

![[images/slide_038.png]]

![[Pasted image 20260202124511.png]]
This is centrifugal and coriolis forces

---

Full Euler-Lagrange model
![[images/slide_039.png]]
We need to isolate acceleration
- $\ddot q = B^-1 (q)(\tau - C(q,\dot q) \dot q - g(q))$


---

![[images/slide_040.png]]

- 

---
Kinematics of two-joint robot  in DH-notation
![[images/slide_041.png]]
theta is the angle we actuate through q1 and q2
 

---

![[images/slide_042.png]]

Homogeneuous transformation between the coordinate frames
Translations
	a1 is l1
	a2 is l2

---
Model of Acrobot - center of mass
![[images/slide_043.png]]

notation
	c: cosine
	s: sine
see above slide
start with pos of COM, everything is in relation to base frame

---

![[images/slide_044.png]]
Joint 2 is located at frame 1
Joint 1 is located at frame 0 (base frame)
A01 --> translation from 0 to 1
A12 --> translation from 1 to 2



---

Positional and Orientational Jacobians
![[images/slide_045.png]]
we need one jacobian per body, as each has a COM (not just the end-effector)



---

![[images/slide_046.png]]

PER DH NOTATION: translation along z-axis, and rotation about z-axis
so the jacobian is written in terms of z-axis 
"The point we are studying" is pi
Below is translation along z-axis

![[Pasted image 20260202125416.png]]
X: cross product (from origin of base frame to com of l1)

Below is rotation about z-axis
![[Pasted image 20260202125457.png]]


---

![[images/slide_047.png]]

- 

---

![[images/slide_048.png]]

- 

---

![[images/slide_049.png]]

This is radius(es) from point of origin to point of study
![[Pasted image 20260202125912.png]]

---

![[images/slide_050.png]]

- 

---

![[images/slide_051.png]]

potential energy --> comes from position
kinetic energy --> comes from velocity


---
![[images/slide_052.png]]

- 

---

![[images/slide_053.png]]

- 

---

![[images/slide_054.png]]

- pl1 and pl2 are the two bodies
- g0 is the gravitational acceleration vector for x y and z


---

![[images/slide_055.png]]

- 

---

![[images/slide_056.png]]

- 

---

![[images/slide_057.png]]

- 

---

![[images/slide_058.png]]

Translational part
	![[Pasted image 20260202130112.png]]
Rotational part
	![[Pasted image 20260202130131.png]]

Constant in local coordinates but CAN have varying base coordinates

---

![[images/slide_059.png]]

we rotate to fit base-frame axis by multiplying with base frame

---

![[images/slide_060.png]]
pi and wi are given in terms of base frame

---

![[images/slide_061.png]]

- 

---

![[images/slide_062.png]]

This ensures we can compute Ekin in terms of kinematics directly


---

![[images/slide_063.png]]

- 

---

![[images/slide_064.png]]

- 

---

![[images/slide_065.png]]



---

![[images/slide_066.png]]

- 

---

![[images/slide_067.png]]

- 

---
Overview of Procedure for Modelling
![[images/slide_068.png]]

- 0 is using dh-parameters as that is standard in robotics

---

![[images/slide_069.png]]

- 

---

![[images/slide_070.png]]

---

---

![[images/slide_071.png]]

- 

---

![[images/slide_072.png]]

Model of (Underactuated) System
If tau is not completely "filled" it is underactuated --> so m < n (number of spaces in tau)
![[Pasted image 20260202130859.png]]

 

---

