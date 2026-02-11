+++
title = "Lab2: IMU"
date = "2026-02-11"
+++

In this lab, I integrated an inertial measurement unit (IMU) sensor into the autonomous robot platform I previously built, using the SparkFun RedBoard Artemis Nano microcontroller. The goal is to sense the robot's motion and orientation in real time. Signal processing techniques were applied to achieve reliable state estimation.

# Setup IMU
First, I set up the sensor and established communication between it and the board using the I2C interface. Then, I connected the one-to-three port QWIIC connector to the processor and IMU using the braided I2C QWIIC cable. This cable connects VCC, GND, SDA, and SCL to the corresponding microcontroller pins.

{{ image(path="content/posts/lab2/IMU_setup.png", alt="setup", width=400, class="center" )}}

For the software, I installed the SparkFun 9DOF IMU Breakout ICM 20948 Arduino Library from the Arduino Library Manager. 

Finally, I ran the Example1_Basics test program to verify that the board could successfully initialize the sensor and read the accelerometer and gyroscope values.

[Video Here](https://youtube.com/shorts/Umetk0np0nc)
<div style="width:100%;height:0;position:relative;padding-bottom:64.923%;">
  <iframe
    src="https://youtube.com/shorts/Umetk0np0nc"
    frameborder="0"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
    allowfullscreen
    style="width:100%;height:100%;position:absolute;left:0;top:0;overflow:hidden;">
  </iframe>
</div>

The sensor generates values based on the right hand rule.
The accelerater provided linear acceleration along the x, y, and z axes. When I placed the sensor on the table in stationar, both acceleration on the x and y axes ranged around 0, while the magnitude on z axis was approximately 1 g. When we rotate it on  x and y axes, the gravity changes. So, we can use it to estimate the gravity vector and compute static orientation (pitch and roll). However, there are spikes during rapid movement, aking the raw values less reliable for direct angle estimation.

The gyroscope measured angular velocity, responding smoothly to rotations. When I placed the sensor on the table, all three components read 0. As I rotated the board, the values increased.

The `AD0_VAL` parameter indicates the value of the last bit of the I2C address.On the SparkFun 9DoF IMU breakout, this bit depends on the state of the ADR jumper. When the jumper is open, the pin is pulled high and the address bit equals 1; when the jumper is closed, the bit becomes 0. In this case, we select one to match the hardware configuration..

I will also add a visual indicator to show that the board is running and collecting data for later use in the lab. The LED will blink three times slowly when the board starts up. It will also turn on every time the `START_COLLECT_DATA` command is called and turn off when the host stops collecting data and requests it.

# Accelerometer


# Record a stunt
After playing around with the car, I found that I could make it flip over by quickly switching between forward and backward. This feature allows the car to perform an emergency stop or turn back. The board may recognize this kind of motion if there are significant changes in the IMUs.

[Video Here](https://youtube.com/shorts/qFvlq4UfhxE)
<div style="width:100%;height:0;position:relative;padding-bottom:64.923%;">
  <iframe
    src="https://youtube.com/shorts/qFvlq4UfhxE"
    frameborder="0"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
    allowfullscreen
    style="width:100%;height:100%;position:absolute;left:0;top:0;overflow:hidden;">
  </iframe>
</div>


# Collabration
I referenced [Lucca’s](https://correial.github.io/LuccaFastRobots/) and [Stephan's](https://fast.synthghost.com//) site for code debugging and plot formatting. I used ChatGPT for plot and website formatting/development.