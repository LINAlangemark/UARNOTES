Acrobot (upper actuated):
![[Pasted image 20260302075701.png]]
"It has long been known [16] that fully actuated robots are feedback linearizable by nonlinear feedback. For underactuated robots it is known that the portion of the dynamics corresponding to the actuated (or ac- tive) degrees of freedom may be linearized by nonlinear feedback"
	"The remaining portion of the dynamics after such partial feedback linearization is nonlinear and represents internal dynamics."
![[Pasted image 20260302080616.png]]
![[Pasted image 20260302080635.png]]
![[Pasted image 20260302080659.png]]

![[Pasted image 20260302080809.png]]

At output og input er **collocated** betyder, at de er koblet til **de samme led (joints)**.
- τ = aktiveringsmomentet (input) → virker på de **aktive joints*
- y2=q2​ → positionerne for de **aktive joints**
Altså: Vi måler præcis det, vi direkte styrer.
Når output er collocated med input:
- Systemet opfører sig "pænt"
- Standard feedback linearization virker direkte
- Ingen skjult dynamik (zero dynamics) skaber problemer

**Non-collocated control**
“Output corresponds to the passive joints and is not collocated with the input”
- Input τ virker på **aktive joints**
- Output y1=q1 er de **passive joints**
Altså: Vi prøver at styre noget, vi **ikke direkte kan påvirke**

Og her opstår problemer:
- Systemet bliver **underactuated**
- Der opstår **internal/zero dynamics**
- Almindelig feedback linearization virker ikke direkte


**“Strong Inertial Coupling”**
Det betyder, at bevægelsen i de aktive joints **automatisk påvirker** de passive joints via massetræghed.
![[Pasted image 20260302081728.png]]

---
![[Pasted image 20260302081906.png]]

![[Pasted image 20260302082847.png]]
![[Pasted image 20260302081828.png]]
![[Pasted image 20260302082549.png]]


![[Pasted image 20260302082622.png]]

| Begreb                   | Betydning                              | Fysisk intuition                |
| ------------------------ | -------------------------------------- | ------------------------------- |
| Collocated               | Måler det man styrer                   | Speeder → hastighed             |
| Non-collocated           | Måler noget man ikke direkte styrer    | Styre albue via skulder         |
| Strong inertial coupling | Aktive led påvirker passive via inerti | Sving i kroppen flytter ben     |
| (M_{11}) invertible      | Ingen singularitet                     | Normal fysisk opførsel          |
| Eliminere nonlinearitet  | Gøre systemet lineært via feedback     | “Fjerne fysikken”               |
| Zero dynamics            | Skjult intern dynamik                  | Det systemet gør “bag kulissen” |

---
**Non-collocated Input/Output Linearization**
In this section we show, under a condition regarding the degree of coupling between the active and passivejoints, that instead of linearizing the active degrees of freedom 42, we may linearize the passive degrees of freedom q1 by nonlinear feedback. This result can be thought of as a combination of partial feedback lin- earization with the method of integrator backstepping.



