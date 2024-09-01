Tags: [[Motion Planning]]
### Background Derivation

The generalized equation for *linear motion* is given by:

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
\end{bmatrix}
$$
### State Transition Function
$n \times 1$

FIXME: Given that the motion is oscillatory, we can model the position and angular dynamics as simple harmonic motion (SHM):
$$
\mathbf{f}(\mathbf{x}_k) = \begin{bmatrix}
x_k + \dot{x}_k \Delta t + \frac{1}{2}(-\omega_x^2 x_k + u_x)\Delta t^2 \\
y_k + \dot{y}_k \Delta t + \frac{1}{2}(-\omega_y^2 y_k + u_y)\Delta t^2 \\
z_k + \dot{z}_k \Delta t + \frac{1}{2}(-\omega_z^2 z_k + u_z)\Delta t^2 \\
\dot{x}_k + (-\omega_x^2 x_k + u_x)\Delta t \\
\dot{y}_k + (-\omega_y^2 y_k + u_y)\Delta t \\
\dot{z}_k + (-\omega_z^2 z_k + u_z)\Delta t \\
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

**note**: $\partial f_1 / \partial x_k$ can be approximated to 1 due to focusing on dominant terms (simplicity in linearization which is common in EKF)
### State Transition Jacobian (𝐹)
$n \times n$
$$
\mathbf{F} = \begin{bmatrix}
1 & 0 & 0 & \Delta t & 0 & 0 \\
0 & 1 & 0 & 0 & \Delta t & 0 \\
0 & 0 & 1 & 0 & 0 & \Delta t \\
-\omega_x^2 \Delta t & 0 & 0 & 1 & 0 & 0 \\
0 & -\omega_y^2 \Delta t & 0 & 0 & 1 & 0 \\
0 & 0 & -\omega_z^2 \Delta t & 0 & 0 & 1 \\
\end{bmatrix}
$$

### Measurement Function
$m \times 1$
$$
\mathbf{\vec{z}} = \begin{bmatrix}
x_k \\
y_k \\
z_k \\
\end{bmatrix}
$$

### Measurement Jacobian (𝐻)
$m \times n$

$$
\mathbf{H} = \begin{bmatrix}
1 & 0 & 0 & 0 & 0 & 0 \\
0 & 1 & 0 & 0 & 0 & 0 \\
0 & 0 & 1 & 0 & 0 & 0 \\
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


void Kalman::initializeStateTransitionMatrix() {
    // Update position based on velocity
    jacobian_F(0, 3) = dt;  
    jacobian_F(1, 4) = dt;   
    jacobian_F(2, 5) = dt;    
    jacobian_F(3, 0) = omega_x^2*dt;  
    jacobian_F(4, 1) = omega_y^2*dt;  
    jacobian_F(5, 2) = omega_z^2*dt; 
    jacobian_F(6, 8) = dt;   
    jacobian_F(7, 9) = dt;   
    jacobian_F(8, 6) = omega_roll^2*dt;  
    jacobian_F(9, 7) = omega_pitch^2*dt;  
}


void Kalman::initializeMeasurementMappingMatrix() {
    // Set the diagonal elements to 1 for the measured states
    for (int i = 0; i < measurement_dim; ++i) {
        jacobian_H(i, i) = 1;
    }
}

```