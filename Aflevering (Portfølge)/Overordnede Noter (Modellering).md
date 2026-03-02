Euler-Lagrange er brugt fordi i systemer hvor der er rigtigt mange krafter at modellere, er Newton-modellen alt for kompliceret

Euler-Lagrange kan modellere et system ud fra mængden af potentiel og kinetisk energi på et givent tidspunkt --> som kan give os hastighed, acceleration, position

Forward Kinematics kan findes fra modellen?

Linearisering bliver brugt, så vi kan designe en controller til systemet

Simulink bliver brugt som en sekundær metode til at verificere at vores modellering er korrekt

![[Pasted image 20260223075402.png]]Modellen inddeles yderligere i en "hierarkisk struktur"
![[Pasted image 20260223075536.png]]Parrot eksemplet kan åbnes fra help>examples>søg på "quadcoptor" også åben livescript
eksemplet kan åbnes i simulink med: openProject('asbQuadcopter');
Bruger 6DOF (Euler Angles) for nu --> Må man bruge Multirotor modellen?


Control Architecture?
![[Pasted image 20260223082849.png]]

Denne blok tager Forces (Translational) og Moments (Rotational) som input
![[Pasted image 20260223084708.png]]

![[Pasted image 20260223084829.png]]

Gravity skal KUN håndteres et sted...

ENU --> East, North, Up --> rotation relativt jorden
![[Pasted image 20260223085010.png]]

![[Pasted image 20260223085212.png]]
Ligesom at Cart og Pole blev delt op??

Så det her er åbenbart noget lort
![[Pasted image 20260223091758.png]]Moment of inertia er dobbelt så stort på z-akse
![[Pasted image 20260223112900.png]]
![[Pasted image 20260223112910.png]]
