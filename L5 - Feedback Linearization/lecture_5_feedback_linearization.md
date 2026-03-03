

![[images/lecture_5_feedback_linearization/slide_001.png]]

- Inordeer to design system as second order systems --> need to first think of it as a mechancal system

---

![[images/lecture_5_feedback_linearization/slide_002.png]]

- 

---

![[images/lecture_5_feedback_linearization/slide_003.png]]

- 

---

![[images/lecture_5_feedback_linearization/slide_004.png]]

- 

---

![[images/lecture_5_feedback_linearization/slide_005.png]]

- Derive system model
- analyse energy graphs
- stability, linerization, controllability/observability
- simulation in simulink
- pole placement control
- check error dynamics

---

![[images/lecture_5_feedback_linearization/slide_006.png]]

- 

---
Control of nonlinear systems

![[images/lecture_5_feedback_linearization/slide_007.png]]

- pole-placement works just locally around the chosen fixed point
- 

---

![[images/lecture_5_feedback_linearization/slide_008.png]]

- 

---
Inverted Pendulum Example
![[images/lecture_5_feedback_linearization/slide_009.png]]

for thetad = 0, thetadotd = 0, thetadotdotd = 0
- control input: tau = g(q), as there should be no torque

---

![[images/lecture_5_feedback_linearization/slide_010.png]]

- for second order system the dynamics can be categorized into 
	overdamped, 
	critically damped and 
	underdamped 
--> vi vil gerne vælge polerne så de er kritisk dæmpede, og det gør vi ved først at bestemme polerne med den karakteristiske ligning: det(AI-lambda) = 0
- vi vil gerne have at error går asymptotisk mod nul, det gør de hvis polerne er reelle

---

![[images/lecture_5_feedback_linearization/slide_011.png]]

- 

---

![[images/lecture_5_feedback_linearization/slide_012.png]]

- 

---

![[images/lecture_5_feedback_linearization/slide_013.png]]

- 

---

![[images/lecture_5_feedback_linearization/slide_014.png]]

- 

---

![[images/lecture_5_feedback_linearization/slide_015.png]]

- 

---

![[images/lecture_5_feedback_linearization/slide_016.png]]

- the control input is the acceleration --> so this equation equals the original system model

---

![[images/lecture_5_feedback_linearization/slide_017.png]]

- 

---

![[images/lecture_5_feedback_linearization/slide_018.png]]

- 

---

![[images/lecture_5_feedback_linearization/slide_019.png]]

- q_d_dotdot er et feedforward term

---

![[images/lecture_5_feedback_linearization/slide_020.png]]

- 

---

![[images/lecture_5_feedback_linearization/slide_021.png]]

- high level vs low level controllers
	- low level sker inde i "robot" (ex current control af en motor?)
	- "robotten" kan også have ulinerariteter som vi ikke modellere i high-level control
	- hvis der er et gap mellem ulineariteterne i outputtet fra "robotten" og dem vi modellere, og kompensere for i Inverse Dynamics control, ville inputtet til robot være forkert --> derfor basere vi det på en PID-kontroller, så disse ulineariteter kan kompenseres væk
---

![[images/lecture_5_feedback_linearization/slide_022.png]]

- 

---

![[images/lecture_5_feedback_linearization/slide_023.png]]

- 

---

![[images/lecture_5_feedback_linearization/slide_024.png]]

- 

---

![[images/lecture_5_feedback_linearization/slide_025.png]]

- 

---
Need to do seperation between passive and active joints by splitting the dynamics into q1 and g2 (for the linearlized system)
![[images/lecture_5_feedback_linearization/slide_026.png]]

- q2: active joints 
- q1: passive joints

---

![[images/lecture_5_feedback_linearization/slide_027.png]]

- 

---

![[images/lecture_5_feedback_linearization/slide_028.png]]

- 

---

![[images/lecture_5_feedback_linearization/slide_029.png]]

- 

---

![[images/lecture_5_feedback_linearization/slide_030.png]]

- 

---
Seperate q1 first 
![[images/lecture_5_feedback_linearization/slide_031.png]]

- 

---
substitute the inverse dynamics of q1 into the system equation for q2, in order to obtain feedback linearized controller where the dynamics of q1 are eliminated, so q1 can be controlled from q2
![[images/lecture_5_feedback_linearization/slide_032.png]]

- control input for a collocated/active joint 

---

![[images/lecture_5_feedback_linearization/slide_033.png]]

- stronly coupled joints: we cannot control both joint dynamics simultaneously --> we can either control active joint with active joint (collocated) or passive joint with active joint (non-collocated)

---

![[images/lecture_5_feedback_linearization/slide_034.png]]

- 

---

![[images/lecture_5_feedback_linearization/slide_035.png]]

- 

---

![[images/lecture_5_feedback_linearization/slide_036.png]]

- 

---

![[images/lecture_5_feedback_linearization/slide_037.png]]

- 

---
(Control engeneering notation for closed loop dynamics)
![[images/lecture_5_feedback_linearization/slide_038.png]]

- This form is good for analysis

---
(Control engeneering notation for closed loop dynamics)
![[images/lecture_5_feedback_linearization/slide_039.png]]

- solution is exponential decay
- omega: zero-dynamics

---

![[images/lecture_5_feedback_linearization/slide_040.png]]

- 

---

![[images/lecture_5_feedback_linearization/slide_041.png]]

- if z is stable we can ensure that zeta is stable

---

![[images/lecture_5_feedback_linearization/slide_042.png]]

- 

---

![[images/lecture_5_feedback_linearization/slide_043.png]]

- 

---

![[images/lecture_5_feedback_linearization/slide_044.png]]

- 

---
To control passive joint, use non-collocated linearization
![[images/lecture_5_feedback_linearization/slide_045.png]]


- linear dependance between two rows (passive and active) creates a link (strong inertial coupling)
	- this is required to be able to control passive joints with active joints

---
This means we can qrite the pseudo-inverse of (in this case) B_12
![[images/lecture_5_feedback_linearization/slide_046.png]]

- Then we can design (v2) non-collocated control input 
- 

---

![[images/lecture_5_feedback_linearization/slide_047.png]]

- check if this is correct?

---

![[images/lecture_5_feedback_linearization/slide_048.png]]

- passive dofs are linearized and decoupled from the rest of the system dynamics

---

![[images/lecture_5_feedback_linearization/slide_049.png]]

- 

---

![[images/lecture_5_feedback_linearization/slide_050.png]]

- torque input (tau): final control-law from collocated linearization
- v2 is applied to rewrite the control-law into a non-collocated linearization (tau)

---
desired trajectory (yd) tracking
![[images/lecture_5_feedback_linearization/slide_051.png]]

- 

---
This is just one more step than the collocated version
![[images/lecture_5_feedback_linearization/slide_052.png]]

- the inputs are supposed to be q1 (not q2)

---

![[images/lecture_5_feedback_linearization/slide_053.png]]

- for the collocated version we focus on z, as this is related to the active joint, now we look at eta to focus on the passive joint

---

![[images/lecture_5_feedback_linearization/slide_054.png]]

- 

---

![[images/lecture_5_feedback_linearization/slide_055.png]]

- 

---

![[images/lecture_5_feedback_linearization/slide_056.png]]

- look at zero dynamics to ensure controller is stable

---

![[images/lecture_5_feedback_linearization/slide_057.png]]

- 

---

![[images/lecture_5_feedback_linearization/slide_058.png]]

- controlling the active joint based on the passive (using non-collocated control law)

---

![[images/lecture_5_feedback_linearization/slide_059.png]]

- 

---

![[images/lecture_5_feedback_linearization/slide_060.png]]

- 

---

![[images/lecture_5_feedback_linearization/slide_061.png]]

- 

---

![[images/lecture_5_feedback_linearization/slide_062.png]]

- 

---

![[images/lecture_5_feedback_linearization/slide_063.png]]

- 

---

![[images/lecture_5_feedback_linearization/slide_064.png]]

- 

---

