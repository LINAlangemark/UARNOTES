Vi har et ulineært system (eksempel med linerarisering omkring et punkt)
![[Pasted image 20260302083618.png]]
**3 måder at lave kontrol på**
1: Linearization (for control) can produce instability
2: Non-linear control methods though, are often too complex to use and implement

3: The last approach is Feedback Linearization!
	Basically vil vi gerne udligne ulineariteterne med et feedback loop!
	![[Pasted image 20260302082549.png]]
Fra video/Hjemmeside: 
(https://aleksandarhaber.com/introduction-to-feedback-linearization/)
![[Pasted image 20260302095333.png]]

Når systemet får en step-respons
![[Pasted image 20260302101136.png]]

Når inputtet er feedback linearized control-logik
![[Pasted image 20260302101214.png]]

---
2 types of Feedback Linearization
![[Pasted image 20260302072007.png]]  
Video gennemgår:
Input - State Linearization
![[Pasted image 20260302072826.png]]

---

| Begreb                   | Betydning                              | Fysisk intuition                |
| ------------------------ | -------------------------------------- | ------------------------------- |
| Collocated               | Måler det man styrer                   | Speeder → hastighed             |
| Non-collocated           | Måler noget man ikke direkte styrer    | Styre albue via skulder         |
| Strong inertial coupling | Aktive led påvirker passive via inerti | Sving i kroppen flytter ben     |
| (M_{11}) invertible      | Ingen singularitet                     | Normal fysisk opførsel          |
| Eliminere nonlinearitet  | Gøre systemet lineært via feedback     | “Fjerne fysikken”               |
| Zero dynamics            | Skjult intern dynamik                  | Det systemet gør “bag kulissen” |
