# The new `task_buoy` package

1. The new `task_buoy` is capable of detecting three buoys at the same time and thresholding it.
There is a new variable that is used to switch between different buoys : 0, 1, 2 for blue, green and red. `BGR` contains six integer
namely `blue_min`, `blue_max`, `green_min`, `green_max`, `red_min` and `red_max`. These six values are required whenever we want to threshold any
three channel image. `BGR` is a two dimensional matrix(just for the sake of keeping it short and clean).

2. Another feature is that it can save all parameters from the running node itself, so there is no need to note down the values of all 
the newly calibrated values. 

3. We can calibrate all the 18 values needed for thresholding all the three buoys by just using six paramters in the dynamic reconfigure.

4. There are other parameters like `save` and `threshold` which are used to save and allow the thresholding respectively.

5. To know how to calibrate the values refer to this [link](https://github.com/ksakash/hello-world/blob/master/three_buoy.md)
