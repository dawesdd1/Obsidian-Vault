Tags: [[Motion Planning]]

*Rev (8-20-24):* this revised EKF used the state vector to estimate values for $\omega$ dynamically without the need to collect data and perform FFT analysis on the oscillatory motion of the **pool noodle**

### Background Derivation

## Motion Model

The generalized equation for simple harmonic motion (SHM) is given by:
$$
x(t)=Acos(ωt+ϕ)
$$

The second derivative of this equation with respect to time (i.e., acceleration) is:
$$  \ddot{x}(t) = −ω^2(A cos(ωt+ϕ)) $$

$$  \ddot{x}(t) = −ω^2x(t)  $$
similar derivation happens for the $\ddot{θ}_{roll}​ = −ω_{roll}^2​ θ_{roll} ​+ u_{θ_{roll​}​}$ equation...

### State Vector 
$n \times 1$
$$
\mathbf{x} = \begin{bmatrix}
x \\
y \\
z \\
\dot{x} \\
\dot{y} \\
\dot{z} \\
\theta_{\text{roll}} \\
\theta_{\text{pitch}} \\
\dot{\theta}_{\text{roll}} \\
\dot{\theta}_{\text{pitch}} \\
\omega_{x} \\
\omega_{y} \\
\omega_{z} \\
\omega_{\text{roll}} \\
\omega_{\text{pitch}} \\
\end{bmatrix}
$$
### State Transition Function

#### Explaining discrete estimation of $x$ and $\dot{x}$

The general for of the discrete estimation of position $x_{k+1}$ is given by the Taylor series expansion:
$x_{k+1} = x_k + \dot{x}_k\Delta t + \frac{1}{2} \ddot{x}_k\Delta + ...$

where, $\Delta t$ is the discrete time step interval

$x_k + (-\omega_x^2 x_k + u_x)\Delta t$

The general for of the discrete estimation velocity of $\dot{x}_{k+1}$ is:
$\dot{x}_{k+1} = \dot{x}_k + \Delta t (\ddot{x})$

where, $\Delta t$ is the discrete time step interval

$\dot{x}_{k+1} = \dot{x}_k + (-\omega_x^2 x_k)\Delta t$
$\dot{x}_{k+1} = \dot{x}_k + (-\omega_x^2 x_k + u_x)\Delta t$    ...with a given control input `u_k`

#### Full State Transition Equation
Given that the motion is oscillatory, we can model the position and angular dynamics as simple harmonic motion (SHM):
$$
\mathbf{f}(\mathbf{x}_k) = \begin{bmatrix}
x_k + \dot{x}_k \Delta t + \frac{1}{2}(-\omega_x^2 x_k + u_x)\Delta t^2 \\
y_k + \dot{y}_k \Delta t + \frac{1}{2}(-\omega_y^2 y_k + u_y)\Delta t^2 \\
z_k + \dot{z}_k \Delta t + \frac{1}{2}(-\omega_z^2 z_k + u_z)\Delta t^2 \\
\dot{x}_k + (-\omega_x^2 x_k + u_x)\Delta t \\
\dot{y}_k + (-\omega_y^2 y_k + u_y)\Delta t \\
\dot{z}_k + (-\omega_z^2 z_k + u_z)\Delta t \\
\theta_{\text{roll}_k} + \dot{\theta}_{\text{roll}_k} \Delta t + \frac{1}{2}(-\omega_{\text{roll}}^2 \theta_{\text{roll}_k} + u_{\theta_{\text{roll}}})\Delta t^2 \\
\theta_{\text{pitch}_k} + \dot{\theta}_{\text{pitch}_k} \Delta t + \frac{1}{2}(-\omega_{\text{pitch}}^2 \theta_{\text{pitch}_k} + u_{\theta_{\text{pitch}}})\Delta t^2 \\
\dot{\theta}_{\text{roll}_k} + (-\omega_{\text{roll}}^2 \theta_{\text{roll}_k} + u_{\theta_{\text{roll}}})\Delta t \\
\dot{\theta}_{\text{pitch}_k} + (-\omega_{\text{pitch}}^2 \theta_{\text{pitch}_k} + u_{\theta_{\text{pitch}}})\Delta t \\
\omega_{x_k} \\ 
\omega_{y_k} \\ 
\omega_{z_k} \\ 
\omega_{\text{roll}_k} \\ 
\omega_{\text{pitch}_k}
\end{bmatrix}
$$
Where:
- $\omega_x, \omega_y, \omega_z$​ are the angular frequencies for the oscillatory motion in each direction.
- $\omega_{\text{roll}}, \omega_{\text{pitch}}$ are the angular frequencies for roll and yaw oscillations.
- $u_x, u_y, u_z, u_{\theta_{\text{roll}}}, u_{\theta_{\text{yaw}}}$​​ represent control inputs or external disturbances.

### Partial Derivatives (all not listed)

$\partial f_1 / \partial x_k = 1 - \frac{1}{2} \omega^2_x \Delta t^2 \approx 1$ 
$\partial f_1 / \partial  y_k = 0$
$\partial f_1 / \partial z_k = 0$
$\partial f_1 / \partial \dot{x_k} = \Delta t$ 
$\partial f_1 / \partial \dot{y_k} = 0$
$\partial f_1 / \partial \dot{z_k} = 0$
$\partial f_1 / \partial \theta_{roll} = 0$
$\partial f_1 / \partial \theta_{pitch} = 0$
$\partial f_1 / \partial \dot{\theta}_{roll}= 0$
$\partial f_1 / \partial \dot{\theta}_{pitch}= 0$
$\partial f_1 / \partial \omega_{x} = 0$
$\partial f_1 / \partial \omega_{y} = 0$
$\partial f_1 / \partial \omega_{z} = 0$
$\partial f_1 / \partial \omega_{roll} = 0$
$\partial f_1 / \partial \omega_{pitch} = 0$

**note**: $\partial f_1 / \partial x_k$ can be approximated to 1 due to focusing on dominant terms (simplicity in linearization which is common in EKF)
### State Transition Jacobian (𝐹)
$n \times n$
$$
\mathbf{F} = \begin{bmatrix}
1 & 0 & 0 & \Delta t & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 1 & 0 & 0 & \Delta t & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 1 & 0 & 0 & \Delta t & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
-\omega_x^2 \Delta t & 0 & 0 & 1 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & -\omega_y^2 \Delta t & 0 & 0 & 1 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & -\omega_z^2 \Delta t & 0 & 0 & 1 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 & \Delta t & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 & \Delta t & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & -\omega_{\text{roll}}^2 \Delta t & 0 & 1 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & -\omega_{\text{pitch}}^2 \Delta t & 0 & 1 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 \\
\end{bmatrix}
$$

### Measurement Vector
$m \times 1$

The Jacobian matrix H is sparse and has a 1's in the positions corresponding to the measured states (positions and angles) and 0's elsewhere.

$$
\mathbf{h}(\mathbf{x}_k) = \begin{bmatrix}
x_k \\
y_k \\
z_k \\
\theta_{\text{roll}_k} \\
\theta_{\text{pitch}_k}
\end{bmatrix}
$$

### Measurement Jacobian (𝐻)
$m \times n$
$$
\mathbf{H} = \begin{bmatrix}
1 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 1 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 1 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 & 0 & 0 & 0 & 0 & 0 & 0
\end{bmatrix}
$$
### Control Input Jacobian (*B*)

N/A because there are no known control inputs $\vec{u}$

---

Pseudo code for the a cpp EKF implementation 

```
// EKF.cpp

jacobian_F(Eigen::MatrixXd::Identity(state_dim, state_dim)),
jacobian_H(Eigen::MatrixXd::Zero(measurement_dim, state_dim)),
state_dim(10},               // Extended state dimension overrides base class
measurement_dim(5)           // Extended measurement dimension

...


void ExtendedKalman::initializeStateTransitionJacobianMatrix(double dt) {

    // Update position based on velocity
    jacobian_F(0, 3) = dt;
    jacobian_F(1, 4) = dt;
    jacobian_F(2, 5) = dt;

    // Include omega updates
    jacobian_F(3, 0) = pow(state(10), 2) * dt;          // omega_x
    jacobian_F(4, 1) = pow(state(11), 2) * dt;          // omega_y
    jacobian_F(5, 2) = pow(state(12), 2) * dt;          // omega_z
    jacobian_F(6, 8) = dt;
    jacobian_F(7, 9) = dt;
    jacobian_F(8, 6) = pow(state(13), 2) * dt;          // omega_roll
    jacobian_F(9, 7) = pow(state(14), 2) * dt;          // omega_pitch
}


void Kalman::initializeMeasurementMappingMatrix() {
    // Set the specific elements to 1 for the measured states
    jacobian_H(0, 0) = 1;
    jacobian_H(1, 1) = 1;
    jacobian_H(2, 2) = 1;
    jacobian_H(3, 6) = 1;
    jacobian_H(4, 7) = 1;
}

```


## Noise Model
$n \times n$
The generalized equation for simple harmonic motion (SHM) 

### State Covariance $P$
$n \times n$

### Process Noise Covariance $Q$
$m \times m$
$$
\mathbf{Q} = \begin{bmatrix}
\sigma_{x}^2 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & \sigma_{y}^2 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & \sigma_{z}^2 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & \sigma_{\dot{x}}^2 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & \sigma_{\dot{y}}^2 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & \sigma_{\dot{z}}^2 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & \sigma_{\theta_{roll}}^2 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & \sigma_{\theta_{pitch}}^2 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0  & 0 & \sigma_{\dot{\theta}_{roll}}^2 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & \sigma_{\dot{\theta}_{pitch}}^2 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & \sigma_{\omega_{x}}^2 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & \sigma_{\omega_{y}}^2 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & \sigma_{\omega_{z}}^2 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & \sigma_{\omega_{roll}}^2 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & \sigma_{\omega_{pitch}}^2 \\
\end{bmatrix}
$$


### Measurement Noise Covariance $R$
$n \times n$
$$
\mathbf{R} = \begin{bmatrix}
\sigma^2_{x} & 0 & 0 & 0 & 0  \\
0 & \sigma^2_{y} & 0 & 0 & 0  \\
0 & 0 & \sigma^2_{z} & 0 & 0  \\
0 & 0 & 0 & \sigma^2_{roll} & 0  \\
0 & 0 & 0 & 0 & \sigma^2_{pitch} 
\end{bmatrix}
$$


---

Pseudo Code for Noise Matrices

```
Q(Eigen::MatrixXd::Zero(state_dim, state_dim)),
R(Eigen::MatrixXd::Zero(measurement_dim, measurement_dim))

...

void Kalman::initializeMeasurementNoiseCovarianceMatrix() {
    // set noise coeffs along matrix diagonal
    for (int i=0; i < measurement_dim; i++) {
        R(i, i) = 0.03;         // <---- arbitrary small value for now
    }
}


void Kalman::initializeProcessNoiseCovarianceMatrix() {
    // set noise coeffs along matrix diagonal
    for (int i=0; i < state_dim; i++) {
        Q(i, i) = 0.03;         // <---- arbitrary small value for now
    }
}
```