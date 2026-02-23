![[Pasted image 20260216113405.png]]
We cannot analyze (easily) a non-linear system
linearization is done, so that we can design a controller for the system (using linear control theory tools)

Example:
Non-linear system ( bluexdot)
 ![[Pasted image 20260216113635.png]]
 Linear approximation of the system:
	  around point x = 1 => xdot = x - 0.5
	  around point x  = -2 => xdot = -2x - 2
 ![[Pasted image 20260216113703.png]]
 ![[Pasted image 20260216113742.png]]
system: Pendulum with friction
![[Pasted image 20260216113752.png]]
![[Pasted image 20260216113919.png]]


reduction --> rhs is (by the equilibrium definition) zero
![[Pasted image 20260216113954.png]]
xdot er vores lineære approksimation rundt om et equlibrium
![[Pasted image 20260216114602.png]]
f er en matrix af funktionerne f1 og f2, afhængigt af variablerne x1 og x2
![[Pasted image 20260216114443.png]]
 Derfor  skal vi bruge en jacobian for at bestemme xdot
![[Pasted image 20260216114450.png]]
![[Pasted image 20260216114430.png]]  
