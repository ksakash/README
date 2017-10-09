# Two new header files 

## PRE_PROCESSING_H

This contains all the common functions needed by all the tasks in the `task_hadler_layer`. 

1. Function `balance_white()` is used for white balancing the raw image.

2. `color_correction()` is used to redistribute the color intesities across the image to enhance all the colors. 

3. `denoise()` is to reduce colored noise in the image.

4. `empty_contour_handler()` is used to send signals to the `motion_layer` in case the buoy has gone out of frame of the camera 
its last position with respect to the buoy.

5. `edge_case_handler()` is used to send signal to the `motion_layer` in case the buoy is within the frame of the camera but not 
outside.

6. `get_buoys_params()` gets all the values from the parameter server running in the `rosmaster`.

7. `threshold()` function helps in threholding purposes and can be used only during calibration.

8. `set_buoy_params()` sets the value of the parameters in the parameter server to be dumped in a yaml file.

9. `update_values()` helps in updating the values in the `dynamic_reconfigure` window as it is different from that displaced intially.

10. `threshold_values_update()` it puts the values, that are changed dynamically, in the `BGR` matrix.

## LIB_H

This contains all the common libraries that are used in the tasks.


  
