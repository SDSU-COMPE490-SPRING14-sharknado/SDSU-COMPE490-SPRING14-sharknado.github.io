---
layout: post
title: GPS Milestone
---

I've been prototyping with our GPS unit. My first goal was to read my current location. This was pretty straight-forward to get going. I left the unit running in one spot on my desk for 10 min to get an idea of the GPS jitter. 

![GPS Jitter](/public/images/gps_samples.png)

Very interesting results: The above plot has > 400 samples, however, the values of the longitude are all constant and the latitude only changes 3 times. So, in this test, the GPS is very stable. I still need to test it walking in the field at school.

Next, I wanted to write the routine that will calculate how far it is to another GPS location. On paper, this is a simple Pythagorus trig problem, however, in application and across large geographies on a globe, this is non-trivial. Others, since the '70s have been researching this problem the most popular routine is called: Vincenty's formula. Sharknado implements Vincenty's Inverse formula to computes the geographical distance and azimuth (heading) between two given points. Vincenty's original research is here:


  [ http://www.ngs.noaa.gov/PUBS_LIB/inverse.pdf ]( http://www.ngs.noaa.gov/PUBS_LIB/inverse.pdf  "Title")
 
 Vincenty's algorithm seemed interesting in theory and was proven in practice, however, it was difficult for me to adapt to code. Thankfully, I found it already implemented in Google's Android project and I quickly adapted it from java to c++ for sharknado.

So here's the test:

I want to calculate the distance and heading to the house directly east across the street from me. So since it is directly east the heading should be about 270 degrees and according to Google maps, about 27 meters. 

![Google maps](/public/images/mapstreet.png)

Lets see our new algorithm works:

![Alt text](/public/images/gps_log.png)

And there we, go: as expected we need to at a heading of 270 degrees for about 25 meters.

Now, theoretically, if we had a compass we should be able to follow that heading for 25 meters and make it to the target. This is what sharknado will do w/ the target beacons and the magnetometer sensor.

--Alex