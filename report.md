
# Ray-Traced Camera Animation Using SE(3) Interpolation

## Overview

##For this assignment, I extended my ray tracing project by adding a moving camera animation.
Instead of moving the camera using only standard Euclidean interpolation, I represented each
camera keyframe as a rigid transformation matrix in SE(3). These transformations store both
the camera rotation and translation in one 4x4 pose matrix.

## Trajectory Construction

I created several camera keyframes around the scene. Each keyframe was generated using a
look-at camera function, where the camera position changes while the camera continues to
face the center of the scene. This gives a smooth orbit-style movement around the objects.

## Lie-Group Interpolation Method

For the main animation, I used SE(3) interpolation. Given two poses T1 and T2, I computed
the relative motion between them:

    inv(T1) @ T2

Then I used the matrix logarithm to map this motion into the tangent space. After scaling
that tangent-space motion by alpha, I mapped it back to SE(3) using the matrix exponential.
The final interpolation formula was:

    T(alpha) = T1 @ exp(alpha * log(inv(T1) @ T2))

This creates motion that stays on the SE(3) manifold and treats rotation and translation as
one rigid-body transformation.

## Euclidean Comparison

I also implemented a simpler comparison method. In that method, the rotation is interpolated
separately and the translation is linearly interpolated between camera positions. This works,
but it separates rotation and translation instead of treating the camera pose as one geometric
object. The SE(3) interpolation gives a more consistent rigid-body camera motion.

## Results and Observations

The final output includes a ray-traced animation from the SE(3) camera trajectory and a
comparison animation using the Euclidean method. I also plotted the camera centers and
keyframes to visualize the path. The SE(3) version produces smooth motion through the
keyframes and keeps each camera pose as a valid rigid transformation.
