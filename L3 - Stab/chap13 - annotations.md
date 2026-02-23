> ([[chap13_lypanov_stability.pdf#page=2&selection=21,0,151,8&color=important|p.2]])
> We have already seen some examples of both stable and unstable systems. The objective of this chapter is to formalize the notion of internal stability for general nonlinear state-space models. Apart from defining the various notions of stability, we define an entity known as a Lyapunov function and relate it to these various stability notions.


[[chap13_lypanov_stability.pdf#page=2&selection=153,0,160,1&color=note|p.2]]
![[Pasted image 20260216082959.png]]


Vi siger, at et punkt $\bar{x}$ er et **ligevægtspunkt** fra tiden $t_0$ for det kontinuerte system (CT), 
	hvis: $f(\bar{x}, t) = 0, \quad \forall\, t \ge t_0.$

Vi siger, at $\bar{x}$ er et **ligevægtspunkt** fra tiden $k_0$ for det diskrete system (DT)
	hvis: $f(\bar{x}, k) = \bar{x}, \quad \forall\, k \ge k_0$

Hvis systemet starter i tilstanden $\bar{x}$ ved tiden $t_0$ (for CT) eller $k_0$ (for DT), vil det forblive i denne tilstand for alle fremtidige tidspunkter.

> ([[chap13_lypanov_stability.pdf#page=2&selection=615,0,745,2&color=note|p.2]])
> The most fruitful notion of stability for an equilibrium point of a nonlinear system is given by the definition below. We shall assume that the equilibrium point of interest is at the origin, since if x_bar =/= 0, a simple translation can always be applied to obtain an equivalent system with the equilibrium at 0.
![[Pasted image 20260216083441.png]]
> ([[chap13_lypanov_stability.pdf#page=3&selection=176,0,435,1&color=note|p.3]])
> The first condition requires that the state trajectory can be confined to an arbitrarily small "ball" centered at the equilibrium point and of radius eta, when released from an arbitrary initial condition in a ball of suficiently small (but positive) radius delta_1 . This is called stability in the sense of Lyapunov (i.s.L.). 
> 
> It is possible to have stability in the sense of Lyapunov without having asymptotic stability, in which case we refer to the equilibrium point as marginally stable. Nonlinear systems also exist that satisfy the second requirement without being stable i.s.L., as the following example shows. An equilibrium point that is not stable i.s.L. is termed unstable
![[Pasted image 20260216083824.png]]

Radius r er givet ved: r = sqrt(x₁² + x₂²)
Vinklen θ er givet ved: 0 ≤ θ = arctan(x₂ / x₁) ≤ 2π
Det er let at se, at systemet har præcis to ligevægtspunkter:
- Ét i origo
- Ét ved r = 1 og θ = 0
![[Pasted image 20260216084104.png]]
Alle trajektorier ender i punktet: r = 1, θ = 0
Dette ligevægtspunkt er dog **ikke stabilt i.s.L.** (i stabil i Lyapunovs forstand).
Grunden er, at trajektorierne ikke kan holdes inde i en vilkårligt lille kugle omkring ligevægtspunktet, når de starter fra vilkårlige punkter i en (uanset hvor lille) omegn af dette punkt.

