---
layout: post
title: Noisy Signals, State Machines
---

Sharknado has been progressing rapidly. Last time we checked in we were able to navigate to a fixed compass heading using the PID controller. Since then we have added GPS support. We are now able to navigate to arbitrary lat/lng waypoints using the GPS and magnetometer sensors. One sensor, that we had to abandon however, is the quadrature encoders on the wheels. We discovered the DC motors introduce a lot of noise into our system which can be confused for encoder signals since they operate on interrupts. We tried to mitigate the noise issues, however, we made an executive decision to just move on w/o them instead of wasting more time. The encoders would have been a nice feature but are not critical for navigation. 

Regardless we are now able to navigate to a lat/lng with pretty good accuracy +/- 3 meters on average using only GPS and magnetometer. Our next step is to navigate to multiple lat/lng's in a series and drop a golf ball at the points. Also, we have the added requirement to avoid obstacles in our way. In order to implement this system we decided to utilizes a state machine design pattern.  The alternative to this is source code riddled w/ if statement logic. This system has worked well for us; see the diagram below:

![state machine](/public/images/Sharknado_state.png)

The system starts in the `Start` state, where it initializes the sensors: GPS fix, magnetometer calibration, etc. It will then transition to the `Search` state which is where most of the time is spent. 

The `Search` state samples the GPS sensor and Mag. sensor and updates the motor controller. If the GPS looses a fix of the mag. breaks, it will transition back to the `Start` state where it will stop the motors and try to re-initialize the sensors. If a beacon is detected, it will transition to the `Beacon` state.

In the `Beacon` state we try to get as close to the beacon as possible using the RF antenna. Once we get an RSSI reading of >90 we will stop sharknado and drop a golfball (payload delivery system powered by a servo). We then will tell the system load the next lat/lng and transition back to the `Search` state. However, if there are no more lat/lngs in the queue it will transition to the `Stop` state where it will wait.

Also, if an obstacle is detected it will transition to the `Escape` state where we will use ultra-sonic sensor readings to clear the obstacle and then transition back to `Search` state.

Another added benefit of the state machine design pattern is that is clears up the main render loop. It is nice and clean:

<script src="https://gist.github.com/eggie5/11313714.js"></script>

Here is the sharknado state machine in action:

<iframe width="560" height="315" src="//www.youtube.com/embed/FLbw3nqLap8?rel=0" frameborder="0" allowfullscreen></iframe>


--Alex