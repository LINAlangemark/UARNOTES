### The problem Euler-Lagrange is solving
Newton’s method:
- Draw free-body diagrams
- Write force balances for each body
- Painful for multi-body, constrained systems
Euler–Lagrange:
- Avoids forces _inside_ the system
- Uses **energy** instead
- Scales beautifully to robots, multibody systems, constraints

**Core idea:** If you know how energy changes, you know how the system moves.
	 “Instead of free-body diagrams we use Ekin and Epot as base for our model”

---
### Conservative vs non-conservative systems (why Q exists)
**Conservative system**
- No friction, no damping, no losses
- Total mechanical energy is constant
- Model uses **only** kinetic + potential energy
Mathematically:
![[Pasted image 20260204090318.png]]
**Non-conservative system**
- Friction, damping, motors, external forces
- These **do not come from potential energy**
- They must be added manually
That’s where **generalized forces Q** come in:
![[Pasted image 20260204090342.png]]

***Only dissipative / external forces go into Q — otherwise they are modeled twice***
Gravity (NO) → already in $E_{pot}$ 
Motor torque (YES) → goes into $Q$

---
#### Formel forklaring
![[Pasted image 20260204092517.png]]
***q beskriver bevægelsesmulighederne — Q beskriver påvirkningerne i de samme retninger.***

**L (Lagrangian)**
![[Pasted image 20260204092618.png]]

---
**Første Led**
![[Pasted image 20260204092920.png]]

**Andet Led**
![[Pasted image 20260204092938.png]]

**MINUSSET betyder:**
> “Ændring i impuls” minus “energidrevne kræfter”  
> = det, der skal forklares af noget udefra
Hvis der **ikke** er noget udefra → højresiden = 0

---
![[Pasted image 20260204092821.png]]

---
### **Generalized coordinates**
![[Pasted image 20260204083736.png]]
This is _why_ Euler–Lagrange feels abstract: you choose coordinates that make sense, not forces.
- Describe the configuration of the system
- Don’t need to be Cartesian
- Just enough variables to describe everything
**Minimal**
- No redundant coordinates
**Independent**
- No constraints between them
**Complete**
- Fully describes the system configuration

---
### Hamiltons Principle
***instead of free-body diagrams we use Ekin and Epot as base for our model***
![[Pasted image 20260204090718.png]]

**Interpretation:** 
The system moves along the path that makes the _action_ stationary (usually minimal).
	You don’t compute this integral directly.  
	When you apply calculus of variations to it, you get the **Euler–Lagrange equations**.
So Hamilton’s Principle is the _philosophy_, Euler–Lagrange is the _tool_.

---
## The Euler–Lagrange equation
***The Euler-Lagrange Equation is used in place of Newtons second law of motion (F = am)***
![[Pasted image 20260204083827.png]]
![[Pasted image 20260204091047.png]]
Interpretation:
![[Pasted image 20260204091032.png]]

***"Only dissipative forces go into Q --> otherwise they are modelled twice"***

---
### Lagrange D'Alamberts principle
![[Pasted image 20260204084100.png]]

---
### Potential energy (gravity)
![[Pasted image 20260204091222.png]]
This is just:

![[Pasted image 20260204091232.png]]

Written in vector form so it works in **3D and robotics**.

Key ideas:
- pi(q) = position of center of mass
- Must be expressed in an **inertial frame**
- Gravity is constant

![[Pasted image 20260204091612.png]]

---
## Kinetic energy
![[Pasted image 20260204091336.png]]
### B(q) [the inertia tensor in Base Frame]
- The **mass / inertia matrix**
- Encodes how hard it is to accelerate the system
- Always:
    - symmetric
    - positive definite
    - invertible

> “this is invertible (?) so the kinetic energy is positive (?)”

---
### Euler-Lagrange Model in "Robot Form"
![[Pasted image 20260204091756.png]]
![[Pasted image 20260204091809.png]]

"We need to isolate acceleration"
![[Pasted image 20260204091823.png]]
This is the form used for:
- simulation
- control
- numerical integration

---
####  Coriolis & centrifugal terms
![[Pasted image 20260204084200.png]]

The important takeaway:
- They come from **velocity-dependent kinetic energy**
- They disappear if q˙​=0
- They matter at high speeds
You **do not derive these manually** in practice.  
Software (MATLAB, Maple, robotics toolboxes) handles this.

***Conceptually: If mass is moving and the geometry changes, extra forces appear.***
That’s all Coriolis is.

---
#### Underactuated System
![[Pasted image 20260204092231.png]]

This matters for **control**, not derivation.  
The model is the same — you just have fewer inputs.
![[Pasted image 20260202130859.png]]

---
#### Procedure
![[images/slide_072.png]]
![[Pasted image 20260204092326.png]]