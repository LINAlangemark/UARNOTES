![[Pasted image 20260202130859.png]]

![[Pasted image 20260223110928.png]]
![[Pasted image 20260216125604.png]]
![[Pasted image 20260216125628.png]]

HUSK KØR DET HELE MED R23

Ryd op i simscape -->
	evt lav subsystems
	få styr på transformationer, rotationer og frames

opskriv lineariseret model --> også i simscape??
lav stabilittetsanalyse af modeller
tilføj friktion (er det egentligt bare aerodynamik?)
tilføj sensorer? --> til at bestemme observerbarhed og kontrollerbarhed

Hvordan ved jeg at inputtet demuxer rigtigt?


![[Pasted image 20260308114833.png]]Kinematics
↓
Dynamics (Newton-Euler intuition)
↓
Energi (Ekin, Epot)
↓
Lagrangian L = Ekin - Epot
↓
Euler–Lagrange ligninger
↓
Standardform
B(q)q¨ + C(q,q˙)q˙ + g(q) = τ
↓
Underactuated form
B(q)q¨ + C(q,q˙)q˙ + g(q) = G(q)u
↓
Rotor model
↓
State-space

![[Pasted image 20260309095100.png]]