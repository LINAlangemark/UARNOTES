# Chapter 2 – Mathematical Model of the Quadcopter

This note contains the complete mathematical model of the quadrotor as derived in Chapter 2 of *Modelling and Control of Quadcopter* by Teppo Luukkonen (Aalto University, 2011). It combines the executive summary with the extended derivations, merging explanations for readers without a prior background in rigid-body dynamics.

**Split notes for focused reading:**
- [[Ch2_01_Frames_and_State]]
- [[Ch2_02_Rotation_Matrix_and_Kinematics]]
- [[Ch2_03_Rotor_Forces_and_Torques]]
- [[Ch2_04_Newton_Euler_Dynamics]]
- [[Ch2_05_Euler_Lagrange_and_Coriolis]]

---

## Overview

A quadrotor is a helicopter with four rotors arranged in a symmetric cross formation, each driven by an electric motor. Control is achieved entirely by adjusting the four rotor speeds — there are no swashplates, tilting rotors, or other mechanical control surfaces. This simplicity in hardware comes at the cost of complexity in software: the controller must coordinate four inputs to manage six degrees of freedom simultaneously.

Chapter 2 builds the mathematical model needed to simulate and control this system. It defines the coordinate frames, state variables, kinematic transformations, rotor force laws, and the full rigid-body equations of motion. Two equivalent derivations are presented — one via Newton–Euler force balance, one via Euler–Lagrange energy methods — and their relationship is established.

---

# Coordinate Frames and State Variables

## Why Two Frames?

The quadrotor moves through the world but also rotates. Quantities tied to the Earth — position, gravity, target waypoints — are most naturally described in a fixed world frame. Quantities tied to the drone's own body — rotor thrust direction, angular spin rates, aerodynamic forces — are most naturally described in a body-fixed frame. Using both keeps each equation as clean as possible. The rotation matrix (introduced below) translates between them.

---

## The Inertial Frame

A fixed, Earth-centred coordinate system with orthogonal axes $x$, $y$, $z$. Newton's laws hold directly in this frame without correction terms. The quadrotor's absolute position is:

$$
\xi =
\begin{bmatrix}
x \\
y \\
z
\end{bmatrix}
$$

Gravity always acts along the negative inertial $z$-axis:

$$
G =
\begin{bmatrix}
0 \\
0 \\
-g
\end{bmatrix}
$$

where $g = 9.81\ \text{m/s}^2$. Expressing gravity in the inertial frame gives a constant vector — a significant simplification for the equations of motion.

---

## The Body Frame

A coordinate system whose origin is at the quadrotor's centre of mass and whose axes $x_B$, $y_B$, $z_B$ rotate with the vehicle. The $z_B$-axis points upward through the drone, in the direction the rotors push air. If the drone tilts, $z_B$ tilts with it — this is how horizontal motion is produced.

The orientation of the body frame relative to the inertial frame is described by three **Euler angles**:

$$
\eta =
\begin{bmatrix}
\phi \\
\theta \\
\psi
\end{bmatrix}
$$

| Symbol | Name | Rotation axis | Physical meaning |
|--------|------|---------------|-----------------|
| $\phi$ | Roll | $x_B$ | Tilting left/right |
| $\theta$ | Pitch | $y_B$ | Tilting forward/backward |
| $\psi$ | Yaw | $z_B$ | Rotating the heading |

In drone terminology, *attitude* refers specifically to $(\phi, \theta, \psi)$ — orientation relative to the horizon, not position in space. Flight controllers in "attitude mode" stabilise these three angles directly.

---

## Full State Vector

Position and attitude are combined into the **state vector**:

$$
q =
\begin{bmatrix}
\xi \\
\eta
\end{bmatrix}
=
\begin{bmatrix}
x \\
y \\
z \\
\phi \\
\theta \\
\psi
\end{bmatrix}
$$

This six-component vector fully defines the configuration of the quadrotor at any instant. Since there are six independent coordinates, the vehicle has **six degrees of freedom (6-DOF)**. However, it has only **four control inputs** (rotor speeds), making it an *underactuated* system.

---

# Velocities

## Linear Velocity in the Body Frame

$$
V_B =
\begin{bmatrix}
v_{x,B} \\
v_{y,B} \\
v_{z,B}
\end{bmatrix}
$$

Velocity components along the drone's own axes. For example, $v_{x,B}$ is the speed in the direction the nose is pointing. Aerodynamic forces depend on how the drone moves through the air relative to its own orientation, so the body frame is the natural place for these.

---

## Angular Velocity — Body Rates

$$
\nu =
\begin{bmatrix}
p \\
q \\
r
\end{bmatrix}
$$

Physical rotational velocities about the body axes, measured directly by gyroscopes:

- $p$ — roll rate (spinning about $x_B$)
- $q$ — pitch rate (spinning about $y_B$)
- $r$ — yaw rate (spinning about $z_B$)

These are **not** the same as the time derivatives of the Euler angles — a distinction that becomes important in the kinematic transformations below.

---

## Inertia Matrix

The quadrotor's symmetric structure means the inertia tensor is diagonal (cross-products of inertia vanish):

$$
I =
\begin{bmatrix}
I_{xx} & 0 & 0 \\
0 & I_{yy} & 0 \\
0 & 0 & I_{zz}
\end{bmatrix}
$$

Because the vehicle is symmetric about both $x_B$ and $y_B$:

$$
I_{xx} = I_{yy}
$$

The diagonal entries describe resistance to angular acceleration about each axis. Larger values mean more torque is needed to achieve the same angular acceleration. 

---

# Rotation Matrix

The rotation matrix $R$ transforms body-frame vectors into inertial-frame vectors. It is constructed by composing three elementary rotations in the order roll–pitch–yaw:

$$
R = R_z(\psi)\, R_y(\theta)\, R_x(\phi)
$$

The full $3 \times 3$ matrix is:

$$
R =
\begin{bmatrix}
C_\psi C_\theta &
C_\psi S_\theta S_\phi - S_\psi C_\phi &
C_\psi S_\theta C_\phi + S_\psi S_\phi \\[6pt]
S_\psi C_\theta &
S_\psi S_\theta S_\phi + C_\psi C_\phi &
S_\psi S_\theta C_\phi - C_\psi S_\phi \\[6pt]
-S_\theta &
C_\theta S_\phi &
C_\theta C_\phi
\end{bmatrix}
$$

where $S_x = \sin(x)$, $C_x = \cos(x)$.

The matrix is **orthogonal**: $R^{-1} = R^T$. This means the inverse (transforming inertial → body) is simply the transpose — a free computation.

Velocity transformation: $V_I = R\, V_B$.

---

# Euler Angle Rates and Body Rates

Euler angle rates $\dot{\eta} = [\dot{\phi},\ \dot{\theta},\ \dot{\psi}]^T$ and body angular rates $\nu = [p,\ q,\ r]^T$ are different quantities. Euler angles are sequential rotations; body rates are the physical spin measured in the body frame. They are related through the transformation matrix $W_\eta$:

$$
\nu = W_\eta\, \dot{\eta}, \qquad \dot{\eta} = W_\eta^{-1}\, \nu
$$

### From Body Rates to Euler Rates

$$
\begin{bmatrix}
\dot{\phi} \\[4pt]
\dot{\theta} \\[4pt]
\dot{\psi}
\end{bmatrix}
=
\begin{bmatrix}
1 & S_\phi T_\theta & C_\phi T_\theta \\[4pt]
0 & C_\phi & -S_\phi \\[4pt]
0 & \dfrac{S_\phi}{C_\theta} & \dfrac{C_\phi}{C_\theta}
\end{bmatrix}
\begin{bmatrix}
p \\[4pt]
q \\[4pt]
r
\end{bmatrix}
$$

### From Euler Rates to Body Rates

$$
\begin{bmatrix}
p \\[4pt]
q \\[4pt]
r
\end{bmatrix}
=
\begin{bmatrix}
1 & 0 & -S_\theta \\[4pt]
0 & C_\phi & C_\theta S_\phi \\[4pt]
0 & -S_\phi & C_\theta C_\phi
\end{bmatrix}
\begin{bmatrix}
\dot{\phi} \\[4pt]
\dot{\theta} \\[4pt]
\dot{\psi}
\end{bmatrix}
$$

where $T_x = \tan(x)$. The transformation is singular when $\cos\theta = 0$ (pitch of $\pm 90°$), a phenomenon known as **gimbal lock**. This model is valid for flight near hover where $|\theta| \ll 90°$.

---

# Rotor Forces and Torques

## Rotor Thrust

Each rotor spinning at angular velocity $\omega_i$ produces thrust:

$$
f_i = k\, \omega_i^2
$$

The quadratic dependence on $\omega_i$ follows from aerodynamic lift theory. The lift coefficient $k$ depends on blade geometry and air density. All four thrusts act along the body $z_B$-axis; the total thrust is:

$$
T = \sum_{i=1}^{4} f_i = k\sum_{i=1}^{4}\omega_i^2, \qquad
T_B =
\begin{bmatrix}
0 \\ 0 \\ T
\end{bmatrix}
$$

## Rotor Drag Torque

A spinning rotor also experiences aerodynamic drag opposing its rotation. By Newton's third law, this produces a reaction torque on the drone body:

$$
\tau_{M_i} = b\, \omega_i^2 + I_M\, \dot{\omega}_i
$$

where $b$ is the drag coefficient and $I_M$ is the rotor moment of inertia. The $I_M\dot{\omega}_i$ term is typically neglected as rotor angular acceleration is small.

## Body Torques

Let $l$ be the distance from the centre of mass to each rotor. Torques are generated by *differences* in rotor speeds, not their sum:

$$
\tau_\phi = l\, k\left(-\omega_2^2 + \omega_4^2\right)
$$

$$
\tau_\theta = l\, k\left(-\omega_1^2 + \omega_3^2\right)
$$

$$
\tau_\psi = \sum_{i=1}^{4} \tau_{M_i}
$$

Combined:

$$
\tau_B =
\begin{bmatrix}
\tau_\phi \\[4pt]
\tau_\theta \\[4pt]
\tau_\psi
\end{bmatrix}
=
\begin{bmatrix}
l\, k\left(-\omega_2^2 + \omega_4^2\right) \\[4pt]
l\, k\left(-\omega_1^2 + \omega_3^2\right) \\[4pt]
\displaystyle\sum_{i=1}^{4} \tau_{M_i}
\end{bmatrix}
$$

Rotors 1 and 3 spin opposite to rotors 2 and 4 so that their drag torques cancel during hover. Yaw is controlled by making one diagonal pair faster than the other, creating a net $\tau_\psi$.

---

# Newton–Euler Dynamics

## Translational Motion (Inertial Frame)

$$
m\ddot{\xi} = G + R\, T_B
$$

Component form:

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

When the drone is level ($\phi = \theta = 0$), all thrust is vertical. Any tilt redirects thrust horizontally, producing lateral acceleration.

## Translational Motion (Body Frame)

$$
m\dot{V}_B + \nu \times (m V_B) = R^T G + T_B
$$

The centrifugal term $\nu \times (mV_B)$ arises because the body frame is rotating. The inertial-frame gravity $G$ must be rotated into the body frame via $R^T$.

## Rotational Motion

$$
I\dot{\nu} + \nu \times (I\nu) + \Gamma = \tau_B
$$

Expanded component form:

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

where the net rotor speed (gyroscopic term) is:

$$
\omega_\Gamma = \omega_1 - \omega_2 + \omega_3 - \omega_4
$$

Angular accelerations in the inertial frame are then obtained via:

$$
\ddot{\eta} = \frac{d}{dt}\!\left(W_\eta^{-1}\nu\right) = \frac{d}{dt}\!\left(W_\eta^{-1}\right)\nu + W_\eta^{-1}\dot{\nu}
$$

## Aerodynamic Drag

A linear drag model is added to the translational equations:

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

where $A_x = A_y = A_z = 0.25\ \text{kg/s}$ in the simulation. More complex aerodynamic effects (blade flapping, angle-of-attack dependence) are excluded as they are significant only at high speeds.

---

# Euler–Lagrange Formulation

## The Lagrangian

$$
\mathcal{L}(q, \dot{q}) = \frac{m}{2}\, \dot{\xi}^T \dot{\xi} + \frac{1}{2}\, \nu^T I\, \nu - mgz
$$

The Euler–Lagrange equation with external generalised forces:

$$
\frac{d}{dt}\!\left(\frac{\partial \mathcal{L}}{\partial \dot{q}_i}\right) - \frac{\partial \mathcal{L}}{\partial q_i} = Q_i
$$

For the linear coordinates this recovers the same translational equation as Newton–Euler. For the angular coordinates it gives:

$$
\tau_B = J(\eta)\, \ddot{\eta} + C(\eta, \dot{\eta})\, \dot{\eta}
$$

where $J(\eta) = W_\eta^T I W_\eta$ is the rotational Jacobian matrix:

$$
J(\eta) =
\begin{bmatrix}
I_{xx} & 0 & -I_{xx} S_\theta \\[6pt]
0 & I_{yy} C_\phi^2 + I_{zz} S_\phi^2 & (I_{yy} - I_{zz}) C_\phi S_\phi C_\theta \\[6pt]
-I_{xx} S_\theta & (I_{yy} - I_{zz}) C_\phi S_\phi C_\theta & I_{xx} S_\theta^2 + I_{yy} S_\phi^2 C_\theta^2 + I_{zz} C_\phi^2 C_\theta^2
\end{bmatrix}
$$

Solving for angular acceleration:

$$
\ddot{\eta} = J^{-1}\bigl(\tau_B - C(\eta, \dot{\eta})\, \dot{\eta}\bigr)
$$

---

# Coriolis Matrix $C(\eta, \dot{\eta})$

The Coriolis matrix encodes the coupling between axes that arises from the quadrotor's rotation. It appears in the Euler–Lagrange rotational equation and is the structured counterpart of the $\nu \times (I\nu)$ cross-product term in the Newton–Euler equations. At small angles and low rates, all entries are negligible; the full matrix is required for accurate simulation at larger angles.

$$
C(\eta, \dot{\eta}) =
\begin{bmatrix}
C_{11} & C_{12} & C_{13} \\[4pt]
C_{21} & C_{22} & C_{23} \\[4pt]
C_{31} & C_{32} & C_{33}
\end{bmatrix}
$$

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

---

# Notation Cheat Sheet

| Symbol | Meaning | Units |
|--------|---------|-------|
| $\xi = [x,\ y,\ z]^T$ | Position in inertial frame | m |
| $\eta = [\phi,\ \theta,\ \psi]^T$ | Euler angles (roll, pitch, yaw) | rad |
| $q = [\xi^T,\ \eta^T]^T$ | Full state vector | — |
| $V_B$ | Linear velocity in body frame | m/s |
| $\nu = [p,\ q,\ r]^T$ | Body angular rates | rad/s |
| $R$ | Rotation matrix (body → inertial) | — |
| $W_\eta$ | Angular rate transformation | — |
| $I$ | Inertia tensor | kg·m² |
| $m$ | Total mass | kg |
| $g$ | Gravitational acceleration ($9.81\ \text{m/s}^2$) | m/s² |
| $l$ | Rotor distance from centre of mass | m |
| $k$ | Thrust (lift) coefficient | N·s²/rad² |
| $b$ | Drag coefficient | N·m·s²/rad² |
| $f_i = k\omega_i^2$ | Thrust of rotor $i$ | N |
| $T = \sum f_i$ | Total thrust | N |
| $\tau_{M_i} = b\omega_i^2$ | Drag torque of rotor $i$ | N·m |
| $\tau_\phi,\ \tau_\theta,\ \tau_\psi$ | Roll, pitch, yaw torques | N·m |
| $I_M$ | Rotor moment of inertia | kg·m² |
| $\omega_\Gamma = \omega_1 - \omega_2 + \omega_3 - \omega_4$ | Net rotor speed | rad/s |
| $A_x, A_y, A_z$ | Translational drag coefficients | kg/s |
| $J(\eta)$ | Rotational Jacobian matrix | kg·m² |
| $C(\eta, \dot{\eta})$ | Coriolis matrix | kg·m²/s |
| $S_x,\ C_x,\ T_x$ | $\sin(x),\ \cos(x),\ \tan(x)$ | — |
