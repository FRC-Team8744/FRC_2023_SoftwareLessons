# Creating our own subsystem

Now that we can drive the base around, we need to be able to control a shooter, right?  The first step is being able to set a constant velocity for shooter flywheel's motor.  A brushless motor will be used, but we don't have that driver available in RobotBuilder. So what do we do?

**Note: Before starting this lesson, verify your Robot Builder code works on the robot.**

## Exercise: Build a test setup and spin a motor!
1. First, install the [REV Robotics Hardware Client](https://docs.revrobotics.com/sparkmax/rev-hardware-client/getting-started-with-the-rev-hardware-client#installation-instructions) on your laptop.
2. Using the robot test base (Scrappy!), connect a Spark Max controller and a NEO motor to the Power Distribution Panel following the [REV Getting Started With Spark MAX Guide](https://docs.revrobotics.com/sparkmax/gs-sm). **Ask for help adding the Anderson Powerpole connectors.**
3. Make sure the NEO motor is secured so that it doesn't damage anything when it starts to spin.
4. Write the CAN address you use on a piece of masking tape with a Sharpie, and attach it to the Spark Max.
5. Make it spin!
6. Finish the configuration: Set the Spark Max to **Brake Mode** when idle and save before powering down.
7. Make the robot safe for others and clean up any tools.

### Challenge: Add a shooter subsystem and buttons to start/stop
1. Bring up RobotBuilder.  Create a subsystem called Shooter and add a Victor SPX as a placeholder.
2. Create commands for RunShooter and StopShooter.
3. Create two joystick buttons and connect them to the commands for RunShooter and StopShooter.
4. Export your Java code.
5. In VS Code, create a new subsystem called Shooter2.  We will connect this to buttons after it is set up.
6. Open the subsystem Shooter2 and create a method to set the motor speed. (Look at what we did in Drivetrain for a reminder.)

    Hints:

    Syntax for initializing a new motor controller object:
    ```
    CANSparkMax m_shootMotor;
    m_shootMotor = new CANSparkMax([CANiD number], MotorType.kBrushless);
    m_shootMotor.restoreFactoryDefaults();
    ```
    Syntax for setting the motor speed:
    ```
    m_shootMotor.set(speed);
    ```
    **SET A LOW SPEED (LIKE 0.1) FOR TESTING**
7. Once you have a method in the subsystem, open up the commands for RunShooter and StopShooter.  Edit them to call your new method in Execute.  **DON'T FORGET SAFETY: SET THE SPEED TO ZERO IN end()**
8. In RobotContainer, delare Shooter2 like Shooter.  Then modify the button bindings to point to Shooter2.
9. Build your project and try it out on the robot!

[REV SparkMax Documentation](https://docs.revrobotics.com/sparkmax/)

### Libraries: Download and install motor driver code
[Phoenix Framework Installer (Victor Motor Driver)](https://store.ctr-electronics.com/software/)

[Kauai Labs (Gyro/IMU)](https://pdocs.kauailabs.com/navx-mxp/software/roborio-libraries/)

[REV Robotics (SparkMax Motor Driver)](https://docs.revrobotics.com/sparkmax/software-resources/spark-max-api-information)

### Consider adding [Robot Builder Extensions](https://github.com/GraniteCityGearheads3244/RobotBuilderExtensions)