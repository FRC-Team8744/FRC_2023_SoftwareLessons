# Running start with Romi

Do not save any code to the Desktop!  It will quickly become difficult to organize and find what you need.

> Each laptop has a folder called C:\Users\\[PabloComputer].
> Create a folder named “FRC2023” and save all your code there.

## Visual Studio Shortcuts
* `Ctrl+Shift+P` brings up the command palette. (or click the red hexagon with the W)
* All First Robotics commands will start with `WPILib:`
* `F5` simulates the code (or runs a Romi)
* `Ctrl+F5` loads the code into a RoboRio (the competition robot)

## Documentation
https://docs.wpilib.org

This is your best resource.  Remember it!
(Make sure you are reading the most up to date version)
We will be using Java to program the robots to keep things simple.  If you see a code example, sometimes you have to switch it to a Java view.

### Important Documentation Sections:
* [Zero to Robot](https://docs.wpilib.org/en/stable/docs/zero-to-robot/introduction.html)
* [VS Code Overview](https://docs.wpilib.org/en/stable/docs/software/vscode-overview/index.html)
* [Basic Programming](https://docs.wpilib.org/en/stable/docs/software/basic-programming/index.html)
* [Getting Started with Romi](https://docs.wpilib.org/en/stable/docs/romi-robot/index.html)
* [Command-based programming](https://docs.wpilib.org/en/stable/docs/software/commandbased/index.html)

## Quick Start
1. Boot up your Romi. (There is a very small button labeled "Power" on the corner of the board)
2. Find your Romi WiFi access point and connect to it. (you will temporarily lose Internet access)
3. Make sure you have a joystick or controller connected to your laptop.
4. Start Visual Studio Code (2022 WPILib version).
5. Type `Ctrl-Shift-P` (or push the little red hexagon with a W) and select `WPILib:Create a new project`.
    * Select Example -> Java -> RomiReference.
    * The base folder is your FRC2023 folder!
    * Yes, create a new folder from the project name (your choice).
    * Team Number: 8744 (important when using the big robot)
    * Do **Not** enable desktop support.
6. Once it is done configuring, type `F5` to compile the code and start a simulation to run the Romi.
7. Look for data from the accelerometer to verify that you have a connection to the Romi.
8. Add your joystick/controller as an input device. (Drag "System Joysticks" to "JoySticks")
9. In the driver station simulation, change **Robot State** to "Teleoperated".
9. **Drive!**

### Coding Exercise - Customise the joystick
1. Shutdown the simulation (close the simulation window or click on the red square in VSC).
2. Open the file named RobotContainer.java and find the method `getArcadeDriveCommand()`.
3. Change the axis numbers so that throttle and steering are on the same control stick (or however you like).  Remember that you can see the joystick data in the simulation window.
4. Hit `F5` to restart the simulation and test it out.
5. You will notice one control has a negative sign on it to get the direction right.  Try multiplying an axis by 0.5 to see if you can adjust the sensitivity of the joystick.

## Software Structure
[Inroduction to FRC software development (old, but still very useful)](https://youtu.be/64hPDvphcfA)

As shown in the video, we use commands to 'do things' on the robot and subsystems to interact with the actual hardware.  For our first coding challenge, we will create a command to light up the LED on the Romi and then bind that command to a button press.

### Coding Challenge - Bind a LED to a button
1. Shutdown the simulation (close the simulation window or click on the red square in VSC).
2. Right click on the `commands` folder and select the option at the bottom for `Create a new class/command`.  Select `Command (New)` from the list. Name it `UpdateLight`.
3. Make your code look like this:
```
// Copyright (c) FIRST and other WPILib contributors.
// Open Source Software; you can modify and/or share it under the terms of
// the WPILib BSD license file in the root directory of this project.
 
package frc.robot.commands;
 
import edu.wpi.first.wpilibj2.command.CommandBase;
import frc.robot.subsystems.OnBoardIO;
 
public class UpdateLight extends CommandBase {
  private final OnBoardIO m_romi_io;
  private final boolean YellowState;
  /** Creates a new UpdateLight. */
  public UpdateLight(OnBoardIO thisRomi, boolean YellowLED) {
    // Use addRequirements() here to declare subsystem dependencies.
    m_romi_io = thisRomi;
    addRequirements(thisRomi);
 
    YellowState = YellowLED;
  }
 
  // Called when the command is initially scheduled.
  @Override
  public void initialize() {}
 
  // Called every time the scheduler runs while the command is scheduled.
  @Override
  public void execute() {
    if (YellowState == true) {
      m_romi_io.setYellowLed(true);
    } else {
      m_romi_io.setYellowLed(false);
    }
 
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

4. Now go back to RobotContainer.java and find the method `configureButtonBindings()`.
5. After the section demonstrating the on-board button, add this:
```
    m_onboardIO.setYellowLed(false);  // Set default state
    JoystickButton ThisButton = new JoystickButton(m_controller, 1);
    ThisButton
      .whenActive(new UpdateLight(m_onboardIO, true))
      .whenInactive(new UpdateLight(m_onboardIO, false));
```
6. Clear up all warnings and errors (ask about QuickFix!), then start a simulation to see if it works!

[Documentation for Binding Commands to Joystick Buttons](https://docs.wpilib.org/en/stable/docs/software/commandbased/binding-commands-to-triggers.html)


## Useful Romi Documentation
[Romi Control Board Schematic](https://www.pololu.com/file/0J1258/romi-32u4-control-board-schematic-diagram.pdf)

[Romi Control Board Pinout](https://www.pololu.com/file/0J1261/romi-32u4-control-board-pinout-power.pdf)

[Romi Control Board User's Guide](https://www.pololu.com/docs/0J69)
