# Rotation Matrix and Kinematics

This is the second of five notes covering Chapter 2 of *Modelling and Control of Quadcopter* by Teppo Luukkonen. It explains how the drone's orientation is encoded as a matrix, how to convert vectors between the two reference frames, and why Euler angle rates and body angular rates are two different things that must be carefully related.

**Navigation**
- Previous → [[Ch2_01_Frames_and_State]]
- You are here → **2. Rotation Matrix & Kinematics**
- Next → [[Ch2_03_Rotor_Forces_and_Torques]]
- [[Ch2_00_Master|Return to master note]]

---

## The Rotation Matrix

When the drone tilts, the direction of its thrust changes relative to the world. To compute how much of the rotor thrust acts upward (fighting gravity) versus sideways (accelerating the drone horizontally), we need to know exactly how the body frame is oriented relative to the inertial frame. The **rotation matrix** $R$ encodes this information.

$R$ is a $3 \times 3$ matrix that transforms any vector expressed in body-frame coordinates into the equivalent vector in inertial-frame coordinates:

$$
\mathbf{v}_{\text{inertial}} = R\, \mathbf{v}_{\text{body}}
$$

The rotation is built up from three **elementary rotations** applied in sequence — first roll $\phi$ about $x$, then pitch $\theta$ about $y$, then yaw $\psi$ about $z$:

$$
R = R_z(\psi)\, R_y(\theta)\, R_x(\phi)
$$

Note that matrix multiplication is applied right-to-left: $R_x(\phi)$ acts first, then $R_y(\theta)$, then $R_z(\psi)$. The order matters because 3D rotations do not commute — applying them in a different sequence produces a different final orientation.

---

### Where the Elementary Matrices Come From

Each elementary matrix describes a pure rotation about one coordinate axis. The derivation follows from asking: if a vector rotates by angle $\alpha$ about a given axis, what are the new coordinates of its tip?

Consider rotating a point in the $y$–$z$ plane about the $x$-axis by angle $\phi$. The $x$-component is unchanged. The $y$ and $z$ components mix according to basic trigonometry:

$$
y' = y \cos\phi - z \sin\phi, \qquad z' = y \sin\phi + z \cos\phi
$$

Writing this in matrix form gives $R_x(\phi)$. The same reasoning applied to the other two axes gives $R_y(\theta)$ and $R_z(\psi)$, with one sign difference in $R_y$ due to the handedness of the $y$-axis in a right-handed coordinate system.

---

### The Three Elementary Rotation Matrices

**Roll** — rotation by $\phi$ about the $x$-axis ($x_B$ unchanged, $y_B$–$z_B$ plane rotates):

$$
R_x(\phi) =
\begin{bmatrix}
1 & 0 & 0 \\[4pt]
0 & \cos_\phi & -\sin_\phi \\[4pt]
0 & \sin_\phi & \cos_\phi
\end{bmatrix}
$$

**Pitch** — rotation by $\theta$ about the $y$-axis ($y_B$ unchanged, $x_B$–$z_B$ plane rotates). Note the swapped sign on $S_\theta$ compared to the other two — this comes from the right-hand rule applied to the $y$-axis, which points in the opposite sense to $x$ and $z$ in a right-handed system:

$$
R_y(\theta) =
\begin{bmatrix}
\cos_\theta & 0 & \sin_\theta \\[4pt]
0 & 1 & 0 \\[4pt]
-\sin_\theta & 0 & \cos_\theta
\end{bmatrix}
$$

**Yaw** — rotation by $\psi$ about the $z$-axis ($z_B$ unchanged, $x_B$–$y_B$ plane rotates):

$$
R_z(\psi) =
\begin{bmatrix}
\cos_\psi & -\sin_\psi & 0 \\[4pt]
\sin_\psi & \cos_\psi & 0 \\[4pt]
0 & 0 & 1
\end{bmatrix}
$$

Each of these matrices is itself orthogonal: $R_x^{-1} = R_x^T$, and likewise for $R_y$ and $R_z$. You can verify this by inspection — each row (and column) is a unit vector*, and all rows (and columns) are mutually perpendicular.
_*A unit vector is a vector with a magnitude (length) of exactly 1, used specifically to represent direction in space without accounting for size._

---

### Building the Full Matrix by Multiplication

The full rotation matrix is obtained by multiplying the three elementary matrices in order. First, $R_y(\theta)\, R_x(\phi)$:

$$
R_y(\theta)\, R_x(\phi) =
\begin{bmatrix}
\cos_\theta & \sin_\theta \sin_\phi & \sin_\theta \cos_\phi \\[4pt]
0 & \cos_\phi & -\sin_\phi \\[4pt]
-\sin_\theta & \cos_\theta \sin_\phi & \cos_\theta \cos_\phi
\end{bmatrix}
$$

Then pre-multiplying by $R_z(\psi)$ gives the final result:

$$
R = R_z(\psi)\, R_y(\theta)\, R_x(\phi) =
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

where the shorthand notation used is:
$$
S_x = \sin(x), \qquad C_x = \cos(x)
$$

Each entry of $R$ is a combination of sines and cosines of the three Euler angles. The bottom-left entry $-S_\theta$ is one of the cleanest — it tells you directly that horizontal acceleration in the $x$-direction scales with $\sin\theta$, i.e. how much the drone is pitched forward.

### Sanity Check: Level Flight

When $\phi = \theta = \psi = 0$, all sines vanish and all cosines equal 1. The matrix becomes:

$$
R\big|_{\phi=\theta=\psi=0} =
\begin{bmatrix}
1 & 0 & 0 \\
0 & 1 & 0 \\
0 & 0 & 1
\end{bmatrix}
= I_3
$$

The identity matrix — body frame and inertial frame are perfectly aligned, as expected for a level, unrotated drone.

### Orthogonality

The rotation matrix is **orthogonal**, which means its inverse equals its transpose:

$$
R^{-1} = R^T
$$

This is a powerful property. Computing the inverse of a general $3 \times 3$ matrix requires significant arithmetic; for an orthogonal matrix it is free — just flip rows and columns. Physically, orthogonality reflects the fact that rotation is a rigid transformation: it preserves lengths and angles.

To convert a vector from the *inertial* frame back to the *body* frame, multiply by $R^T$:

$$
\mathbf{v}_{\text{body}} = R^T\, \mathbf{v}_{\text{inertial}}
$$

---

## Velocity Transformation

The same matrix that rotates orientation vectors also transforms velocity vectors. If the drone's velocity is measured in the body frame as $V_B$, its velocity in the inertial (world) frame is:

$$
V_I = R\, V_B
$$

This is used in navigation: the IMU measures $V_B$ directly, but autopilots/flight controllers and GPS systems need $V_I$ to track position over the ground.

Conversely, the gravitational force $G$ is constant and vertical in the inertial frame. To write it in the body frame (needed for Newton–Euler equations expressed in body coordinates), multiply by $R^T$:

$$
G_{\text{body}} = R^T G
$$

---

## Euler Angle Rates vs. Body Angular Rates

This is one of the most frequently misunderstood points in rigid-body kinematics, and it is worth spending time on.

The **body angular rates** $\nu = [p,\ q,\ r]^T$ are the physical rotational velocities of the drone, measured by its gyroscopes. They represent how fast the drone is spinning about each of its own body axes *right now*.

The **Euler angle rates** $\dot{\eta} = [\dot{\phi},\ \dot{\theta},\ \dot{\psi}]^T$ are the time derivatives of the three Euler angles. They describe how fast the angles are changing.

These are *not* the same thing. Here is the intuitive reason: Euler angles are defined as sequential rotations. When you compute $\dot{\psi}$ — the rate of change of the yaw angle — you are differentiating the * *third* rotation in a chain of three. The physical spin component $r$ is the projection of the total angular velocity onto the *current* $z_B$ axis, which has already been rotated by the first two Euler angles. So $r \neq \dot{\psi}$ in general. 

*By "third rotation in a chain of three", recall the sequence in which the rotation matrix must be computed; $R_x \times R_y$ followed by $R_{xy} \times R_z$. So when you differentiate $R$ with respect to time to find how the orientation is changing, the $\dot{\psi}​$ contribution gets mixed with the current values of $\phi$ and $\theta$  — because $R_z$ sits on top of an already-rotated frame. 

The relationship between the two is given by the transformation matrix $W_\eta$:

$$
\nu = W_\eta\, \dot{\eta}
$$
$$
\dot{\eta} = W_\eta^{-1}\, \nu
$$

To understand where $W_\eta$ comes from, recall how the Euler angles are defined: $\dot{\phi}$ is the rate of the *first* rotation (about the original $x$-axis), $\dot{\theta}$ is the rate of the *second* rotation (about the once-rotated $y$-axis), and $\dot{\psi}$ is the rate of the *third* rotation (about the twice-rotated $z$-axis). Each of these is a spin about a *different* axis — axes that are no longer aligned with one another once rotations have been applied. The total physical angular velocity $\nu$ is the vector sum of all three contributions, but each one must first be expressed in the *current* body frame before they can be added.

The three contributions, written in body-frame coordinates, are:

- $\dot{\phi}$ acts about the original $x$-axis. After pitch and yaw are applied, the original $x$-axis has moved relative to the body. Its body-frame representation is $[1,\ 0,\ 0]^T$ — roll acts about $x_B$ directly, before any other rotation.

- $\dot{\theta}$ acts about the once-rolled $y$-axis. In body-frame coordinates this becomes $[0,\ C_\phi,\ -S_\phi]^T$ — it has a component that mixes into $y_B$ and $z_B$ depending on how much roll has already been applied.

- $\dot{\psi}$ acts about the twice-rotated $z$-axis. In body-frame coordinates this becomes $[-S_\theta,\ C_\theta S_\phi,\ C_\theta C_\phi]^T$ — it mixes into all three body axes depending on both roll and pitch.

Adding these three contributions with their respective rates gives the total body angular velocity:

$$

\nu =

\dot{\phi}

\begin{bmatrix} 1 \\ 0 \\ 0 \end{bmatrix}

+

\dot{\theta}

\begin{bmatrix} 0 \\ C_\phi \\ -S_\phi \end{bmatrix}

+

\dot{\psi}

\begin{bmatrix} -S_\theta \\ C_\theta S_\phi \\ C_\theta C_\phi \end{bmatrix}

$$


Writing these three column vectors as the columns of a matrix and collecting the three Euler rates into a single vector — the **Euler rate vector**:

$$

\dot{\eta} =

\begin{bmatrix}

\dot{\phi} \\[4pt]

\dot{\theta} \\[4pt]

\dot{\psi}

\end{bmatrix}

$$

— gives exactly $\nu = W_\eta\, \dot{\eta}$, with $W_\eta$ being that matrix of columns:

$$

W_\eta =

\begin{bmatrix}

1 & 0 & -S_\theta \\[4pt]

0 & C_\phi & C_\theta S_\phi \\[4pt]

0 & -S_\phi & C_\theta C_\phi

\end{bmatrix}

$$

The inverse direction $\dot{\eta} = W_\eta^{-1}\, \nu$ is the form used in practice: the flight controller reads $\nu = [p,\ q,\ r]^T$ from the gyroscope and multiplies by $W_\eta^{-1}$ to obtain the Euler angle rates, which can then be integrated to track $\phi$, $\theta$, and $\psi$ over time. Crucially, $W_\eta^{-1}$ depends on the *current* values of $\phi$ and $\theta$, so this integration must be performed continuously and the matrix must be recomputed at every step as the drone's attitude changes.

So the one-sentence summary: $r$ is what the hardware measures, $\dot{\psi}$ is what the geometry of the Euler angle sequence implies, and $W_\eta$ is the bridge between the two.

### From Body Rates to Euler Rates

This is the direction the flight controller uses in practice. The gyroscope outputs $[p,\ q,\ r]^T$ and the controller needs to know how fast the Euler angles are changing so it can integrate them to track attitude:
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
where $T_x = \tan(x)$. This is $\dot{\eta} = W_\eta^{-1}\nu$, written out in full.

Carrying out the matrix–vector multiplication row by row gives three explicit scalar equations, one for each Euler rate:

$$

\dot{\phi} = p + q\, S_\phi T_\theta + r\, C_\phi T_\theta

$$
$$

\dot{\theta} = q\, C_\phi - r\, S_\phi

$$
$$

\dot{\psi} = q\, \frac{S_\phi}{C_\theta} + r\, \frac{C_\phi}{C_\theta}

$$

Reading each equation directly shows how $p$, $q$, and $r$ contribute:

- **$\dot{\phi}$**: the roll Euler rate receives a direct contribution from $p$ (the body roll rate), plus bleed-in from $q$ and $r$ weighted by $\tan\theta$. At level flight $\tan\theta = 0$, so $\dot{\phi} = p$ exactly. As the drone pitches forward, $\tan\theta$ grows and both $q$ and $r$ increasingly contaminate $\dot{\phi}$ — the roll axis is tilting away from the world horizontal.

- **$\dot{\theta}$**: the pitch Euler rate has no contribution from $p$ at all. It is purely a mixture of $q$ and $r$, with $\phi$ controlling the weighting. At zero roll $\dot{\theta} = q$ directly; as the drone rolls, $r$ starts contributing because the body pitch axis is no longer aligned with the world pitch direction.

- **$\dot{\psi}$**: the yaw Euler rate also has no contribution from $p$. It depends on $q$ and $r$ divided by $C_\theta$. The $1/C_\theta$ denominator is the critical term — it is bounded for any pitch angle except $\pm 90°$, where it diverges (gimbal lock).

A useful special case to build intuition: if the drone is perfectly level ($\phi = 0$, $\theta = 0$), all the trigonometric mixing terms vanish and the equations collapse to $\dot{\phi} = p$, $\dot{\theta} = q$, $\dot{\psi} = r$ — body rates and Euler rates are identical, as expected when both frames are aligned.
### From Euler Rates to Body Rates

This is the forward direction $\nu = W_\eta\, \dot{\eta}$: given how fast you want the Euler angles to change, what body spin does that correspond to? This form is useful when designing trajectories or desired attitude profiles, where you specify $\dot{\eta}$ and need to know what $[p,\ q,\ r]^T$ to command:

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

This is the matrix $W_\eta$ derived earlier from the three column vectors. Reading it row by row:

- **$p$** (first row): the roll body rate is $\dot{\phi}$ minus a $\dot{\psi}$ correction scaled by $S_\theta$. A pure yaw-rate command ($\dot{\psi} \neq 0$) contributes to $p$ if the drone is pitched, because the yaw axis is no longer perpendicular to the roll axis when $\theta \neq 0$.

- **$q$** (second row): the pitch body rate mixes $\dot{\theta}$ and $\dot{\psi}$, weighted by the current roll angle $\phi$. With no roll, $q = \dot{\theta}$ directly.

- **$r$** (third row): the yaw body rate is a combination of $\dot{\theta}$ and $\dot{\psi}$ projected through $S_\phi$ and $C_\phi$, scaled by $C_\theta$. With no roll and no pitch, $r = \dot{\psi}$ directly — confirming the intuition that only at zero attitude do body rates and Euler rates coincide.

---

## Gimbal Lock — A Singularity in the Transformation

The matrix $W_\eta$ contains $1/C_\theta = 1/\cos\theta$ terms. When the pitch angle reaches $\pm 90°$, $\cos\theta = 0$ and these terms blow up — the transformation becomes singular, meaning it has no unique inverse.

$$
\text{Singularity when} \quad \cos\theta = 0 \quad \Longleftrightarrow \quad \theta = \pm\, 90°
$$

This phenomenon is called **gimbal lock**. At exactly $\pm 90°$ pitch, one rotational degree of freedom is lost — roll and yaw become indistinguishable in the Euler angle representation. This is a fundamental limitation of Euler angles, not a physical limitation of the drone itself.

For the flight regimes considered in this paper — small angles, near-hover — gimbal lock is not a concern. The model is valid as long as $|\theta|$ stays well below $90°$. For aerobatic manoeuvres, alternative parameterisations such as quaternions would be required.

---

## Worked Example: Velocity Transformation

**Scenario:** The drone is flying forward in its own body frame at $1\ \text{m/s}$ with no sideways or vertical component. Its attitude is $\phi = 10°$, $\theta = 0°$, $\psi = 30°$ (only yaw is non-trivial here).

Body velocity:

$$
V_B =
\begin{bmatrix} 1 \\ 0 \\ 0 \end{bmatrix}
\text{ m/s}
$$

Because $\theta = 0$, the rotation matrix reduces to a pure yaw rotation:

$$
R =
\begin{bmatrix}
\cos 30° & -\sin 30° & 0 \\
\sin 30° & \cos 30° & 0 \\
0 & 0 & 1
\end{bmatrix}
$$

Inertial velocity:

$$
V_I = R\, V_B =
\begin{bmatrix}
\cos 30° \\
\sin 30° \\
0
\end{bmatrix}
\approx
\begin{bmatrix}
0.866 \\
0.500 \\
0
\end{bmatrix}
\text{ m/s}
$$

**Interpretation:** Even though the drone is flying "straight ahead" in its own frame, an observer on the ground sees it moving at an angle because the nose is pointing $30°$ off the north axis.

---

## Quick Reference

| Symbol | Meaning |
|--------|---------|
| $R$ | Rotation matrix: body frame → inertial frame |
| $R^T = R^{-1}$ | Inverse rotation: inertial frame → body frame |
| $\nu = [p,\ q,\ r]^T$ | Body angular rates (gyroscope output) |
| $\dot{\eta} = [\dot{\phi},\ \dot{\theta},\ \dot{\psi}]^T$ | Euler angle rates |
| $W_\eta$ | Transformation matrix: $\nu = W_\eta \dot{\eta}$ |
| $W_\eta^{-1}$ | Inverse: $\dot{\eta} = W_\eta^{-1} \nu$ |
| $S_x,\ C_x,\ T_x$ | $\sin(x),\ \cos(x),\ \tan(x)$ |

---

## Where This Leads

With the rotation matrix and the angular velocity transformation in hand, we can now express forces and torques in whichever frame is most convenient. The next step is to derive what forces and torques the rotors actually produce:

→ [[Ch2_03_Rotor_Forces_and_Torques]]
