/*----------------------------------------------------------------------------*/
/* Copyright (c) 2017-2018 FIRST. All Rights Reserved.                        */
/* Open Source Software - may be modified and shared by FRC teams. The code   */
/* must be accompanied by the FIRST BSD license file in the root directory of */
/* the project.                                                               */
/*----------------------------------------------------------------------------*/

package org.usfirst.frc.team72.robot;

import edu.wpi.first.wpilibj.AnalogGyro;
import edu.wpi.first.wpilibj.IterativeRobot;
import edu.wpi.first.wpilibj.Joystick;
import edu.wpi.first.wpilibj.Spark;
import edu.wpi.first.wpilibj.SpeedControllerGroup;
import edu.wpi.first.wpilibj.Timer;
import edu.wpi.first.wpilibj.Victor;
import edu.wpi.first.wpilibj.drive.DifferentialDrive;
import edu.wpi.first.wpilibj.interfaces.Gyro;

/**
 * The VM is configured to automatically run this class, and to call the
 * functions corresponding to each mode, as described in the IterativeRobot
 * documentation. If you change the name of this class or the package after
 * creating this project, you must also update the manifest file in the resource
 * directory.
 */
public class Robot extends IterativeRobot {
	private Joystick m_stick = new Joystick(0);
	private Timer timer = new Timer();
	int autonmode;
	double time;
	Victor rightfront = new Victor(0);
	Victor rightrear = new Victor(1);
	Victor leftfront = new Victor(2);
	Victor leftrear = new Victor(3);
	SpeedControllerGroup leftside = new SpeedControllerGroup(leftfront, leftrear);
	SpeedControllerGroup rightside = new SpeedControllerGroup(rightfront, rightrear);
	DifferentialDrive drivetrain = new DifferentialDrive(leftside, rightside);
	Gyro gyro = new AnalogGyro(1);
	double Kp = 0.03;
	/**
	 * This function is run when the robot is first started up and should be
	 * used for any initialization code.
	 */
	@Override
	public void robotInit() {
	}

	/**
	 * This function is run once each time the robot enters autonomous mode.
	 */
	@Override
	public void autonomousInit() {
		timer.reset();
		timer.start();
		autonmode = 1;
		gyro.reset();
		
	}

	/**
	 * This function is called periodically during autonomous.
	 */
	@Override
	public void autonomousPeriodic() {
		time = timer.get();
		if (time < 1.0) {
			float angle = (float) gyro.getAngle(); // get heading
			drivetrain.tankDrive(-1.0, -angle * Kp); // turn to correct heading
		} 
		
		else {
			drivetrain.arcadeDrive(0.0, 0.0); // stop robot
			timer.reset();
			timer.start();
		}
		
		/*if(autonmode == 1) {
			if (time < 2.0) {
				drivetrain.arcadeDrive(0.5, 0.0); // drive forwards half speed
			} 
			
			else {
				drivetrain.arcadeDrive(0.0, 0.0); // stop robot
				timer.reset();
				timer.start();
				autonmode++;
			}	
		}*/
		
		if(autonmode == 2) {
			if (time < 2.0) {
				drivetrain.arcadeDrive(0.5, 0.0); // drive forwards half speed
			} 
			
			else {
				drivetrain.arcadeDrive(0.0, 0.0); // stop robot
				timer.reset();
				timer.start();
				autonmode++;
			}	
		}
		
		if(autonmode == 3) {
			if (time < 2.0) {
				drivetrain.arcadeDrive(0.5, 0.0); // drive forwards half speed
			} 
			
			else {
				drivetrain.arcadeDrive(0.0, 0.0); // stop robot
				timer.reset();
				timer.start();
				autonmode++;
			}	
		}
		
		if(autonmode == 4) {
			if (time < 2.0) {
				drivetrain.arcadeDrive(0.5, 0.0); // drive forwards half speed
			} 
			
			else {
				drivetrain.arcadeDrive(0.0, 0.0); // stop robot
				timer.reset();
				timer.start();
				autonmode++;
				
			}	
		}
	}

	/**
	 * This function is called once each time the robot enters teleoperated mode.
	 */
	@Override
	public void teleopInit() {
	}

	/**
	 * This function is called periodically during teleoperated mode.
	 */
	@Override
	public void teleopPeriodic() {
		drivetrain.arcadeDrive(m_stick.getY(), m_stick.getX());
	}

	/**
	 * This function is called periodically during test mode.
	 */
	@Override
	public void testPeriodic() {
	}
}
