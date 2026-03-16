# Coordinate Frames and State Variables

This is the first of five notes covering Chapter 2 of *Modelling and Control of Quadcopter* by Teppo Luukkonen. It introduces the two reference frames used throughout the model, the state variables that describe where the quadrotor is and how it is oriented, and the velocity quantities used in later dynamics.

**Navigation**
- You are here → **1. Frames & State**
- Next → [[Ch2_02_Rotation_Matrix_and_Kinematics]]
- [[Ch2_00_Master|Return to master note]]

---

## Why Two Frames?

A quadrotor moves through space, but it also rotates. These two aspects of motion are most naturally described in *different* reference frames:

- Quantities that are fixed relative to the Earth — such as position, gravity, and the target destination — are simplest to express in a **world-fixed frame**.
- Quantities that are fixed relative to the drone's own body — such as the direction of its rotors, the forces they produce, and its spin rates — are simplest to express in a **body-fixed frame**.

Having both frames is not a complication; it is a deliberate design choice that keeps each equation as clean as possible. The only cost is that we need a mathematical tool to translate between the two, which is the rotation matrix covered in the next note.

---

## The Inertial Frame

The **inertial frame** is a fixed, Earth-centred coordinate system with three orthogonal axes labelled $x$, $y$, and $z$.

"Inertial" means the frame does not accelerate — Newton's laws hold directly in it without correction terms. For the low-speed flight considered in this paper, treating the Earth surface as inertial is a valid approximation.

The position of the quadrotor's centre of mass in this frame is written as a column vector:

$$
\xi =
\begin{bmatrix}
x \\
y \\
z
\end{bmatrix}
$$

Gravity acts in the negative $z$-direction of the inertial frame:

$$
G =
\begin{bmatrix}
0 \\
0 \\
-g
\end{bmatrix}
$$

where $g = 9.81\ \text{m/s}^2$. Because gravity is always vertical relative to the Earth, expressing it in the inertial frame gives a constant vector — a significant simplification.

---

## The Body Frame

The **body frame** is a coordinate system whose origin is fixed at the quadrotor's centre of mass and whose axes $x_B$, $y_B$, $z_B$ rotate with the vehicle.

Because this frame moves and rotates with the drone, it is the natural place to express:

- the thrust produced by each rotor (which always acts along $z_B$)
- aerodynamic drag forces
- rotor angular velocities and the torques they generate

The $z_B$-axis points upward through the drone, in the direction the rotors push air downward. If the drone is level, $z_B$ aligns with the inertial $z$-axis. If the drone tilts, $z_B$ tilts with it — this is precisely how the drone steers, by redirecting its thrust vector.

---

## Euler Angles and Attitude

The orientation of the body frame relative to the inertial frame is described by three **Euler angles**, collected into the vector $\eta$:

$$
\eta =
\begin{bmatrix}
\phi \\
\theta \\
\psi
\end{bmatrix}
$$

Each angle describes a rotation about one axis:

| Symbol | Name | Rotation axis | Physical meaning |
|--------|------|---------------|-----------------|
| $\phi$ | Roll | $x_B$ | Tilting left/right |
| $\theta$ | Pitch | $y_B$ | Tilting forward/backward |
| $\psi$ | Yaw | $z_B$ | Rotating the heading (compass direction) |

In drone terminology, *attitude* refers specifically to the orientation described by these three angles — it tells you how the drone is pointed, not where it is. Flight controllers that operate in "attitude mode" stabilise $\phi$, $\theta$, and $\psi$ directly.

The Euler angles are applied in a specific sequence: first roll, then pitch, then yaw. This order matters because rotations in 3D do not commute — rotating in a different order produces a different final orientation. The mathematical consequence of this sequential application is explored in [[Ch2_02_Rotation_Matrix_and_Kinematics]].

---

## The Full State Vector

The complete configuration of the quadrotor at any instant is captured by combining position and attitude into a single **state vector** $q$:

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

This six-component vector defines exactly where the quadrotor is in space and how it is oriented. Since there are six independent coordinates, the quadrotor has **six degrees of freedom (6-DOF)**.

A critical point for control: the quadrotor has **only four control inputs** (the angular velocities of its four rotors), yet must regulate all six degrees of freedom. This makes the quadrotor an *underactuated* system, and it is the fundamental challenge that motivates the control design in later chapters.

---

## Velocity Quantities

Motion of the quadrotor involves both translation (changing position) and rotation (changing orientation). Each type of motion has an associated velocity.

### Linear Velocity in the Body Frame

$$
V_B =
\begin{bmatrix}
v_{x,B} \\
v_{y,B} \\
v_{z,B}
\end{bmatrix}
$$

These are the components of the drone's velocity measured along its own body axes. For example, $v_{x,B}$ is the velocity in the direction the drone's nose is pointing. Expressing velocity in the body frame is useful because aerodynamic forces are tied to the direction of airflow relative to the drone, which is most naturally described in the body frame.

### Angular Velocity — Body Rates

$$
\nu =
\begin{bmatrix}
p \\
q \\
r
\end{bmatrix}
$$

These are the rotational velocities of the drone about its own body axes:

- $p$ — roll rate (spinning about $x_B$)
- $q$ — pitch rate (spinning about $y_B$)
- $r$ — yaw rate (spinning about $z_B$)

These quantities are measured directly by the gyroscopes in the drone's inertial measurement unit (IMU). They are the rates at which the body is physically spinning, not the rates at which the Euler angles are changing — those two things are subtly different, as explained in [[Ch2_02_Rotation_Matrix_and_Kinematics]].

---

## Inertia Matrix

The inertia tensor is the rotational analogue of mass. Just as mass resists linear acceleration ($F = ma$), the inertia tensor resists angular acceleration ($\tau = I\alpha$). In general, it is a full $3 \times 3$ symmetric matrix:

$$

I_{\text{general}} =

\begin{bmatrix}

I_{xx} & I_{xy} & I_{xz} \\

I_{xy} & I_{yy} & I_{yz} \\

I_{xz} & I_{yz} & I_{zz}

\end{bmatrix}

$$

The diagonal entries $I_{xx}$, $I_{yy}$, $I_{zz}$ are the **moments of inertia** — each one measures how the mass is distributed around a single axis. The off-diagonal entries $I_{xy}$, $I_{xz}$, $I_{yz}$ are the **products of inertia** — each one measures how mass is distributed *across* two axes simultaneously. A non-zero product of inertia means that applying a torque about one axis tends to also produce rotation about another axis, which couples the rotational equations.

For the quadrotor, the four arms and motors are arranged in a symmetric cross pattern, so for every mass element on one side of a body-frame plane, there is an equal mass element mirrored on the other side. This mirror symmetry about both the $x_B$–$z_B$ and $y_B$–$z_B$ planes causes all products of inertia to cancel out exactly:

$$

I_{xy} = I_{xz} = I_{yz} = 0

$$


The inertia tensor therefore reduces to a diagonal matrix:

$$

I =

\begin{bmatrix}

I_{xx} & 0 & 0 \\

0 & I_{yy} & 0 \\

0 & 0 & I_{zz}

\end{bmatrix}

$$

  
**This is a significant simplification**: the rotational equations for each axis decouple from one another at the level of the inertia matrix (though they remain coupled through the Coriolis terms at high angular rates).

### Why $I_{xx} = I_{yy}$

A standard quadrotor has four arms of equal length arranged at $90°$ intervals, with identical motors and propellers at each tip. This means the mass distribution about the $x_B$-axis is identical to the mass distribution about the $y_B$-axis — rotating the drone $90°$ about $z_B$ produces an indistinguishable configuration. As a direct consequence, the resistance to angular acceleration is the same for roll and pitch:
$$

I_{xx} = I_{yy}

$$

This equality does **not** extend to yaw. The $z_B$-axis runs through the centre of the drone, and the rotors, arms, and frame all sit at a radius from it — the mass is distributed further from $z_B$ than from $x_B$ or $y_B$. As a result $I_{zz} > I_{xx} = I_{yy}$, meaning the drone is harder to spin in yaw than to roll or pitch. This is also why yaw control is the weakest of the four control actions: less torque authority for more resistance.

---

## Quick Reference

| Symbol | Meaning | Units |
|--------|---------|-------|
| $\xi = [x,\ y,\ z]^T$ | Position in inertial frame | m |
| $\eta = [\phi,\ \theta,\ \psi]^T$ | Roll, pitch, yaw (Euler angles) | rad |
| $q = [\xi^T,\ \eta^T]^T$ | Full state vector | — |
| $V_B = [v_{x,B},\ v_{y,B},\ v_{z,B}]^T$ | Linear velocity in body frame | m/s |
| $\nu = [p,\ q,\ r]^T$ | Angular velocity in body frame | rad/s |
| $I$ | Inertia tensor | kg·m² |
| $g$ | Gravitational acceleration | m/s² |

---

## Where This Leads

With the frames and variables defined, the next step is to understand how the body frame orientation relates to the inertial frame mathematically. This requires the **rotation matrix** $R$ and the **angular velocity transformation** $W_\eta$, both of which are developed in:

→ [[Ch2_02_Rotation_Matrix_and_Kinematics]]
