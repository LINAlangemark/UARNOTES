# Newton–Euler Dynamics

This is the fourth of five notes covering Chapter 2 of *Modelling and Control of Quadcopter* by Teppo Luukkonen. It applies Newton's second law to the quadrotor as a rigid body, deriving the equations that govern how forces and torques produce translational and rotational accelerations. It also extends the translational model to include aerodynamic drag.

**Navigation**
- Previous → [[Ch2_03_Rotor_Forces_and_Torques]]
- You are here → **4. Newton–Euler Dynamics**
- Next → [[Ch2_05_Euler_Lagrange_and_Coriolis]]
- [[Ch2_00_Master|Return to master note]]

---

## The Rigid-Body Assumption

The paper assumes the quadrotor is a **rigid body** — it does not flex, vibrate, or deform during flight. This means that the relative positions of all mass elements are fixed, and the full motion of the vehicle can be described by just six coordinates: three for position and three for orientation. This is exactly the state vector $q$ defined in [[Ch2_01_Frames_and_State]].

The Newton–Euler equations are the direct application of Newton's second law to a rigid body:

- **Translational:** net force = mass × linear acceleration
- **Rotational:** net torque = moment of inertia × angular acceleration (with additional coupling terms)

---

## Translational Dynamics

### In the Inertial Frame

Gravity $G$ and the rotor thrust $R\, T_B$ (rotated from the body frame into the inertial frame) are the only forces acting on the drone (ignoring drag for now). Newton's second law gives:

$$
m\ddot{\xi} = G + R\, T_B
$$

Writing out the component form:

$$
\begin{bmatrix}
\ddot{x} \\[4pt]
\ddot{y} \\[4pt]
\ddot{z}
\end{bmatrix}
=
-g
\begin{bmatrix}
0 \\ 0 \\ 1
\end{bmatrix}
+
\frac{T}{m}
\begin{bmatrix}
C_\psi S_\theta C_\phi + S_\psi S_\phi \\[4pt]
S_\psi S_\theta C_\phi - C_\psi S_\phi \\[4pt]
C_\theta C_\phi
\end{bmatrix}
$$

The first column on the right is gravity pulling straight down. The second column is the thrust vector, which has been rotated by $R$ so that it reflects how much of the rotor force acts in each inertial direction depending on the drone's tilt.

**Key insight:** When the drone is perfectly level ($\phi = \theta = 0$), the thrust column simplifies to $[0,\ 0,\ 1]^T$ — all thrust is vertical. Any tilt redirects some thrust into a horizontal direction, producing horizontal acceleration.

### In the Body Frame

The same Newton's second law written in the rotating body frame requires an extra term — the **centrifugal force** $\nu \times (mV_B)$ — because the body frame is not inertial:

$$
m\dot{V}_B + \nu \times (m V_B) = R^T G + T_B
$$

The $\nu \times (mV_B)$ term arises mathematically from differentiating vectors in a rotating frame. Intuitively, it represents the apparent force felt by an observer riding on the drone due to the frame's rotation. This formulation is used when working with body-frame sensors and aerodynamic models.

---

## Rotational Dynamics

### Angular Momentum Equation

The rotational analogue of $F = ma$ for a rigid body about its centre of mass is:

$$
I\dot{\nu} + \nu \times (I\nu) + \Gamma = \tau_B
$$

where:
- $I\dot{\nu}$ — the torque needed to change the angular velocity
- $\nu \times (I\nu)$ — the **centripetal term**, a gyroscopic coupling that arises because different axes of the rotating body have different angular momenta
- $\Gamma$ — the **gyroscopic term** due to the spinning rotors themselves (see below)
- $\tau_B$ — the net external torque from the rotors

### The Gyroscopic Term

Each spinning rotor acts as a gyroscope. When the drone tilts, the gyroscopic effect of the rotor's angular momentum resists the tilting — this is the $\Gamma$ term. It is given by:

$$
\Gamma = I_r \left(\nu \times
\begin{bmatrix}
0 \\ 0 \\ 1
\end{bmatrix}
\right) \omega_\Gamma
$$

where $I_r$ is the rotor moment of inertia and $\omega_\Gamma$ is the **net rotor speed**:

$$
\omega_\Gamma = \omega_1 - \omega_2 + \omega_3 - \omega_4
$$

The alternating signs reflect the alternating spin directions of the rotors. During hover with equal speeds, $\omega_\Gamma = 0$ and the gyroscopic term vanishes.

### Expanded Component Form

Solving $I\dot{\nu} = \tau_B - \nu \times (I\nu) - \Gamma$ for each component using the diagonal inertia matrix $I = \text{diag}(I_{xx}, I_{yy}, I_{zz})$:

$$
\begin{bmatrix}
\dot{p} \\[4pt]
\dot{q} \\[4pt]
\dot{r}
\end{bmatrix}
=
\begin{bmatrix}
\dfrac{(I_{yy} - I_{zz})\, q\, r}{I_{xx}} \\[10pt]
\dfrac{(I_{zz} - I_{xx})\, p\, r}{I_{yy}} \\[10pt]
\dfrac{(I_{xx} - I_{yy})\, p\, q}{I_{zz}}
\end{bmatrix}
- I_r
\begin{bmatrix}
q / I_{xx} \\[4pt]
-p / I_{yy} \\[4pt]
0
\end{bmatrix}
\omega_\Gamma
+
\begin{bmatrix}
\tau_\phi / I_{xx} \\[4pt]
\tau_\theta / I_{yy} \\[4pt]
\tau_\psi / I_{zz}
\end{bmatrix}
$$

The first column contains the **centripetal coupling terms** — they are zero only if the drone is not rotating, or if $I_{xx} = I_{yy} = I_{zz}$. For the quadrotor, $I_{xx} = I_{yy} \neq I_{zz}$, so coupling between roll/pitch and yaw exists but coupling between roll and pitch does not.

The second column contains the **gyroscopic correction** from the rotor inertia.

The third column is the direct effect of the applied torques divided by the relevant moment of inertia.

### Converting Body Accelerations to Euler Angle Accelerations

The equations above give $\dot{\nu} = [\dot{p},\ \dot{q},\ \dot{r}]^T$ — the rate of change of body angular rates. To integrate these into Euler angle positions ($\phi$, $\theta$, $\psi$), we need to convert back through $W_\eta^{-1}$:

$$
\ddot{\eta} \quad = \quad \frac{d}{dt}\left(W_\eta^{-1}\, \nu\right) \quad = \quad \frac{d}{dt}\left(W_\eta^{-1}\right)\nu + W_\eta^{-1}\, \dot{\nu}
$$

The first term on the right accounts for the fact that $W_\eta^{-1}$ itself changes over time as the Euler angles change. The full expanded form is given in the paper (Equation 12) and is reproduced in the master note [[Ch2_00_Master]].

---

## Aerodynamic Drag

The basic model above ignores air resistance. To make the simulation more realistic, the paper adds a **linear drag force** opposing translational velocity:

$$
\begin{bmatrix}
\ddot{x} \\[4pt]
\ddot{y} \\[4pt]
\ddot{z}
\end{bmatrix}
=
-g
\begin{bmatrix}
0 \\ 0 \\ 1
\end{bmatrix}
+
\frac{T}{m}
\begin{bmatrix}
C_\psi S_\theta C_\phi + S_\psi S_\phi \\[4pt]
S_\psi S_\theta C_\phi - C_\psi S_\phi \\[4pt]
C_\theta C_\phi
\end{bmatrix}
-
\frac{1}{m}
\begin{bmatrix}
A_x & 0 & 0 \\
0 & A_y & 0 \\
0 & 0 & A_z
\end{bmatrix}
\begin{bmatrix}
\dot{x} \\[4pt]
\dot{y} \\[4pt]
\dot{z}
\end{bmatrix}
$$

Here $A_x$, $A_y$, $A_z$ are the **drag coefficients** (units: kg/s) for each inertial-frame direction. These are purely phenomenological — they do not come from a detailed aerodynamic model but are chosen so that the drone decelerates realistically when thrust is removed. In the paper's simulation, $A_x = A_y = A_z = 0.25\ \text{kg/s}$.

More complex aerodynamic effects — blade flapping, angle-of-attack dependence, rotor-wake interactions — are explicitly excluded from this model. They would be important at high speeds, but for near-hover flight the simple drag model is adequate.

---

## Summary of the Dynamic Equations

The complete Newton–Euler model consists of two coupled sets of differential equations:

**Translational (inertial frame, with drag):**

$$
m\ddot{\xi} = G + R\, T_B - A\, \dot{\xi}
$$

where $A = \text{diag}(A_x, A_y, A_z)$.

**Rotational (body frame):**

$$
I\dot{\nu} = -\nu \times (I\nu) - \Gamma + \tau_B
$$

These equations, together with the kinematic relations $\dot{\xi}$ integrated to $\xi$ and $\dot{\eta} = W_\eta^{-1}\nu$ integrated to $\eta$, form the **complete 12-state simulation model** of the quadrotor: six state positions $[\xi, \eta]$ and six velocities $[\dot{\xi}, \nu]$.

---

## Quick Reference

| Symbol | Meaning |
|--------|---------|
| $m$ | Total mass of quadrotor |
| $T$ | Total thrust (scalar) |
| $T_B = [0,\ 0,\ T]^T$ | Thrust in body frame |
| $G = [0,\ 0,\ -g]^T$ | Gravity in inertial frame |
| $\nu = [p,\ q,\ r]^T$ | Body angular rates |
| $\omega_\Gamma = \omega_1 - \omega_2 + \omega_3 - \omega_4$ | Net rotor speed (gyroscopic) |
| $I_r$ | Rotor moment of inertia |
| $A_x, A_y, A_z$ | Translational drag coefficients |
| $\tau_B = [\tau_\phi,\ \tau_\theta,\ \tau_\psi]^T$ | Body torque vector |

---

## Where This Leads

The Newton–Euler formulation gives the dynamics directly from force and torque balance. The Euler–Lagrange formulation derives the same equations from an energy perspective and yields the Coriolis matrix used in the rotational dynamics. That alternative derivation, and its relationship to the equations above, is the topic of the final note:

→ [[Ch2_05_Euler_Lagrange_and_Coriolis]]
