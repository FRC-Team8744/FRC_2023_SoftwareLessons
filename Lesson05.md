# Simple Motor Control
### Fix Robot Builder motor driver support
[CTRE Installer](https://docs.ctre-phoenix.com/en/stable/ch05_PrepWorkstation.html?highlight=robotbuilder#option-1-windows-installer-strongly-recommended)

[REV Robotics Installer](https://docs.revrobotics.com/sparkmax/rev-hardware-client/getting-started-with-the-rev-hardware-client#installation-instructions)


## Open Loop (no sensor feedback)
1. Rebuild the Scrappy test base software using Robot Builder.
> Hints:
> - [Lesson02](Lesson02.md)
> - [Robot Builder Tank Drive](https://docs.wpilib.org/en/stable/docs/software/wpilib-tools/robotbuilder/writing-code/robotbuilder-drive-tank.html)
> - or Cheat: [Scrappy YAML for Robot Builder](./Lesson05_files/Scrappy.yaml)
2. Verify that you can connect to Scrappy and spin the wheels correctly.
3. Verify REV motor and SparkMax wiring: [REV Wiring](https://docs.revrobotics.com/sparkmax/gs-sm/wiring-the-spark-max)
4. If you haven't already, verify that the SparkMax has a CAN address (between 1 and 62) and spin the motor with the configuration software.  [Motor Test](https://docs.revrobotics.com/sparkmax/gs-sm/connect-a-spark-max-over-usb)
5. Create a subsystem called "Shooter" to control the REV motor.  You will need methods for: **SpinMotor, StopMotor**
> Hints:
> - [Creating a subsystem](https://docs.wpilib.org/en/stable/docs/software/commandbased/subsystems.html#simple-subsystem-example)
> - Here is some example code to get you started: (must be modified)
>```
>import com.revrobotics.CANSparkMax;
>import com.revrobotics.CANSparkMaxLowLevel.MotorType;
>
>private static final int SparkMaxDeviceID = 5;
>private CANSparkMax m_motor;
>m_motor = new CANSparkMax(SparkMaxDeviceID, MotorType.kBrushless);
>m_motor.restoreFactoryDefaults();
>m_motor.set(0.0);
>```
> - Ask questions!

6. Once you have methods to call, create commands for: **SpinShooter, StopShooter**.  Hint: [Creating a command to do something](https://docs.wpilib.org/en/stable/docs/software/commandbased/commands.html)
> **DON'T FORGET SAFETY: SET THE SPEED TO ZERO IN end()**
>
> **SET A LOW SPEED (LIKE 0.1) FOR TESTING**

7. Bind joystick buttons to the commands.  [Binding a command to a joystick button](https://docs.wpilib.org/en/stable/docs/software/commandbased/binding-commands-to-triggers.html#binding-a-command-to-a-joystick-button)

8. Build your project and try it out on the robot!

## Challenge: Make the shooter change speed based on a joystick axis.

### Extra credit: Help attach an encoder for precision speed control.
Next time: [Velocity Closed Loop Control](https://github.com/REVrobotics/SPARK-MAX-Examples/tree/master/Java/Velocity%20Closed%20Loop%20Control)

### Homework:
[REV SparkMax Code Examples](https://github.com/REVrobotics/SPARK-MAX-Examples)

[FRC 0 to Autonomous (Team 6814)](https://www.youtube.com/watch?v=ihO-mw_4Qpo)
