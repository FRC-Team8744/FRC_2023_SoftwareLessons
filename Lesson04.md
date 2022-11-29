# Driving in a straight line

1. Open up VS Code and create a new RomiReference project called 'DriveStraightTutorial'.
2. Build and simulate the project (run the code on your Romi). Turn the robot in your hands.  What range of values do you see in RomiGyro.angle_z as you turn from the starting position? What happens if you rotate the robot multiple times in one direction?

> ### Race Time!
> See who can drive the furthest down the hallway - **WITHOUT USING ANY STEERING**. Only use the throttle.

3. We are going to change the code so that it follows a desired gyro heading, instead of direct steering input from the joystick.  Change the manual driving command ArcadeDrive.java to this:

```
// Copyright (c) FIRST and other WPILib contributors.
// Open Source Software; you can modify and/or share it under the terms of
// the WPILib BSD license file in the root directory of this project.
 
package frc.robot.commands;
 
import frc.robot.subsystems.Drivetrain;
import edu.wpi.first.wpilibj.smartdashboard.SmartDashboard;
import edu.wpi.first.wpilibj2.command.CommandBase;
import java.util.function.Supplier;
 
public class ArcadeDrive extends CommandBase {
  private final Drivetrain m_drivetrain;
  private final Supplier<Double> m_xaxisSpeedSupplier;
  private final Supplier<Double> m_zaxisRotateSupplier;
 
  /**
   * Creates a new ArcadeDrive. This command will drive your robot according to the speed supplier
   * lambdas. This command does not terminate.
   *
   * @param drivetrain The drivetrain subsystem on which this command will run
   * @param xaxisSpeedSupplier Lambda supplier of forward/backward speed
   * @param zaxisRotateSupplier Lambda supplier of rotational speed
   */
  public ArcadeDrive(
      Drivetrain drivetrain,
      Supplier<Double> xaxisSpeedSupplier,
      Supplier<Double> zaxisRotateSupplier) {
    m_drivetrain = drivetrain;
    m_xaxisSpeedSupplier = xaxisSpeedSupplier;
    m_zaxisRotateSupplier = zaxisRotateSupplier;
    addRequirements(drivetrain);
  }
  private double targetHeading;  // in degrees
  private double errorHeading;
  private double motorTurnEffort;
  private double motorSpeedEffort;
  private double turnRequest;
  private double degreeScale = 20.0;
  private double turnRate = 5.0;
  private double kP = 0.03;
 
  // Called when the command is initially scheduled.
  @Override
  public void initialize() {
    // Set the starting targetHeading to the robot's current direction
    targetHeading = m_drivetrain.getGyroAngleZ();
  }
 
  // Called every time the scheduler runs while the command is scheduled.
  @Override
  public void execute() {
    // How far is the tragetHeading from the actual heading?
    errorHeading = targetHeading - m_drivetrain.getGyroAngleZ();
 
    // Check if the user is requesting a bigger turn than the error
    turnRequest = degreeScale * m_zaxisRotateSupplier.get();  // Convert joystick request to degrees
    // Don't do anything if turnRequest is small
    if (Math.abs(turnRequest) > 0.05 * degreeScale) {
      if (turnRequest > errorHeading) {
        // Add a small amount to the targetHeading (it will keep increasing while the joystick is held)
        targetHeading += turnRate;
      } else if (turnRequest < errorHeading) {
        // Subtract a small amount from the targetHeading (it will keep decreasing while the joystick is held)
        targetHeading -= turnRate;
      }
    }
 
    // Recalculate errorHeading
    errorHeading = targetHeading - m_drivetrain.getGyroAngleZ();
 
    // Set the motor drive strength depending on how bad the error is
    motorTurnEffort = kP * errorHeading;
 
    if (motorTurnEffort > 1.0) motorTurnEffort = 1.0;  // Hardware limit on motor power
    if (motorTurnEffort < -1.0) motorTurnEffort = -1.0;  // Hardware limit on motor power
 
    // Show all the variables in SmartDashboard for debug
    SmartDashboard.putNumber("Actual Heading", m_drivetrain.getGyroAngleZ());
    SmartDashboard.putNumber("targetHeading", targetHeading);
    SmartDashboard.putNumber("errorHeading", errorHeading);
    SmartDashboard.putNumber("motorTurnEffort", motorTurnEffort);
 
    motorSpeedEffort = m_xaxisSpeedSupplier.get();
    m_drivetrain.arcadeDrive(motorSpeedEffort, motorTurnEffort);
  }
 
  // Called once the command ends or is interrupted.
  @Override
  public void end(boolean interrupted) {}
 
  // Returns true when the command should end.
  @Override
  public boolean isFinished() {
    return false;
  }
}

```

4. Test it out by holding the Romi in your hands and rotating it (no joystick input). It may be necessary to go to [wpilibpi.local](http://wpilibpi.local) and calibrate the gyro if it is drifting too much.
5. Create a plot of targetHeading vs ActualHeading in SmartDashboard.  Create another plot of motorTurnEffort.  What happens as you turn?

> ### Race Time!
> See who can drive the furthest down the hallway - **WITHOUT USING ANY STEERING**. Only use the throttle.   Anything different happen?

## Challenge: Read gyro data on big robot
**Note: Before starting this lesson, verify your Robot Builder code works on the robot.**
1. Load up your code from RobotBuilder.
2. Edit the drive command to output the gyro data to SmartDashboard.
3. Convert the code to operate like the Romi.

### Extra credit reading
[Control System Basics](https://docs.wpilib.org/en/stable/docs/software/advanced-controls/introduction/control-system-basics.html)

[PID Intro Video](https://docs.wpilib.org/en/stable/docs/software/advanced-controls/introduction/pid-video.html)

[PID real hardware tuning demo](https://youtu.be/fusr9eTceEo)

[Slew Rate Limiter](https://docs.wpilib.org/en/stable/docs/software/advanced-controls/filters/slew-rate-limiter.html#slew-rate-limiter)

Next lesson: Kinematics and Odometry for PathWeaver following.
[WPILib Path Planning](https://docs.wpilib.org/en/stable/docs/software/pathplanning/index.html)
