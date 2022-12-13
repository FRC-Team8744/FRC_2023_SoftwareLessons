# Debugging and tuning a PID controlled motor

## Create a known good starting point
1. Before starting, download the zip file of code from: [Robot Builder Extensions](https://github.com/GraniteCityGearheads3244/RobotBuilderExtensions)
2. Copy the directories for "Spark MAX" and "NavX Micro".  Place them in the directory- C:\Users\Public\wpilib\2022\Robotbuilder\extensions
> **IMPORTANT** - Before the next step, open VS Code and **close** the project folder.  Then close VS Code.  If you don't do this, VS Code will try to "help" and recover some files.
3. Go to your FRC2023 directory and delete the "Scrappy" directory.
4. Open up Robotbuilder (verify that a Spark MAX motor object is there) and regenerate Scrappy from scratch using the YAML file from [Lesson05](https://github.com/FRC-Team8744/FRC_2023_SoftwareLessons/blob/main/Lesson05_files/Scrappy.yaml).
> **IMPORTANT** - Do not trust Robotbuilder when it first loads.  It is necessary to explicitly reload the YAML file - even if Robotbuilder seems to bring it up when it starts.
5. Verify that the CAN ID numbers are correct (they are NOT).
6. Create a new subsystem called "Spinner" and put a Spark MAX motor "SpinMotor" in it. (Check SmartDashboard)
7. Create a command called "Spin1" and give it one parameter called "setpoint". Set a "default" preset of 0.1
7. Create a command called "Spin2" and give it one parameter called "setpoint". Set a "default" preset of 1.0
8. Create another command called "DisableSpinner" (no parameters).
9. Link each command to a joystick button.
10. Export from Robotbuilder, build in VS Code and fix errors
> **Remember:** this is a 'new' project, so you have to offline install the REV and CTRE-Phoenix libraries again.
11. In Spinner.java, comment out the line for "burnFlash".  Add the following:
```
    // Put methods for controlling this subsystem
    // here. Call these from Commands.
    public void setMotor(double setpoint) {
        spinMotor.set(setpoint);
    }

    public void stopMotor(){
        spinMotor.stopMotor();
    }
```
12. In Spin1 and Spin2, add the following to execute():
```
    public void execute() {
        m_spinner.setMotor(m_setpoint);
    }
```
13. In DisableSpinner, add the following to end() and set return for isFinished() to true:
```
    public void end(boolean interrupted) {
        m_spinner.stopMotor();
    }
```
14. In Drivetrain: Create methods to send direction and speed updates to the motors.
```
    // Put methods for controlling this subsystem here
    public void teleop(double throttle, double steering) {
        diffDrive.arcadeDrive(throttle, steering);
    }

    public void stop() {
        diffDrive.arcadeDrive(0.0, 0.0);
    }
```
15. In the Teleop command, send the joystick signals to the drivetrain:
```
// Add to execute()
double throttle = RobotContainer.getInstance().getJoystick1().getY();
double steering = RobotContainer.getInstance().getJoystick1().getX();
m_drivetrain.teleop(throttle, steering);

// Add to end()
m_drivetrain.teleop(0.0, 0.0);
```
16. Build, Deploy, **Test**

## Test debug code
15. Modify the Spinner subsystem to include the debug code shown in FRC_2023_SoftwareLessons\Lesson06_files (or just copy over Spinner.java).
16. Test built-in encoder (hall effect sensors) reading using Shuffleboard. (rotate the motor and see if the numbers change)
### Test Velocity PID
17. Test speed setpoint PID. Enable the "VELOCITY_TEST" constant.
### Test Position PID
18. Test position setpoint PID. Enable the "POSITION_TEST" constant. (turn off other tests)
### Test External Encoder
19. Power off the robot and switch the sensor cable.
20. Enable the "EXTERNAL_SENSOR_TEST" constant and repeat the tests.

### Challenge
Start the process of installing and reading from wheel encoders.

# Important Homework
* Sign yourself up for a github account before next week.

#### References used in this lesson
[Rev Robotics Encoder Example](https://github.com/REVrobotics/SPARK-MAX-Examples/tree/master/Java/Encoder%20Feedback%20Device)
