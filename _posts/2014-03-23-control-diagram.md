---
layout: post
title: Software Control Diagram
---

This is the software control diagram I just designed:

![Alt text](/public/images/sharknado-control.png)

The majority of the work is in the circle delta s & delta theta control modules. Every clock cycle, they take the differnce between the expected value and the actual value and +1/-1 respectivly. That new value is sent to the motors and the feedback look continues.

Also, another important aspect of the design, is the center node, which has the current lat, lng position and compass heading. These values are the samples from the sensors (GPS, Encoders, Compass) after they have been Kalman filtered. A Kalman filter may be a little overkill for the project and I may switch to something simpiler, like a complimentary filter.

I still need to add in the routines for beacon detection and obstacle avoidance.

-- Alex Egg