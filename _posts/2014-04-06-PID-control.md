---
layout: post
title: PID Control
---

A common technique in robotic controls in called PID (proportional-integral-derivative). This is a feedback mechanism designed to cause an output to converge upon an input. For example, in order to go 3 m/s we would sample the current speed of the car and take the difference of that and 3 m/s and if that is > 0 we decelerate or if it is < 0 we accelerate. This cycle repeats until convergence. 

![diagram](https://upload.wikimedia.org/wikipedia/commons/thumb/9/91/PID_en_updated_feedback.svg/500px-PID_en_updated_feedback.svg.png)
In the diagram above (from wikipedia) u is the output to the system and e is the difference between expected and actual. As the system compensates for the error (diff of actual and expected) it compensates by increasing or decreasing the output. With this system, rather than having implementation details about output machine, you simply increase/decrease output based on inputs. Kp Ki Kd, are weight coefficients for the inputs and must be adjusted w/ experimentation to achieve the response you want. A visual example of the system response is below:

![response](http://upload.wikimedia.org/wikipedia/commons/2/2b/Change_with_Kp.png)
In this example we want to get to 1 from a start of 0. As you can see w/ different values of coefficients we have different tradeoffs between speed/accuracy/oscillation. However, eventually, the system should converge.

Sharknado is using this for speed control and steering control. 


![PID output](/public/images/PIDoutput.png)
Focus on the pink highlights for now. In this example, I programmed Sharknado to drive to heading 0 (North). On the first output you can see that our orientation was 0.52, almost dead-north, so we only adjust a little as indicated by motor_turning_coeff of 3. Then we notice the next sample is at heading: 358, so we adjust to the right a little harder at a magnitude of 22. Then the next sample is at orientation of 359 degrees and we adjust turing accordingly. 

The speed control is in the green, however, I didn't have it running for this demo. However, you can still see the PID working as the expected speed is 0, and the speed coefficient is 0, meaning the system has converged and is at perfect equilibrium. 


This PID control system should allow sharknado to run at a constant speed and heading. The PID system has advantages over the alternative which would be simply have implementation knowledge of your motors and telling them to go x m/s and and an angle theta.



--Alex