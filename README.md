## Quadrotor Controller
### Scenario 1: Intro
The aim of this scenario was to make the drone hover. This was achieved by adjusting the weight of the drone in `QuadControlParams.txt`. The scenario is passed with the following being printed on the console,
```
PASS: ABS(Quad2.PosFollowErr) was less than 0.250000 for at least 3.000000 seconds
```

### Scenario 2:
The `GenerateMotorCommands` method needs to be coded resolving this equations:
[Moments equation](/res/moments_force_eq.gif)
Where all the `F_1` to `F_4` are the motor's thrust, `tao(x,y,z)` are the moments on each direction, `F_t` is the total thrust, kappa is the drag/thrust ratio and l is the drone arm length over square root of two. These equations come from the classroom lectures.

The second step is to implement the `BodyRateControl` method applying a P controller and the moments of inertia. At this point, the `kpPQR` parameter has to be tuned to stop the drone from flipping, but first, some thrust needs to be commanded in the altitude control because we don't have thrust commanded on the `GenerateMotorCommands` anymore. A good value is `thurst = mass * CONST_GRAVITY`.

Once this is done, we move on to the `RollPitchControl` method. For this implementation, you need to apply a few equations. You need to apply a P controller to the elements R13 and R23 of the rotation matrix from body-frame accelerations and world frame accelerations:
[P Contriller Roll Pitch](/res/roll_pitch_p_controller.gif)
But the problem is you need to output roll and pitch rates; so, there is another equation to apply:
[Roll Pitch](/res/roll_pitch_from_b_to_pq.gif)
It is important to notice you received thrust and thrust it need to be inverted and converted to acceleration before applying the equations. After the implementation is done, start tuning `kpBank` and `kpPQR` until the drone flies more or less stable upward:
The scenario is passed with the following being printed on the console,
```
PASS: ABS(Quad.Roll) was less than 0.025000 for at least 0.750000 seconds
PASS: ABS(Quad.Omega.X) was less than 2.500000 for at least 0.750000 seconds
```

### Scenario 3: Position/velocity and yaw angle control
There are three methods to implement here:
* `AltitudeControl`: This is a `PD controller` to control the acceleration meaning the thrust needed to control the altitude.
* `LateralPositionControl`: A `PID controller` to control acceleration on `x` and `y`.
* `YawControl`: A `P controller` optimize for yaw to be between `[-pi, pi]`.
When `kpYaw`, `kpPosXY`,  `kpVelXY`,  `kpPosZ` and `kpVelZ` is set to zero, the following is printed on the console,
```
PASS: ABS(Quad1.Pos.X) was less than 0.100000 for at least 1.250000 seconds
PASS: ABS(Quad2.Pos.X) was less than 0.100000 for at least 1.250000 seconds
PASS: ABS(Quad2.Yaw) was less than 0.100000 for at least 1.000000 seconds
```

### Scenario 4: Non-idealities and robustness
In this scenario, we just had to transform a PD controller into a PID controller. The scenario is passed with the following being printed on the console,
```
PASS: ABS(Quad1.PosFollowErr) was less than 0.100000 for at least 1.500000 seconds
PASS: ABS(Quad2.PosFollowErr) was less than 0.100000 for at least 1.500000 seconds
PASS: ABS(Quad3.PosFollowErr) was less than 0.100000 for at least 1.500000 seconds
```

### Scenario 5: Tracking trajectories
This scenario shows the errors a drone makes while following a trajectory. These errors force you to tune some parameters again. The scenario is passed with the following being printed on the console,
```
PASS: ABS(Quad2.PosFollowErr) was less than 0.250000 for at least 3.000000 seconds
```


