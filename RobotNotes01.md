# Robot Development Notes

Observations from 1/24/2023 testing: When set to zero velocity and enabled, nothing would move unless something disturbed a wheel.  Then one side would ramp out of control and the other would oscillate violently.  The side running out of control must have a negated direction, so the PID isn't working.  The side oscillating has too high of gain (probably the feed forward).


## Debug of the robot base motion
1. Comment out all PID and wheel speed calculations.  Set wheels to turn at a fixed positive rate very slowly (like 0.1 rps).
2. Verify that the wheels are turning the correct direction and speed.
3. Disable the motors. Verify that one turn of a wheel on each side is equal to the correct encoder count and direction.
4. WITHOUT enabling feed forward, attempt to put the PID control back in the loop. If it fails, consider using the built in SparkMax PID like the "Offboard" example below.

### References
[Java Color Code Example](https://www.javatpoint.com/java-color-codes)

[WPILib Diff Drive Example](https://github.com/wpilibsuite/allwpilib/tree/main/wpilibjExamples/src/main/java/edu/wpi/first/wpilibj/examples/differentialdrivebot)
[WPILib Drive Distance Offboard](https://github.com/wpilibsuite/allwpilib/tree/main/wpilibjExamples/src/main/java/edu/wpi/first/wpilibj/examples/drivedistanceoffboard)
[WPILib Drivetrain Example without PID](https://docs.wpilib.org/en/stable/docs/software/pathplanning/trajectory-tutorial/creating-drive-subsystem.html)
[WPILib Gyro Command Example](https://github.com/wpilibsuite/allwpilib/tree/main/wpilibjExamples/src/main/java/edu/wpi/first/wpilibj/examples/gyrodrivecommands)
[RevRobotics SparkMax Code Examples](https://docs.revrobotics.com/sparkmax/software-resources/spark-max-code-examples)
