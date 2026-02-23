NOTER TIL VIDEO:
	https://www.youtube.com/watch?v=5DnKot3mMSc&t=3s

![[Pasted image 20260209084445.png]]

![[Pasted image 20260209084454.png]]
Forward Kinematics: given all angels and translational degrees of motion of rbot (motors and actuator position) --> the combination is gonna lead to end effector being in a "certain pos"
	from joint space to physical position in 3d (of robot)
Inverse Kinematics: the opposite --> you want robot to be in specific positipn/orientation --> how do you position actuators?
	tougher problem, usually iterative solver
	matlab does this for us

MATLAB import (Rigid Body Tree) --> Forward + Inverse Kinematics
Simulink import (Simscape Multibody) --> Dynamic Simulation (environment for contact dynamics, state flow chart for supervisor logic)
	You can also add actuator models themselves (e.g. electric motor, hydraulic actuator) into robot to test out low-level controls (not demonstrated)

---
FILER I projektet
(URDF)robot description file 
openManipulatorIK.m

## Inverse Kinematics

	![[Pasted image 20260209085300.png]]
adding paths for demo, importing robot, and show [robot]
	gives 3D rep of robot
	![[Pasted image 20260209085346.png]]

Folder package for ex. Gazebo can be added directly to RST (Robotics System Toolbox)

Targets
![[Pasted image 20260209085434.png]]
	cartesian coordinates
	trajectory: interpolate the natural "spline" in 3D between points
	![[Pasted image 20260209085505.png]]
	done through "curve fitting toolbox"
	we can choose how many points are sampled for a trajectory
	![[Pasted image 20260209085545.png]]

Now we need an Inverse Kinematics Solver
	We add an end-effector frame
	![[Pasted image 20260209085617.png]]

Now we can tell exactly which point we want the IK-solver to solve for
	![[Pasted image 20260209085646.png]]
	This uses a set of weights --> usually the first 3 rotations (x,y,z) and tralsations (on x,y,z)
		weight of how important each is for the solution --> so her potions have higher weight than rotation, as they are more important
Evaluate trajectory
![[Pasted image 20260209085814.png]]
	For loop to call IK solver
	![[Pasted image 20260209085837.png]]
	from RST --> gives back a transformation matrix
	all the solvers require homogenous coordinates (format)

Visualize
![[Pasted image 20260209085955.png]]

![[Pasted image 20260209090012.png]]

This was "offline"

---
now for "online" --> openManipulatorWaypointTracking
![[Pasted image 20260209090126.png]]
Mechanics Explorer
![[Pasted image 20260209090203.png]]

---
![[Pasted image 20260209090306.png]]
![[Pasted image 20260209090254.png]]

---
State Flow Chart
![[Pasted image 20260209090353.png]]

![[Pasted image 20260209090411.png]]
	this splits control of robot motion and control of gripper action into two
	