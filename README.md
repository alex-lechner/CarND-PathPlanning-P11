# Path Planning
---

**Path Planning Project**

The goals / steps of this project are the following:

* The vehicle must safely navigate around a virtual highway
* The vehicle should drive as close as possible to the 50 MPH speed limit and pass slower traffic
* The vehicle should avoid hitting other cars and drive inside of the marked road lane
* The vehicle should be able to make one complete loop around the 6946m highway
* The vehicle should not experience total acceleration over 10 m/s^2 and jerk that is greater than 10 m/s^3

[//]: # (References)
[simulator]: https://github.com/udacity/self-driving-car-sim/releases
[win 10 update]: https://support.microsoft.com/de-de/help/4028685/windows-get-the-windows-10-creators-update
[uWebSocketIO]: https://github.com/uWebSockets/uWebSockets
[linux on win 10]: https://www.howtogeek.com/249966/how-to-install-and-use-the-linux-bash-shell-on-windows-10/
[MinGW]: http://www.mingw.org/
[CMake]: https://cmake.org/install/
[eigen lib]: http://eigen.tuxfamily.org/index.php?title=Main_Page
[udacity code]: https://github.com/udacity/CarND-Path-Planning-Project
[Path Planning Walkthrough]: https://www.youtube.com/watch?v=3QP3hJHm4WM

---

## Files Submitted & Code Quality

### 1. Submission includes all required files and every TODO task has been accomplished 

For this project, I have used the [Path Planning Project Starter Code][udacity code] from Udacity and I have modified the following files:
```cpp
main.cpp
spline.h
```
The `spline.h` file has been downloaded from http://kluge.in-chemnitz.de/opensource/spline/

### 2. Code must compile without errors

This project was done on Windows 10. In order to set up this project I had to:
* update my Windows 10 Version with the [Windows 10 Creators Update][win 10 update]
* install the [Linux Bash Shell][linux on win 10] (with Ubuntu 16.04) for Windows 10
* set up and install [uWebSocketIO][uWebSocketIO] through the Linux Bash Shell for Windows 10
* [download the simulator from Udacity][simulator]
* [download the Eigen library][eigen lib]  which is already part of the repo in the `src` folder

For a description of the sent back data from the simulator, please refer to the [DATA.md][data md] file.
**To update the Linux Bash Shell to Ubuntu 16.04 the Windows 10 Creators Update has to be installed!**

Also, [CMake][CMake] and a gcc/g++ compiler like [MinGW][MinGW] is required in order to compile and build the project.

Once the install for uWebSocketIO is complete, the main program can be built and run by doing the following from the project top directory in the Linux Bash Shell.

1. `mkdir build`
2. `cd build`
3. `cmake .. -G "Unix Makefiles" && make` on Windows 10 or `cmake .. && make` on Linux or Mac
4. `./path_planning`

Then the simulator has to be started and *Project 5: MPC Controller* has to be selected. When everything is set up the Linux Bash Shell should print: 
```bash 
Listening to Port 4567
Connected
```

### 3. Reflection
For this project, I referred to the [Path Planning Walkthrough by David Silver and Aaron Brown][Path Planning Walkthrough]. However, I have implemented the cost function/lane change behavior on my own. By using the sensor fusion data we can detect the position of other vehicles around us. We are using this information to measure the distance of cars nearby. If we get too close to the car in front of us we decelerate the vehicle and try to pass it by changing lanes. Furthermore, we check if other vehicles are around us (on the left and right lane) in order to change lanes safely. 
We only take measurements from vehicles heading in our direction of motion which also allows us to stay in our lanes and to not move beyond our lanes. If a car is on the left and/or right lane we set the `left_lane_car` and `right_lane_car` values on line 279 & 284  in `main.cpp` to `true` and prevent the vehicle from changing lanes if a car in front of us is driving slower than we do.

---

## Discussion

### 1. Briefly discuss any problems / issues you faced in your implementation of this project.
The vehicle detection was kind of challenging. However, my main issue was that I have initialized the flag `right_lane_car` (line 257 in `main.cpp`) as `true` instead of `false` which always - of course - led my vehicle to change to the left lane even though there was no car on the right side. Therefore, I was always stuck in lane 0 (the very left lane).