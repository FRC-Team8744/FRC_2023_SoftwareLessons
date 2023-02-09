# Robot Development Notes

2/9/2023 - You're on your own, Carson!  Here is a list of notes and changes to work on.

## General
1. I updated your laptop and the driver's station to the latest VS Code compiler, but it doesn't fix the problem of the joysticks not being recognized when starting the driver station.

## Debug of teleop and arm motors
1. I tested the software on Wednesday - the joystick axies for throttle and steering need adjustment.  The throttle was inverted and you need to change the steering joystick axis to X instead of the twist around Z.
2. Add code to set the current limit in Amps to the arm,wrist,gripper, and elevator.  The function is named setSmartCurrentLimit​(int limit).  Set the NEO550s (the little ones) to 10 Amps and set the NEOs to 20 Amps. This should limit how much they jump around when you push the buttons.
3. Add code to turn off the arm, wrist, gripper, and elevator motors when the command finishes (stopMotor).

## Debug of AutonomousCommand
1. The last I saw, this "kind of" worked - in that it stopped after the wheels turned for a while.  That's good.  Add on in small, testable increments.
2. I noticed in the code that you command the robot to turn and then immediately command it to throttle forward.  There is no need to turn during this routine, so please remove the steering commands.
3. Verify that it does what you expect.
4. Change the command to pass in two double arguments - for distance and speed.  You want to be able to specify how far and fast to drive when you call the command.  Add it. Test that it does what you expect.
5. Calibration.  How far is it actually moving?  If you tell it to move 1.0 meters, do the wheels turn about that much?
6. Edge cases. What happens when you tell it to drive a negative distance? (i.e. Reverse)

## Create a new command for AutoTurn
1. This will be very similar to the AutoDrive command (you changed the name so people know what it does, right?).
2. What will you need to tell it? (pass in degrees to turn and speed)
3. How will the logic in the command work?

### References
[WPILib Diff Drive Example](https://github.com/wpilibsuite/allwpilib/tree/main/wpilibjExamples/src/main/java/edu/wpi/first/wpilibj/examples/differentialdrivebot)
[WPILib Drive Distance Offboard](https://github.com/wpilibsuite/allwpilib/tree/main/wpilibjExamples/src/main/java/edu/wpi/first/wpilibj/examples/drivedistanceoffboard)
[WPILib Drivetrain Example without PID](https://docs.wpilib.org/en/stable/docs/software/pathplanning/trajectory-tutorial/creating-drive-subsystem.html)
[WPILib Gyro Command Example](https://github.com/wpilibsuite/allwpilib/tree/main/wpilibjExamples/src/main/java/edu/wpi/first/wpilibj/examples/gyrodrivecommands)
[RevRobotics SparkMax Code Examples](https://docs.revrobotics.com/sparkmax/software-resources/spark-max-code-examples)
