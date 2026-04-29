## Cart System: Optimal Control Benchmark

This repository explores the optimal control of a cart system subject to drag [^1]. It provides a comprehensive comparison between the analytical solution (derived via Pontryagin's Minimum Principle) and various numerical direct methods, including shooting, collocation and pseudospectral techniques.

### Implementation & Requirements
The code is available for both MATLAB/Octave and Python.

#### Python Setup
Ensure you have Python 3.11+ installed. You can install the dependencies via pip:

``` bash
pip install casadi numpy matplotlib
```
Notebooks: Interactive .ipynb files for all methods are located in the /python folder.

Utility: lglpsmethods.py contains the logic for differentiation matrices and LGL node generation.

#### MATLAB / Octave Setup
Environment: MATLAB or Octave 6.1.0+

Dependency: CasADi 3.5.5+ must be added to your path


### OCP
We aim to find the force $f(t)$ that minimizes the control effort required to move a cart to a state-dependent target position within a fixed time $T=2$.
The cart starts from rest and the final position is dependent on the velocity of the cart at final time.

#### System variables
1. $z_1$ - position of the cart
2. $z_2$ - velocity of the cart
3. $f$ - force applied to the cart

#### Mathematical Formulation

$$
\begin{gathered}
\text{min.}~~\int_{0}^{2}f^2dt\\
\frac{d}{dt}\begin{bmatrix}
z_1\\
z_2
\end{bmatrix}=
\begin{bmatrix}
z_2\\
-z_2+f
\end{bmatrix}\\
\begin{bmatrix}
z_1\\
z_2
\end{bmatrix}(0)=
\begin{bmatrix}
0\\
0
\end{bmatrix}\\
z_1(2)-2.694528z_2(2)+1.155356=0
\end{gathered}
$$

#### Analytical solution
Using Pontryagin's Minimization Principle, the exact solution for the state trajectories and control law is determined to be:

$$
\begin{aligned}
\mathrm{Obj.}&=0.577678\\
z_{1}(t)&=\frac{-3}{8}e^{-t}+\frac{1}{8}e^{t}-\frac{t}{2}+\frac{1}{4}\\
z_{2}(t)&=\frac{3}{8}e^{-t}+\frac{1}{8}e^{t}-\frac{1}{2}\\
f(t)&=\frac{1}{4}e^{t}-\frac{1}{2}\\
\end{aligned}
$$

### Numerical methods for optimal control
This project implements several Direct Methods to transform the continuous optimal control problem (OCP) into a nonlinear programming (NLP) problem.

### Numerical methods implemented

#### MATLAB/Octave

|method|control parameterization|files|
|-|-|-|
|trapezoidal piecewise | piecewise constant control|```trapezoidal_constant.m```|
|Hermite Simpson |piecewise linear control|```simpson.m```|
|Runge Kutta 4 (single shooting) | piecewise constant control|```single_shooting.m```|
|Runge Kutta 4 (multiple shooting) | piecewise constant control|```multiple_shooting.m```|
|Legendre Gauss Lobatto [^3]|global polynomial|```LGL pseudospectral.m, legslb.m, legslbdiff.m, lepoly.m, lepolym.m```|

#### Python
All numerical methods in `directmethods.py`

### Results
The numerical methods closely approximate the analytical curves. Below are the comparisons of the phase plot and control effort.

![image](phaseplot.svg)
![image](control.svg)

### References

[^1]: Conway, B. A. and K. Larson (1998). Collocation versus differential inclusion in direct optimization. Journal of Guidance, Control, and Dynamics, 21(5), 780–785
[^2]: Diehl, Moritz, and Sébastien Gros. "Numerical optimal control." Optimization in Engineering Center (OPTEC) (2011).
[^3]: Shen, Jie, Tao Tang, and Li-Lian Wang. Spectral methods: algorithms, analysis and applications. Vol. 41. Springer Science & Business Media, 2011.
