# Euler–Lagrange Formulation and the Coriolis Matrix

This is the fifth and final note covering Chapter 2 of *Modelling and Control of Quadcopter* by Teppo Luukkonen. It derives the rotational equations of motion from the energy-based Euler–Lagrange framework, and presents the full Coriolis/gyroscopic matrix that captures the coupling between rotational axes.

**Navigation**
- Previous → [[Ch2_04_Newton_Euler_Dynamics]]
- You are here → **5. Euler–Lagrange & Coriolis**
- [[Ch2_00_Master|Return to master note]]

---

## Why an Alternative Derivation?

The Newton–Euler approach in [[Ch2_04_Newton_Euler_Dynamics]] derives the equations directly from force and torque balance. It is physically intuitive but requires careful bookkeeping of which frame each vector is expressed in.

The **Euler–Lagrange** approach works entirely from **energy**, which is a scalar quantity with no frame-dependence issues. It is more systematic for complex mechanical systems and naturally produces the **Coriolis and centripetal terms** in a structured matrix form that is useful for control design.

Both approaches yield **equivalent equations of motion** — they are two paths to the same destination. The choice between them is one of mathematical convenience.

---

## The Lagrangian

The Lagrangian $\mathcal{L}$ is defined as the difference between the kinetic energy $E_{\text{kin}}$ and the potential energy $E_{\text{pot}}$:

$$
\mathcal{L}(q, \dot{q}) = E_{\text{kin}} - E_{\text{pot}}
$$

For the quadrotor, kinetic energy has two contributions — translational and rotational:

$$
\mathcal{L}(q, \dot{q}) =
\underbrace{
  \underbrace{\frac{m}{2}\, \dot{\xi}^T \dot{\xi}}_{\text{translational}}
  +
  \underbrace{\frac{1}{2}\, \nu^T I\, \nu}_{\text{rotational}}
}_{\text{kinetic}}
-
\underbrace{mg z}_{\text{potential}}
$$

The translational kinetic energy is $\frac{1}{2}mv^2$ expressed in vector form. The rotational kinetic energy is the rotational analogue $\frac{1}{2}I\omega^2$, using the inertia matrix $I$ and the body angular velocity vector $\nu$. Potential energy is simply gravitational: $mgh = mgz$.

---

## The Euler–Lagrange Equation

The equations of motion follow from the Euler–Lagrange equation with external generalised forces $Q_i$:

$$
\frac{d}{dt}\left(\frac{\partial \mathcal{L}}{\partial \dot{q}_i}\right) - \frac{\partial \mathcal{L}}{\partial q_i} = Q_i
$$

For the quadrotor's generalised coordinates $q = [\xi^T,\ \eta^T]^T$, the external forces are the rotor thrust $f = RT_B$ and torques $\tau_B$.

Because the linear and angular coordinates do not couple through the kinetic energy (the cross terms vanish for this vehicle model), the translational and rotational parts can be treated separately.

---

## Translational Euler–Lagrange Equations

For the linear coordinates $\xi$, the Euler–Lagrange equation reproduces the same result as Newton's second law:

$$
f = R\, T_B = m\ddot{\xi} + mg
\begin{bmatrix}
0 \\ 0 \\ 1
\end{bmatrix}
$$

This is identical to Equation (10) in the paper and equivalent to the Newton–Euler translational equation from [[Ch2_04_Newton_Euler_Dynamics]].

---

## The Jacobian Matrix $J(\eta)$

For the rotational part, the Euler–Lagrange formulation introduces the **Jacobian matrix** $J(\eta)$, which maps body angular rates $\nu$ to Euler angle rates $\dot{\eta}$ in the context of rotational kinetic energy:

$$
J(\eta) = W_\eta^T\, I\, W_\eta
$$

Expanded:

$$
J(\eta) =
\begin{bmatrix}
I_{xx} & 0 & -I_{xx} S_\theta \\[6pt]
0 & I_{yy} C_\phi^2 + I_{zz} S_\phi^2 & (I_{yy} - I_{zz}) C_\phi S_\phi C_\theta \\[6pt]
-I_{xx} S_\theta & (I_{yy} - I_{zz}) C_\phi S_\phi C_\theta & I_{xx} S_\theta^2 + I_{yy} S_\phi^2 C_\theta^2 + I_{zz} C_\phi^2 C_\theta^2
\end{bmatrix}
$$

This matrix allows the rotational kinetic energy to be written in terms of Euler angle rates:

$$
E_{\text{rot}} = \frac{1}{2}\, \nu^T I\, \nu = \frac{1}{2}\, \dot{\eta}^T J(\eta)\, \dot{\eta}
$$

---

## Rotational Euler–Lagrange Equations

Applying the Euler–Lagrange equation to the angular coordinates $\eta$ gives:

$$
\tau_B = J(\eta)\, \ddot{\eta} + \frac{d}{dt}\bigl(J(\eta)\bigr)\, \dot{\eta} - \frac{1}{2}\frac{\partial}{\partial \eta}\left(\dot{\eta}^T J(\eta)\, \dot{\eta}\right)
$$

This can be written more compactly as:

$$
\tau_B = J(\eta)\, \ddot{\eta} + C(\eta, \dot{\eta})\, \dot{\eta}
$$

where $C(\eta, \dot{\eta})$ is the **Coriolis and centripetal matrix**, which contains all the angle- and rate-dependent coupling terms.

Solving for $\ddot{\eta}$:

$$
\ddot{\eta} = J^{-1}\bigl(\tau_B - C(\eta, \dot{\eta})\, \dot{\eta}\bigr)
$$

This is the fundamental equation of the rotational dynamics in the Euler–Lagrange formulation, equivalent to the Newton–Euler result in [[Ch2_04_Newton_Euler_Dynamics]].

---

## The Coriolis Matrix $C(\eta, \dot{\eta})$

The Coriolis matrix captures how the coupling between angular degrees of freedom creates apparent torques when the drone is both rotating and changing its orientation. If the drone were not rotating, $C = 0$ and each axis would be independent. The coupling terms become significant at high angular rates.

The matrix has the form:

$$
C(\eta, \dot{\eta}) =
\begin{bmatrix}
C_{11} & C_{12} & C_{13} \\[4pt]
C_{21} & C_{22} & C_{23} \\[4pt]
C_{31} & C_{32} & C_{33}
\end{bmatrix}
$$

with individual components:

$$
C_{11} = 0
$$

$$
C_{12} = (I_{yy} - I_{zz})\bigl(\dot{\theta}\, C_\phi S_\phi + \dot{\psi}\, S_\phi^2 C_\theta\bigr)
        + (I_{zz} - I_{yy})\, \dot{\psi}\, C_\phi^2 C_\theta
        - I_{xx}\, \dot{\psi}\, C_\theta
$$

$$
C_{13} = (I_{zz} - I_{yy})\, \dot{\psi}\, C_\phi S_\phi C_\theta^2
$$

$$
C_{21} = (I_{zz} - I_{yy})\bigl(\dot{\theta}\, C_\phi S_\phi + \dot{\psi}\, S_\phi C_\theta\bigr)
        + (I_{yy} - I_{zz})\, \dot{\psi}\, C_\phi^2 C_\theta
        + I_{xx}\, \dot{\psi}\, C_\theta
$$

$$
C_{22} = (I_{zz} - I_{yy})\, \dot{\phi}\, C_\phi S_\phi
$$

$$
C_{23} = -I_{xx}\, \dot{\psi}\, S_\theta C_\theta
        + I_{yy}\, \dot{\psi}\, S_\phi^2 S_\theta C_\theta
        + I_{zz}\, \dot{\psi}\, C_\phi^2 S_\theta C_\theta
$$

$$
C_{31} = (I_{yy} - I_{zz})\, \dot{\psi}\, C_\theta^2 S_\phi C_\phi
        - I_{xx}\, \dot{\theta}\, C_\theta
$$

$$
C_{32} = (I_{zz} - I_{yy})\bigl(\dot{\theta}\, C_\phi S_\phi S_\theta + \dot{\phi}\, S_\phi^2 C_\theta\bigr)
        + (I_{yy} - I_{zz})\, \dot{\phi}\, C_\phi^2 C_\theta \\
        \phantom{C_{32} =\ }
        + I_{xx}\, \dot{\psi}\, S_\theta C_\theta
        - I_{yy}\, \dot{\psi}\, S_\phi^2 S_\theta C_\theta
        - I_{zz}\, \dot{\psi}\, C_\phi^2 S_\theta C_\theta
$$

$$
C_{33} = (I_{yy} - I_{zz})\, \dot{\phi}\, C_\phi S_\phi C_\theta^2
        - I_{yy}\, \dot{\theta}\, S_\phi^2 C_\theta S_\theta
        - I_{zz}\, \dot{\theta}\, C_\phi^2 C_\theta S_\theta
        + I_{xx}\, \dot{\theta}\, C_\theta S_\theta
$$

### What Each Entry Represents

The matrix is not symmetric in general, and each entry mixes contributions from the three inertia values and the current angular rates. A few observations:

- **$C_{11} = 0$** always. The roll acceleration has no self-coupling through the Coriolis term (at first order).
- **Diagonal entries** $C_{11}$, $C_{22}$, $C_{33}$ — self-coupling of each axis with its own angular rate.
- **Off-diagonal entries** — cross-coupling between axes. For example, $C_{12}$ couples the $\dot{\theta}$ and $\dot{\psi}$ rates into the roll equation.
- When $I_{xx} = I_{yy}$ (as for the symmetric quadrotor), many cross terms partially simplify, but they do not vanish completely.
- At small angles and low angular rates, all Coriolis terms are small and can be neglected for a simplified linear model. The full matrix is required for accurate simulation at larger angles.

---

## Relationship to the Newton–Euler Formulation

The Newton–Euler rotational equation from [[Ch2_04_Newton_Euler_Dynamics]] was:

$$
I\dot{\nu} + \nu \times (I\nu) = \tau_B - \Gamma
$$

The Euler–Lagrange equation is:

$$
J(\eta)\, \ddot{\eta} + C(\eta, \dot{\eta})\, \dot{\eta} = \tau_B
$$

These two are **mathematically equivalent**. The Coriolis matrix $C(\eta, \dot{\eta})$ is the Euler–Lagrange counterpart of the cross-product term $\nu \times (I\nu)$ and the gyroscopic term $\Gamma$, expressed in terms of Euler angles rather than body rates. Converting between the two forms requires applying the $W_\eta$ transformation and its time derivative, as shown in the paper's Equation (12).

---

## Quick Reference

| Symbol | Meaning |
|--------|---------|
| $\mathcal{L}$ | Lagrangian = kinetic − potential energy |
| $J(\eta) = W_\eta^T I W_\eta$ | Rotational Jacobian matrix |
| $C(\eta, \dot{\eta})$ | Coriolis and centripetal matrix |
| $\ddot{\eta} = J^{-1}(\tau_B - C\dot{\eta})$ | Angular acceleration (Euler–Lagrange) |
| $S_x,\ C_x$ | $\sin(x),\ \cos(x)$ |

---

## Chapter 2 Complete

This note concludes the five-part series on Chapter 2. Together the five notes cover:

1. [[Ch2_01_Frames_and_State]] — Reference frames, state variables, and the inertia matrix
2. [[Ch2_02_Rotation_Matrix_and_Kinematics]] — The rotation matrix and angular velocity transformations
3. [[Ch2_03_Rotor_Forces_and_Torques]] — How rotor speeds produce thrust and torques
4. [[Ch2_04_Newton_Euler_Dynamics]] — Newton's laws applied to translational and rotational motion
5. **You are here** — Energy-based derivation and the full Coriolis matrix

The complete model derived across all five notes underpins the simulation and controller design presented in Chapters 3–5 of the paper.

→ [[Ch2_00_Master|Return to master note for the full consolidated view]]
