/*----------------------------------------------------------------------------*/
/* Copyright (c) 2017-2018 FIRST. All Rights Reserved.                        */
/* Open Source Software - may be modified and shared by FRC teams. The code   */
/* must be accompanied by the FIRST BSD license file in the root directory of */
/* the project.                                                               */
/*----------------------------------------------------------------------------*/

package org.usfirst.frc.team72.robot;


import edu.wpi.first.wpilibj.ADXRS450_Gyro;
import edu.wpi.first.wpilibj.IterativeRobot;
import edu.wpi.first.wpilibj.Joystick;
import edu.wpi.first.wpilibj.SpeedControllerGroup;
import edu.wpi.first.wpilibj.Timer;
import edu.wpi.first.wpilibj.Victor;
import edu.wpi.first.wpilibj.drive.DifferentialDrive;


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
	Victor intake = new Victor(4);
	Victor outtake = new Victor(5);
	SpeedControllerGroup leftside = new SpeedControllerGroup(leftfront, leftrear);
	SpeedControllerGroup rightside = new SpeedControllerGroup(rightfront, rightrear);
	DifferentialDrive drivetrain = new DifferentialDrive(leftside, rightside);
	ADXRS450_Gyro gyro = new ADXRS450_Gyro();
	int gangle;
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
		gangle = (int) gyro.getAngle();
		if(gangle > 0 && gangle < 180) {
			drivetrain.tankDrive(0.5, -0.5);
		}
		if(gangle < 360 && gangle > 180) {
			drivetrain.tankDrive(0.5, -0.5);
		}
		
		
		// move
		if(autonmode == 1) {
			if (time < 0.2) {
				drivetrain.tankDrive(-0.5, 0.0); // drive forwards half speed
				intake.set(1.0);
			} 
			
			else {
				drivetrain.tankDrive(0.0, 0.0); // stop robot
				intake.set(0.0);
				timer.reset();
				timer.start();
				autonmode++;
			}	
		}
		/*
		// turn
		if(autonmode == 2) {
			if (time < 0.8) {
				drivetrain.tankDrive(-0.5, 0.5); 
			} 
			
			else {
				drivetrain.arcadeDrive(0.0, 0.0); // stop robot
				timer.reset();
				timer.start();
				autonmode++;
			}	
		}
		// move forward
		if(autonmode == 3) {
			if (time < 0.5) {
				drivetrain.arcadeDrive(-0.5, 0.0); // drive forwards half speed
			} 
			
			else {
				drivetrain.arcadeDrive(0.0, 0.0); // stop robot
				timer.reset();
				timer.start();
				autonmode++;
			}	
		}
		// shoot balls
		if(autonmode == 4) {
			if (time < 2.0) {
				outtake.set(1.0);
			} 
			
			else {
				drivetrain.arcadeDrive(0.0, 0.0); // stop robot
				outtake.set(0.0);
				timer.reset();
				timer.start();
				autonmode++;
				
			}	
		}*/
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
