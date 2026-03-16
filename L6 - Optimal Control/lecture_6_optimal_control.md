

![[images/lecture_6_optimal_control/slide_001.png]]

- 

---

![[images/lecture_6_optimal_control/slide_002.png]]

- 

---

![[images/lecture_6_optimal_control/slide_003.png]]

- 

---

![[images/lecture_6_optimal_control/slide_004.png]]

- 

---

![[images/lecture_6_optimal_control/slide_005.png]]

- 

---

![[images/lecture_6_optimal_control/slide_006.png]]

- 

---

![[images/lecture_6_optimal_control/slide_007.png]]

- 

---

![[images/lecture_6_optimal_control/slide_008.png]]

- 

---
The Optimal control problem
first define: the goal, the constraints, then design u, so that we minimize the cost function (J), which is based on the constraints and the goal
![[images/lecture_6_optimal_control/slide_009.png]]

- 

---

![[images/lecture_6_optimal_control/slide_010.png]]

- 

---

![[images/lecture_6_optimal_control/slide_011.png]]

- t_f --> er t_finite
- def of function: input is scalar, output is scalar
- def of functional: input is function,  output is function

---

![[images/lecture_6_optimal_control/slide_012.png]]

- 

---

![[images/lecture_6_optimal_control/slide_013.png]]

- 

---

![[images/lecture_6_optimal_control/slide_014.png]]

SUMMARY LQR
- Q is used to tune x, R is used to tune u
- From ARE, find P so system is stable
- From P, define u* 
- u* is the optimal control input, which is determined by the goal and the constraints
- the goal in LQR  is to minimize bad performance (x), while minimizing effort (u)
- the constraint in LQR is to minimize J, which is the cost function defined in slide 11

---

![[images/lecture_6_optimal_control/slide_015.png]]

- 

---

![[images/lecture_6_optimal_control/slide_016.png]]

- 

---

![[images/lecture_6_optimal_control/slide_017.png]]

- 

---

![[images/lecture_6_optimal_control/slide_018.png]]

- 

---

![[images/lecture_6_optimal_control/slide_019.png]]

- 

---

![[images/lecture_6_optimal_control/slide_020.png]]

- 

---

![[images/lecture_6_optimal_control/slide_021.png]]

- 

---

![[images/lecture_6_optimal_control/slide_022.png]]

- 
Dvs. at s(t)s(t)^-1 = identitetsmatrice
![[Pasted image 20260313122715.png]]

---

![[images/lecture_6_optimal_control/slide_023.png]]

- 

---

![[images/lecture_6_optimal_control/slide_024.png]]

- 

---

![[images/lecture_6_optimal_control/slide_025.png]]

- 

---

![[images/lecture_6_optimal_control/slide_026.png]]

- 

---

![[images/lecture_6_optimal_control/slide_027.png]]

- 

---

![[images/lecture_6_optimal_control/slide_028.png]]

- 

---

![[images/lecture_6_optimal_control/slide_029.png]]

- 

---

![[images/lecture_6_optimal_control/slide_030.png]]

- 

---

![[images/lecture_6_optimal_control/slide_031.png]]

- 

---

![[images/lecture_6_optimal_control/slide_032.png]]

- A and B are also time varying 
- we linearize the system at each sample, so we consider it time varying

---

![[images/lecture_6_optimal_control/slide_033.png]]

- 

---

![[images/lecture_6_optimal_control/slide_034.png]]

- 

---

![[images/lecture_6_optimal_control/slide_035.png]]

- 

---

![[images/lecture_6_optimal_control/slide_036.png]]

- 

---

![[images/lecture_6_optimal_control/slide_037.png]]

- 

---

![[images/lecture_6_optimal_control/slide_038.png]]

- 

---

![[images/lecture_6_optimal_control/slide_039.png]]

- iLQR

---

![[images/lecture_6_optimal_control/slide_040.png]]

- 

---

![[images/lecture_6_optimal_control/slide_041.png]]
f(x) --> cost function

These four terms represent foundational techniques in mathematical optimization, ranging from linear and quadratic programming to iterative algorithms for non-linear problems.
    LP (Linear Programming): Optimizes a linear objective function subject to linear equality and inequality constraints. It is used for large-scale, simpler problems (e.g., resource allocation) and is often solved using simplex or interior-point methods.
    QP (Quadratic Programming): Optimizes a quadratic (second-order) objective function subject to linear equality/inequality constraints. It extends LP by allowing for quadratic terms, such as minimizing variance in financial portfolio optimization.
    SQP (Sequential Quadratic Programming): An iterative method for solving non-linear, constrained optimization problems. It solves a sequence of QP subproblems, where each subproblem approximates the Lagrangian (objective) and linearizes the constraints at a given point. It is essentially applying Newton's method to the Karush-Kuhn-Tucker (KKT) conditions.
    DP (Dynamic Programming): A method for solving complex problems by breaking them down into simpler, smaller subproblems, often used in sequential decision-making or multi-stage optimization (e.g., control theory)

---

![[images/lecture_6_optimal_control/slide_042.png]]

- 

---

![[images/lecture_6_optimal_control/slide_043.png]]

- 

---

![[images/lecture_6_optimal_control/slide_044.png]]

- 

---

![[images/lecture_6_optimal_control/slide_045.png]]

- 

---

![[images/lecture_6_optimal_control/slide_046.png]]

- 

---

![[images/lecture_6_optimal_control/slide_047.png]]

- 

---

![[images/lecture_6_optimal_control/slide_048.png]]

- 

---

![[images/lecture_6_optimal_control/slide_049.png]]

- 

---

![[images/lecture_6_optimal_control/slide_050.png]]

- f --> cost function
- h and g are constraints
- 
b
---

![[images/lecture_6_optimal_control/slide_051.png]]

- KKT --> optimality conditions

---

![[images/lecture_6_optimal_control/slide_052.png]]

- 

---

![[images/lecture_6_optimal_control/slide_053.png]]

- 

---

![[images/lecture_6_optimal_control/slide_054.png]]

- 

---

![[images/lecture_6_optimal_control/slide_055.png]]

- 

---

![[images/lecture_6_optimal_control/slide_056.png]]

- 

---

![[images/lecture_6_optimal_control/slide_057.png]]

- 

---

![[images/lecture_6_optimal_control/slide_058.png]]

- 

---

![[images/lecture_6_optimal_control/slide_059.png]]

- 

---

![[images/lecture_6_optimal_control/slide_060.png]]

- 
 y(t): output should match s(t): setpoint
---

![[images/lecture_6_optimal_control/slide_061.png]]

- 

---

![[images/lecture_6_optimal_control/slide_062.png]]

- 

---

![[images/lecture_6_optimal_control/slide_063.png]]

- 

---

![[images/lecture_6_optimal_control/slide_064.png]]

- 

---

![[images/lecture_6_optimal_control/slide_065.png]]

- 

---

![[images/lecture_6_optimal_control/slide_066.png]]

- 

---

![[images/lecture_6_optimal_control/slide_067.png]]

- 

---

![[images/lecture_6_optimal_control/slide_068.png]]

- 

---

![[images/lecture_6_optimal_control/slide_069.png]]

- 

---

![[images/lecture_6_optimal_control/slide_070.png]]

- 

---

![[images/lecture_6_optimal_control/slide_071.png]]

- 

---

![[images/lecture_6_optimal_control/slide_072.png]]

- 

---

