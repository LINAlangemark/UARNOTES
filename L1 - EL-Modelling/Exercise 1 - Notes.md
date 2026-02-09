![[Pasted image 20260202081013.png]]
![[Pasted image 20260204100504.png]]
## (1) Coordinates/Kinematics
![[Pasted image 20260204101655.png]]

så xc er "pivot-point" for pole, men også det punkt vi definere til at være det som flytter sig på cart
Finder Center of Mass for hhv cart og pole
![[Pasted image 20260204101717.png]]
![[Pasted image 20260204085320.png]]

---
### Vi kan nu finde hastighed via Jacobian
![[Pasted image 20260204101903.png]]
Vi differentiere positionerne mht q og samler det på matrix form
![[Pasted image 20260204101944.png]]

![[Pasted image 20260204085336.png]]
![[Pasted image 20260204103041.png]]
*Her der tager vi jakobianten af positionerne (differentiere mht q på matrix form)*
![[Pasted image 20260204110807.png]]

---
## **Potentiel energi**
***“Konstante energiled udelades, da de ikke påvirker Euler–Lagrange-ligningerne.”***

![[Pasted image 20260204102525.png]]

**Tyngdeaccelerations-vektoren:** peger i tyngdefeltets retning (0x, -y)
![[Pasted image 20260204102400.png]]
 **Cartens center of mass ændre ikke højde** — og gravitationel potentiel energi afhænger _kun_ af højde. --> så den potentielle energi for cart = 0
![[Pasted image 20260204102850.png]]
V: potentiel energi
cos(theta) fordi det er y-komposanten, som er den interresante for gravitationel potentiel energi (højde-afhængig)
![[Pasted image 20260204104606.png]]
Hastighederne kan findes ved:
![[Pasted image 20260204103208.png]]

(bemærk dx + dtheta l cos(theta) --> når vi så smider -g ind, så bliver det negativt, og den nederste positiv)
![[Pasted image 20260204102840.png]]

![[Pasted image 20260204103507.png]]

Potentiel energi fundet i matlab for y-koordinaten
![[Pasted image 20260204103519.png]]

---
## **Kinetisk Energi**

![[Pasted image 20260204102136.png]]

![[Pasted image 20260204104924.png]]
I matlab findes deb kinetiske energi ved:
![[Pasted image 20260204103423.png]]

---
### Så findes Lagrangian
![[Pasted image 20260204085447.png]]
![[Pasted image 20260204103453.png]]
På tavlen:
![[Pasted image 20260204085549.png]]
I matlab:
![[Pasted image 20260204103530.png]]

---
**Q-matricen**
![[Pasted image 20260204110930.png]]
![[Pasted image 20260204103804.png]]
qd --> qdot
dLdqd:
![[Pasted image 20260204105256.png]]
dldq:
![[Pasted image 20260204105315.png]]
![[Pasted image 20260204103848.png]]

ddt_dLdqd:
![[Pasted image 20260204105539.png]]

---
![[Pasted image 20260204105710.png]]
![[Pasted image 20260204105642.png]]

### MED INPUT (tavle)
![[Pasted image 20260204085601.png]]

![[Pasted image 20260204105428.png]]

---
**OMSKRIVNING**
![[Pasted image 20260204105944.png]]
![[Pasted image 20260204105808.png]]
![[Pasted image 20260204111046.png]]
![[Pasted image 20260204105827.png]]

---
Euler Lagrange:
![[Pasted image 20260204110222.png]]
I Matlab direkte:

![[Pasted image 20260204110242.png]]
Samler led...
![[Pasted image 20260204110322.png]]
![[Pasted image 20260204110418.png]]
![[Pasted image 20260204110347.png]]

rhs (right hand side)
![[Pasted image 20260204110409.png]]


![[Pasted image 20260204085632.png]]

![[Pasted image 20260204085650.png]]


**Løser for acceleration af cart og pendul**

![[Pasted image 20260204104008.png]]
![[Pasted image 20260204110451.png]]


ddx_sol = cart acceleration
ddth_sol = pendul acceleration

---
Simulation????