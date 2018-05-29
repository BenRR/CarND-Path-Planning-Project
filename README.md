# CarND-Path-Planning-Project
Self-Driving Car Engineer Nanodegree Program

### Goals
In this project your goal is to safely navigate around a virtual highway with other traffic that is driving +-10 MPH of the 50 MPH speed limit. You will be provided the car's localization and sensor fusion data, there is also a sparse map list of waypoints around the highway. The car should try to go as close as possible to the 50 MPH speed limit, which means passing slower traffic when possible, note that other cars will try to change lanes too. The car should avoid hitting other cars at all cost as well as driving inside of the marked road lanes at all times, unless going from one lane to another. The car should be able to make one complete loop around the 6946m highway. Since the car is trying to go 50 MPH, it should take a little over 5 minutes to complete 1 loop. Also the car should not experience total acceleration over 10 m/s^2 and jerk that is greater than 10 m/s^3.

### Solution Decisions
This solution is mainly based on the project walk through video. Below are the main solution decisions

* Calculate two path points from the last two previous path points if available. If not use current car location and speed go backwards (line 299 - 322)
* Work out target lane (stay or change lane see details below) then create 3 far future path points as anchor points (line 325 - 341)
* Use spline to fit these 5 path points (line 351 - 352)
* Keep all previous path points if any left in the new path points
* Create new path points until we have 50 points
* To make new points smooth calculate a spline point every 20ms * speed in a total 30 meters on x axis (line 363 - 388)
* Speed calculation in details section below


### Other Details
The lane and speed logics are listed below

* Go through all other car information and loop over all three lanes to filter out all available lanes line 255 - 273

 * if any car is in front and distance less than 30 meters then mark the lane not available
  * if any car is not on the same lane but behind you in less 5 meters mark the lane not available

* Go through all filtered available lanes line 275 - 289

 * if current lane is available and car is slower than 49.5 then increase speed
 * if current lane is not available try to switch to left lane and if left lane not available try right
 * if no lane is available slow down
