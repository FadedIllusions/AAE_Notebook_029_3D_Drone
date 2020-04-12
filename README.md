# AAE_Notebook_029_3D_Drone

Until this point, all of the dynamics we've discussed have been to control a 2D vehicle in the y and z vertical plans. Thankfully, all the concepts related to PID cascaded control can, also, be applied to 3D. (Feel free to review the previous notebooks as needed.)

![2D Dynamics Review](/images/2d_review.png) 

***   ***   ***   ***   ***   ***   ***   ***   ***

## World vs Body Frame

In 3D, we're going to do the same general sequence of steps to keep track of the vehicle state but with slight modifications to, also, keep track of rotations -- vehicle attitude. To do this, we'll need to think in two different coordinate systesm, the world frame and the body frame.

![World Vs Body Frame](/images/world_vs_body_frame.png)

When working in 3D, we have to keep track of both of these frames, especially when we have to do math within the rotating body frame and then convert those rotations into euler angles inside the world frame.

Ultimately, we seek to control the vehicle in the world frame; however, some of our sensor measurements (such as from the IMU, which measures rotations in the body frame) and our controls, especially the moments that we command, have a much more intuitive interpretation when we reason in the body frame.

***   ***   ***   ***   ***   ***   ***   ***   ***

## 3D Dynamics

![3D States](/images/3d_states.png)

Just as we did in 2D, we need to keep track of our vehicle's state -- this time, using 12 variables instead of 6. These variables specify the location of the vehicle in x, y, and z; they specify the orientation/attitude of the vehicle in Euler angles (roll/phi, pitch/theta, yaw/psi); the x, y, and z velocities; and rotation rates within the body frame ('body rates'). 

** Note: we can use quaternions to represent the vehicle's orientation/attitude, if so desired 

For the 2D control case, our body rates were a single quantity, phi_dot. In the 3D case, we must consider rotation rates about all three axes. Note that these are /not/ the same as phi_dot, theta_dot, and psi_dot -- these are rotations within the world frame. The p, q, r variables give the angular rate of change within the body frame of the vehicle as measured by a gyroscope mounted on the vehicle itself. (It's easier to work with p, q, r than it is to work with Euler angle rates.)

![3D Dynamics](/images/3d_dynamics.png)

Next, we must identify the forces and moments acting on the vehicle in the body frame. At this point, we have to figure out the translational motions from the forces (x_dot_dot, x_dot, x, etc) and consider how the moments influence the rotational motion (p_dot, p, phi, etc).

Thus, we have three things to do:
  1. Figure out how four prop turn rates turn into a net force and three moments.
  2. Track the influence of force on translational moment
  3. Track the influence of the three moments on rotational motion

** Note: p gives the angular rotation rate about the x-axis (in the body frame), q gives the rotation rate about the y-axis, and r gives the rotation rate about the z-axis. (In rads/s.)
