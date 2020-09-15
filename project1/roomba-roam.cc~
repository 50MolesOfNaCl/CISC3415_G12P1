/*
 *  roomba-roam.cc
 * 
 *  Modified code for a robot that is suitable for use with the Roomba
 *  and Create. Robot moves in a square trajectory. Command Line arguments give user option to enter number of squares for robot to complete. As well, as option to adjust speed
 * 
 *  Based on an example provided by Monica Anderson and Jeff Forbes,
 *  via Carlos Jaramillo, and changed to (hopefully) make it easier to
 *  understand.
 *
 *  Group 12: Jennie Kang, Edward Lam, Jamila Toaha
 *  Date:        16th September 2020
 *  Last change: 15th September 2020
 *  
 */
#define _USE_MATH_DEFINES

#include <iostream>
#include <cstdlib>
#include <libplayerc++/playerc++.h>
#include <math.h>

//Program can take command line arguments: 
	//argv[1] = number of complete squares robot should make, default 1
	//argv[2] = speed robot should travel at, default .5
int main(int argc, char *argv[])
{  
  using namespace PlayerCc;  

  // Set up proxy. Proxies are the datastructures that Player uses to
  // talk to the simulator and the real robot.

  PlayerClient    robot("localhost");  
  Position2dProxy pp(&robot,0);       // The 2D proxy is used to send 
                                      // motion commands.

  //How many complete squares robot should make
  double numSquares = argc >= 2 ? atof(argv[1]) : 1; 
				      // setting default to 1 complete square as per request intruction professor
	                              // numTurns * 4 = 1 square 
			 	      // numOfSquares * x, for when you want multiple runs.

  double defaultSpeed = argc >= 3 ? atof(argv[2]) : .5; 
				      // setting default to 1 complete square as per request intruction professor
				// Speed at which robot should move forward

  int timer = 0;                      // A crude way to time what we do
                                      // is to count.

  double turnrate, speed;	      // Kept for telemetry

  int numTurns = 0; 		      // Track each forward and turn the robot makes, tracks number of complete clock cycles(0 to interval2), i.e. 4 makes a complete square trajectory


  //Interval settings - our settings gives 6 seconds for robot to move forward and 3 seconds to turn
  int forwardInterval = 30 / defaultSpeed;  //0 to forwardInterval, adjust for defaultSpeed so that it reaches 3 meters i.e. move forward for 3 seconds if speed is 1, and for 6 seconds if defaultSpeed is .5


  double turnInterval = 90; 	      //interval1 to interval2, i.e. 9 seconds. 


  // Allow the program to take charge of the motors (take care now)
  pp.SetMotorEnable(true);

  // Control loop
  while(true) 
    {    

      // Increment the counter.
      timer++;

      // Read from the proxies.
      robot.Read();

      // First make the rbot go straight ahead, then make it turn, and 
      // finally make it stop.


 		if(timer < forwardInterval ){
		//0 to forwardInterval, Go forward
		speed = defaultSpeed; 
		turnrate = 0; 
     		}
    		else
      		if((timer >= forwardInterval ) && (timer < turnInterval)){
		//forwardInterval to turnInterval, Start turning
		speed = 0;
		  if(timer % 10 == 0) {
		    std::cout << "timer mod"<< timer << std::endl;
		  int  divideDegrees = (turnInterval - forwardInterval)/10; //added this so it can work for any speed

		   turnrate = -(M_PI/2/divideDegrees); //; <-this is problematic, it seems robot internal program probably rounds this off if divided by 2 large of a number like 30. //ties turnrate to time, i.e. 90 degrees distributed over this interval. The greater the diff the slower it turns
        	  }

       		 }
       		else{ //timer == forwardInterval
		timer = 0; //reset timer
		speed = 0;
		turnrate = 0;
		numTurns++;
		if(numTurns == 4*numSquares) break;
        std::cout << "----------------------------------"<< std::endl;
        	}
	
     
      
      // Print out what we decided to do?
      std::cout << "Turn: " << (numTurns/4)+1 << speed << std::endl;
      std::cout << " | Timer: " << timer << std::endl;  
      std::cout << " | Speed: " << speed << std::endl;      
      std::cout << " | Turn rate: " << turnrate << std::endl << std::endl;
    
      // Send the motion commands that we decided on to the robot.
      pp.SetSpeed(speed, turnrate);  
    }
  
}


