# Initial Software Design

We are going to define our robot's capabilities, and then create a program structure that will fit.  We start out by asking - what do we want this robot to do?  We are starting with the pre-season mockup, so:

1. Drive around with the typical differential steering.
2. Shoot a ball a consistent distance and direction.
3. ???

This is close to how we will start the game season.  We will know what the robot needs to do, but not too much about *how*.  This means that we may have to do a lot of modification of our plans as we go on, so I will be introducing the concept of the tool Robot Builder.  With a graphical interface, we can define the major functions of the robot without needing to do a lot of typing - and we can adjust it as we go along.

Think of the commands we need the robot to perform:

1. Command: Driving in teleop mode
2. Command: Shooter spin up
3. Command: Shooter stop

## Exercise: Build a program to teleop drive the demo robot
Roughly following instructions in: [Robot Builder Tank Drive](https://docs.wpilib.org/en/stable/docs/software/wpilib-tools/robotbuilder/writing-code/robotbuilder-drive-tank.html)

1. Open up that folder on your desktop for **WPILib Tools**.
2. Start [Robot Builder](https://docs.wpilib.org/en/stable/docs/software/wpilib-tools/robotbuilder/introduction/robotbuilder-overview.html).
3. Create a Drivetrain subsystem. (from scratch)
4. Inside the Drivetrain add a **Differential Drive** object for a two motor drive. There is a left motor and right motor as part of the **Differential Drive** class.
Since we want to use more then two motors to drive the robot, inside the **Differential Drive**, create two **Motor Controller Groups**. These will group multiple motor controllers so they can be used with **Differential Drive**.
5. Add two **Victor SPX** modules in each **Motor Controller Group**.
6. Add a joystick to the Operator Interface.
7. Export the program to Java and check over the results.
8. Back to RobotBuilder: Create a method to send direction and speed updates to the motors on the subsystem.

```
    // Put methods for controlling this subsystem here
    public void teleop(double throttle, double steering) {
        differentialDrive1.arcadeDrive(throttle, steering);
    }

    public void stop() {
        differentialDrive1.arcadeDrive(0.0, 0.0);
    }
```

9. Create a command called **TeleopDrive**.  Its purpose will be to read the joystick values and send them to the **Drivetrain** subsystem. Notice that this command requires the Drivetrain subsystem. This will cause it to stop running whenever anything else tries to use the Drivetrain.
10. Export the program to Java and switch to VisualStudio.
11. Add the code to call the command:
```
 // Called every time the scheduler runs while the command is scheduled.

    @Override

    public void execute() {

        m_drivetrain.teleop(RobotContainer.getInstance().getJoystick1().getY(), RobotContainer.getInstance().getJoystick1().getX());

    }


    // Called once the command ends or is interrupted.

    @Override

    public void end(boolean interrupted) {

        m_drivetrain.teleop(0.0, 0.0);

    }
```

This adds code to the execute method to do the actual driving. The Joystick object axis inputs are passed to the Drivetrain subsystem. The subsystem just uses them for the arcade steering method on its DifferentialDrive object.

We also filled in the end() method so that when this command is interrupted or stopped, the motors will be stopped as a safety precaution.

12. The last step is to make the Teleop command be the “Default Command” for the Drivetrain subsystem. This means that when no other command is using the Drivetrain, the human operator will be in control. When the autonomous code is running, it will also require the Drivetrain and interrupt the Teleop command. When the autonomous code is finished, the Teleop command will restart automatically (because it is the default command), and the human operators will be back in control. If you write any code that does teleop automatic driving, those commands should also “require” the Drivetrain so that they will interrupt the Teleop command and have full control.
13. Export and test.

## Managing Code Libraries
Many component vendors provide software libraries that integrate into WPILib VS Code.  When creating a new project, it is often necessary to install the drivers for certain component.  Here are the instructions for the process: [Managing 3rd Party Libraries](https://docs.wpilib.org/en/stable/docs/software/vscode-overview/3rd-party-libraries.html?highlight=libraries#rd-party-libraries)

### Libraries: Download and install motor driver code
[Phoenix Framework Installer (Victor Motor Driver)](https://store.ctr-electronics.com/software/)

[Kauai Labs (Gyro/IMU)](https://pdocs.kauailabs.com/navx-mxp/software/roborio-libraries/)

[REV Robotics (SparkMax Motor Driver)](https://docs.revrobotics.com/sparkmax/software-resources/spark-max-api-information)

### Challenge: Add code to start and stop the shooter
We used a Victor SPX as a placeholder for the REV Robotics motor driver. Convert the subsystem to a REV SparkMax.

[REV SparkMax Documentation](https://docs.revrobotics.com/sparkmax/)
